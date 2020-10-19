### inPack CLI

inPack CLI 为 inPack 的命令行工具, 使用 golang 开发，在本地golang环境配置正确后，安装如下

``` shell
go install github.com/sysinner/inpack/cmd/inpack
```

安装完成后，可以检测下命令的可用性
``` shell
[action@localhost ~]$ inpack
2018/01/02 22:00:46 No Args Found
```

### 设置密钥
inpack 的部分子命令需要与 inpack-server 读写操作，这涉及权限认证，需要在本地配置访问密钥.

inpack-server 以 http/restful 方式向 inpack-cli 提供服务，其内置的权限认证基于 hooto IAM/Keys 模块, 进入用户中心，在 Keys 栏目下找到 inPack Server 的访问密钥. 将其中的 Access Key ID 和 Secret 复制到本地电脑的目标文件 ~/.inpack 中，格式如下:

``` toml
[test] # 别名
access_key_id = 5a2a6086a9719120 
access_key_secret = g7ty8ymX1CfRrbYOY6T9sMjSlAGEA2AQOMzysWrF
user = sysadmin
service_url = http://localhost:9530

[prod]
access_key_id = <....>
access_key_secret = <....>
user = sysadmin
service_url = https://www.sysinner.cn
```

> 注: 其中 [test], [prod] 为自定义别名，支持多个 inpack-server 共存

