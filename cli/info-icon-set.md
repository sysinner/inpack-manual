### inpack info-icon-set

为指定包设置或修改图标

``` shell
inpack info-icon-set -name nginx -type 11 -icon_path nginx.11.png
icon set /opt/src/inpack/icons/uploads/nginx.11.png
OK


inpack info-icon-set -name nginx -type 21 -icon_path nginx.21.png
icon set /opt/src/inpack/icons/uploads/nginx.21.png
OK
```

inpack-server 支持两种尺寸规格的图片, 即 -type 11 (比例 1:1)，和 -type 21 (比例 2:1)

