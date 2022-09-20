# traceroute

----

[toc]



## usage

```shell
# traceroute [参数] 域名/IP
```







## 安装

```shell
[chenchen@grpc01 ~]$ whereis traceroute
traceroute:[chenchen@grpc01 ~]$ sudo yum install -y traceroute
Loaded plugins: fastestmirror
Determining fastest mirrors
 * base: mirrors.bfsu.edu.cn
 * extras: mirrors.bfsu.edu.cn
 * updates: mirrors.huaweicloud.com
base                                                                                                                                                                                                                                  | 3.6 kB  00:00:00     
docker-ce-stable                                                                                                                                                                                                                      | 3.5 kB  00:00:00     
extras                                                                                                                                                                                                                                | 2.9 kB  00:00:00     
updates                                                                                                                                                                                                                               | 2.9 kB  00:00:00     
updates/7/x86_64/primary_db                                                                                                                                                                                                           |  16 MB  00:00:02     
Resolving Dependencies
--> Running transaction check
---> Package traceroute.x86_64 3:2.0.22-2.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=============================================================================================================
 Package					Arch					Version					Repository					Size
=============================================================================================================
Installing:
 traceroute					x86_64					3:2.0.22-2.el7					base					59 k

Transaction Summary
=============================================================================================================
Install  1 Package

Total download size: 59 k
Installed size: 92 k
Downloading packages:
traceroute-2.0.22-2.el7.x86_64.rpm                                                                                                                                                                                                    |  59 kB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : 3:traceroute-2.0.22-2.el7.x86_64                                                                                                                                                                                                          1/1 
  Verifying  : 3:traceroute-2.0.22-2.el7.x86_64                                                                                                                                                                                                          1/1 

Installed:
  traceroute.x86_64 3:2.0.22-2.el7                                                                                                                                                                                                                           

Complete!
[chenchen@grpc01 ~]$ 
```





