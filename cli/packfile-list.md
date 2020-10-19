### inpack packfile-list

查询指定包所包含的所有版本文件

``` shell
inpack packfile-list --repo test --name nginx
Found 3
+-------+---------+---------+------+------+------------+
| Name  | Version | Release | Dist | Arch | Updated    |
+-------+---------+---------+------+------+------------+
| nginx | 1.18.0  | 1       | el8  | x64  | 2020-10-01 |
| nginx | 1.18.0  | 1       | el7  | x64  | 2020-10-01 |
+-------+---------+---------+------+------+------------+
```
