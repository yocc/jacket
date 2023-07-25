# AWS

[toc]

## Overview

```sh
# SSH 客户端
ssh -i "press.sports.pem" ec2-user@ec2-3-26-242-80.ap-southeast-2.compute.amazonaws.com
```



## Troubleshooting

```sh
# ssh 客户端链接失败
问题:
[chenchen@dev3_10.211.21.18 ~]$ ssh -i "press.sports.pem" ec2-user@ec2-3-26-242-80.ap-southeast-2.compute.amazonaws.com
The authenticity of host 'ec2-3-26-242-80.ap-southeast-2.compute.amazonaws.com (3.26.242.80)' can't be established.
ECDSA key fingerprint is b8:55:89:de:ea:af:29:6b:55:f2:21:3b:f2:f1:a9:5a.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'ec2-3-26-242-80.ap-southeast-2.compute.amazonaws.com,3.26.242.80' (ECDSA) to the list of known hosts.
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'press.sports.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
bad permissions: ignore key: press.sports.pem
Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
[chenchen@dev3_10.211.21.18 ~]$ 

原因:
“press.sports.pem”的0644权限过于开放。
要求您的私钥文件不能被其他人访问。
此私钥将被忽略。
错误权限：忽略键：press.sports.pem
权限被拒绝（公钥、gssapi-keyex、带麦克风的gssapi）。

解决:


```



## FAQ



## See Also

