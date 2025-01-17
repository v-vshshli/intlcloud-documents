## 操作场景

本文档主要指导您如何创建生成包，以便将生成包文件上传部署到游戏服务器伸缩，进行游戏托管。

## 操作步骤

1. 登录 [游戏服务器伸缩控制台](https://console.cloud.tencent.com/gse/asset)，单击左侧菜单【生成包】。
2. 选择左上侧服务区域，单击【新建】。
3. 新建生成包有两种方式：页面上传、命令行上传。

### 页面上传
填写包基本信息，生成包名、生成包版本、操作系统、提交方式、上传代码，单击【确定】。
   - 生成包名：创建生成包的描述性名称，非唯一且可编辑更新。
   - 生成包版本：描述生成包版本的详细信息，可用来区分生成包不同的版本。
   - 操作系统：指定游戏服务器生成包的操作系统，生成包创建完成后无法更改，默认为 CentOS7.16。
   - 提交方式：现仅支持本地上传 ZIP 包。
   - 上传代码：生成包需 [集成 gRPC框架](https://intl.cloud.tencent.com/document/product/1055/37418)。生成包目录必须包含运行您的游戏服务器所需的所有组件，包括游戏服务器可执行文件、依赖包、安装脚本，将其打包成 ZIP 包。
     - 游戏服务器可执行文件：运行游戏服务器所需的文件。生成包可以包括多个游戏服务器可执行文件，但前提是它们都为相同平台构建的。
     - 依赖包：运行游戏服务器可执行文件所需的任何相关文件。
     - 安装脚本：用于处理在游戏服务器伸缩的托管服务器上，完成安装生成包所需执行的任务。此文件必须放置到生成包目录的根目录中，安装脚本在创建舰队过程中运行一次。详情请参考 [游戏进程启动配置](https://intl.cloud.tencent.com/document/product/1055/39885)。
![](https://main.qcloudimg.com/raw/776e5e422d9ce4afb68de44356e4a303.png)

### 命令行上传

#### 步骤1：下载脚本
下载 [uploadasset.py 脚本](https://uploadasset-1301007756.cos.ap-guangzhou.myqcloud.com/uploadasset.zip?_ga=1.169449890.2037398510.1594263380)。

#### 步骤2： 安装脚本依赖包
脚本运行需要依赖以下两个安装包运行：
-  **tencentcloud-sdk-python**
 -  安装命令：`pip install tencentcloud-sdk-python`。
 -  版本支持：支持腾讯云云API SDK Python V2.7 - V3.6版本，详情可参考 [Python SDK](https://intl.cloud.tencent.com/document/product/494/7244)。
- **cos-python-sdk-v5**
 - 安装命令：`pip install -U cos-python-sdk-v5`。
 - 版本支持：详情可参考 [Python SDK](https://intl.cloud.tencent.com/document/product/436/12269)。

#### 步骤3：运行
**1. 修改配置**
脚本中需要配置用户的认证信息，将 `secret_id` 替换为用户的 `secret_id`，并将 `secret_key` 替换为用户的 `secret_key`。

**2. 执行命令**
执行以下命令运行脚本：
```
python uploadasset.py  --local_path ./game_folder/
```
运行命令代码后，界面将出现 `help` 帮助代码，单击 `help` 可查看更多参数信息。
