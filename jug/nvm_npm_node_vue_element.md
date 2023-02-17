# nvm npm node vue element

----

[toc]



## Overview



### nvm

#### See Also

https://github.com/nvm-sh/nvm#installing-and-updating

#### Introduction

nvm 是专门用于 Node 版本的管理工具. 通过 ~/.bashrc 在 shell 登录时运行后, 活在内存里的工具. nvm 安装后并不是二进制文件, 而是在 `~/.nvm` 目录下的一个 shell 脚本的函数方法. (通过 `~/.bashrc` 运行, 本体在 `~/.nvm` 里). 如果在公用开发机上, 以上操作需要在自己用户下执行, 不要以 root 执行.(因为 nvm 的执行是通过 `~/.bashrc` 的, 所以一定是用户级别的而不是系统级别的)

#### Install & Update

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh -k | bash
```

⚠️ **-k** 参数是 curl 关闭验证

⚠️ 升级就是重装, 新装和重装后要想生效, 均需执行 `$ . ~/.bashrc`

```sh
~/.bashrc 中被新增了以下一段
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

#### Usage

```sh
[root@344954de90f9 ~]# command -v nvm
nvm
[root@344954de90f9 ~]# nvm -v
0.39.3
[root@344954de90f9 ~]# nvm help
[root@344954de90f9 ~]# nvm ls
[root@344954de90f9 ~]# nvm ls-remote
[root@344954de90f9 ~]# nvm install node
[root@344954de90f9 ~]# nvm install node -g
[root@344954de90f9 ~]# nvm install 18
[root@344954de90f9 ~]# nvm install 17
[root@344954de90f9 ~]# nvm use 18
[root@344954de90f9 ~]# nvm alias default 19
[root@344954de90f9 ~]# nvm uninstall 17

# node 是指代最后的 Node.js 版本
```

#### 切换 Node 版本

通过 nvm 命令可以指定使用 Node 的那个版本, 其中又分为 ***临时指定*** 和 ***永久指定***

**临时切换**: `nvm use <版本号>`、`nvm use 18`

**永久切换**: `nvm alias default <版本号>` 、 `nvm alias default node`、`nvm alias default 8.1.0`



### npm

1. 自己升级自己

    ```sh
    $ npm install -g npm
    ```
    
    必须 -g, 是系统级安装, npm 是系统级的工具. 



2. 

    ```sh
    ```



3. fd 

   ```sh
   ```

4. 



### node





### vue





### element











## FAQ







## See Also







