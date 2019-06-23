## inPack Spec Define

inPack Spec 配置以文本形式放置在项目根目录的指定位置: project/inpack.spec 或 project/misc/inpack/inpack.spec , 打包程序会自动在这两个位置读取 inpack.spec 并作后续操作.

一个标准的 inpack.spec 文件可以参考一个 nginx inpack.spec 实例，如下:

``` shell
[project]
name = nginx
version = 1.12.2
vendor = nginx.org
homepage = http://nginx.org/
groups = dev/sys-srv
description = High Performance Load Balancer, Web Server, Reverse Proxy

%build

PREFIX="{{.project__prefix}}"

cd {{.inpack__pack_dir}}/deps

if [ ! -f "nginx-{{.project__version}}.tar.gz" ]; then
    wget http://nginx.org/download/nginx-{{.project__version}}.tar.gz
fi
if [ -d "nginx-{{.project__version}}" ]; then
    rm -rf nginx-{{.project__version}}
fi
tar -zxf nginx-{{.project__version}}.tar.gz


cd nginx-{{.project__version}}
./configure \
    --user=action \
    --group=action \
    --prefix=$PREFIX \
    --sbin-path=$PREFIX/bin/nginx \
    --modules-path=$PREFIX/modules \
    --conf-path=$PREFIX/conf/nginx.conf \
    --error-log-path=$PREFIX/var/log/error.log \
    --http-log-path=$PREFIX/var/log/access.log \
    --pid-path=$PREFIX/var/run.nginx.pid \
    --lock-path=$PREFIX/var/run.nginx.lock \
    --http-client-body-temp-path=$PREFIX/var/cache/nginx/client_temp \
    --http-proxy-temp-path=$PREFIX/var/cache/nginx/proxy_temp \
    --http-fastcgi-temp-path=$PREFIX/var/cache/nginx/fastcgi_temp \
    --http-uwsgi-temp-path=$PREFIX/var/cache/nginx/uwsgi_temp \
    --http-scgi-temp-path=$PREFIX/var/cache/nginx/scgi_temp \
    --with-compat \
    --with-file-aio \
    --with-threads \
    --with-http_addition_module \
    --with-http_auth_request_module \
    --with-http_dav_module \
    --with-http_flv_module \
    --with-http_gunzip_module \
    --with-http_gzip_static_module \
    --with-http_mp4_module \
    --with-http_random_index_module \
    --with-http_realip_module \
    --with-http_secure_link_module \
    --with-http_slice_module \
    --with-http_ssl_module \
    --with-http_stub_status_module \
    --with-http_sub_module \
    --with-http_v2_module \
    --with-mail \
    --with-mail_ssl_module \
    --with-stream \
    --with-stream_realip_module \
    --with-stream_ssl_module \
    --with-stream_ssl_preread_module

make -j2

des_tmp=/tmp/nginx_build_tmp
mkdir -p $des_tmp
make install DESTDIR=$des_tmp


rm -rf   {{.buildroot}}/*
cp -rp   $des_tmp/$PREFIX/* {{.buildroot}}/

strip -s {{.buildroot}}/bin/nginx

rm -f    {{.buildroot}}/bin/nginx.old

mkdir -p {{.buildroot}}/conf/conf.d/
mkdir -p {{.buildroot}}/modules
mkdir -p {{.buildroot}}/var/cache/nginx/{client_temp,proxy_temp,fastcgi_temp,uwsgi_temp,scgi_temp}

cd {{.inpack__pack_dir}}

install misc/nginx.conf.tpl             {{.buildroot}}/conf/nginx.conf

sed -i 's/{\[worker_processes\]}/1/g'             {{.buildroot}}/conf/nginx.conf
sed -i 's/{\[events_worker_connections\]}/8192/g' {{.buildroot}}/conf/nginx.conf
sed -i 's/{\[http_server_default_listen\]}/80/g'  {{.buildroot}}/conf/nginx.conf

cd {{.inpack__pack_dir}}/deps
rm -rf nginx-{{.project__version}}

%files
```

### inpack.spec - project

**[project]** 标识定义包的基本信息，比如包名,版本号,描述等等,...

``` shell
[project]
name = nginx
version = 1.12.2
vendor = nginx.org
homepage = http://nginx.org/
groups = dev/sys-srv
description = High Performance Load Balancer, Web Server, Reverse Proxy
```

其中 groups 为 inPack 的分类标识，完整的定义清单如下:

Name | Description
---|---
app/biz | Business
app/co | Collaboration
app/prod | Productivity
app/dev | Development Tools
app/other | App Others
dev/web-static | Web Frontend Static Library
dev/web-lib | Web Backend Library
dev/sys-lib | System Library
dev/db | Database Server or Service
dev/stor | Storage Server or Service
dev/sys-srv | System Server or Service
dev/sys-runtime | System Runtime Environments
dev/other | Dev Others

### inpack.spec - build
**%build** 标识定义编译脚本,　如
``` shell
%build
PREFIX="{{.project__prefix}}"

cd {{.inpack__pack_dir}}/deps

./configure --prefix=/home/action/apps/nginx
make -j4
... ...
```
在 build 脚本块中，包含动态模版参数，格式为 ```{{.var_name}}```, 其完整的参数清单如下:

Name | Description
---|---
buildroot | 打包编译的临时目录
inpack__pack_dir | 当前打包项目的物理路径
project__version | 当前打包项目的版本号
project__release | 当前打包项目的发行号
project__dist | 当前打包环境的操作系统别名 (目前仅支持 el7, 兼容 CentOS7 和 rhel 7)
project__arch | 当前打包环境的硬件架构，固定为 x64 (即 x86_64或amd64)
project__prefix | 项目安装的默认根路径，部分软件在运行时依赖这个参数，在 sysinner 系统下统一设置为 /home/action/apps/project-name


### inpack.spec - files
**%files** 标识定义静态文件,　用于批量复制相同路径的文件到目标包，一般用于静态文件，或者不需要编译的脚本类文件,如
``` shell
%files
/public/static/
/libs/
```
