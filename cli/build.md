## inpack build

inpack build 用于在当前项目的主目录执行，它最终生成一个 name-version-release.dist.arch.txz 格式的包文件，通过一个实例演示 build 子命令的使用:

``` shell
git clone git@github.com:inpack/nginx.git
cd nginx
inpack build

Building
  package: nginx
  version: 1.12.2
  release: 1
  dist:    el7
  arch:    x64
  vendor:  nginx.org
  OK
    nginx-1.12.2-1.el7.x64.txz

```

成功执行后, nginx-1.12.2-1.el7.x64.txz 就是最终的包文件 (下一步将使用 [inpack push](cli/push.md) 将这个包推送到 inpack-server)

> 如果build失败，可能是本地缺少必要的依赖库: yum install readline-devel pcre-devel openssl-devel gcc curl

