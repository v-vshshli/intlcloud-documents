## 简介

本文档提供关于对象的简单操作、分块操作等其他操作相关的 API 概览以及 SDK 示例代码。

**简单操作**

| API                                                          | 操作名         | 操作描述                       |
| ------------------------------------------------------------ | -------------- | ------------------------------ |
| [GET Bucket（List Objects）](https://intl.cloud.tencent.com/document/product/436/30614) | 查询对象列表   | 查询存储桶下的部分或者全部对象 |
| [PUT Object](https://intl.cloud.tencent.com/document/product/436/7749) | 简单上传对象   | 上传一个对象至存储桶           |
| [HEAD Object](https://intl.cloud.tencent.com/document/product/436/7745) | 查询对象元数据 | 查询对象的元数据信息           |
| [GET Object](https://intl.cloud.tencent.com/document/product/436/7753) | 下载对象       | 下载一个对象至本地             |
| [PUT Object - Copy](https://intl.cloud.tencent.com/document/product/436/10881) | 设置对象复制   | 复制文件到目标路径             |
| [DELETE Object](https://intl.cloud.tencent.com/document/product/436/7743) | 删除单个对象   | 在存储桶中删除指定对象         |
| [DELETE Multiple Objects](https://intl.cloud.tencent.com/document/product/436/8289) | 删除多个对象   | 在存储桶中批量删除对象         |

**分块操作**

| API                                                          | 操作名         | 操作描述                             |
| ------------------------------------------------------------ | -------------- | ------------------------------------ |
| [List Multipart Uploads](https://intl.cloud.tencent.com/document/product/436/7736) | 查询分块上传   | 查询正在进行中的分块上传信息         |
| [Initiate Multipart Upload](https://intl.cloud.tencent.com/document/product/436/7746) | 初始化分块上传 | 初始化分块上传任务                   |
| [Upload Part](https://intl.cloud.tencent.com/document/product/436/7750) | 上传分块       | 分块上传文件                         |
| [Upload Part - Copy](https://intl.cloud.tencent.com/document/product/436/8287) | 复制分块       | 将其他对象复制为一个分块             |
| [List Parts](https://intl.cloud.tencent.com/document/product/436/7747) | 查询已上传块   | 查询特定分块上传操作中的已上传的块   |
| [Complete Multipart Upload](https://intl.cloud.tencent.com/document/product/436/7742) | 完成分块上传   | 完成整个文件的分块上传               |
| [Abort Multipart Upload](https://intl.cloud.tencent.com/document/product/436/7740) | 终止分块上传   | 终止一个分块上传操作并删除已上传的块 |

**其他操作**

| API                                                          | 操作名       | 操作描述                           |
| ------------------------------------------------------------ | ------------ | ---------------------------------- |
| [POST Object restore](https://intl.cloud.tencent.com/document/product/436/12633) | 恢复归档对象 | 将归档类型的对象取回访问           |
| [PUT Object acl](https://intl.cloud.tencent.com/document/product/436/7748) | 设置对象 ACL | 设置存储桶中某个对象的访问控制列表 |
| [GET Object acl](https://intl.cloud.tencent.com/document/product/436/7744) | 查询对象 ACL | 查询对象的访问控制列表             |

## 简单操作

### 查询对象列表

#### 功能说明

查询存储桶下的部分或者全部对象。

#### 方法原型

```cpp
cos_status_t *cos_list_object(const cos_request_options_t *options,
                              const cos_string_t *bucket, 
                              cos_list_object_params_t *params, 
                              cos_table_t **resp_headers);
```

#### 参数说明

| 参数名称           | 参数描述                                                     | 类型    |
| ------------------ | ------------------------------------------------------------ | ------- |
| options            | COS 请求选项                                                 | Struct  |
| bucket             | 存储桶名称，Bucket 的命名规则为 BucketName-APPID ，此处填写的存储桶名称必须为此格式 | String  |
| params             | 列表操作参数信息                                          | Struct  |
| encoding_type      | 规定返回值的编码方式                                         | String  |
| prefix             | 前缀匹配，用来规定返回的文件前缀地址                         | String  |
| marker             | 默认以 UTF-8 二进制顺序列出条目，所有列出条目从 marker 开始  | String  |
| delimiter          | 查询分隔符，用于对对象键进行分组                             | String  |
| max_ret            | 单次返回最大的条目数量，默认1000                             | Struct  |
| truncated          | 返回条目是否被截断，'true' 或者 'false'                      | Boolean |
| next_marker        | 假如返回条目被截断，则返回 NextMarker 就是下一个条目的起点   | String  |
| object_list        | Get Bucket 操作返回的对象信息列表                            | Struct  |
| key                | Get Bucket 操作返回的 Object 名称                            | Struct  |
| last_modified      | Get Bucket 操作返回的 Object 最后修改时间                    | Struct  |
| etag               | Get Bucket 操作返回的对象的 SHA-1 算法校验值                 | Struct  |
| size               | Get Bucket 操作返回的对象大小，单位 Byte                     | Struct  |
| owner_id           | Get Bucket 操作返回的对象拥有者 UID 信息                     | Struct  |
| storage_class      | Get Bucket 操作返回的对象存储级别                            | Struct  |
| common_prefix_list | 将 Prefix 到 delimiter 之间的相同路径归为一类，定义为 Common Prefix | Struct  |
| resp_headers       | 返回 HTTP 响应消息的头域                                     | Struct  |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```cpp
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_table_t *resp_headers = NULL;

//创建内存池
cos_pool_create(&p, NULL);

//初始化请求选项
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);

//获取对象列表
cos_list_object_params_t *list_params = NULL;
cos_list_object_content_t *content = NULL;
list_params = cos_create_list_object_params(p);
s = cos_list_object(options, &bucket, list_params, &resp_headers);
if (cos_status_is_ok(s)) {
    printf("list object succeeded\n");
    cos_list_for_each_entry(cos_list_object_content_t, content, &list_params->object_list, node) {
        key = printf("%.*s\n", content->key.len, content->key.data);
    }
} else {
    printf("list object failed\n");
}

//销毁内存池
cos_pool_destroy(p); 
```

### 简单上传对象

#### 功能说明

上传一个对象至存储桶，最大支持上传不超过5GB的对象，5GB以上对象请使用 [分块上传](#.E5.88.86.E5.9D.97.E6.93.8D.E4.BD.9C) 或 [高级接口](#.E9.AB.98.E7.BA.A7.E6.8E.A5.E5.8F.A3.EF.BC.88.E6.8E.A8.E8.8D.90.EF.BC.89) 上传。

#### 方法原型

```cpp
cos_status_t *cos_put_object_from_file(const cos_request_options_t *options,
                                       const cos_string_t *bucket, 
                                       const cos_string_t *object, 
                                       const cos_string_t *filename,
                                       cos_table_t *headers, 
                                       cos_table_t **resp_headers);
```

#### 参数说明

| 参数名称     | 参数描述                                                     | 类型   |
| ------------ | ------------------------------------------------------------ | ------ |
| options      | COS 请求选项                                                 | Struct |
| bucket       | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String |
| object       | Object 名称                                                  | String |
| filename     | Object 本地保存文件名称                                      | String |
| headers      | COS 请求附加头域                                             | Struct |
| resp_headers | 返回 HTTP 响应消息的头域                                     | Struct |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |


#### 示例1：上传对象

```cpp
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_string_t file;
cos_table_t *resp_headers = NULL;

//创建内存池
cos_pool_create(&p, NULL);

//初始化请求选项
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);

//上传对象
cos_str_set(&file, TEST_DOWNLOAD_NAME);
cos_str_set(&object, TEST_OBJECT_NAME);
s = cos_put_object_from_file(options, &bucket, &object, &file, NULL, &resp_headers);
if (cos_status_is_ok(s)) {
    printf("put object succeeded\n");
} else {
    printf("put object failed\n");
}

//销毁内存池
cos_pool_destroy(p); 
```

#### 示例2：创建文件夹

COS 上可以将以 `/` 分隔的对象路径看做一个虚拟文件夹，根据此特性，可以上传一个空的流，并且命名以 `/` 结尾，可实现在 COS 上创建一个空文件夹。

```c
cos_pool_t *p = NULL;
int is_cname = 0; 
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_table_t *resp_headers;
cos_table_t *headers = NULL;
cos_list_t buffer;
  
cos_pool_create(&p, NULL);
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);
cos_str_set(&object, "folder/");
cos_list_init(&buffer);
s = cos_put_object_from_buffer(options, &bucket, &object, 
		&buffer, headers, &resp_headers);
if (cos_status_is_ok(s)) {
    printf("put object succeeded\n");
} else {
    printf("put object failed\n");
}
cos_pool_destroy(p);
```

#### 示例3：上传文件到虚拟目录

上传由 `/` 分隔的对象名，可自动创建包含文件的文件夹。想要在此文件夹中添加新文件时，只需要在上传文件至 COS 时，将 Key 填写为此目录前缀即可。

```c
cos_pool_t *p = NULL;
int is_cname = 0; 
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_string_t file;
cos_table_t *resp_headers;
cos_table_t *headers = NULL;
cos_list_t buffer;
  
cos_pool_create(&p, NULL);
options = cos_request_options_create(p);
init_test_request_options(options, is_cname);
cos_str_set(&bucket, TEST_BUCKET_NAME);
cos_str_set(&file, "examplefile");
cos_str_set(&object, "folder/exampleobject");
s = cos_put_object_from_file(options, &bucket, &object, &file, NULL, &resp_headers);
if (cos_status_is_ok(s)) {
    printf("put object succeeded\n");
} else {
    printf("put object failed\n");
}
cos_pool_destroy(p);
```

### 查询对象元数据

#### 功能说明

查询对象的元数据信息。

#### 方法原型

```cpp
cos_status_t *cos_head_object(const cos_request_options_t *options, 
                              const cos_string_t *bucket, 
                              const cos_string_t *object,
                              cos_table_t *headers, 
                              cos_table_t **resp_headers);
```

#### 参数说明

| 参数名称     | 参数描述                                                     | 类型   |
| ------------ | ------------------------------------------------------------ | ------ |
| options      | COS 请求选项                                                 | Struct |
| bucket       | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String |
| object       | Object 名称                                                  | String |
| headers      | COS 请求附加头域                                             | Struct |
| resp_headers | 返回 HTTP 响应消息的头域                                     | Struct |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```cpp
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_table_t *resp_headers = NULL;

//创建内存池
cos_pool_create(&p, NULL);

//初始化请求选项
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);

//获取对象元数据
cos_str_set(&object, TEST_OBJECT_NAME);
s = cos_head_object(options, &bucket, &object, NULL, &resp_headers);
if (cos_status_is_ok(s)) {
    printf("head object succeeded\n");
} else {
    printf("head object failed\n");
}

//销毁内存池
cos_pool_destroy(p); 
```

### 下载对象

#### 功能说明

下载一个对象至本地。该操作需要对目标对象具有读权限或该目标对象已对所有人都开放了读权限（公有读）。

#### 方法原型

```cpp
cos_status_t *cos_get_object_to_file(const cos_request_options_t *options,
                                     const cos_string_t *bucket, 
                                     const cos_string_t *object,
                                     cos_table_t *headers, 
                                     cos_table_t *params,
                                     cos_string_t *filename, 
                                     cos_table_t **resp_headers);
```

#### 参数说明

| 参数名称     | 参数描述                                                     | 类型   |
| ------------ | ------------------------------------------------------------ | ------ |
| options      | COS 请求选项                                                 | Struct |
| bucket       | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String |
| object       | Object 名称                                                  | String |
| headers      | COS 请求附加头域                                             | Struct |
| params       | COS 请求操作参数                                             | Struct |
| filename     | Object 本地保存文件名称                                      | String |
| resp_headers | 返回 HTTP 响应消息的头域                                     | Struct |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```cpp
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_string_t file;
cos_table_t *resp_headers = NULL;

//创建内存池
cos_pool_create(&p, NULL);

//初始化请求选项
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);

//获取对象
cos_str_set(&file, TEST_DOWNLOAD_NAME);
cos_str_set(&object, TEST_OBJECT_NAME);
s = cos_get_object_to_file(options, &bucket, &object, NULL, NULL, &file, &resp_headers);
if (cos_status_is_ok(s)) {
    printf("get object succeeded\n");
} else {
    printf("get object failed\n");
}

//销毁内存池
cos_pool_destroy(p); 
```

### 设置对象复制

#### 功能说明

复制文件到目标路径。

#### 方法原型

```cpp
cos_status_t *cos_copy_object(const cos_request_options_t *options,
                              const cos_string_t *src_bucket,
                              const cos_string_t *src_object,
                              const cos_string_t *src_endpoint,
                              const cos_string_t *dest_bucket, 
                              const cos_string_t *dest_object,
                              cos_table_t *headers,
                              cos_copy_object_params_t *copy_object_param,
                              cos_table_t **resp_headers);
```

#### 参数说明

| 参数名称          | 参数描述                                                     | 类型   |
| ----------------- | ------------------------------------------------------------ | ------ |
| options           | COS 请求选项                                                 | Struct |
| src_bucket        | 源存储桶                                                     | String |
| src_object        | 源对象名称                                                   | String |
| src_endpoint      | 源对象访问域名信息                                           | String |
| dest_bucket       | 目的存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String |
| dest_object       | 目的 Object 名称                                             | String |
| headers           | COS 请求附加头域                                             | Struct |
| copy_object_param | Put Object Copy 操作参数                                     | Struct |
| etag              | 返回文件的 MD5 算法校验值                                    | String |
| last_modify       | 返回文件最后修改时间，GMT 格式                               | String |
| resp_headers      | 返回 HTTP 响应消息的头域                                     | Struct |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```cpp
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_string_t src_bucket;
cos_string_t src_object;
cos_string_t src_endpoint;
cos_table_t *resp_headers = NULL;

//创建内存池
cos_pool_create(&p, NULL);

//初始化请求选项
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);

//设置对象复制
cos_str_set(&object, TEST_OBJECT_NAME);
cos_str_set(&src_bucket, TEST_BUCKET_NAME);
cos_str_set(&src_endpoint, "ap-guangzhou.myqcloud.com");
cos_str_set(&src_object, "test.txt");

cos_copy_object_params_t *params = NULL;
params = cos_create_copy_object_params(p);
s = cos_copy_object(options, &src_bucket, &src_object, &src_endpoint, &bucket, &object, NULL, params, &resp_headers);
if (cos_status_is_ok(s)) {
    printf("put object copy succeeded\n");
} else {
    printf("put object copy failed\n");
}

//销毁内存池
cos_pool_destroy(p);  
```

### 删除单个对象

#### 功能说明

在存储桶中删除指定对象。

#### 方法原型

```cpp
cos_status_t *cos_delete_object(const cos_request_options_t *options,
                                const cos_string_t *bucket, 
                                const cos_string_t *object, 
                                cos_table_t **resp_headers);
```

#### 参数说明

| 参数名称     | 参数描述                                                     | 类型   |
| ------------ | ------------------------------------------------------------ | ------ |
| options      | COS 请求选项                                                 | Struct |
| bucket       | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String |
| object       | Object 名称                                                  | String |
| resp_headers | 返回 HTTP 响应消息的头域                                     | Struct |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |


#### 示例1：删除对象

```cpp
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_table_t *resp_headers = NULL;

//创建内存池
cos_pool_create(&p, NULL);

//初始化请求选项
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);

//删除单个对象
cos_str_set(&object, TEST_OBJECT_NAME);
s = cos_delete_object(options, &bucket, &object, &resp_headers);
if (cos_status_is_ok(s)) {
    printf("delete object succeeded\n");
} else {
    printf("delete object failed\n");
}

//销毁内存池
cos_pool_destroy(p); 

```

#### 示例2：删除文件夹

该请求不会删除文件夹内的对象，只会删除指定的 key。

```
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_table_t *resp_headers = NULL;

//创建内存池
cos_pool_create(&p, NULL);

//初始化请求选项
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);

//删除目录对象
cos_str_set(&object, "folder/");
s = cos_delete_object(options, &bucket, &object, &resp_headers);
if (cos_status_is_ok(s)) {
    printf("delete object succeeded\n");
} else {
    printf("delete object failed\n");
}

//销毁内存池
cos_pool_destroy(p); 
```



### 删除多个对象

#### 功能说明

在存储桶中批量删除对象，最大支持单次删除1000个对象。对于返回结果，COS 提供 Verbose 和 Quiet 两种结果模式。Verbose 模式将返回每个 Object 的删除结果。Quiet 模式只返回报错的 Object 信息。

#### 方法原型

```cpp
cos_status_t *cos_delete_objects(const cos_request_options_t *options,
                                 const cos_string_t *bucket, 
                                 cos_list_t *object_list, 
                                 int is_quiet,
                                 cos_table_t **resp_headers, 
                                 cos_list_t *deleted_object_list);
```

#### 参数说明

| 参数名称            | 参数描述                                                     | 类型    |
| ------------------- | ------------------------------------------------------------ | ------- |
| options             | COS 请求选项                                                 | Struct  |
| bucket              | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String  |
| object_list         | Object 待删除列表                                            | Struct  |
| key                 | 待删除 Object 名称                                           | String  |
| is_quiet            | 决定是否启动 Quiet 模式：<br>True(1)：启动 Quiet 模式，False(0)：启动 Verbose 模式。默认为 False(0) | Boolean |
| resp_headers        | 返回 HTTP 响应消息的头域                                     | Struct  |
| deleted_object_list | Object 删除信息列表                                          | Struct  |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |



#### 示例1：删除多个对象

```cpp
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_table_t *resp_headers = NULL;

//创建内存池
cos_pool_create(&p, NULL);

//初始化请求选项
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);

//设置批量删除对象
char *object_name1 = TEST_OBJECT_NAME1;
char *object_name2 = TEST_OBJECT_NAME2;
cos_object_key_t *content1 = NULL;
cos_object_key_t *content2 = NULL;
cos_list_t object_list;
cos_list_t deleted_object_list;
cos_list_init(&object_list);
cos_list_init(&deleted_object_list);
content1 = cos_create_cos_object_key(p);
cos_str_set(&content1->key, object_name1);
cos_list_add_tail(&content1->node, &object_list);
content2 = cos_create_cos_object_key(p);
cos_str_set(&content2->key, object_name2);
cos_list_add_tail(&content2->node, &object_list);

//批量删除对象
int is_quiet = COS_TRUE;
cos_str_set(&object, TEST_OBJECT_NAME);
s = cos_delete_objects(options, &bucket, &object_list, is_quiet, &resp_headers, &deleted_object_list);
if (cos_status_is_ok(s)) {
    printf("delete objects succeeded\n");
} else {
    printf("delete objects failed\n");
}

//销毁内存池
cos_pool_destroy(p); 

```

#### 示例2：删除文件夹及其文件

对象存储中本身是没有文件夹和目录的概念的，为了满足用户使用习惯，用户可通过分隔符/来模拟“文件夹”。

删除文件夹及其文件这一场景，实际在 COS 上相当于删除一批有着同样前缀的对象。目前 COS C SDK 没有提供一个接口去实现这样的操作，但是可以通过组合**查询对象列表**加上**批量删除对象**的基本操作，达到删除文件夹及其文件的效果。

```c
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_table_t *resp_headers;
int is_truncated = 1;
cos_string_t marker;
  
cos_pool_create(&p, NULL);
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);
  
//list object (get bucket)
cos_list_object_params_t *list_params = NULL;
list_params = cos_create_list_object_params(p);
cos_str_set(&list_params->encoding_type, "url");
cos_str_set(&list_params->prefix, "folder/");
cos_str_set(&marker, "");
while (is_truncated) {
	list_params->marker = marker;
	s = cos_list_object(options, &bucket, list_params, &resp_headers);
	if (!cos_status_is_ok(s)) {
		printf("list object failed, req_id:%s\n", s->req_id);
		break;
	}
	cos_list_object_content_t *content = NULL;
	cos_list_for_each_entry(cos_list_object_content_t, content, &list_params->object_list, node) {
        s = cos_delete_object(options, &bucket, &content->key, &resp_headers);
        if (!cos_status_is_ok(s)) {
            printf("delete object[%s] failed, req_id:%s\n", content->key.data, s->req_id);
        }
	}
	is_truncated = list_params->truncated;
	marker = list_params->next_marker;
}    
cos_pool_destroy(p);
```



## 分块操作

### 查询分块上传

#### 功能说明

查询正在进行中的分块上传信息。单次最多列出1000个正在进行中的分块上传。

#### 方法原型

```cpp
cos_status_t *cos_list_multipart_upload(const cos_request_options_t *options,
                                        const cos_string_t *bucket, 
                                        cos_list_multipart_upload_params_t *params, 
                                        cos_table_t **resp_headers);
```

#### 参数说明

| 参数名称              | 参数描述                                                     | 类型    |
| --------------------- | ------------------------------------------------------------ | ------- |
| options               | COS 请求选项                                                 | Struct  |
| bucket                | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String  |
| params                | List Multipart Uploads 操作参数                              | Struct  |
| encoding_type         | 规定返回值的编码方式                                         | String  |
| prefix                | 前缀匹配，用来规定返回的文件前缀地址                         | String  |
| delimiter             | 界符为一个符号：<br><li>如果有 Prefix，则将 Prefix 到 delimiter 之间的相同路径归为一类，定义为 Common Prefix，然后列出所有 Common Prefix<br><li>如果没有 Prefix，则从路径起点开始 | String  |
| max_ret               | 单次返回最大的条目数量，默认1000                             | String  |
| key_marker            | 与 upload-id-marker 一起使用：<br><li>当 upload-id-marker 未被指定时，ObjectName 字母顺序大于 key-marker 的条目将被列出<br><li>当 upload-id-marker 被指定时，ObjectName 字母顺序大于 key-marker 的条目被列出，ObjectName 字母顺序等于 key-marker 同时 UploadID 大于 upload-id-marker 的条目将被列出 | String  |
| upload_id_marker      | 与 key-marker 一起使用：<br><li>当 key-marker 未被指定时，upload-id-marker 将被忽略<br><li>当 key-marker 被指定时，ObjectName 字母顺序大于 key-marker 的条目被列出，ObjectName 字母顺序等于 key-marker 同时 UploadID 大于 upload-id-marker 的条目将被列出 | String  |
| truncated             | 返回条目是否被截断，'true' 或者 'false'                      | Boolean |
| next_key_marker       | 假如返回条目被截断，则返回 NextKeyMarker 就是下一个条目的起点   | String  |
| next_upload_id_marker | 假如返回条目被截断，则返回 UploadId 就是下一个条目的起点     | String  |
| upload_list           | 分块上传的信息                                               | Struct  |
| key                   | Object 的名称                                                | String  |
| upload_id             | 标示本次分块上传的 ID                                        | String  |
| initiated             | 标示本次分块上传任务的启动时间                               | String  |
| resp_headers          | 返回 HTTP 响应消息的头域                                     | Struct  |

```
typedef struct {
    cos_list_t node;
    cos_string_t key;
    cos_string_t upload_id;
    cos_string_t initiated;
} cos_list_multipart_upload_content_t;
```

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```cpp
cos_pool_t *p = NULL;
cos_string_t bucket;
int is_cname = 0;
cos_table_t *resp_headers = NULL;
cos_request_options_t *options = NULL;
cos_status_t *s = NULL;
cos_list_multipart_upload_params_t *list_multipart_params = NULL;

//创建内存池 & 初始化请求选项
cos_pool_create(&p, NULL);
options = cos_request_options_create(p);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
cos_str_set(&bucket, TEST_BUCKET_NAME);

//查询分块上传
list_multipart_params = cos_create_list_multipart_upload_params(p);
list_multipart_params->max_ret = 999;
s = cos_list_multipart_upload(options, &bucket, list_multipart_params, &resp_headers);
log_status(s);

//销毁内存池
cos_pool_destroy(p);

```

### 初始化分块上传

#### 功能说明

Initiate Multipart Upload 请求实现初始化分片上传，成功执行此请求以后会返回 Upload ID 用于后续的 Upload Part 请求。

#### 方法原型

```cpp
cos_status_t *cos_init_multipart_upload(const cos_request_options_t *options, 
                                        const cos_string_t *bucket, 
                                        const cos_string_t *object, 
                                        cos_string_t *upload_id, 
                                        cos_table_t *headers,
                                        cos_table_t **resp_headers);
```

#### 参数说明

| 参数名称     | 参数描述                                                     | 类型   |
| ------------ | ------------------------------------------------------------ | ------ |
| options      | COS 请求选项                                                 | Struct |
| bucket       | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String |
| object       | Object 名称                                                  | String |
| upload_id    | 操作返回的 Upload ID                                         | String |
| headers      | COS 请求附加头域                                             | Struct |
| resp_headers | 返回 HTTP 响应消息的头域                                     | Struct |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```cpp
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_string_t file;
cos_table_t *resp_headers = NULL;

//创建内存池
cos_pool_create(&p, NULL);

//初始化请求选项
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);

//初始化分块上传
cos_str_set(&object, TEST_OBJECT_NAME);
s = cos_init_multipart_upload(options, &bucket, &object, 
                              &upload_id, headers, &resp_headers);
if (cos_status_is_ok(s)) {
    printf("init multipart upload succeeded\n");
} else {
    printf("init multipart upload failed\n");
}

//销毁内存池
cos_pool_destroy(p); 
```



### 上传分块

#### 功能说明

分块上传文件。Upload Part 请求实现在初始化以后的分块上传，支持的块的数量为1 - 10000，块的大小为1MB - 5GB。在每次请求 Upload Part 时，需要携带 partNumber 和 uploadID，partNumber 为块的编号，支持乱序上传。

#### 方法原型

```cpp
cos_status_t *cos_upload_part_from_file(const cos_request_options_t *options,
                                        const cos_string_t *bucket, 
                                        const cos_string_t *object,
                                        const cos_string_t *upload_id, 
                                        int part_num, 
                                        cos_upload_file_t *upload_file,
                                        cos_table_t **resp_headers);
```

#### 参数说明

| 参数名称     | 参数描述                                                     | 类型   |
| ------------ | ------------------------------------------------------------ | ------ |
| options      | COS 请求选项                                                 | Struct |
| bucket       | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String |
| object       | Object 名称                                                  | String |
| upload_id    | 上传任务编号                                                 | String |
| part_num     | 分块编号                                                     | Int    |
| upload_file  | 待上传本地文件信息                                           | Struct |
| resp_headers | 返回 HTTP 响应消息的头域                                     | Struct |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```cpp
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_string_t file;
cos_table_t *resp_headers = NULL;
int part_num = 1;
int64_t pos = 0;
int64_t file_length = 0;

//创建内存池
cos_pool_create(&p, NULL);

//初始化请求选项
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);

//上传分块
int res = COSE_OK;
cos_upload_file_t *upload_file = NULL;
cos_file_buf_t *fb = cos_create_file_buf(p);
res = cos_open_file_for_all_read(p, TEST_MULTIPART_FILE, fb);
if (res != COSE_OK) {
    cos_error_log("Open read file fail, filename:%s\n", TEST_MULTIPART_FILE);
    return;
}
file_length = fb->file_last;
apr_file_close(fb->file);
while(pos < file_length) {
    upload_file = cos_create_upload_file(p);
    cos_str_set(&upload_file->filename, TEST_MULTIPART_FILE);
    upload_file->file_pos = pos;
    pos += 2 * 1024 * 1024;
    upload_file->file_last = pos < file_length ? pos : file_length; //2MB
    s = cos_upload_part_from_file(options, &bucket, &object, &upload_id,
                                  part_num++, upload_file, &resp_headers);

    if (cos_status_is_ok(s)) {
        printf("upload part succeeded\n");
    } else {
        printf("upload part failed\n");
    }
}

//销毁内存池
cos_pool_destroy(p); 
```

### 复制分块

#### 功能说明

将其他对象复制为一个分块。

#### 方法原型

```cpp
cos_status_t *cos_upload_part_copy(const cos_request_options_t *options,
                                   cos_upload_part_copy_params_t *params, 
                                   cos_table_t *headers, 
                                   cos_table_t **resp_headers);
```

#### 参数说明

| 参数名称     | 参数描述                                                     | 类型   |
| ------------ | ------------------------------------------------------------ | ------ |
| options      | COS 请求选项                                                 | Struct |
| params       | 复制分块参数信息                                             | Struct |
| copy_source  | 源文件路径                                                   | String |
| dest_bucket  | 目的 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String |
| dest_object  | 目的 Object 名称                                             | String |
| upload_id    | 上传任务编号                                                 | String |
| part_num     | 分块编号                                                     | Int    |
| range_start  | 源文件起始偏移                                               | Int    |
| range_end    | 源文件终止偏移                                               | Int    |
| rsp_content  | 复制分块结果信息                                             | Struct |
| etag         | 返回文件的 MD5 算法校验值                                    | String |
| last_modify  | 返回文件最后修改时间，GMT 格式                               | String |
| resp_headers | 返回 HTTP 响应消息的头域                                     | Struct |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```cpp
cos_pool_t *p = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_string_t file;
int is_cname = 0;
cos_string_t upload_id;
cos_list_upload_part_params_t *list_upload_part_params = NULL;
cos_upload_part_copy_params_t *upload_part_copy_params1 = NULL;
cos_upload_part_copy_params_t *upload_part_copy_params2 = NULL;
cos_table_t *headers = NULL;
cos_table_t *query_params = NULL;
cos_table_t *resp_headers = NULL;
cos_table_t *list_part_resp_headers = NULL;
cos_list_t complete_part_list;
cos_list_part_content_t *part_content = NULL;
cos_complete_part_content_t *complete_content = NULL;
cos_table_t *complete_resp_headers = NULL;
cos_status_t *s = NULL;
int part1 = 1;
int part2 = 2;
char *local_filename = "test_upload_part_copy.file";
char *download_filename = "test_upload_part_copy.file.download";
char *source_object_name = "cos_test_upload_part_copy_source_object";
char *dest_object_name = "cos_test_upload_part_copy_dest_object";
FILE *fd = NULL;
cos_string_t download_file;
cos_string_t dest_bucket;
cos_string_t dest_object;
int64_t range_start1 = 0;
int64_t range_end1 = 6000000;
int64_t range_start2 = 6000001;
int64_t range_end2;
cos_string_t data;

cos_pool_create(&p, NULL);
options = cos_request_options_create(p);

//创建一个10MB本地随机文件    
make_rand_string(p, 10 * 1024 * 1024, &data);
fd = fopen(local_filename, "w");
fwrite(data.data, sizeof(data.data[0]), data.len, fd);
fclose(fd);    

//使用本地文件上传对象
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
cos_str_set(&bucket, "source-1253666666");
cos_str_set(&object, "cos_test_upload_part_copy_source_object");
cos_str_set(&file, local_filename);
s = cos_put_object_from_file(options, &bucket, &object, &file, NULL, &resp_headers);
log_status(s);

//初始化分块上传
cos_str_set(&object, dest_object_name);
s = cos_init_multipart_upload(options, &bucket, &object, 
                              &upload_id, NULL, &resp_headers);
log_status(s);

//使用已上传对象复制分块1
upload_part_copy_params1 = cos_create_upload_part_copy_params(p);
cos_str_set(&upload_part_copy_params1->copy_source, "mybucket-1253666666.cn-south.myqcloud.com/cos_test_upload_part_copy_source_object");
cos_str_set(&upload_part_copy_params1->dest_bucket, TEST_BUCKET_NAME);
cos_str_set(&upload_part_copy_params1->dest_object, dest_object_name);
cos_str_set(&upload_part_copy_params1->upload_id, upload_id.data);
upload_part_copy_params1->part_num = part1;
upload_part_copy_params1->range_start = range_start1;
upload_part_copy_params1->range_end = range_end1;
headers = cos_table_make(p, 0);
s = cos_upload_part_copy(options, upload_part_copy_params1, headers, &resp_headers);
log_status(s);
printf("last modified:%s, etag:%s\n", upload_part_copy_params1->rsp_content->last_modify.data, upload_part_copy_params1->rsp_content->etag.data);

//使用已上传对象复制分块2
resp_headers = NULL;
range_end2 = get_file_size(local_filename) - 1;
upload_part_copy_params2 = cos_create_upload_part_copy_params(p);
cos_str_set(&upload_part_copy_params2->copy_source, "mybucket-1253666666.cn-south.myqcloud.com/cos_test_upload_part_copy_source_object");
cos_str_set(&upload_part_copy_params2->dest_bucket, TEST_BUCKET_NAME);
cos_str_set(&upload_part_copy_params2->dest_object, dest_object_name);
cos_str_set(&upload_part_copy_params2->upload_id, upload_id.data);
upload_part_copy_params2->part_num = part2;
upload_part_copy_params2->range_start = range_start2;
upload_part_copy_params2->range_end = range_end2;
headers = cos_table_make(p, 0);
s = cos_upload_part_copy(options, upload_part_copy_params2, headers, &resp_headers);
log_status(s);
printf("last modified:%s, etag:%s\n", upload_part_copy_params1->rsp_content->last_modify.data, upload_part_copy_params1->rsp_content->etag.data);

//列出已上传对象
list_upload_part_params = cos_create_list_upload_part_params(p);
list_upload_part_params->max_ret = 10;
cos_list_init(&complete_part_list);

cos_str_set(&dest_bucket, TEST_BUCKET_NAME);
cos_str_set(&dest_object, dest_object_name);
s = cos_list_upload_part(options, &dest_bucket, &dest_object, &upload_id, 
                         list_upload_part_params, &list_part_resp_headers);
log_status(s);
cos_list_for_each_entry(cos_list_part_content_t, part_content, &list_upload_part_params->part_list, node) {
    complete_content = cos_create_complete_part_content(p);
    cos_str_set(&complete_content->part_number, part_content->part_number.data);
    cos_str_set(&complete_content->etag, part_content->etag.data);
    cos_list_add_tail(&complete_content->node, &complete_part_list);
}

//完成分块上传
headers = cos_table_make(p, 0);
s = cos_complete_multipart_upload(options, &dest_bucket, &dest_object, 
                                  &upload_id, &complete_part_list, headers, &complete_resp_headers);
log_status(s);

//对比复制分块上传生成的对象和本地文件是否匹配
headers = cos_table_make(p, 0);
cos_str_set(&download_file, download_filename);
s = cos_get_object_to_file(options, &dest_bucket, &dest_object, headers, 
                           query_params, &download_file, &resp_headers);
log_status(s);
printf("local file len = %"APR_INT64_T_FMT", download file len = %"APR_INT64_T_FMT, get_file_size(local_filename), get_file_size(download_filename));
remove(download_filename);
remove(local_filename);

//销毁内存池
cos_pool_destroy(p);    
```



### 查询已上传块

#### 功能说明

查询特定分块上传操作中的已上传的块。

#### 方法原型

```cpp
cos_status_t *cos_list_upload_part(const cos_request_options_t *options,
                                   const cos_string_t *bucket, 
                                   const cos_string_t *object, 
                                   const cos_string_t *upload_id, 
                                   cos_list_upload_part_params_t *params,
                                   cos_table_t **resp_headers);
```

#### 参数说明

| 参数名称                | 参数描述                                                     | 类型    |
| ----------------------- | ------------------------------------------------------------ | ------- |
| options                 | COS 请求选项                                                 | Struct  |
| bucket                  | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String  |
| object                  | Object 名称                                                  | String  |
| upload_id               | 上传任务编号                                                 | String  |
| params                  | List Parts 操作参数                                          | Struct  |
| part_number_marker      | 默认以 UTF-8 二进制顺序列出条目，所有列出条目从 marker 开始  | String  |
| encoding_type           | 规定返回值的编码方式                                         | String  |
| max_ret                 | 单次返回最大的条目数量，默认1000                             | String  |
| truncated               | 返回条目是否被截断，'true' 或者 'false'                      | Boolean |
| next_part_number_marker | 假如返回条目被截断，则返回 NextMarker 就是下一个条目的起点   | String  |
| part_list               | 完成分块的信息                                               | Struct  |
| part_number             | 分块编号                                                     | String  |
| size                    | 分块大小，单位 Byte                                          | String  |
| etag                    | 分块的 SHA-1 算法校验值                                      | String  |
| last_modified           | 分块最后修改时间                                             | String  |
| resp_headers            | 返回 HTTP 响应消息的头域                                     | Struct  |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```cpp
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_string_t file;
cos_table_t *resp_headers = NULL;
cos_list_part_content_t *part_content = NULL;
cos_complete_part_content_t *complete_part_content = NULL;
int part_num = 1;
int64_t pos = 0;
int64_t file_length = 0;

//创建内存池
cos_pool_create(&p, NULL);

//初始化请求选项
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);

//查询已上传块
params = cos_create_list_upload_part_params(p);
params->max_ret = 1000;
cos_list_init(&complete_part_list);
s = cos_list_upload_part(options, &bucket, &object, &upload_id, 
                         params, &resp_headers);

if (cos_status_is_ok(s)) {
    printf("List multipart succeeded\n");
} else {
    printf("List multipart failed\n");
    cos_pool_destroy(p);
    return;
}

cos_list_for_each_entry(cos_list_part_content_t, part_content, &params->part_list, node) {
    complete_part_content = cos_create_complete_part_content(p);
    cos_str_set(&complete_part_content->part_number, part_content->part_number.data);
    cos_str_set(&complete_part_content->etag, part_content->etag.data);
    cos_list_add_tail(&complete_part_content->node, &complete_part_list);
}

//完成分块上传
s = cos_complete_multipart_upload(options, &bucket, &object, &upload_id,
                                  &complete_part_list, complete_headers, &resp_headers);

if (cos_status_is_ok(s)) {
    printf("Complete multipart upload from file succeeded, upload_id:%.*s\n", 
           upload_id.len, upload_id.data);
} else {
    printf("Complete multipart upload from file failed\n");
}

//销毁内存池
cos_pool_destroy(p); 
```






### 完成分块上传

#### 功能说明

完成整个文件的分块上传。当您已经使用 Upload Parts 上传所有块以后，您可以用该 API 完成上传。在使用该 API 时，您必须在 Body 中给出每一个块的 PartNumber 和 ETag，用来校验块的准确性。

#### 方法原型

```cpp
cos_status_t *cos_complete_multipart_upload(const cos_request_options_t *options,
                                            const cos_string_t *bucket, 
                                            const cos_string_t *object, 
                                            const cos_string_t *upload_id, 
                                            cos_list_t *part_list, 
                                            cos_table_t *headers,
                                            cos_table_t **resp_headers);
```

#### 参数说明

| 参数名称     | 参数描述                                                     | 类型   |
| ------------ | ------------------------------------------------------------ | ------ |
| options      | COS 请求选项                                                 | Struct |
| bucket       | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String |
| object       | Object 名称                                                  | String |
| upload_id    | 上传任务编号                                                 | String |
| part_list    | 完成分块上传的参数                                           | Struct |
| part_number  | 分块编号                                                     | String |
| etag         | 分块的 ETag 值，为 sha1 校验值，需要在校验值前后加上双引号，如 "3a0f1fd698c235af9cf098cb74aa25bc" | String |
| headers      | COS 请求附加头域                                             | Struct |
| resp_headers | 返回 HTTP 响应消息的头域                                     | Struct |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```cpp
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_string_t file;
cos_table_t *resp_headers = NULL;
cos_list_part_content_t *part_content = NULL;
cos_complete_part_content_t *complete_part_content = NULL;
int part_num = 1;
int64_t pos = 0;
int64_t file_length = 0;

//创建内存池
cos_pool_create(&p, NULL);

//初始化请求选项
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);

//查询已上传分块
params = cos_create_list_upload_part_params(p);
params->max_ret = 1000;
cos_list_init(&complete_part_list);
s = cos_list_upload_part(options, &bucket, &object, &upload_id, 
                         params, &resp_headers);

if (cos_status_is_ok(s)) {
    printf("List multipart succeeded\n");
} else {
    printf("List multipart failed\n");
    cos_pool_destroy(p);
    return;
}

cos_list_for_each_entry(cos_list_part_content_t, part_content, &params->part_list, node) {
    complete_part_content = cos_create_complete_part_content(p);
    cos_str_set(&complete_part_content->part_number, part_content->part_number.data);
    cos_str_set(&complete_part_content->etag, part_content->etag.data);
    cos_list_add_tail(&complete_part_content->node, &complete_part_list);
}

//完成分块上传
s = cos_complete_multipart_upload(options, &bucket, &object, &upload_id,
                                  &complete_part_list, complete_headers, &resp_headers);

if (cos_status_is_ok(s)) {
    printf("Complete multipart upload from file succeeded, upload_id:%.*s\n", 
           upload_id.len, upload_id.data);
} else {
    printf("Complete multipart upload from file failed\n");
}

//销毁内存池
cos_pool_destroy(p); 
```

### 终止分块上传

#### 功能说明

终止一个分块上传操作并删除已上传的块。当您调用 Abort Multipart Upload 时，如果有正在使用这个 Upload Parts 上传块的请求，则 Upload Parts 会返回失败。

#### 方法原型

```cpp
cos_status_t *cos_abort_multipart_upload(const cos_request_options_t *options,
                                         const cos_string_t *bucket, 
                                         const cos_string_t *object, 
                                         cos_string_t *upload_id, 
                                         cos_table_t **resp_headers);
```

#### 参数说明

| 参数名称     | 参数描述                                                     | 类型   |
| ------------ | ------------------------------------------------------------ | ------ |
| options      | COS 请求选项                                                 | Struct |
| bucket       | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String |
| object       | Object 名称                                                  | String |
| upload_id    | 上传任务编号                                                 | String |
| resp_headers | 返回 HTTP 响应消息的头域                                     | Struct |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```cpp
cos_pool_t *p = NULL;
cos_string_t bucket;
cos_string_t object;
int is_cname = 0;
cos_table_t *headers = NULL;
cos_table_t *resp_headers = NULL;
cos_request_options_t *options = NULL;
cos_string_t upload_id;
cos_status_t *s = NULL;

//创建内存池 & 初始化请求选项
cos_pool_create(&p, NULL);
options = cos_request_options_create(p);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
headers = cos_table_make(p, 1);
cos_str_set(&bucket, TEST_BUCKET_NAME);
cos_str_set(&object, TEST_MULTIPART_OBJECT);

//初始化分块上传
s = cos_init_multipart_upload(options, &bucket, &object, 
                              &upload_id, headers, &resp_headers);

if (cos_status_is_ok(s)) {
    printf("Init multipart upload succeeded, upload_id:%.*s\n", 
           upload_id.len, upload_id.data);
} else {
    printf("Init multipart upload failed\n"); 
    cos_pool_destroy(p);
    return;
}

//终止分块上传
s = cos_abort_multipart_upload(options, &bucket, &object, &upload_id, 
                               &resp_headers);

if (cos_status_is_ok(s)) {
    printf("Abort multipart upload succeeded, upload_id::%.*s\n", 
           upload_id.len, upload_id.data);
} else {
    printf("Abort multipart upload failed\n"); 
}    

//销毁内存池
cos_pool_destroy(p);
```

## 其他操作

### 恢复归档对象

#### 功能说明

将归档类型的对象取回访问。

#### 方法原型

```cpp
cos_status_t *cos_post_object_restore(const cos_request_options_t *options,
                                      const cos_string_t *bucket, 
                                      const cos_string_t *object,
                                      cos_object_restore_params_t *restore_params,
                                      cos_table_t *headers,
                                      cos_table_t *params,
                                      cos_table_t **resp_headers);
```

#### 参数说明

| 参数名称       | 参数描述                                                     | 类型   |
| -------------- | ------------------------------------------------------------ | ------ |
| options        | COS 请求选项                                                 | Struct |
| bucket         | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String |
| object         | Object 名称                                                  | String |
| restore_params | Post Object Restore 操作参数                                 | Struct |
| days           | Post Object Restore 操作设置的临时副本过期时间               | Int    |
| tier           | Post Object Restore 操作指定 CAS 支持的三种恢复类型，分别为 Expedited、Standard、Bulk | String |
| headers        | COS 请求附加头域                                             | Struct |
| params         | COS 请求操作参数                                             | Struct |
| resp_headers   | 返回 HTTP 响应消息的头域                                     | Struct |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```cpp
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_table_t *resp_headers = NULL;

//创建内存池
cos_pool_create(&p, NULL);

//初始化请求选项
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);
cos_str_set(&object, TEST_OBJECT_NAME);

//恢复归档对象
cos_object_restore_params_t *restore_params = cos_create_object_restore_params(p);
restore_params->days = 30;
cos_str_set(&restore_params->tier, "Standard");
s = cos_post_object_restore(options, &bucket, &object, restore_params, NULL, NULL, &resp_headers);
if (cos_status_is_ok(s)) {
    printf("post object restore succeeded\n");
} else {
    printf("post object restore failed\n");
}

//销毁内存池
cos_pool_destroy(p); 
```

### 设置对象 ACL

#### 功能说明

设置存储桶中某个对象的访问控制列表。

#### 方法原型

```cpp
cos_status_t *cos_put_object_acl(const cos_request_options_t *options, 
                                 const cos_string_t *bucket,
                                 const cos_string_t *object,  
                                 cos_acl_e cos_acl,
                                 const cos_string_t *grant_read,
                                 const cos_string_t *grant_write,
                                 const cos_string_t *grant_full_ctrl,
                                 cos_table_t **resp_headers);
```

#### 参数说明

| 参数名称        | 参数描述                                                     | 类型   |
| --------------- | ------------------------------------------------------------ | ------ |
| options         | COS 请求选项                                                 | Struct |
| bucket          | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String |
| object          | Object 名称                                                  | String |
| cos_acl         | 允许用户自定义权限。有效值：COS_ACL_PRIVATE(0)，COS_ACL_PUBLIC_READ(1)<br>默认值：COS_ACL_PRIVATE(0) | Enum   |
| grant_read      | 读权限授予者                                                 | String |
| grant_write     | 写权限授予者                                                 | String |
| grant_full_ctrl | 读写权限授予者                                               | String |
| resp_headers    | 返回 HTTP 响应消息的头域                                     | Struct |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```cpp
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_table_t *resp_headers = NULL;

//创建内存池
cos_pool_create(&p, NULL);

//初始化请求选项
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);

//设置对象 ACL
cos_str_set(&object, TEST_OBJECT_NAME);
cos_string_t read;
cos_str_set(&read, "id=\"qcs::cam::uin/12345:uin/12345\", id=\"qcs::cam::uin/45678:uin/45678\"");
s = cos_put_object_acl(options, &bucket, &object, cos_acl, &read, NULL, NULL, &resp_headers);
if (cos_status_is_ok(s)) {
    printf("put object acl succeeded\n");
} else {
    printf("put object acl failed\n");
}

//销毁内存池
cos_pool_destroy(p);  
```

### 查询对象 ACL

#### 功能说明

查询对象的访问控制列表。

#### 方法原型

```cpp
cos_status_t *cos_get_object_acl(const cos_request_options_t *options, 
                                 const cos_string_t *bucket,
                                 const cos_string_t *object,
                                 cos_acl_params_t *acl_param, 
                                 cos_table_t **resp_headers)
```

#### 参数说明

| 参数名称     | 参数描述                                                     | 类型   |
| ------------ | ------------------------------------------------------------ | ------ |
| options      | COS 请求选项                                                 | Struct |
| bucket       | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String |
| object       | Object 名称                                                  | String |
| acl_param    | 请求操作参数                                                 | Struct |
| owner_id     | 请求操作返回的 Bucket 持有者 ID                              | String |
| owner_name   | 请求操作返回的 Bucket 持有者的名称                           | String |
| object_list  | 请求操作返回的被授权者信息与权限信息                         | Struct |
| type         | 请求操作返回的被授权者账户类型                               | String |
| id           | 请求操作返回的被授权者用户 ID                                | String |
| name         | 请求操作返回的被授权者用户名称                               | String |
| permission   | 请求操作返回的被授权者权限信息                               | String |
| resp_headers | 返回 HTTP 响应消息的头域                                     | Struct |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```cpp
cos_pool_t *p = NULL;
int is_cname = 0;
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_table_t *resp_headers = NULL;

//创建内存池
cos_pool_create(&p, NULL);

//初始化请求选项
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);

//获取对象 ACL
cos_acl_params_t *acl_params2 = NULL;
acl_params2 = cos_create_acl_params(p);
s = cos_get_object_acl(options, &bucket, &object, acl_params2, &resp_headers);
if (cos_status_is_ok(s)) {
    printf("get object acl succeeded\n");
    printf("acl owner id:%s, name:%s\n", acl_params2->owner_id.data, acl_params2->owner_name.data);
    acl_content = NULL;
    cos_list_for_each_entry(cos_acl_grantee_content_t, acl_content, &acl_params2->grantee_list, node) {
        printf("acl grantee id:%s, name:%s, permission:%s\n", acl_content->id.data, acl_content->name.data, acl_content->permission.data);
    }
} else {
    printf("get object acl failed\n");
}

//销毁内存池
cos_pool_destroy(p); 
```

## 高级接口（推荐）

### 上传对象（断点续传）

#### 功能说明

上传接口根据用户文件的长度，自动切分数据， 降低用户的使用门槛，用户无需关心分块上传的每个步骤， 且可以对分块上传未完成的文件会进行断点续传。 

#### 方法原型

```cpp
cos_status_t *cos_resumable_upload_file(cos_request_options_t *options,
                                          cos_string_t *bucket,
                                          cos_string_t *object,
                                          cos_string_t *filepath,
                                          cos_table_t *headers,
                                          cos_table_t *params,
                                          cos_resumable_clt_params_t *clt_params,
                                          cos_progress_callback progress_callback,
                                          cos_table_t **resp_headers,
                                          cos_list_t *resp_body)
```

#### 参数说明

| 参数名称          | 参数描述                                                     | 类型     |
| ----------------- | ------------------------------------------------------------ | -------- |
| options           | COS 请求选项                                                 | Struct   |
| bucket            | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String   |
| object            | Object 名称                                                  | String   |
| filepath          | Object 本地文件名称                                          | String   |
| headers           | COS 请求附加头域                                             | Struct   |
| params            | COS 请求操作参数                                             | Struct   |
| clt_params        | 上传对象控制参数                                             | Struct   |
| part_size         | 块大小，单位为 bytes，如果用户指定的 part_size 小于 1048576（1 MB）， 由 C SDK 自动切分 | Int      |
| thread_num        | 线程池大小，默认为1                                          | Int      |
| enable_checkpoint | 是否使能断点续传                                             | Int      |
| checkpoint_path   | 当使能断点续传时，表示保存上传进度的文件路径，默认路径为`<filepath>.cp`，其中 filepath 为 Object 本地文件名称 | String   |
| progress_callback | 上传进度回调函数                                             | Function |
| resp_headers      | 返回 HTTP 响应消息的头域                                     | Struct   |
| resp_body         | 保存完成分块上传请求时返回的数据                             | Struct   |

#### 返回结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```c
cos_pool_t *p = NULL;
cos_string_t bucket;
cos_string_t object;
cos_string_t filename;
cos_status_t *s = NULL;
int is_cname = 0;
cos_table_t *headers = NULL;
cos_table_t *resp_headers = NULL;
cos_request_options_t *options = NULL;
cos_resumable_clt_params_t *clt_params;

cos_pool_create(&p, NULL);
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);

headers = cos_table_make(p, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);
cos_str_set(&object, TEST_MULTIPART_OBJECT);
cos_str_set(&filename, TEST_MULTIPART_FILE);

// 设置上传控制参数
clt_params = cos_create_resumable_clt_params_content(p, 0, 1, COS_TRUE, NULL);
// upload
s = cos_resumable_upload_file(options, &bucket, &object, &filename, headers, NULL,
						clt_params, NULL, &resp_headers, NULL);

if (cos_status_is_ok(s)) {
	printf("upload succeeded\n");
} else {
	printf("upload failed\n");
}

cos_pool_destroy(p);
```

### 下载对象（断点续传）

#### 功能说明

分块下载接口根据用户对象的长度，自动使用 Range 下载数据，实现并发下载。 

#### 方法原型

```cpp
cos_status_t *cos_resumable_download_file(cos_request_options_t *options,
                                          cos_string_t *bucket,
                                          cos_string_t *object,
                                          cos_string_t *filepath,
                                          cos_table_t *headers,
                                          cos_table_t *params,
                                          cos_resumable_clt_params_t *clt_params,
                                          cos_progress_callback progress_callback);
```

#### 参数说明

| 参数名称          | 参数描述                                                     | 类型     |
| ----------------- | ------------------------------------------------------------ | -------- |
| options           | COS 请求选项                                                 | Struct   |
| bucket            | 存储桶名称，存储桶的命名格式为 BucketName-APPID，此处填写的存储桶名称必须为此格式 | String   |
| object            | Object 名称                                                  | String   |
| filepath          | Object 本地文件名称                                          | String   |
| headers           | COS 请求附加头域                                             | Struct   |
| params            | COS 请求操作参数                                             | Struct   |
| clt_params        | 下载对象控制参数                                             | Struct   |
| part_size         | 块大小，单位为 bytes，如果用户指定的 part_size 小于 1048576（1 MB），由 C SDK 自动切分 | Int      |
| thread_num        | 线程池大小，默认为1                                          | Int      |
| enable_checkpoint | 是否使能断点续传                                             | Int      |
| checkpoint_path   | 当使能断点续传时，表示保存上传进度的文件路径，默认路径为`<filepath>.cp`，其中 filepath 为 Object 本地文件名称 | String   |
| progress_callback | 下载进度回调函数                                             | Function |

#### 结果说明

| 返回结果   | 描述        | 类型   |
| ---------- | ----------- | ------ |
| code       | 错误码      | Int    |
| error_code | 错误码内容  | String |
| error_msg  | 错误码描述  | String |
| req_id     | 请求消息 ID | String |

#### 示例

```c
cos_pool_t *p = NULL;
int is_cname = 0; 
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;
cos_string_t filepath;
cos_resumable_clt_params_t *clt_params;
      
cos_pool_create(&p, NULL);
options = cos_request_options_create(p);
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);

cos_str_set(&bucket, TEST_BUCKET_NAME);
cos_str_set(&object, TEST_MULTIPART_OBJECT);
cos_str_set(&filepath, TEST_DOWNLOAD_NAME);

clt_params = cos_create_resumable_clt_params_content(p, 5*1024*1024, 3, COS_FALSE, NULL);
s = cos_resumable_download_file(options, &bucket, &object, &filepath, NULL, NULL, clt_params, NULL);
if (cos_status_is_ok(s)) {
	printf("download succeeded\n");
} else {
	printf("download failed\n");
}
cos_pool_destroy(p);
```

### 移动对象

#### 功能说明

移动对象主要包括两个操作：复制源对象到目标位置，删除源对象。

由于 COS 通过存储桶名称（Bucket）和对象键（ObjectKey）来标识对象。移动对象也就意味着修改这个对象的标识，COS C SDK 目前没有提供修改对象唯一标识名的单独接口，但是可以通过组合**复制对象**加上**删除对象**的基本操作，来达到修改对象标识的目的，从而实现移动对象。

- 复制对象方法详细说明请参见 [设置对象复制](#.E8.AE.BE.E7.BD.AE.E5.AF.B9.E8.B1.A1.E5.A4.8D.E5.88.B6)
- 删除对象方法详细说明见：[删除对象](#.E5.88.A0.E9.99.A4.E5.8D.95.E4.B8.AA.E5.AF.B9.E8.B1.A1)

#### 示例
```c
cos_pool_t *p = NULL;                                                                    
int is_cname = 0;                                                                        
cos_status_t *s = NULL;
cos_request_options_t *options = NULL;
cos_string_t bucket;
cos_string_t object;                                                                     
cos_string_t src_object;                                                                 
cos_string_t src_endpoint;
cos_table_t *resp_headers = NULL;
  
//创建内存池
cos_pool_create(&p, NULL);                                                               
//初始化请求选项
options = cos_request_options_create(p);                                                 
options->config = cos_config_create(options->pool);
cos_str_set(&options->config->endpoint, TEST_COS_ENDPOINT);
cos_str_set(&options->config->access_key_id, TEST_ACCESS_KEY_ID);
cos_str_set(&options->config->access_key_secret, TEST_ACCESS_KEY_SECRET);
cos_str_set(&options->config->appid, TEST_APPID);
options->config->is_cname = is_cname;
options->ctl = cos_http_controller_create(options->pool, 0);
cos_str_set(&bucket, TEST_BUCKET_NAME);

//设置对象复制
cos_str_set(&object, TEST_OBJECT_NAME1);
cos_str_set(&src_endpoint, "ap-guangzhou.myqcloud.com");
cos_str_set(&src_object, "example");
cos_copy_object_params_t *params = NULL;
params = cos_create_copy_object_params(p);
s = cos_copy_object(options, &bucket, &src_object, &src_endpoint, &bucket, &object, NULL, params, &resp_headers);
if (cos_status_is_ok(s)) {
	s = cos_delete_object(options, &bucket, &src_object, &resp_headers);
} else {
	printf("move object failed\n");
}
//销毁内存池
cos_pool_destroy(p);
```
