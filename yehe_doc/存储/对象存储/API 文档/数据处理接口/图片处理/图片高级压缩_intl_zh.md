## 功能描述

COS 通过数据万象 imageMogr2 接口提供图片高级压缩功能。图片高级压缩可以更加高效地将图片转换为 TPG 或 HEIF 等高压缩比格式，有效降低图片传输链路及加载耗时，降低带宽及流量成本。

| 功能      | 简介                                                         |
| :-------- | :----------------------------------------------------------- |
| TPG 压缩  | TPG 是腾讯推出的自研图片格式，可将 JPG、PNG、GIF、WEBP 等格式图片转换为 TPG 格式，大幅减小图片大小。 |
| HEIF 压缩 | 针对 iOS 环境的图片使用场景，可将 JPG、PNG、GIF、WEBP 等格式图片转换为 HEIF 格式，HEIF 格式有着超高压缩率。 |

>?
>
>- TPG 是腾讯自研的图片格式，如需使用请确认**图片加载环境支持 TPG 解码**，腾讯云数据万象提供集成 TPG 解码器的 iOS、Android、[Windows](https://main.qcloudimg.com/raw/851dd252378813d250eeca5ed55ffd36/TPG_win_SDK.zip) 终端 SDK，可帮助您快速接入和使用 TPG。
>- 目前 iOS 11以上及 Android P 系统已原生支持 HEIF 格式。
>- 图片高级压缩为付费服务，由数据万象收取，具体费用请参见数据万象的 [定价文档](https://intl.cloud.tencent.com/document/product/1045/33431)。

## 接口形式

```plaintext
download_url?imageMogr2/format/<Format>
```

## 参数说明

| 参数             | 含义                                                         |
| :--------------- | :----------------------------------------------------------- |
| download_url     | 文件的访问链接，具体构成为`<BucketName-APPID>.cos.<picture region>.<domain>.com/<picture name>`， 例如`examplebucket-1250000000.cos.ap-shanghai.myqcloud.com/picture.jpeg`。 |
| /format/<Format> | 压缩格式，目标缩略图的图片格式为 TPG 或 HEIF。               |

## 实际案例

假设原图格式为 PNG，图片大小为1335.2KB，如下图所示。
![img](https://main.qcloudimg.com/raw/8d539dcbea299f55ea786feb26f5c21b.png)

将原图转换为 TPG 格式，URL 地址如下。

```plaintext
http://example-1258125638.cos.ap-shanghai.myqcloud.com/sample.png?imageMogr2/format/tpg
```

将原图转换为 HEIF 格式，URL 地址如下。

```plaintext
http://example-1258125638.cos.ap-shanghai.myqcloud.com/sample.png?imageMogr2/format/heif
```

**压缩率对比**

| 格式        | 图片大小               |
| :---------- | :--------------------- |
| PNG（原图） | 1335.2KB               |
| TPG         | 36.67KB（压缩率97.3%） |
| HEIF        | 52.87KB（压缩率96.0%） |
