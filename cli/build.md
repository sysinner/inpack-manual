## inpack build

inpack build 用于在当前项目的主目录执行，它最终生成一个 name-version-release.dist.arch.txz 格式的包文件，通过一个实例演示 build 子命令的使用:

``` shell
git clone git@github.com:inpack/nginx.git
cd nginx
inpack build

Building
  package: nginx
  version: 1.18.0
  release: 1
  dist:    el8
  arch:    x64
  vendor:  nginx.org
  OK
    nginx-1.18.0-1.el8.x64.txz

```

成功执行后, nginx-1.18.0-1.el8.x64.txz 就是最终的包文件 (下一步将使用 [inpack push](cli/push.md) 将这个包推送到 inpack-server)


> 如果build失败，可能是本地缺少必要的依赖库: yum install readline-devel pcre-devel openssl-devel gcc curl


注:

* 对于 C, C++ 等依赖操作系统运行库环境的包，目前 InnerStack 支持 CentOS-8/7 和 RHEL-8/7 x86_64 位系统，所以也需要在此系统环境下打包.
* 对于基于虚拟环境运行(java JVM)，或者可编译为静态二进制的包(static make, golang, ...)，可以传入参数 ```--dist linux``` 将同时兼容以上各类运行环境。
* 对于纯脚本类，或者通过源码分发的包，可以传入 ```--dist linux --arch src``` 这可支持夸系统和CPU架构的支持(比如后期计划支持 ARM, RISC-V等)。

