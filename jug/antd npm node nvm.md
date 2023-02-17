

# antd npm node nvm

---



## 目录

[TOC]



## 目标

本文档是为了说明如何从零到antd的全过程






## 过程

安装配置过程

nvm -> Node.js -> npm -> antd



### 步骤总览
1. 我们最终需要的是 antd, <https://ant.design/docs/react/introduce-cn>
2. 而 antd 官网建议需要通过 npm 来安装
3. 而 npm 是 Node.js 写的, 所以随 Node.js 安装而同时被安装的,  <https://docs.npmjs.com/downloading-and-installing-node-js-and-npm>. (其实 npm 有两种安装方式, 1, 最快, 通过安装 Node.js 时同时自动完成 npm 安装; 2, 最高级专业可持续, 通过 nvm 安装.)
4. 而 安装 Node.js 官方强烈推荐通过 nvm 来完成安装 (Node version manager)
5. 而 nvm 的安装需要先了解当前操作系统的环境, 版本, 系统位数, 安装位置, 是否可用等等



### 掌握系统环境

#### 查看系统版本
- lsb_release
```shell
[root@dev3_10.211.21.18 ~]# lsb_release -a           
bash: lsb_release: command not found		// 表示没有安装 lsb_release
[root@dev3_10.211.21.18 ~]# yum -y install redhat-lsb
[chenchen@grpc01 ~]$ lsb_release -a
LSB Version:    :core-4.1-amd64:core-4.1-noarch:cxx-4.1-amd64:cxx-4.1-noarch:desktop-4.1-amd64:desktop-4.1-noarch:printing-4.1-amd64:printing-4.1-noarch
Distributor ID: CentOS
Description:    CentOS Linux release 7.7.1908 (Core)
Release:        7.7.1908
Codename:       Core
[chenchen@grpc01 ~]$ 
```

- cat /etc/os-release
```shell
[root@dev3_10.211.21.18 ~]$ cat /etc/os-release
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```

- cat /etc/redhat-release
```shell
[root@dev3_10.211.21.18 ~]$ cat /etc/redhat-release
CentOS Linux release 7.3.1611 (Core) 
```

- rpm -q centos-release
```shell
[chenchen@dev3_10.211.21.18 ~]$ rpm -q centos-release
centos-release-7-3.1611.el7.centos.x86_64
```

#### 查看内核版本
- cat /proc/version
```shell
[chenchen@dev3_10.211.21.18 ~]$ cat /proc/version
Linux version 3.10.0-514.el7.x86_64 (builder@kbuilder.dev.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-11) (GCC) ) #1 SMP Tue Nov 22 16:42:41 UTC 2016
```

- uname -a
```shell
[chenchen@dev3_10.211.21.18 ~]$ uname -a
Linux dev5 3.10.0-514.el7.x86_64 #1 SMP Tue Nov 22 16:42:41 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux
```

#### 查看系统位数
- getconf LONG_BIT
```shell
[chenchen@dev3_10.211.21.18 ~]$ getconf LONG_BIT
64
```

- arch
```shell
[chenchen@dev3_10.211.21.18 ~]$ arch
x86_64
```



### nvm 安装配置
1. 文档: nvm 安装文档, <https://docs.npmjs.com/downloading-and-installing-node-js-and-npm#using-a-node-version-manager-to-install-node-js-and-npm>

2. git: 点击 nvm, 打开 git, <https://github.com/nvm-sh/nvm>

3. 安装: 根据 nvm git 中的文档来安装, `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash`

    ```shell
    # ~/.bashrc, 由于需要翻墙, 需要给 Linux OS 配置代理, 在 ~/.bashrc 文件中配置, 以便后面通过 curl 可以访问资源
    # http://10.210.97.118/proxy.pac
    # 10.255.0.186:3128
    # 10.81.254.21:3128
    #export http_proxy="http://10.81.254.21:3128"
    #export https_proxy="http://10.81.254.21:3128"
    export http_proxy="http://10.255.0.186:3128"
    export https_proxy="http://10.255.0.186:3128"
    # unset http_proxy, 删除环境变量
    # unset https_proxy, 删除环境变量
    
    
    # 继续 git 文档
    [chenchen@localhost ~]$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 14926  100 14926    0     0   2810      0  0:00:05  0:00:05 --:--:--  4024
    => Downloading nvm as script to '/home/chenchen/.nvm'
    
    => nvm source string already in /home/chenchen/.bashrc
    => bash_completion source string already in /home/chenchen/.bashrc
    => Close and reopen your terminal to start using nvm or run the following to use it now:
    
    export NVM_DIR="$HOME/.nvm"
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
    [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
    [chenchen@localhost ~]$ 
    ```

    ```shell
    # https://github.com/nvm-sh/nvm#installing-and-updating
    [chenchen@grpc01 ~]$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
      0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
    curl: (60) Peer's Certificate has expired.
    More details here: http://curl.haxx.se/docs/sslcerts.html
    
    curl performs SSL certificate verification by default, using a "bundle"
     of Certificate Authority (CA) public keys (CA certs). If the default
     bundle file isn't adequate, you can specify an alternate file
     using the --cacert option.
    If this HTTPS server uses a certificate signed by a CA represented in
     the bundle, the certificate verification probably failed due to a
     problem with the certificate (it might be expired, or the name might
     not match the domain name in the URL).
    If you'd like to turn off curl's verification of the certificate, use
     the -k (or --insecure) option.
    ⚠️ -k 参数是 curl 关闭验证
    [chenchen@grpc01 ~]$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh -k | bash
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 15037  100 15037    0     0  20666      0 --:--:-- --:--:-- --:--:-- 20655
    => Downloading nvm from git to '/home/chenchen/.nvm'
    => Initialized empty Git repository in /home/chenchen/.nvm/.git/
    => Compressing and cleaning up git repository
    
    => nvm source string already in /home/chenchen/.bashrc
    => bash_completion source string already in /home/chenchen/.bashrc
    => Close and reopen your terminal to start using nvm or run the following to use it now:
    
    export NVM_DIR="$HOME/.nvm"
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
    [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
    [chenchen@grpc01 ~]$ 
    ```

    

4. 检查: 安装后通过 `command -v nvm` 来检查是否安装 nvm 成功, 成功返回 nvm 这几个字, 否则都是错误

    ```shell
    [chenchen@dev3_10.211.21.18 ~]$ nvm ls		# 列出通过 nvm 已经安装的版本号
    ->      v12.0.0
    default -> node (-> v12.0.0)
    node -> stable (-> v12.0.0) (default)
    stable -> 12.0 (-> v12.0.0) (default)
    iojs -> N/A (default)
    unstable -> N/A (default)
    lts/* -> lts/dubnium (-> N/A)
    lts/argon -> v4.9.1 (-> N/A)
    lts/boron -> v6.17.1 (-> N/A)
    lts/carbon -> v8.16.0 (-> N/A)
    lts/dubnium -> v10.15.3 (-> N/A)
    [chenchen@dev3_10.211.21.18 ~]$ nvm help	# nvm help
    
    Node Version Manager
    
    Note: <version> refers to any version-like string nvm understands. This includes:
      - full or partial version numbers, starting with an optional "v" (0.10, v0.1.2, v1)
      - default (built-in) aliases: node, stable, unstable, iojs, system
      - custom aliases you define with `nvm alias foo`
    
    .
    .
    .
    
    # 20230214
    [root@28cdf5453e1e tmp]# nvm ls
                N/A
    iojs -> N/A (default)
    node -> stable (-> N/A) (default)
    unstable -> N/A (default)
    [root@28cdf5453e1e tmp]# nvm ls-remote			# 返回很多
    
    [root@28cdf5453e1e tmp]# nvm install node
    Downloading and installing node v19.6.0...
    Downloading https://nodejs.org/dist/v19.6.0/node-v19.6.0-linux-x64.tar.xz...
    ########################################################################################################################################################################################################################### 100.0%
    Computing checksum with sha256sum
    Checksums matched!
    Now using node v19.6.0 (npm v9.4.0)
    Creating default alias: default -> node (-> v19.6.0 *)
    [root@28cdf5453e1e tmp]# node -v
    v19.6.0
    [root@28cdf5453e1e tmp]# npm -v
    9.4.0
    [root@28cdf5453e1e tmp]# nvm ls
    ->      v19.6.0 *
    default -> node (-> v19.6.0 *)
    iojs -> N/A (default)
    node -> stable (-> v19.6.0 *) (default)
    stable -> 19.6 (-> v19.6.0 *) (default)
    unstable -> N/A (default)
    lts/argon -> v4.9.1 (-> N/A)
    lts/boron -> v6.17.1 (-> N/A)
    lts/carbon -> v8.17.0 (-> N/A)
    lts/dubnium -> v10.24.1 (-> N/A)
    lts/erbium -> v12.22.12 (-> N/A)
    lts/fermium -> v14.21.2 (-> N/A)
    lts/gallium -> v16.19.0 (-> N/A)
    lts/hydrogen -> v18.14.0 (-> N/A)
    lts/* -> lts/hydrogen (-> N/A)
    [root@28cdf5453e1e tmp]# 
    ```

    查看 nvm 远程 node 版本号 `$ nvm ls-remote`

5. 通常官网 git 建议重连终端, 我理解就是 source ~/.bashrc 需要执行

    ```bash
    # $HOME/.bashrc 文件末尾由上面的脚本增加了几行配置, 由于 ~/.bashrc 的修改, 所以需要重新加载
    export NVM_DIR="$HOME/.nvm"
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
    [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
    ```

6. 如果在公用开发机上, 以上操作需要在自己用户下执行, 不要以 root 执行

    nvm 安装后并不是二进制文件, 而是在 ~/.nvm 目录下的一个 shell 脚本的函数方法

    nvm 是特指 Node 版本的管理工具, 所以通过 nvm 命令可以指定使用 Node 的那个版本, 其中又分为 ==临时指定== 和 ==永久指定==

    临时切换 `nvm use <版本号>`

    永久切换 `nvm alias default <版本号>` 、 `nvm alias default node`

7. 加快 nvm install 的速度

    ```shell
    方案一：临时解决方案（每次安装替换成淘宝镜像源）
    NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node nvm install stable
    
    方案二：linux环境下设置默认镜像源
    nvm的默认配置文件安装在~/.nvm目录下，找到 nvm.sh 修改 NVM_NODEJS_ORG_MIRROR 的默认参数即可。
    
    方案三：linux下设置永久环境变量
    在 ~/.bashrc 文件中添加 export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
    ```

    

### Node 和 npm 安装配置

1. 通过 nvm 安装 Node, `nvm install node`, 通过 nvm 安装最新版本 node, 或者指定版本号安装 `nvm install 6.14.4`

    ```shell
    [chenchen@envtest01 ~]$ nvm install node # 通过 nvm 安装最新版本 node, 或者指定版本号安装 nvm install 6.14.4
    Downloading and installing node v12.0.0...
    Downloading https://nodejs.org/dist/v12.0.0/node-v12.0.0-linux-x64.tar.xz...
    ######################################################################## 100.0%
    Computing checksum with sha256sum
    Checksums matched!
    Now using node v12.0.0 (npm v6.9.0)
    Creating default alias: default -> node (-> v12.0.0)
    ```

    ```shell
    # install, update
    [chenchen@grpc01 ~]$ node -v
    v16.6.1
    [chenchen@grpc01 ~]$ nvm install node
    Downloading and installing node v17.7.2...
    Downloading http://npm.taobao.org/mirrors/node/v17.7.2/node-v17.7.2-linux-x64.tar.xz...
    ######################################################################## 100.0%
    Computing checksum with sha256sum
    Checksums matched!
    tar: bin/node: time stamp 2022-03-18 03:26:44 is 71024.677882196 s in the future
    tar: share/systemtap/tapset: time stamp 2022-03-18 03:26:44 is 71024.676103298 s in the future
    tar: share/systemtap: time stamp 2022-03-18 03:26:44 is 71024.676072077 s in the future
    tar: share/doc/node/gdbinit: time stamp 2022-03-18 03:21:13 is 70693.675916341 s in the future
    ......
    ......
    ......
    tar: include: time stamp 2022-03-18 03:26:44 is 71023.965668564 s in the future
    tar: README.md: time stamp 2022-03-18 03:26:46 is 71025.965614825 s in the future
    tar: LICENSE: time stamp 2022-03-18 03:26:46 is 71025.963899613 s in the future
    tar: CHANGELOG.md: time stamp 2022-03-18 03:26:46 is 71025.95854791 s in the future
    tar: bin/corepack: time stamp 2022-03-18 03:26:44 is 71023.958449795 s in the future
    tar: bin/npx: time stamp 2022-03-18 03:26:44 is 71023.958362106 s in the future
    tar: bin/npm: time stamp 2022-03-18 03:26:44 is 71023.958326239 s in the future
    tar: bin: time stamp 2022-03-18 03:26:44 is 71023.95831456 s in the future
    Now using node v17.7.2 (npm v8.5.2)
    [chenchen@grpc01 ~]$ node -v
    v17.7.2
    [chenchen@grpc01 ~]$ npm -v
    8.5.2
    [chenchen@grpc01 ~]$ whereis node
    node: /home/chenchen/.nvm/versions/node/v17.7.2/bin/node
    [chenchen@grpc01 ~]$ whereis npm
    npm: /home/chenchen/.nvm/versions/node/v17.7.2/bin/npm
    [chenchen@grpc01 ~]$ 
    ```

    

2. 验证 node 和 npm 是否安装成功: `$ node -v` 和 `$ npm -v`

    ```shell
    [chenchen@dev3_10.211.21.18 ~]$ node -v    # 验证 node 安装成功
    v12.0.0
    [chenchen@dev3_10.211.21.18 ~]$ npm -v    # 验证 npm 安装成功
    6.9.0
    [chenchen@dev3_10.211.21.18 ~]$ whereis node    # node 安装目录
    node: /usr/home/chenchen/.nvm/versions/node/v12.0.0/bin/node
    [chenchen@dev3_10.211.21.18 ~]$ which node
    ~/.nvm/versions/node/v12.0.0/bin/node
    [chenchen@dev3_10.211.21.18 ~]$ whereis npm    # npm 安装目录
    npm: /usr/home/chenchen/.nvm/versions/node/v12.0.0/bin/npm
    [chenchen@dev3_10.211.21.18 ~]$ which npm
    ~/.nvm/versions/node/v12.0.0/bin/npm
    [chenchen@dev3_10.211.21.18 bin]$ pwd
    /usr/home/chenchen/.nvm/versions/node/v12.0.0/bin        # node, npm 二进制命令所在位置
    [chenchen@dev3_10.211.21.18 bin]$ l
    total 42M
    680416 -rwxr-xr-x 1 chenchen chenchen  42M Apr 23 20:38 node
    680418 lrwxrwxrwx 1 chenchen chenchen   38 Apr 23 20:38 npx -> ../lib/node_modules/npm/bin/npx-cli.js
    680417 lrwxrwxrwx 1 chenchen chenchen   38 Apr 23 20:38 npm -> ../lib/node_modules/npm/bin/npm-cli.js
    680415 drwxr-xr-x 2 chenchen chenchen 4.0K Apr 23 20:38 .
    685654 drwxrwxr-x 6 chenchen chenchen 4.0K Apr 25 14:35 ..
    ```

3. 有一点要提到的是, 如果更新 node 版本的话, 那么连带 npm 版本也会一同更新了, 所以如果你只是想更新npm 版本, 那么只需运行 `npm install npm -g` 就可以单独更新 npm 版本, 而不需要更新 node 版本, 这里注意 npm 要全局安装. 事实上 npm 只有"全局"安装这一种情况. 这种"全局"是当前账号下的"全局", 即 node 安装位置.



### npm 使用

1. npm 查看本地包的版本号

    ```shell
    [chenchen@dev3_10.211.21.18 node_modules]$ npm ls moment
    kkk@1.0.0 /data1/www/htdocs/chenchen/tmp/kkk
    ├─┬ antd@3.16.6
    │ ├── moment@2.24.0  deduped
    │ ├─┬ rc-calendar@9.12.4
    │ │ └── moment@2.24.0  deduped
    │ └─┬ rc-time-picker@3.6.4
    │   └── moment@2.24.0  deduped
    └── moment@2.24.0
    ```

2. 查看 npm 下载源

    ```shell
    [chenchen@dev3_10.211.21.18 ~]$ npm config list
    ; cli configs
    metrics-registry = "https://registry.npm.taobao.org/"
    scope = ""
    user-agent = "npm/6.9.0 node/v12.0.0 linux x64"
    
    ; userconfig /usr/home/chenchen/.npmrc
    registry = "https://registry.npm.taobao.org/"
    
    ; node bin location = /usr/home/chenchen/.nvm/versions/node/v12.0.0/bin/node
    ; cwd = /usr/home/chenchen
    ; HOME = /usr/home/chenchen
    ; "npm config ls -l" to show all defaults.
    ```

    ```shell
    [chenchen@grpc01 bin]$ npm ls moment
    nvm@0.39.1 /home/chenchen/.nvm
    └── (empty)
    
    [chenchen@grpc01 bin]$ npm config list
    ; "user" config from /home/chenchen/.npmrc
    
    disturl = "https://npm.taobao.org/dist" 
    registry = "https://registry.npm.taobao.org/" 
    
    ; "project" config from /home/chenchen/.nvm/.npmrc
    
    package-lock = false 
    
    ; node bin location = /home/chenchen/.nvm/versions/node/v17.7.2/bin/node
    ; cwd = /home/chenchen/.nvm/versions/node/v17.7.2/bin
    ; HOME = /home/chenchen
    ; Run `npm config ls -l` to show all defaults.
    [chenchen@grpc01 bin]$ 
    ```

    

3. 修改 npm 下载源, 改为国内

    ```shell
    # 通过命令行修改配置
    [chenchen@dev3_10.211.21.18 ppp]$ npm config set registry https://registry.npm.taobao.org
    ```

    ```shell
    # 命令行对要安装的模块临时指定配置
    npm --registry=https://registry.npm.taobao.org install module
    
    # 可以把这个设置写到 ~/.npmrc 文件中
    registry=https://registry.npm.taobao.org/
    disturl=https://npm.taobao.org/dist
    ```

4. 清除缓存

    ```shell
    [chenchen@dev3_10.211.21.18 ppp]$ npm cache clean --force
    npm WARN using --force I sure hope you know what you are doing.
    ```

5. -g 区别

    npm 的包安装分为本地安装(local), 全局安装(global)两种, 从命令行来看, 差别仅仅是有没有 -g 而已, 比如:

    ```shell
    npm install grunt			# 本地安装
    npm install -g grunt-cli	# 全局安装
    ```

    本地安装:

    - 将安装包放在 ==工作目录 ./node_modules== 目录下 (运行 npm 时所在的目录)
    - 可以通过 require() 来引入本地安装包

    全局安装:

    - 将安装包放在 ==node 安装目录下的 ./node_modules 目录==
    - 可以直接在命令行里使用

6. 查看 npm 包管理工具

    ```shell
    [chenchen@dev3_10.211.21.18 ~]$ npm version
    {
      npm: '6.9.0',
      ares: '1.15.0',
      brotli: '1.0.7',
      cldr: '34.0',
      http_parser: '2.8.0',
      icu: '63.1',
      llhttp: '1.1.1',
      modules: '72',
      napi: '4',
      nghttp2: '1.38.0',
      node: '12.0.0',
      openssl: '1.1.1b',
      tz: '2018e',
      unicode: '11.0',
      uv: '1.28.0',
      v8: '7.4.288.21-node.16',
      zlib: '1.2.11'
    }
    ```

7. npm 自身升级:

    `npm install npm -g`

    ==这里必须用 -g, 表示全局 npm 安装==, 所谓全局是指 npm 在 node 安装目录下的 npm, 而不是自己项目的工作目录 node_modules 里的 npm. 即, npm 没有必要本地安装, 而各个 js 模块才有可能需要本地安装. 

    ```shell
    [chenchen@dev3_10.211.21.18 ppp]$ npm install npm -g
    # npm install npm@latest -g 安装最新版本
    /usr/home/chenchen/.nvm/versions/node/v12.0.0/bin/npm -> /usr/home/chenchen/.nvm/versions/node/v12.0.0/lib/node_modules/npm/bin/npm-cli.js
    /usr/home/chenchen/.nvm/versions/node/v12.0.0/bin/npx -> /usr/home/chenchen/.nvm/versions/node/v12.0.0/lib/node_modules/npm/bin/npx-cli.js
    + npm@6.9.0
    updated 1 package in 6.701s
    ```

8. cnpm

    可能对翻墙有帮助, 在任何使用 npm 的地方都能使用 cnpm

    `npm install -g cnpm --registry=https://registry.npm.taobao.org` 同理安装 cnpm, 角色等同 npm

9. 初始化 npm 安装其他软件包

    在工作区目录的根目录下执行 `npm init -y` 初始化命令后自动在当前位置生成 ==./package.json== 文件

    ```shell
    [chenchen@dev3_10.211.21.18 lll]$ npm init -y
    Wrote to /data1/www/htdocs/chenchen/tmp/lll/package.json:
    
    {
      "name": "lll",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      },
      "keywords": [],
      "author": "",
      "license": "ISC"
    }
    ```

    > 为了将来使用时不报 warn 提示, 需要将 package.json 文件手动修改, 增加 "private": true

10. package-lock.json 文件

   package-lock.json 是在 npm install 安装时生成的一份文件, 用以记录当前状态下实际安装的各个 npm package 的具体来源和版本号, 如果没有这个文件的话, 那么 npm install 将下载大版本下的最新的包，具体可参考: <https://www.cnblogs.com/cangqinglang/p/8336754.html>

11. -S 和 -D

    | 命令                          | 目录                             | package.json                 | 之后 npm install module 时 | 之后 npm install module -production 时或者注明 NODE_ENV 变量值为 production 时 |
    | ----------------------------- | -------------------------------- | ---------------------------- | -------------------------- | ------------------------------------------------------------ |
    | npm install module            | 工作区 ./node_module 目录        | 不会修改                     | 不会自动安装 module 包     | null                                                         |
    | npm install module -g         | node 安装目录 ./node_module 目录 | 不会修改                     | 不会自动安装 module 包     | null                                                         |
    | npm install module --save     | 工作区 ./node_module 目录        | 会修改, dependencies 节点    | 会自动安装 module 包       | 会自动安装                                                   |
    | npm install module --save-dev | 工作区 ./node_module 目录        | 会修改, DevDependencies 节点 | 会自动安装 module 包       | 不会自动安装                                                 |

    都是作用于 package.json 文件中的 dependencies 和 devDependencies 节点, 在 npm install 时只会自动安装 dependencies 节点下的包, 所以开发使用的工具包都应该放在 devDependencies 节点下

    dependencies 与 devDependencies 有什么区别:

    devDependencies     里面的插件只用于==开发==环境，不用于生产环境

    dependencies            是需要发布到==生产==环境的

    由于是自动写入 package.json 文件, 省去了手动修改 package.json 文件的步骤

    ```shell
    npm install module_name -S    # 即, npm install module_name --save     写入dependencies
    npm install module_name -D    # 即, npm install module_name --save-dev 写入devDependencies
    npm install module_name -g 	  # 全局安装(命令行使用, 安装在 node 安装目录的 ./node_modules 下)
    npm install module_name 	  # 本地安装(将安装包放在工作区 ./node_modules 下)
    ```

12. 常见命令

    ```shell
    # 查看 npm 仓库地址是否为国内:
    npm config list
    
    # 修改为国内:
    npm config set registry https://registry.npm.taobao.org
    
    # 清除npm缓存:
    npm cache clean --force
    
    # 帮助文档
    npm help
    npm help list
    npm help cache
    
    # 查看已经安装了那些包
    npm list
    npm ls		# 同上, 写法不同
    
    # 查看 npm 本地和全局 都安装了那些包, 其中 --dept 0 是简化显示, 便于人看
    [chenchen@dev3_10.211.21.18 ppp]$ npm ls --dept 0    	# 查看本地局部
    ppp@1.0.0 /data1/www/htdocs/chenchen/tmp/ppp
    ├── babel-cli@6.26.0
    └── babel-preset-react-app@3.1.2
    
    [chenchen@dev3_10.211.21.18 ppp]$ npm ls --dept 0 -g    # 查看本地全局
    /usr/home/chenchen/.nvm/versions/node/v12.0.0/lib
    └── npm@6.9.0
    
    # 查找远程 npm 仓库支持那些 包
    npm search <keyword>
    npm search express
    
    # 查看远程包
    npm info <packageName>
    npm view <packageName> versions --json
    
    # 安装
    npm install express			# 本地, 工作目录下
    npm install express -g		# 全局, node 安装目录下
    
    # 卸载
    npm uninstall express
    
    # 更新
    npm update express
    ```



### antd 安装和配置

- 安装大纲

```shell
mkdir myapp
cd myapp
npm init -y
vim package.json		# private: true
npm install --save-dev webpack webpack-cli @babel/preset-react babel-loader @babel/core @babel/preset-env webpack-dev-server css-loader style-loader
# npm install @babel/plugin-proposal-class-properties --save-dev    # 为了认出更多语法
npm install react react-dom react-hot-loader moment antd

npm update core-js
npm install babel-plugin-import --save-dev          # 减少加载
```

- 安装快照

```shell
[chenchen@dev3_10.211.21.18 lll]$ npm init -y
Wrote to /data1/www/htdocs/chenchen/tmp/lll/package.json:

{
  "name": "lll",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}


[chenchen@dev3_10.211.21.18 lll]$ l
total 12K
25691676 drwxr-xr-x 21 chenchen www      4.0K Jun 17 18:17 ..
25695265 drwxrwxr-x  2 chenchen chenchen 4.0K Jun 18 17:04 .
25695266 -rw-rw-r--  1 chenchen chenchen  217 Jun 18 17:04 package.json
[chenchen@dev3_10.211.21.18 lll]$ vim package.json 
[chenchen@dev3_10.211.21.18 lll]$ npm install --save-dev webpack webpack-cli @babel/preset-react babel-loader @babel/core @babel/preset-env webpack-dev-server css-loader style-loader

> core-js-pure@3.1.4 postinstall /data1/www/htdocs/chenchen/tmp/lll/node_modules/core-js-pure
> node scripts/postinstall || echo "ignore"

Thank you for using core-js ( https://github.com/zloirock/core-js ) for polyfilling JavaScript standard library!

The project needs your help! Please consider supporting of core-js on Open Collective or Patreon: 
> https://opencollective.com/core-js 
> https://www.patreon.com/zloirock 

Also, the author of core-js ( https://github.com/zloirock ) is looking for a good job -)


> webpack-cli@3.3.4 postinstall /data1/www/htdocs/chenchen/tmp/lll/node_modules/webpack-cli
> node ./bin/opencollective.js



                            Thanks for using webpack!
                 Please consider donating to our Open Collective
                        to help us maintain this package.



                 Donate: https://opencollective.com/webpack/donate


npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.9 (node_modules/fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.9: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})

+ css-loader@3.0.0
+ babel-loader@8.0.6
+ @babel/preset-env@7.4.5
+ @babel/preset-react@7.0.0
+ webpack-dev-server@3.7.2
+ webpack-cli@3.3.4
+ style-loader@0.23.1
+ @babel/core@7.4.5
+ webpack@4.34.0
added 670 packages from 370 contributors and audited 10243 packages in 28.226s
found 0 vulnerabilities

[chenchen@dev3_10.211.21.18 lll]$ npm install react react-dom react-hot-loader moment antd
npm WARN deprecated core-js@1.2.7: core-js@<2.6.8 is no longer maintained. Please, upgrade to core-js@3 or at least to actual version of core-js@2.

> core-js@2.6.9 postinstall /data1/www/htdocs/chenchen/tmp/lll/node_modules/core-js
> node scripts/postinstall || echo "ignore"

Thank you for using core-js ( https://github.com/zloirock/core-js ) for polyfilling JavaScript standard library!

The project needs your help! Please consider supporting of core-js on Open Collective or Patreon: 
> https://opencollective.com/core-js 
> https://www.patreon.com/zloirock 

Also, the author of core-js ( https://github.com/zloirock ) is looking for a good job -)

npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.9 (node_modules/fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.9: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})

+ react@16.8.6
+ moment@2.24.0
+ react-hot-loader@4.11.1
+ react-dom@16.8.6
+ antd@3.19.5
added 119 packages from 206 contributors and audited 12359 packages in 15.401s
found 0 vulnerabilities

[chenchen@dev3_10.211.21.18 lll]$ npm update core-js
[chenchen@dev3_10.211.21.18 lll]$ npm install babel-plugin-import --save-dev
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.9 (node_modules/fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.9: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})

+ babel-plugin-import@1.12.0
added 3 packages from 3 contributors and audited 12367 packages in 8.579s
found 0 vulnerabilities

[chenchen@dev3_10.211.21.18 lll]$ l
total 300K
25691676 drwxr-xr-x  21 chenchen www      4.0K Jun 17 18:17 ..
25828404 drwxrwxr-x 584 chenchen chenchen  20K Jun 18 17:23 node_modules
25695266 -rw-rw-r--   1 chenchen chenchen  743 Jun 18 17:23 package.json
25695268 -rw-rw-r--   1 chenchen chenchen 267K Jun 18 17:23 package-lock.json
25695265 drwxrwxr-x   3 chenchen chenchen 4.0K Jun 18 17:23 .
```



### webpack

另见: 



### pnpm

> Fast, disk space efficient package manager

本质上他是一个包管理工具，和npm/yarn没有区别，主要优势在于

- 包安装速度极快
- 磁盘空间利用效率高

使用方法:

```undefined
npm i pnpm -g
```

参见: 

https://pnpm.io/

https://pnpm.io/zh/motivation



## FAQ

### nvm 安装和升级

```shell
# Troubleshooting on Linux
# 安装使用 nvm
# See Also: https://github.com/nvm-sh/nvm
We strongly recommend using a Node version manager like nvm to install Node.js and npm. 
Node version manager === nvm
https://docs.npmjs.com/downloading-and-installing-node-js-and-npm

1. nvm 安装
两种安装方式:
1.1. 手动执行 'install script' (安装器) 一个 shell 脚本, https://github.com/nvm-sh/nvm/blob/v0.39.3/install.sh
1.2. 执行 cURL or Wget 命令
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
1.3. 以上两种方法都是克隆nvm仓库到 '~/.nvm' 目录, 并且向(~/.bash_profile, ~/.zshrc, ~/.profile, or ~/.bashrc)任意文件中添加片段
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
1.4. 自定义安装
可以自定义的变量如下, 四个:
安装来源, install source, NVM_SOURCE, 可以通过 git , curl, wget 任何方式下载
安装目录, directory, NVM_DIR, # 注意结尾一定不能有斜线
系统环境配置文件, profile, PROFILE
安装的版本, version, NODE_VERSION
例如:
curl ... | NVM_DIR="path/to/nvm"

2. 验证安装
command -v nvm
command 命令: 调用指定的指令并执行, 命令执行时不查询 shell 函数. command 命令只能够执行 shell 内部的命令.
-v 打印 COMMAND 命令的描述, 和 `type` 相似
-V
-p

$ command echo "hello world"
hello world
$ command pwd
/home/deng

3. Git 安装
3.1. 克隆仓库到 HOME 根
cd ~/
git clone https://github.com/nvm-sh/nvm.git .nvm
3.2. 迁出
cd ~/.nvm
git checkout v0.39.3
3.3. 启动 
. ./nvm.sh
3.4. 手动添加到 ~/.bashrc, ~/.profile, or ~/.zshrc, 以便登录shell时自动启动
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion



4. 纯手动安装, 基本和 Git 安装类似
4.1. 命令行批处理
export NVM_DIR="$HOME/.nvm" && (
  git clone https://github.com/nvm-sh/nvm.git "$NVM_DIR"		# 克隆
  cd "$NVM_DIR"		# 进入 ~/.nvm 安装目录
  git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)` # 迁出
) && \. "$NVM_DIR/nvm.sh"	# 执行, && 前面的命令成功才执行后面的命令
4.2. 手动添加到 ~/.bashrc, ~/.profile, or ~/.zshrc, 以便登录shell时自动启动
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

5. 手动升级
5.1. 通过 Git 手动升级
(
  cd "$NVM_DIR"		# 1. 进入到 ~/.nvm 安装目录
  git fetch --tags origin		# 查看远程最新版本列表
  git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)`		# 迁出指定tag
) && \. "$NVM_DIR/nvm.sh"		# 执行, && 前面的命令成功才执行后面的命令
# 迁出指定 tag: git checkout , 这里虽然 有 0.9.0版本但实际是还是 0.39.3是 @latest
# 手动修改 ./nvm.sh 文件为可执行, chmod 764, 664, 执行
# 执行后, 需要再把 ~/.bashrc 重新加载一下. 否则 nvm --version 还是老的 ⚠️
# 我找不到 nvm 安装到哪里了, 我感觉根据 ~/.bashrc 里的 nvm 配置, 感觉是 shell 一登录就自动加载到内存了, 并不是物理文件.

```



```shell
# Troubleshooting on Linux
# 通过 nvm 安装 Node.js 和 npm

1. 安装最新版本
nvm install node

2. 安装特定版本
nvm install 14.7.0

3. 列出可用的远程版本
nvm ls-remote

4. 切换版本环境
nvm use node		# 自动创建一个新shell环境下使用指定的版本, node 就是指本地最新的版本, 版本号就是本地特指
nvm use 14
nvm run node --version
nvm exec 4.2 node --version

5. 查看不同版本的 Node.js 安装在哪里
nvm which 12.22

更多参见: https://github.com/nvm-sh/nvm#manual-install

例如:
[chenchen@grpc01 vnjs]$ nvm install node
Downloading and installing node v19.6.0...
Downloading http://npm.taobao.org/mirrors/node/v19.6.0/node-v19.6.0-linux-x64.tar.xz...
######################################################################## 100.0%
Computing checksum with sha256sum
Checksums matched!
Now using node v19.6.0 (npm v)

[chenchen@grpc01 node]$ pwd
/home/chenchen/.nvm/versions/node				# 在 .nvm 安装目录下可以看到多出了v19.6.0
[chenchen@grpc01 node]$ l
total 0
   595688 drwxrwxr-x. 6 chenchen chenchen 108 Aug  9  2021 v16.6.1
  8460115 drwxrwxr-x. 3 chenchen chenchen  18 Aug  9  2021 ..
419454858 drwxrwxr-x. 6 chenchen chenchen 108 Mar 17  2022 v17.7.2
398514831 drwxrwxr-x. 6 chenchen chenchen 108 Feb  8 11:04 v19.6.0
 13612177 drwxrwxr-x. 5 chenchen chenchen  51 Feb  8 11:04 .
[chenchen@grpc01 node]$ 	



```



```shell
# Troubleshooting on Linux
# npm 使用
1. npm 使用前需要先安装 Node.js 和 npm
We strongly recommend using a Node version manager like nvm to install Node.js and npm. 
Node version manager === nvm
https://docs.npmjs.com/downloading-and-installing-node-js-and-npm

1.1 已有 npm, 为了版本最新, 自我升级
npm install -g npm

2. 查看 Node.js 和 npm 版本
node -v
npm -v

3. npm 官方强力推荐用 nvm 来安装 Node.js 和 npm
https://github.com/nvm-sh/nvm
nvm 支持在同一环境上安装不同版本的 Node.js
$ nvm use 16			# 使用 Node.js 16 版本, 前提是已经安装了 16 版本
Now using node v16.9.1 (npm v7.21.1)
$ node -v
v16.9.1
$ nvm use 14			# 使用 Node.js 14 版本, 前提是已经安装了 14 版本
Now using node v14.18.0 (npm v6.14.15)
$ node -v
v14.18.0
$ nvm install 12	# 安装 Node.js 12 版本, 之前没有安装过
Now using node v12.22.6 (npm v6.14.5)
$ node -v
v12.22.6




```

### GLIBC `GLIBC_2.28' not found (required by node)

```shell
[chenchen@grpc01 node]$ nvm use 17
Now using node v17.7.2 (npm v8.5.2)
[chenchen@grpc01 node]$ npm -v
8.5.2
[chenchen@grpc01 node]$ nvm use 19
Now using node v19.6.0 (npm v)
[chenchen@grpc01 node]$ node -v
node: /lib64/libm.so.6: version `GLIBC_2.27' not found (required by node)
node: /lib64/libc.so.6: version `GLIBC_2.25' not found (required by node)
node: /lib64/libc.so.6: version `GLIBC_2.28' not found (required by node)
node: /lib64/libstdc++.so.6: version `CXXABI_1.3.9' not found (required by node)
node: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.20' not found (required by node)
node: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.21' not found (required by node)
[chenchen@grpc01 node]$ 
[chenchen@grpc01 node]$ nvm use 17
Now using node v17.7.2 (npm v8.5.2)
[chenchen@grpc01 node]$ node -v
v17.7.2
[chenchen@grpc01 node]$ npm -v
8.5.2
[chenchen@grpc01 node]$ nvm -v
0.39.3
```

### npm install element-plus --save

```shell
[chenchen@grpc01 vnjs]$ npm install element-plus --save
npm WARN deprecated sourcemap-codec@1.4.8: Please use @jridgewell/sourcemap-codec instead

added 43 packages in 18s
npm notice 
npm notice New major version of npm available! 8.5.2 -> 9.4.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v9.4.2
npm notice Run npm install -g npm@9.4.2 to update!
npm notice 
[chenchen@grpc01 vnjs]$ npm install npm
npm WARN EBADENGINE Unsupported engine {
npm WARN EBADENGINE   package: 'npm@9.4.2',
npm WARN EBADENGINE   required: { node: '^14.17.0 || ^16.13.0 || >=18.0.0' },
npm WARN EBADENGINE   current: { node: 'v17.7.2', npm: '8.5.2' }
npm WARN EBADENGINE }

added 1 package in 4s

16 packages are looking for funding
  run `npm fund` for details
[chenchen@grpc01 vnjs]$ npm fund
vnjs
├─┬ https://github.com/chalk/chalk?sponsor=1
│ │ └── chalk@4.1.2
│ └── https://github.com/chalk/ansi-styles?sponsor=1
│     └── ansi-styles@4.3.0
├── https://github.com/sponsors/sibiraj-s
│   └── ci-info@3.7.1
├── https://github.com/sponsors/isaacs
│   └── glob@7.2.3, minimatch@6.1.6, json-stringify-nice@1.1.4, promise-all-reject-late@1.0.1, promise-call-limit@1.0.1, rimraf@3.0.2
├── https://github.com/sponsors/sindresorhus
│   └── p-map@4.0.0, defaults@1.0.4
├── https://github.com/sponsors/ljharb
│   └── is-core-module@2.11.0
└── https://github.com/sponsors/feross
    └── buffer@6.0.3, base64-js@1.5.1, ieee754@1.2.1

[chenchen@grpc01 vnjs]$ 
```

### npm install npm

```shell
问题:
[chenchen@grpc01 v3ep]$ npm install npm
npm WARN EBADENGINE Unsupported engine {
npm WARN EBADENGINE   package: 'npm@9.4.2',
npm WARN EBADENGINE   required: { node: '^14.17.0 || ^16.13.0 || >=18.0.0' },
npm WARN EBADENGINE   current: { node: 'v17.7.2', npm: '8.5.2' }
npm WARN EBADENGINE }

[chenchen@grpc01 v3ep]$ node -v
node: /lib64/libm.so.6: version `GLIBC_2.27' not found (required by node)
node: /lib64/libc.so.6: version `GLIBC_2.25' not found (required by node)
node: /lib64/libc.so.6: version `GLIBC_2.28' not found (required by node)
node: /lib64/libstdc++.so.6: version `CXXABI_1.3.9' not found (required by node)
node: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.20' not found (required by node)
node: /lib64/libstdc++.so.6: version `GLIBCXX_3.4.21' not found (required by node)

原因:
通过 npm 升级 npm 自身, 提示 WARN, 因为是 node 版本不够, 需要先升级 node 到 18 以上
本质上是 CentOS7 yum 最高到 GLIBC2.17, 需要先升级 GLIBC到 2.28, CentOS8 yum 自带 GLIBC 2.28

解决:
# 下载并解压 glibc-2.28
$ wget https://ftp.gnu.org/gnu/glibc/glibc-2.28.tar.gz
$ tar -xzvf glibc-2.28.tar.gz
$ cd glibc-2.28
# 创建临时文件
$ mkdir build && cd build
$ ../configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin
$ ../configure (采用)

# 这一步时, 发生了错误, 提示大致为
These critical programs are missing or too old: make compiler
参考: https://cloud.tencent.com/developer/article/2021784

checking for python3... python3
configure: error: 
*** These critical programs are missing or too old: make compiler
*** Check the INSTALL file for required versions.

下一步升级 gcc 和 make
升级gcc与make
安装GLIBC所需的依赖 可以在 glibc 目录下的INSTALL中找到, 该版本需要 GCC 4.9 以上 及 make 4.0 以上



```

### npm安装时卡在sill idealTree buildDeps，npm安装速度慢，npm安装卡在一个地方不动

```shell
# 造成上述问题的原因是因为node的默认安装环境在国外，访问速度慢就会卡住。

解决方法一：
1、采用taobao的镜像地址，进入cmd之后输入：
npm config set registry https://registry.npm.taobao.org 
2、查看是否安装成功：
npm config get registry 
3、继续输入之前的npm install 命令进行安装。
npm install

解决方法二：
输入命令：
cnpm install

```



## 参见

nvm: https://github.com/nvm-sh/nvm#installing-and-updating

npm 官网: <https://www.npmjs.com>

webpack 官网: [https://webpack.js.org](https://webpack.js.org/)



