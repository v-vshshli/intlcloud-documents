## 简介

Go SDK 支持客户端加密，将文件加密后再进行上传，并在下载时进行解密，适用于存储敏感数据的客户。

客户端加密只支持以下方式：
KMS 服务托管密钥：用户只需提供 KMS 服务的用户主密钥 ID（即 CMK ID）给 SDK。使用这种方式需要用户开通 KMS 服务，更多 KMS 服务信息参见 [腾讯密钥管理系统](https://intl.cloud.tencent.com/document/product/1030)。

## 注意事项


在对加密数据进行复制或者迁移时，您需要对加密元信息的完整性和正确性负责。因您维护不当导致加密元信息出错或丢失，从而导致加密数据无法解密所引起的一切损失和后果均由您自行承担。

## 上传加密流程
1. 每次上传一个文件对象前，将随机生成一个对称加密密钥。随机生成的密钥通过 KMS 服务进行加密，将加密后的结果 base64 编码存储在对象的元数据中。
2. 进行文件对象的上传，上传时在内存使用 AES256 算法加密。

## 下载解密流程

1. 获取文件元数据中加密必要的信息，Base64 解码后使用 KMS 服务（或者用户提供的密钥）进行解密，得到当时加密数据的密钥。
2. 使用密钥对下载输入流进行使用 AES256 解密，得到解密后的文件输入流。

## 请求示例
完整的示例请参见 [客户端加密 - KMS 加密完整示例]( https://github.com/tencentyun/cos-go-sdk-v5/tree/master/example/crypto/crypto_sample.go)。


#### 请求示例1：简单上传和下载

```go
// 创建原始客户端
u, _ := url.Parse("https://examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com")
b := &cos.BaseURL{BucketURL: u}
c := cos.NewClient(b, &http.Client{
    Transport: &cos.AuthorizationTransport{
        SecretID:  os.Getenv("COS_SECRETID"),
        SecretKey: os.Getenv("COS_SECRETKEY"),
    },
})

// Case1 上传对象
name := "test/example"

// 该标识信息唯一确认一个主加密密钥, 解密时，需要传入相同的标识信息
// KMS加密时，该信息设置成EncryptionContext，最大支持1024字符，如果Encrypt指定了该参数，则在Decrypt 时需要提供同样的参数
materialDesc := make(map[string]string)
//materialDesc["desc"] = "material information of your master encrypt key"

// 创建KMS客户端
kmsclient, _ := coscrypto.NewKMSClient(c.GetCredential(), "ap-guangzhou")
// 创建KMS主加密密钥，标识信息materialDesc和主密钥一一对应
kmsID := os.Getenv("KMSID")
masterCipher, _ := coscrypto.CreateMasterKMS(kmsclient, kmsID, materialDesc)
// 创建加密客户端
client := coscrypto.NewCryptoClient(c, masterCipher)

contentLength := 1024*1024*10 + 1
originData := make([]byte, contentLength)
_, err := rand.Read(originData)
f := bytes.NewReader(originData)
// 加密上传
_, err = client.Object.Put(context.Background(), name, f, nil)
log_status(err)

// 解密下载
resp, err := client.Object.Get(context.Background(), name, nil)
log_status(err)
defer resp.Body.Close()
decryptedData, _ := ioutil.ReadAll(resp.Body)
if bytes.Compare(decryptedData, originData[rangeStart:rangeEnd+1]) != 0 {
    fmt.Println("Error: encryptedData != originData")
}
```

#### 请求示例2：上传文件和下载对象到文件

```go
// 创建原始客户端
u, _ := url.Parse("https://examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com")
b := &cos.BaseURL{BucketURL: u}
c := cos.NewClient(b, &http.Client{
    Transport: &cos.AuthorizationTransport{
        SecretID:  os.Getenv("COS_SECRETID"),
        SecretKey: os.Getenv("COS_SECRETKEY"),
    },
})

// Case1 上传对象
name := "test/example"

// 该标识信息唯一确认一个主加密密钥, 解密时，需要传入相同的标识信息
// KMS加密时，该信息设置成EncryptionContext，最大支持1024字符，如果Encrypt指定了该参数，则在Decrypt 时需要提供同样的参数
materialDesc := make(map[string]string)
//materialDesc["desc"] = "material information of your master encrypt key"

// 创建KMS客户端
kmsclient, _ := coscrypto.NewKMSClient(c.GetCredential(), "ap-guangzhou")
// 创建KMS主加密密钥，标识信息materialDesc和主密钥一一对应
kmsID := os.Getenv("KMSID")
masterCipher, _ := coscrypto.CreateMasterKMS(kmsclient, kmsID, materialDesc)
// 创建加密客户端
client := coscrypto.NewCryptoClient(c, masterCipher)

// 加密上传
_, err = client.Object.PutFromFile(context.Background(), name, filepath, nil)
if err != nil {
    //ERROR
}

// 解密下载
_, err = client.Object.GetToFile(context.Background(), name, "./test.download", nil)
if err != nil {
    //ERROR
}
```

#### 请求示例3：分块上传

```go
// 创建原始客户端
u, _ := url.Parse("https://examplebucket-1250000000.cos.ap-guangzhou.myqcloud.com")
b := &cos.BaseURL{BucketURL: u}
c := cos.NewClient(b, &http.Client{
    Transport: &cos.AuthorizationTransport{
        SecretID:  os.Getenv("COS_SECRETID"),
        SecretKey: os.Getenv("COS_SECRETKEY"),
    },
})

// Case1 上传对象
name := "test/example"

// 该标识信息唯一确认一个主加密密钥, 解密时，需要传入相同的标识信息
// KMS加密时，该信息设置成EncryptionContext，最大支持1024字符，如果Encrypt指定了该参数，则在Decrypt 时需要提供同样的参数
materialDesc := make(map[string]string)
//materialDesc["desc"] = "material information of your master encrypt key"

// 创建KMS客户端
kmsclient, _ := coscrypto.NewKMSClient(c.GetCredential(), "ap-guangzhou")
// 创建KMS主加密密钥，标识信息materialDesc和主密钥一一对应
kmsID := os.Getenv("KMSID")
masterCipher, _ := coscrypto.CreateMasterKMS(kmsclient, kmsID, materialDesc)
// 创建加密客户端
client := coscrypto.NewCryptoClient(c, masterCipher)

filepath := "test"
stat, err := os.Stat(filepath)
if err !- nil {
    // ERROR
}
contentLength := stat.Size()

// 分块上传
// 每个分块需要16字节对齐，且大于等于1MB
partSize := (contentLength / 16 / 3) * 16
if partSize < int64(1024*1024) {
    partSize = int64(1024*1024)
}
cryptoCtx := coscrypto.CryptoContext{
    DataSize: contentLength,
    // 每个分块需要16字节对齐
    PartSize: partSize,
}
// 切分数据
_, chunks, _, err := cos.SplitFileIntoChunks(filepath, cryptoCtx.PartSize)
if err !- nil {
    // ERROR
}

// init mulitupload
v, _, err := client.Object.InitiateMultipartUpload(context.Background(), name, nil, &cryptoCtx)
if err !- nil {
    // ERROR
}

// part upload
optcom := &cos.CompleteMultipartUploadOptions{}
for _, chunk := range chunks {
    fd, err := os.Open(filepath)
    if err !- nil {
        // ERROR
    }        
    opt := &cos.ObjectUploadPartOptions{
        ContentLength: chunk.Size,
    }
    fd.Seek(chunk.OffSet, os.SEEK_SET)
    resp, err := client.Object.UploadPart(context.Background(), name, v.UploadID, chunk.Number, cos.LimitReadCloser(fd, chunk.Size), opt, &cryptoCtx)
    if err !- nil {
        // ERROR
    }
    optcom.Parts = append(optcom.Parts, cos.Object{
        PartNumber: chunk.Number, ETag: resp.Header.Get("ETag"),
    })
}
// complete upload
_, _, err = client.Object.CompleteMultipartUpload(context.Background(), name, v.UploadID, optcom)
if err !- nil {
    // ERROR
}

_, err = client.Object.GetToFile(context.Background(), name, "test.download", nil)
if err !- nil {
    // ERROR
}
```
