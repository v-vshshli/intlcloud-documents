### MongoDB 如何查看实例详情？
在 [实例列表](https://console.cloud.tencent.com/mongodb)，单击实例名可以进入详细信息页面查看实例详情。

### 如何访问 MongoDB 实例？
云数据库 MongoDB 提供多种语言连接方式，如 Shell，PHP，Node.js，Java，Python。
连接示例请参见 [完整的连接说明](https://intl.cloud.tencent.com/document/product/240/7092) 。

### MongoDB 的升级实例规格花费时间与实例已用容量有关吗？
升级实例规格所需的时间取决于实例已用容量，升级期间实例会发生一次切主，切主期间会出现短暂的不可访问，大约十秒左右。

### MongoDB 创建实例的流程？
可通过 [购买页](https://buy.cloud.tencent.com/mongodb) 按需选择规格大小和时长，单击【立即购买】创建实例。

### 如何在项目中查找 MongoDB 已分配项目的实例？
查找已分配项目的实例，可参考接口 [DescribeMongoDBInstances](https://intl.cloud.tencent.com/document/product/240/32137) 查询副本集实例列表。
 
### MongoDB 实例的连接数规格是多少？是否支持升级连接数？
请参见 [连接限制说明](https://intl.cloud.tencent.com/document/product/240/31183)，连接数和实例规格相关，可以通过升级规格以获取更大的连接数。

### MongoDB 如何查看实例的慢查询？
您可以使用 [控制台](https://console.cloud.tencent.com/mongodb/instance) 实例管理页面的【数据库管理】>【慢查询管理】功能获取慢查询详情。

### MongoDB 查询可创建的实例规格?
可以通过 [DescribeMongoDBProduct](https://intl.cloud.tencent.com/document/product/240/32137) 接口查询可创建的实例规格。

