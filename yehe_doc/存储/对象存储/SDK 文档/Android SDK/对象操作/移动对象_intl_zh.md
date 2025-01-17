## 简介


本文档提供关于移动对象的相关的 API 概览以及 SDK 示例代码。

| API                                                          | 操作名                       | 操作描述               |
| :----------------------------------------------------------- | :--------------------------- | :--------------------- |
| [PUT Object - Copy](https://intl.cloud.tencent.com/document/product/436/10881) | 设置对象复制（修改对象属性） | 复制文件到目标路径     |
| [DELETE Object](https://intl.cloud.tencent.com/document/product/436/7743) | 删除单个对象                 | 在存储桶中删除指定对象 |

## 移动对象

移动对象主要包括两个操作：复制源对象到目标位置，删除源对象。

由于 COS 通过存储桶名称（Bucket）和对象键（ObjectKey）来标识对象。移动对象也就意味着修改这个对象的标识，COS Java SDK 目前没有提供修改对象唯一标识名的单独接口，但是可以通过组合**复制对象**加上**删除对象**的基本操作，来达到修改对象标识的目的，从而实现移动对象。

例如将 mybucket-1250000000 这个存储桶中的 picture.jpg 这个对象移动到同个存储桶的 doc 路径下。首先可以复制 picture.jpg 对象到存储桶的 doc 路径下，即对象键设定为 doc/picture.jpg，复制完成后删除 picture.jpg 这个对象，来实现“移动”的效果。

同样的，如果想将 mybucket-1250000000 这个存储桶里的 picture.jpg 这个对象移动到 myanothorbucket-1250000000 这个存储桶里，可以先将对象复制到 myanothorbucket-1250000000  存储桶，然后删除原来的对象。

#### 示例代码

[//]: # (.cssg-snippet-delete-object)
```
final String sourceAppid = "1250000000"; //账号 appid
        final String sourceBucket = "sourcebucket-1250000000"; //"源对象所在的存储桶
        final String sourceRegion = "COS_REGION"; //源对象的存储桶所在的地域
        final String sourceKey = "sourceObject"; //源对象键
        //构造源对象属性
        CopyObjectRequest.CopySourceStruct copySource = new CopyObjectRequest.CopySourceStruct(sourceAppid, sourceBucket,
                sourceRegion, sourceKey);
        String bucket = "examplebucket-1250000000"; //目标存储桶，格式：BucketName-APPID
        String key = "exampleobject"; //目标对象的对象键
        COSXMLCopyTask copyTask = transferManager.copy(bucket, key, copySource);
        copyTask.setCosXmlResultListener(new CosXmlResultListener() {
            @Override
            public void onSuccess(CosXmlRequest request, CosXmlResult result) {
               
                // 复制成功后删除文件
                DeleteObjectRequest deleteObjectRequest = new DeleteObjectRequest(sourceBucket, sourceKey);
                cosXmlService.deleteObjectAsync(deleteObjectRequest, new CosXmlResultListener() {
                    @Override
                    public void onSuccess(CosXmlRequest cosXmlRequest, CosXmlResult cosXmlResult) {
                        // 删除文件成功
                    }

                    @Override
                    public void onFail(CosXmlRequest cosXmlRequest, CosXmlClientException e, CosXmlServiceException e1) {
                        // 删除文件失败
                    }
                });
            }
            @Override
            public void onFail(CosXmlRequest request, CosXmlClientException exception, CosXmlServiceException serviceException) {
            }
        });
```
