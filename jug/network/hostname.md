# hostname

---



## 目录

[TOC]



## 总揽

```shell
# 查询主机名
[chenchen@localhost ~]$ hostname
localhost.localdomain
[chenchen@localhost ~]$ 
# 修改主机名, 永久修改, 重启也不会变回去
[chenchen@localhost ~]$ sudo -s
[root@localhost ~]# hostnamectl set-hostname grpc01
[root@localhost ~]# 
[chenchen@localhost ~]$ hostname
grpc01
[chenchen@localhost ~]$
```

