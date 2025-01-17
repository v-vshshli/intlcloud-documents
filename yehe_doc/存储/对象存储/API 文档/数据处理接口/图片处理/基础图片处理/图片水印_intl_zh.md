## 功能描述
COS 通过数据万象 **watermark** 接口提供图片水印处理功能。目前水印图片必须指定为已存储于数据万象绑定（或新建）的存储桶中的图片。关于图片水印等图片基础处理的限制说明，请参见 [规则与限制](https://intl.cloud.tencent.com/document/product/1045/33425)。


>! 图片处理功能为收费项，由数据万象收取，详细的计费说明请参见数据万象 [计费与定价](https://intl.cloud.tencent.com/document/product/1045/33431)。
>

## 接口形式

```plaintext
download_url?watermark/1/image/<encodedURL>
           		 	/gravity/<gravity>
           		 	/dx/<dx>
           		 	/dy/<dy>
           		 	/blogo/<type>
```

>? 请忽略上面的空格与换行符。
>



## 参数说明

**操作名称**：watermark，数字1，表示水印类型为图片水印。


| 参数         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| download_url | 文件的访问链接，具体构成为`<BucketName-APPID>.cos.<picture region>.<domain>.com/<picture name>`，<br>例如 `examplebucket-1250000000.cos.ap-shanghai.myqcloud.com/picture.jpeg` |
| /image/      | 水印图片地址，需要经过 [URL 安全的 Base64 编码](https://intl.cloud.tencent.com/document/product/1045/33430)。例如，水印图片为`http://examplebucket-1250000000.cos.ap-shanghai.myqcloud.com/shuiyin_2.png`，则该处编码后的字符串为`aHR0cDovL2V4YW1wbGVidWNrZXQtMTI1MDAwMDAwMC5jb3MuYXAtc2hhbmdoYWkubXlxY2xvdWQuY29tL3NodWl5aW5fMi5wbmc`            |
| /gravity/    | 文字水印位置，九宫格位置（[参考九宫格方位图](#1) ），默认值为 SouthEast |
| /dx/         | 水平（横轴）边距，单位为像素，缺省值为0                      |
| /dy/         | 垂直（纵轴）边距，单位为像素，默认值为0                      |
| /blogo/      | 水印图适配功能，适用于水印图尺寸过大的场景（如水印墙）。一共有两种类型：<br><li>当 blogo 设置为1时，水印图会被缩放至与原图相似大小后添加<br><li>当 blogo 设置为2时，水印图会被直接裁剪至与原图相似大小后添加 |
| /scatype/    | 根据原图大小调整水印图大小。<br><li>当 scatype 设置为1时，按宽缩放<br><li>当 scatype 设置为2时，按高缩放<br><li>当 scatype 设置为3时，按面积缩放                      |
| /spcent/     | 水印图片占原图的千分比，取值为[1,1000]，默认为原水印图大小。                      |
| /ignore-error/1            | 当处理参数中携带此参数时，针对文件过大导致处理失败的场景，会直接返回原图而不报错         |

>! 指定的水印图片必须同时满足如下3个条件：  
> - 水印图片与源图片必须位于同一个存储桶下。
> - URL 需使用 COS 域名（不能使用 CDN 加速域名，例如 `examplebucket-1250000000.file.myqcloud.com/shuiyin_2.png` 不可用 ），且需保证水印图可访问（如果水印图读取权限为私有，则需要携带有效签名）。
> - URL 必须以`http://`开始，不能省略 HTTP 头，也不能填 HTTPS 头，例如`examplebucket-1250000000.cos.ap-shanghai.myqcloud.com/shuiyin_2.png`，`https://examplebucket-1250000000.cos.ap-shanghai.myqcloud.com/shuiyin_2.png` 为非法的水印 URL。
> 

<span id="1"></span>

>? 一张图片上，最多添加10张不同的图片水印。
>

## 九宫格方位图

九宫格方位图可为图片的多种操作提供位置参考。红点为各区域位置的原点（通过 gravity 参数选定各区域后位移操作会以相应远点为参照）。 
![](https://main.qcloudimg.com/raw/53a143451229b4fbdd74935afe3832d5.png)


>?
>- 当 gravity 参数设置为 center 时，dx、dy 参数无效。
>- 当 gravity 参数设置为 north 或 south 时，dx 参数无效（水印会水平居中）。
>- 当 gravity 参数设置为 west 或 east 时，dy 参数无效（水印会垂直居中）。


## 实际案例

**添加图片水印**

```plaintext
http://examples-1251000004.cos.ap-shanghai.myqcloud.com/sample.jpeg?watermark/1/image/aHR0cDovL2V4YW1wbGVzLTEyNTEwMDAwMDQucGljc2gubXlxY2xvdWQuY29tL3NodWl5aW4uanBn/gravity/southeast
```

添加图片水印后，效果如下：
![](https://main.qcloudimg.com/raw/6412c0d6eaaadc5c193515f40d736dad.jpeg)

