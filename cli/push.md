## inpack push

inpack push 用于将指定的包文件推送到 inpack-sever , 实例演示:

``` shell
inpack push --repo beta --pack_path /path/of/nginx-1.12.2-1.el7.x64.txz
PUSH /opt/src/inpack/nginx/nginx-1.12.2-3.el7.x64.txz
  ok nginx 100%
  ok nginx
```

注: 

* 这个操作需要密钥校验，如果设置错误，则会失败，具体配置方式请参考 [inPack CLI](cli/index.md)
* 通过 ```--repo beta``` 参数你可以将包推送到多个 innerstack 集群

