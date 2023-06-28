# Git

----

[toc]



## install

```sh
# yum -y install git
git --version
```



## checkout, merge, branch

```shell
# 以下是实际项目中通 git 直接部署动态池的流程:
$ git status
$ git branch -avv
$ git checkout chenchen
$ git add .
$ git commit -m'nil' .
$ git checkout test
$ git pull origin test
$ git merge chenchen
$ git push ogigin test
$ git checkout master
$ git pull origin master
$ git merge test
$ git push origin master
$ git checkout chenchen
$ git status
$ git branch -avv

$ git push origin --delete <远程分支名>   # 删除
$ git push origin --delete chenchen 		# 删除远程chenchen分支
$ git push origin :<远程分支名>					 # 推送空分支到远程(删除远程分支另一种实现)
$ git branch -d <本地分支名>

$ git push <远程主机名> <本地分支名>:<远程分支名>		# 连接远程主机, 然后将本地推到远程, 其中冒号:后不写为当前本地分支
$ git pull <远程主机名> <远程分支名>:<本地分支名>		# 连接远程主机, 然后将远程拉回本地, 其中冒号:后不写为当前本地分支
$ git push origin(远端主机) master(本地分支)

$ git pull origin master:chenchen
$ git pull origin test:test

$ git remote -vvv		# 查看远程仓库地址

# 创建分支, 
# git branch 分支名称 					# 创建, 不切换分支
# git checkout -b 分支名称			# 创建, 切换到新建分支上
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git branch chenchen
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git br
  chenchen              df0984d init
* master                df0984d init
  remotes/origin/master df0984d init
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git checkout -b test
Switched to a new branch 'test'
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git br
  chenchen              df0984d init
  master                df0984d init
* test                  df0984d init
  remotes/origin/master df0984d init
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ 

# 和远程仓库建立关联
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git remote add origin ssh://git@git.intra.weibo.com:2222/weibo_sports/trim.sports.weibo.cn.git			# 在本地项目中给 远程仓库地址 起一个别名叫 origin
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git pull --rebase origin master		# 这里省了, 冒号和当前分支(git pull --rebase origin master:master, 省了最后的 ':master')
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git push origin master		# 这里省了, 当前分支和冒号(git push origin master:master, 省了倒数第二'master:')

# 新建远程分支
# 新建一个本地分支：$ git checkout -b 分支名
# 查看一下现在的分支状态：$ git branch    //星号(*)表示当前所在分支。现在的状态是成功创建的新的分支并且已经切换到新分支上。
# 把新建的本地分支push到远程服务器，远程分支与本地分支同名（当然可以随意起名）：$ git push origin 分支名:分支名
# // 使用git branch -a查看所有分支，会看到remotes/origin/localbranch这个远程分支，说明新建远程分支成功。

# 删除远程分支
# 我比较喜欢的简单方式，推送一个空分支到远程分支，其实就相当于删除远程分支：
$ git push origin :localbranch
# 也可以使用：
$ git push origin --delete localbranch
# 这两种方式都可以删除指定的远程分支
```



## .gitignore

```shell
# 关于git忽略规则以及 .gitignore ⽂件不⽣效的解决途径

前⾔
在 git 中如果想忽略掉某个⽂件, 不让这个⽂件提交到版本库中, 可以使⽤修改根⽬录中 .gitignore ⽂件的⽅法 (如果没有这个⽂件, 则需
⾃⼰⼿⼯建⽴此⽂件)

Git忽略规则：
# 此为注释 – 内容被 Git 忽略
.sample 　　 			 # 忽略所有 .sample 结尾的⽂件
!lib.sample				# 但 lib.sample 除外
/TODO 　　 				 # 仅仅忽略项⽬根⽬录下的 TODO ⽂件，不包括 subdir/TODO
build/ 　　 			 # 忽略 build/ ⽬录下的所有⽂件
doc/.txt 　　			 # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
model/vendor/**/*.git, 其中的 ** 连写, 是多级目录, 每级都被检查并忽略



忽略所有 .a 文件
*.a

但是还要继续跟踪 lib.a, 也就是在上一行 所有 .a 文件中排除 lib.a 文件
!lib.a

只忽略当前目录下的 TODO 文件, 注意, 不是子目录 TODO
/TODO

忽略所有目录名叫 build 目录中的所有文件
build/

忽略 doc/notes.txt 文件, 而不是 doc/server/arch.txt 文件
doc/*.txt

忽略 所有 doc 目录 及其 doc 各级子目录 中的, 所有 .pdf 文件
doc/**/*.pdf



.gitignore 规则不⽣效的解决办法
把某些⽬录或⽂件加⼊忽略规则, 按照上述⽅法定义后发现并未⽣效, 原因是 .gitignore 只能忽略那些原来没有被追踪的⽂件, 如果某些⽂
件已经被纳⼊了版本管理中, 则修改 .gitignore 是⽆效的. 
那么解决⽅法就是先把本地缓存删除(改变成未被追踪状态), 然后再提交: 
$ git rm -r --cached .
$ git add .
$ git commit -m 'update .gitignore'
$ git commit -m "fixed untracked files"



# 以下是实际项目中, 需要忽略所有 .git 文件, 但项目根目录下的 .git 文件除外. 注意规则不生效, 要清里本地缓存. 上面👆
.gitignore 文件位置: /data1/www/htdocs/chenchen/april.sports.sina.com.cn/.gitignore
.gitignore 文件内容如下:
.git
!/.git


{
    "require": {
        "sinasports/sportsdata": "^1.0",
        "sunny-daisy/mongodb": "dev-master"
    },  
    "repositories": [
        {   
            "type": "git",
            "url":  "git@git.staff.sina.com.cn:SINA_SPORTS/sportsdata.git"
        }   
    ]   
}




# no .a files
# 忽略所有.a文件
*.a

# but do track lib.a, even though you're ignoring .a files above
# 表示不忽略(跟踪)匹配到的lib.a文件或目录
!lib.a

# only ignore the TODO file in the current directory,
# 只忽略当前目录中的TODO文件，而不是子目录/TODO
not subdir/TODO /TODO

# ignore all files in the build/ directory
# 忽略build/目录中的所有文件
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
# 忽略doc/notes.txt，但不要忽略doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
# 忽略doc/目录下的所有。pdf文件
doc/**/*.pdf

```



## ~/.ssh/known_hosts

```sh
位置: ~/.ssh/known_hosts
什么是ssh known_hosts文件?
A通过ssh首次连接到B，B会将公钥1（host key）传递给A，A将公钥1存入known_hosts文件中，以后A再连接B时，B依然会传递给A一个公钥2，OpenSSH会核对公钥，通过对比公钥1与公钥2 是否相同来进行简单的验证，如果公钥不同，OpenSSH会发出警告， 避免你受到DNS Hijack之类的攻ji

打开known_hosts文件
vi ~/.ssh/known_hosts 或 vi /root/.ssh/known_hosts
host文件内容格式：ip 公钥

 了解更多ssh known_host, https://www.howtouselinux.com/post/ssh-known_hosts-file

Host key verification failed
三、A通过ssh登陆B时提示 Host key verification failed
 了解更多how to fix Host key verification failed, https://www.howtouselinux.com/post/fix-host-key-verification-failed

原因：A的known_hosts文件中记录的B的公钥1 与 连接时B传过来的公钥2不匹配

解决方法：

方法一：删除A的known_hosts文件中记录的B的公钥（手动进行，不适用于自动化部署情形）
有两只方法删除

通过vi 找到这个对应的ip或者host的host key 然后删除
通过ssh-keygen -r hostname 删除
方法二：修改配置文件，在ssh登陆时不通过known_hosts文件进行验证（安全性有所降低），
也是有两种方法：

编辑对应host的ssh配置文件
vi ~/.ssh/config //编辑配置文件
添加以下两行代码：
StrictHostKeyChecking no
在ssh登录时利用 -i StrictHostKeyChecking=no 这样就不会检查host key。
 如何解决fix too many authentication failures, https://www.howtouselinux.com/post/2-ways-to-fix-ssh-too-many-authentication-failures
 如何处理remote host identification has changed, https://www.howtouselinux.com/post/fix-remote-host-identification-has-changed
```

## ~/.ssh/config

```sh
#
# Main gitlab server
#

Host git.staff.sina.com.cn
# 是否启用RSA认证授权
RSAAuthentication yes
# 私钥的路径和文件名
IdentityFile ~/.ssh/gitlab_rsa
# gitlab 上的帐号
User chenchen@staff.sina.com.cn

###
##
# https://git.intra.weibo.com/
# Host 服务器别名, 只要是合法的变量名称且不重复即可, 可任意指定, ssh命令通过该名称来连接到指定服务器, 比如上面的 ssh hostA / ssh hostB
Host git.intra.weibo.com 
# Hostname 服务器地址, 可以是域名, 也可以是ip地址
Hostname git.intra.weibo.com
# Port 端口号, 默认为22, 只有修改了ssh连接的默认端口才需要配置此参数
Port 2222
# PreferredAuthentications 首选身份验证
PreferredAuthentications publickey
# 是否启用RSA认证授权
RSAAuthentication yes
# IdentityFile ssh 私钥的路径和文件名(不带.pub后缀的文件)
IdentityFile ~/.ssh/gitlabintraweibo_rsa
# User ssh 的登陆用户名, gitlab 上的帐号
User chenchen@staff.sina.com
```



## ssh-keygen

```sh
位置: ~/.ssh/
ssh-keygen -t rsa -C "这里换上你的邮箱"
ssh-keygen -t rsa -C "chenchen@staff.sina.com"
```

## /etc/resolv.conf

```sh
配置本地 DNS 服务器, 位置: /etc/resolv.conf

nameserver 10.210.12.10
search localdomain
```



## git config

```shell
[chenchen@dev3_10.211.21.18 ~]$ git config --list
[chenchen@dev3_10.211.21.18 ~]$ git config -l
[chenchen@dev3_10.211.21.18 ~]$ git config --global -l
[chenchen@dev3_10.211.21.18 ~]$ git config --local -l
user.email=chenchen@staff.sina.com.cn
user.name=chenchen


$ git config --system --list    # 查看系统配置
$ git config --global --list    # 查看当前用户配置
$ git config --local --list     # 查看当前仓库配置
$ git config --list             # 查看全部配置
```



```shell
# git config 的配置分为三个级别:
1. 系统级别, --system, 文件位置: project/.git/config
2. 用户级别(也叫全局), --global, 文件位置: ~/.gitconfig
3. 仓库级别(也叫局部), --local, 文件位置: project/.git/config

# 优先级:
--local > --global > --system

# 查看
# 查看仓库级别(局部), 即在本地的某个仓库|项目下的 .git/config 文件
[chenchen@dev3_10.211.21.18 ~]$ git config --local -l
fatal: unable to read config file '.git/config': No such file or directory
# 查看用户级别(全局), 即在当前用户 HOME 目录下 ~/.gitconfig 文件
[chenchen@dev3_10.211.21.18 ~]$ git config --global -l
user.email=chenchen@staff.sina.com.cn
user.name=chenchen
# 查看系统级别, 在系统配置文件常在位置, /etc/gitconfig 文件
[chenchen@dev3_10.211.21.18 ~]$ git config --system -l
fatal: unable to read config file '/etc/gitconfig': No such file or directory
```



```shell
# git config 文件的配置方法:
1. 找到对应配置文件的位置, 直接 vim 手动编辑修改.
2. 通过 $ git config 命令来配置, 其结果同第一条, 也是会修改对应的配置文件.
$ git config --global --get user.name												# 获取
$ aming
$ git config --global --set user.hobby watchingTV						# 写入
$ git config --global -l																		# 查看
user.name=aming
user.email=fff@fff.com
user.hobby=watchingTV
$ git config --global --unset user.hobby										# 删除
```



## http.proxy, https.proxy, all.proxy

```shell
export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890

# git 代理设置方法解决:
# 用户级别(全局)
$ git config --global http.proxy http://127.0.0.1:7890
$ git config --global https.proxy http://127.0.0.1:7890
$ git config --global all.proxy 'socks5://127.0.0.1:7890'
$ git clone https://github.com/vim/vim.git --config 'https.proxy=http://127.0.0.1:7890'

~/.gitconfig 文件中会新增: 
[http]
    proxy = http://127.0.0.1:7890
[https]
    proxy = http://127.0.0.1:7890
[all]
    proxy = socks5://127.0.0.1:7890

# 删除
$ git config --global --unset http.proxy
$ git config --global --unset https.proxy
$ git config --global --unset all.proxy

npm config delete proxy
```





## git config 帮助

```shell
[chenchen@dev3_10.211.21.18 ~]$ git config
usage: git config [options]

Config file location
    --global              use global config file
    --system              use system config file
    --local               use repository config file
    -f, --file <file>     use given config file
    --blob <blob-id>      read config from given blob object

Action
    --get                 get value: name [value-regex]
    --get-all             get all values: key [value-regex]
    --get-regexp          get values for regexp: name-regex [value-regex]
    --replace-all         replace all matching variables: name value [value_regex]
    --add                 add a new variable: name value
    --unset               remove a variable: name [value-regex]
    --unset-all           remove all matches: name [value-regex]
    --rename-section      rename section: old-name new-name
    --remove-section      remove a section: name
    -l, --list            list all
    -e, --edit            open an editor
    --get-color <slot>    find the color configured: [default]
    --get-colorbool <slot>
                          find the color setting: [stdout-is-tty]

Type
    --bool                value is "true" or "false"
    --int                 value is decimal number
    --bool-or-int         value is --bool or --int
    --path                value is a path (file or directory name)

Other
    -z, --null            terminate values with NUL byte
    --includes            respect include directives on lookup

[chenchen@dev3_10.211.21.18 ~]$ 

# git config 工具用于设置配置变量, 而这些变量有三个级别的存储位置:
1. /etc/gitconfig 文件, 系统级别, 所有用户. 等同于 git config --system
2. ~/.gitconfig 文件, 自己 HOME 目录下. 等同于 git config --global
3. 项目/.git/config 文件. 在项目目录下. 等同于 git config --local
# 优先级: 3 覆盖 2 覆盖 1

# 基于以上两项说明: 
1. 用户名和密码显然要放在 --global
$ git config --global user.name "wirelessqa"  
$ git config --global user.email wirelessqa.me@gmail.com 

# 查看配置
$ git config --system --list    # 查看系统配置
$ git config --global --list    # 查看当前用户配置
$ git config --local --list     # 查看当前仓库配置
$ git config --list             # 查看全部配置


```



## .git/config 文件

```shell
# ~/.gitconfig
[user]
    email = chenchen@staff.sina.com
    name = chenchen

[color]
    ui = auto
[color "branch"]
    current = yellow reverse
    local = yellow
    remote = green
[color "status"]
    added = yellow
    changed = green
    untracked = cyan
[color "diff"]
    meta = yellow
    frag = magenta bold
    commit = yellow bold
    old = red bold
    new = green bold
    whitespace = red reverse
[color "diff-highlight"]
    oldNormal = red bold
    oldHighlight = red bold 52
    newNormal = green bold
    newHighlight = green bold 22

[alias]
    amend = commit --amend
    amendf = commit --amend --no-edit
    br = branch
    ct = commit
    co = checkout
    cp = cherry-pick
    df = diff
    ds = diff --staged
    l = log
    lg = log --graph --all --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(bold white)— %an%C(reset)%C(bold yellow)%d%C(reset)' --abbrev-commit --date=relative
    lp = log --pretty=oneline
    sa = stash apply
    sh = show
    ss = stash save
    st = status

[push]
    default = current
---- 同上
$ git config --global alias.co checkout
    
# .git/config    
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true
[remote "origin"]
    url = ssh://git@git.intra.weibo.com:2222/weibo_sports/mult.sports.weibo.com.git
    fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
    remote = origin
    merge = refs/heads/master
[branch "test"]
    remote = origin
    merge = refs/heads/test
---- 同上
$ git config branch.master.remote origin
$ git config branch.master.merge refs/heads/master

$ git config --local branch.master.remote origin
$ git config --local branch.test.remote origin
$ git config --local branch.<name>.remote xxxx
$ git config --local branch.<name>.merge YYYY
用来配置名字为 name 的 branch 的 remote 和 merge

# 关于 [branch "master"] 这个配置的意思是:
当 git 位于 本地 master 分支时, remote 仓库为 origin, 
当 git pull 的时候没有指定远程的分支的时候, 默认将 remote 的 master 分支 merge 到本地分支.

# 关于 refs/heads/master 和 refs/for/master
这个是 gerrit 的规则, refs/for/* 的提交要经过 code review 才可以提交, refs/heads/* 不需要经过 code review 就可以提交.

# git refs 是个什么东西, 它和 branch 之间是一个什么样的关系
git refs 可以理解为本次提交的一个地址, 它本质上是一个 sha-1, 即对本次提交的文本求了一个 sha-1 的值, 通过该值检索到该提交的文本对象.
而 git branch 是一连串的提交构成的, 但是可以用最后一次提交的 refs 来表示一个 branch.
```



## 设置跟踪分支, 取消跟踪分支

```shell
1. 两个命令
// 设置跟踪分支
git branch (--set-upstream-to=<upstream> | -u <upstream>) [<branchname>]

// 取消跟踪分支
git branch --unset-upstream [<branchname>]

2. gitconfig 文件
2.1 $ git init
[core]
        repositoryformatversion = 0 
        filemode = true
        bare = false
        logallrefupdates = true

2.2 $ git remote add origin git@git.jd.com:wangqiwei7/git-test.git
[core]
        repositoryformatversion = 0 
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = git@git.jd.com:wangqiwei7/git-test.git
        fetch = +refs/heads/*:refs/remotes/origin/*

2.3 $ git branch -u origin/dev dev
[core]
        repositoryformatversion = 0 
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = git@git.jd.com:wangqiwei7/git-test.git
        fetch = +refs/heads/*:refs/remotes/origin/*                                                   
[branch "dev"]
        remote = origin
        merge = refs/heads/dev
        
2.4 $ git branch --unset-upstream dev
[core]
        repositoryformatversion = 0 
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = git@git.jd.com:wangqiwei7/git-test.git
        fetch = +refs/heads/*:refs/remotes/origin/*

```



## proxy 代理

```shell
#
http.proxy
           Override the HTTP proxy, normally configured using the http_proxy, https_proxy, and all_proxy environment variables (see curl(1)). This can be overridden on a per-remote basis; see remote.<name>.proxy
					 覆盖 HTTP 代理, 通常使用 http_proxy、https_proxy 和 all_proxy 环境变量进行配置. 这可以在每个远程的基础上被覆盖.

#
remote.<name>.proxy
           For remotes that require curl (http, https and ftp), the URL to the proxy to use for that remote. Set to the empty string to disable proxying for that remote.
           对于需要 curl（http、https 和 ftp）的遥控器, 用于该遥控器的代理的 URL. 设置为空字符串以禁用该远程的代理.
           (与我们常说的 http 代理是两码事儿, 一般用不到)

#
core.gitProxy
           A "proxy command" to execute (as command host port) instead of establishing direct connection to the remote server when using the Git protocol for fetching. If the variable value is in the "COMMAND for DOMAIN" format, the command is
           applied only on hostnames ending with the specified domain string. This variable may be set multiple times and is matched in the given order; the first match wins.
           在使用 Git 协议进行提取时 ,要执行的 "代理命令" (作为命令主机端口), 而不是建立与远程服务器的直接连接. 
           如果变量值为 "DOMAIN 的命令" 格式，则该命令为仅适用于以指定域字符串结尾的主机名. 
           此变量可以设置多次，并按给定的顺序匹配; 第一场比赛获胜.
          
           Can be overridden by the GIT_PROXY_COMMAND environment variable (which always applies universally, without the special "for" handling).
           可以被 GIT_PROXY_COMMAND 环境变量覆盖(该变量始终普遍适用, 无需特殊的 "for" 处理).

           The special string none can be used as the proxy command to specify that no proxy be used for a given domain pattern. This is useful for excluding servers inside a firewall from proxy use, while defaulting to a common proxy for external domains.
           特殊字符串 none 可用作代理命令, 以指定不将代理用于给定的域模式. 
					 这对于从代理使用中排除防火墙内的服务器, 同时默认为外部域.

; Proxy settings
[core]
gitproxy=proxy-command for kernel.org
gitproxy=default-proxy ; for all the rest
       The hypothetical proxy command entries actually have a postfix to discern what URL they apply to. Here is how to change the entry for kernel.org to "ssh".
           % git config core.gitproxy '"ssh" for kernel.org' 'for kernel.org$'
```



## git://, ssh://, http://

```shell
# Git支持四种协议:
1. 而除本地传输外
2. git://
3. ssh://
4. http(s)://
这些协议又被分为哑协议(HTTP协议)和智能传输协议.

# 对于不同的协议, 代理配置也有差异
2. 使用 git:// 协议时, 设置代理需要配置 core.gitproxy
4. 使用 http(s):// 协议时, 设置代理需要配置 http.proxy



```



## comment 注释

```shell
# 用 # 或者 ; 表示注释

				Given a .git/config like this:

           #
           # This is the config file, and
           # a '#' or ';' character indicates
           # a comment
           #

           ; core variables
           [core]
                   ; Don't trust file modes
                   filemode = false

           ; Our diff algorithm
           [diff]
                   external = /usr/local/bin/diff-wrapper
                   renames = true

           ; Proxy settings
           [core]
                   gitproxy=proxy-command for kernel.org
                   gitproxy=default-proxy ; for all the rest
```



```shell
$ git remote add origin ssh://git@git.intra.weibo.com:2222/weibo_sports/pugr.sports.weibo.com.git
git remote 设置跟踪的远程仓库地址

git remote add <name> <url>, 这是给远程仓库的 url 起个别名, 省去每次都要敲很长的 url, 通常叫 origin



git branch -d test (删本地分支)

git push origin --delete 分支名字 (删远程分支)
git push origin --delete chenchen 


[chenchen@dev3_10.211.21.18 vendor]$ find ./ -type d -name '.git*'
[chenchen@dev3_10.211.21.18 vendor]$ find ./ -type f -name .git*
./sina_sports/phplib/.gitignore
./mongodb/mongodb/.gitignore
./sunny-daisy/mongodb/.gitignore
[chenchen@dev3_10.211.21.18 vendor]$ 


[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git st
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git br -avv
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git init
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git add .
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git commit -m'init' .
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git remote add origin ssh://git@git.intra.weibo.com:2222/weibo_sports/trim.sports.weibo.cn.git
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git pull --rebase origin master
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git push origin master		# 这里省了 当前分支和冒号


拓展：
状态查询命令
git status
git查看远程仓库地址命令
git remote -v
如果想要修改远程仓库地址：
$ git remote set-url origin git@github.com:mkl34367803/WebAjax.git
然后再push：
$ git push origin master

```

```shell
# 通过 web 删了远程分支, 但本地 git branch -avv 时还有显示.
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git br
* chenchen                             c31705a comm
  master                               36dc4c4 Merge branch 'test'
  test                                 c31705a comm
  remotes/origin/master                36dc4c4 Merge branch 'test'
  remotes/origin/remotes/origin/master 2321c5a public/index
  remotes/origin/test                  c31705a comm
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git branch -avv
* chenchen                             c31705a comm
  master                               36dc4c4 Merge branch 'test'
  test                                 c31705a comm
  remotes/origin/master                36dc4c4 Merge branch 'test'
  remotes/origin/remotes/origin/master 2321c5a public/index
  remotes/origin/test                  c31705a comm
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git remote prune origin
Pruning origin
URL: ssh://git@git.intra.weibo.com:2222/weibo_sports/trim.sports.weibo.cn.git
 * [pruned] origin/remotes/origin/master
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git branch -avv
* chenchen              c31705a comm
  master                36dc4c4 Merge branch 'test'
  test                  c31705a comm
  remotes/origin/master 36dc4c4 Merge branch 'test'
  remotes/origin/test   c31705a comm
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ 
```

## tag 和 branch 区别

```shell
区别: 
1. tag 是一系列 commit 的中的一个点, 对应某次 commit, 是一个点, 只能查看, 不能移动. 是 Git 版本库的一个快照, 指向某个 commit 的指针; 
	 而 branch 是一系列串联的 commit 的线, 对应一系列 commit, 是很多点连成的一根线, 可以继续延展. 有一个HEAD 指针, 是可以依靠 HEAD 指针移动的.
2. tag 是静态的, branch 是动态的, 要向前走.
3. 两者的区别决定了使用方式, 改动代码用 branch, 不改动只查看用 tag
```

## clone, fetch 区别

```shell
clone 从无到有的克隆操作
git clone ssh://git@git.intra.weibo.com:2222/weibo_rd_operation/brandevent/backend/sports.git
git clone ssh://git@git.intra.weibo.com:2222/weibo_rd_operation/brandevent/backend/sports.git ./
以上二者区别: 
- 如果不写最后的位置, 那么会在当前执行此命令的位置上再自动新建一个子目录, 子目录名字为 sports.git 里的 sports/, 然后所有代码都下载进这个子目录.
- 如果写有最后的位置, 那么就将 Git 里的根目录的代码直接放到当前执行此命令的目录里, 不会新建子目录.



fetch FETCH_HEAD指: 某个branch在服务器上的最新状态. 这个列表保存在.git/FETCH_HEAD文件中, 其中每一行对应于远程服务器的一个分支.
一般两种情况: 
如果没有显式指定远程分支, 则以master分支为默认的FETCH_HEAD
如果指定了远程分支, 将这个远程分支作为FETCH_HEAD
git fetch origin master //从远程origin仓库的master分支下载代码到本地的origin master



```



## tag

```shell
tag目的: 分支会被改动而不断变化, 而tag会永久的里程碑, 引用时又和分支一样方便.
git tag v1 C1, 建立标签, 指向提交记录 C1, 命名为 v1
git tag v1, 不指定提交记录, 默认是 HEAD 位置.


git 的 tag 功能是为了将代码的某个状态打上一个戳, 通过 tag 我们可以很轻易的找到对应的提交
# 创建 tag
git tag <tagName>							// 创建本地 tag, 最后一次 commit, 省略不写 <commitId>
git tag <tagName> <commitId>	// 创建本地 tag, 以指定特定 <commitId> 的方式
git tag -a V1.2 -m 'release 1.2'	// 同上, 创建的同时增加了附加信息
git push origin <tagName>			// 显式 push 到远程仓库, 默认 git push 并不会推送 tag 到远程, 需显式 git push
git push origin --tags				// 一次批量 push 所有本地 tag 到远程仓库
git log --pretty=online				// 查看当前分支的提交历史, 里面包含 commit id

# 查看 tag
git show <tagName>						// 查看本地指定 tagName 的 tag 的详细信息
git tag												// 查看本地所有 tag
git tag -l										// 同上
git tag -l "2.3.5."						// 同上, 只看 2.3.5.*
git ls-remote --tags origin		// 查看远程所有 tag

# 删除 tag
git tag -d <tagName>					// 删除本地 tag
git push origin :<tagName>		// 删除远程 tag, 用本地空 tagName 覆盖远程指定 tagName
git push origin :refs/tags/V1.2		// 同上, 写法不同而已

# 重命名 tag
# 重命名 = 删掉旧 tagName, 然后再新建新 tagName
## 只重命名本地 tagName = 删掉本地旧 tagName, 然后新建新 tagName
git tag -d <oldTagName>				// 删掉本地旧 tagName
git tag -a <newTagName>				// 然后新建新 tagName
git push origin <newTagName>	// 如果推送远程仓库
## 重命名本地 tagName, 并且同时还要重命名远程仓库中的 tagName = 删除本地旧+删除远程旧, 然后新建本地新+推送远程新
git tag -d <oldTagName>				// 删掉本地旧 tagName
git push origin :<oldTagName>	// 删掉远程旧 tagName, 用空 tagName 覆盖
git tag -a <newTagName>				// 然后新建本地新 tagName
git push origin <newTagName>	// 推送远程新 tagName

# 切换 tag
git checkout <tagName>				// 切换到指定的 tag, 会提示错误, tag 不是分支不能切换
## 因为 tag 本身指向的就是一个 commit, 所以和根据 commit id 检出分支是一个道理
git checkout -b <branchName> <tagName>	// 以 tag 为基点创建一个 branch
## 但是需要特别说明的是, 如果我们想要修改 tag 检出代码分支, 那么虽然分支中的代码改变了, 但是 tag 标记的 commit 还是同一个, 标记的代码是不会变的, 这个要格外的注意

# 获取 tag
git fetch origin tag V1.2			// 准确获取指定的某个版本
```



## github 常见目录

```shell
src: 源码文件
dist: 编译出来的发布版, 编译后或者压缩后的代码. 注: 最后编译发布的时候会将所有的静态资源整合到 /dist/static/ 目录下, 包括assets文件夹中的静态资源.
docs: 文档
examples: 示例文件
assets: 储存js、css、图片等静态资源
static: 储存第三方静态资源(例如jquery.js, bootstrap.css等)
build: 用来构建源码的脚本
test: 用来测试的测试脚本
LICENSE: 授权协议
README.md: 自述文件
.gitignore 哪些文件不要上传到 GitHub, 定义不想在git中提交的文件
```



## Example of case

```shell
# 案例1
本地有3个分支 chenchen, test, master,
远程有2个分支 test, master, 
其中, 远程master 分支已经被别人更新了很多版本了, 我由于没有在add之前先 pull, 所以造成了本地chenchen分支的版本和远程master分支版本不同, 造成了冲突;

# 解决
参考: https://stackoverflow.com/questions/1709177/pull-a-certain-branch-from-the-remote-server/1710474#1710474
以下在 chenchen 上操作:
git fetch origin master				# 先把远程master取回来
git merge origin/master				# 与本地chenchen合并
合并命令之后会有提示, 有些文件自动合后不存在冲突, 有些存在冲突, 有冲突的需要手动解决, 这些待手动的文件会有提示, 按提示手动解决.
git add .											# 解决后就可以正常 add 了
git commit -m'resolve conflicts'

# 过程
[chenchen@dev3_10.211.21.18 april.sports.sina.com.cn]$ git pull origin master:chenchen
From git.staff.sina.com.cn:SINA_SPORTS/super.sports.sina.cn
 ! [rejected]        master     -> chenchen  (non-fast-forward)
[chenchen@dev3_10.211.21.18 april.sports.sina.com.cn]$ git fetch orgin master
fatal: 'orgin' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
[chenchen@dev3_10.211.21.18 april.sports.sina.com.cn]$ git fetch origin master
From git.staff.sina.com.cn:SINA_SPORTS/super.sports.sina.cn
 * branch            master     -> FETCH_HEAD
[chenchen@dev3_10.211.21.18 april.sports.sina.com.cn]$ git merge origin/master
Auto-merging app/players/templates/index.html
CONFLICT (content): Merge conflict in app/players/templates/index.html
Auto-merging app/live/templates/index.html
CONFLICT (content): Merge conflict in app/live/templates/index.html
Automatic merge failed; fix conflicts and then commit the result.
[chenchen@dev3_10.211.21.18 april.sports.sina.com.cn]$ 

手动修改以上两个文件
然后 git add .
git commit -m'resolve conflicts'

[chenchen@dev3_10.211.21.18 april.sports.sina.com.cn]$ git br
* chenchen              bb4d423 resolve conflicts
  master                9b5a5f8 模版刷新
  test                  e89e520 邱添更改live, player 两页面模版20221222
  remotes/origin/master 9b5a5f8 模版刷新
  remotes/origin/test   e89e520 邱添更改live, player 两页面模版20221222
[chenchen@dev3_10.211.21.18 april.sports.sina.com.cn]$ git co test
Switched to branch 'test'
[chenchen@dev3_10.211.21.18 april.sports.sina.com.cn]$ git merge chenchen
Updating e89e520..bb4d423
Fast-forward
 app/leaders/templates/index.html                               |  15 +--------------
 app/live/templates/index.html                                  | 102 +++++++++++++++++++++++++++++++++++++++++++++++++++---------------------------------------------------
 app/players/templates/index.html                               |  88 ++++++++++++++++++++++++++++++++++++++++++++--------------------------------------------
 app/schedule/templates/index.html                              |  44 +++++++++++++++++++++++++++-----------------
 app/st/controller/DefaultController.php                        |  36 ++++++++++++++++++++++++++++++++++++
 app/st/controller/SerialController.php                         | 108 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 app/st/controller/StruggleController.php                       | 108 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 app/st/controller/VariousController.php                        | 108 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 app/standings/templates/index.html                             |   4 ++--
 app/teams/controller/ItemiseController.php                     |   3 +++
 app/teams/controller/SerialController.php                      |   3 +++
 app/teams/controller/VariousController.php                     |   3 +++
 app/teams/templates/index.html                                 |   4 ++--
 model/MiningRohtLiveModel.php                                  |   1 +
 model/SoccerCatalogItemiseTeamsProModel.php                    |   6 +++++-
 model/SoccerDatumVariousLiveModel.php                          |   2 +-
 model/SoccerDatumVariousLiveProModel.php                       |  28 ++++++++++++++--------------
 model/SoccerDatumVariousPlayersDefModel.php                    |  22 ++++++++++++++++++++++
 model/SoccerDatumVariousPlayersModel.php                       |   9 +++++++--
 model/SoccerDatumVariousPlayersProModel.php                    |  84 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++--
 model/SoccerDatumVariousTeamsDefModel.php                      |   1 +
 model/SoccerDatumVariousTeamsModel.php                         |  19 ++++++++++++++++---
 model/SoccerDatumVariousTeamsProModel.php                      |  17 +++++++++--------
 model/SoccerGloryStruggleTeamsProModel.php                     |   6 +++++-
 model/Utils.php                                                |   2 +-
 model/composer.lock                                            |   6 +++---
 model/vendor/composer/installed.json                           |   8 ++++----
 model/vendor/composer/installed.php                            |  10 +++++-----
 model/vendor/sinasports/sportsdata/src/OPTA/Football.php       |   4 ++--
 model/vendor/sinasports/sportsdata/src/OPTA/FootballPlayer.php |  16 ++++++++++++----
 30 files changed, 686 insertions(+), 181 deletions(-)
 create mode 100644 app/st/controller/DefaultController.php
 create mode 100644 app/st/controller/SerialController.php
 create mode 100644 app/st/controller/StruggleController.php
 create mode 100644 app/st/controller/VariousController.php
[chenchen@dev3_10.211.21.18 april.sports.sina.com.cn]$ 
```



## FAQ

### config

```shell
# 查看当前项目的 git 配置
vim ~/.gitconfig
```



### log

```shell
$ git log -p
$ git log -p model/PugrListCronModel.php
$ git log -p --stat  model/PugrListCronModel.php

$ git log -S [keyword]
$ git log -L 15,+1:'model/PugrListCronModel.php'

$ git log --graph
$ git log --stat
$ git log --oneline
$ git log --author="author"
$ git log --pretty=oneline

查看提交历史: git log
查看命令历史: git reflog

$ git show 61d69aa3051b469b4858f707b5035af3450ecf12
$ git show --stat 61d69aa3051b469b4858f707b5035af3450ecf12
```

### diff

```shell
$ git diff filepath
. git diff filepath 工作区与暂存区比较

2. git diff HEAD filepath 工作区与HEAD ( 当前工作分支) 比较

3. git diff --staged 或 --cached filepath 暂存区与HEAD比较

4. git diff branchName filepath 当前分支的文件与branchName 分支的文件进行比较

5. git diff commitId filepath 与某一次提交进行比较

git diff 比较的是工作区和暂存区的差别
git diff HEAD 可以查看工作区和版本库的差别
git diff --cached(===staged) 比较的是暂存区和版本库的差别
```

### HEAD

```shell
HEAD 报告您现在所在(活跃分支)的位置. 
HEAD 表示当前版本
HEAD^ 上一个版本
HEAD^^ 上上一个版本
HEAD~100 上100个版本

HEAD 大写, 当前分支的当前位置
head 小写, 是非当前分支的其他分支或者标签的位置

实际上, 整个 git 只有一条版本线路, 不同分支或者标签在这条线路上的位置不同, 切换不同分支或者标签时, HEAD也会随分支或者标签的变化而变化. 
HEAD 保存在 .git/HEAD 文件里, 比如: ref: refs/heads/master
在 refs/heads/master 文件里, 有一串 40 位的 SHA-1 字符串, 7e136f508b982790db5686482075c60ee3ee4fed, 这是 master 分支上最新提交的 commit id
HEAD 它即可以指代当前分支, 也可以指代当前分支的最新提交, 而当前分支与最新提交本是两个不同层面的概念. 类比于 C/C++ 语言中的指针, 后者即可以指代数组, 也可以指代数组的第一个元素. 凡是出现指针的地方, 都应该小心行事.

查看 HEAD 指向
git symbolic-ref HEAD
$ git symbolic-ref HEAD
refs/heads/master

$ git cat-file -p 82cc50567233979153512361eb6faa83102cf2ce
tree 169805f1d5dbcd2d321c5eaa8681c4f2f0bd07d7
parent b96ea95f69175a32be478c25c28fc3e4c1bacb9d
author chenchen <chenchen@staff.sina.com> 1672369018 +0800
committer chenchen <chenchen@staff.sina.com> 1672369018 +0800

root
第一行, tree 和对应的 hash 值, 根据这个 hash 值我们可以得到本次提交的整个目录树和对应的 hash 值
第二行, parent，是当前查看的 commit 的上一条(第二条) commit 的 id
第三行, 作者信息以及提交时的时间戳
第四行, 提交者的信息以及提交时的时间戳
根据 head 对应的提交信息生成其 sha-1 值的命令如下:
(printf "commit %s\0" $(git cat-file commit HEAD | wc -c); git cat-file commit HEAD) | shasum

$ git cat-file commit HEAD
tree 169805f1d5dbcd2d321c5eaa8681c4f2f0bd07d7
parent b96ea95f69175a32be478c25c28fc3e4c1bacb9d
author chenchen <chenchen@staff.sina.com> 1672369018 +0800
committer chenchen <chenchen@staff.sina.com> 1672369018 +0800

root


git reset --hard commitid	撤销
git reflog 用来记录你的每一次命令

版本号没必要写全，前几位就可以了



```

### reset 和 revert, 回退到指定版本

```shell
git reset 重置当前 HEAD 到特定位置. 用于回退版本. 

带有3个参数
--mixed 等同于不带参数, 工作区不动 workspace, 清空暂存区 stage
--soft 
--hard 


git revert


https://blog.csdn.net/qq_44646343/article/details/125415768
在利用github实现多人合作程序开发的过程中，我们有时会出现错误提交的情况，此时我们希望能撤销提交操作，让程序回到提交前的样子，本文总结了两种解决方法：回退（reset）、反做（revert）。

使用git的每次提交，Git都会自动把它们串成一条时间线，这条时间线就是一个分支。如果没有新建分支，那么只有一条时间线，即只有一个分支，在Git里，这个分支叫主分支，即master分支。有一个HEAD指针指向当前分支（只有一个分支的情况下会指向master，而master是指向最新提交）。每个版本都会有自己的版本信息，如特有的版本号、版本名等。如下图，假设只有一个分支：

解决方法
方法一：git reset
原理： git reset的作用是修改HEAD的位置，即将HEAD指向的位置改变为之前存在的某个版本，
适用场景： 如果想恢复到之前某个提交的版本，且那个版本之后提交的版本我们都不要了，就可以用这种方法。
具体操作：
1. 查看版本号：
可以使用命令“git log”查看：
2. 使用“git reset --hard 目标版本号”命令将版本回退：
再用“git log”查看版本信息，此时本地的HEAD已经指向之前的版本：
3. 使用“git push -f”提交更改：
此时如果用“git push”会报错，因为我们本地库HEAD指向的版本比远程库的要旧：
所以我们要用“git push -f”强制推上去，就可以了：

方法二：git revert
原理： git revert是用于“反做”某一个版本，以达到撤销该版本的修改的目的。比如，我们commit了三个版本（版本一、版本二、 版本三），突然发现版本二不行（如：有bug），想要撤销版本二，但又不想影响撤销版本三的提交，就可以用 git revert 命令来反做版本二，生成新的版本四，这个版本四里会保留版本三的东西，但撤销了版本二的东西。
适用场景： 如果我们想撤销之前的某一版本，但是又想保留该目标版本后面的版本，记录下这整个版本变动流程，就可以用这种方法。

具体操作：
举个例子，现在库里面有三个文件：READ.md、text.txt、text2.txt。
1. 查看版本号：
可以通过命令行查看（输入git log）：
2.使用“git revert -n 版本号”反做，并使用“git commit -m 版本名”提交：
（1）反做，使用“git revert -n 版本号”命令。如下命令，我们反做版本号为8b89621的版本：
注意： 这里可能会出现冲突，那么需要手动修改冲突的文件。而且要git add 文件名。
（2）提交，使用“git commit -m 版本名”，如：
此时可以用“git log”查看本地的版本信息，可见多生成了一个新的版本，该版本反做了“add text.txt”版本，但是保留了“add text2.txt”版本：
3.使用“git push”推上远程库：



撤销变更由底层部分(暂存区的独立文件或者片段)和上层部分(变更到底是通过哪种方式被撤销的)组成.
git reset HEAD~1
回退本地可以, 但是远程分支是无效

git revert HEAD

```

### FE

```shell
index/stage 暂存区, 目的是对工作区内容进行追踪(track), add 添加进暂存区的不是文件名, 而是具体的文件改动内容

push 时需要将当前分支和上游远程分支做关联
$ git push --set-upstream origin master		// origin 是远程仓库的代指. master 是远程仓库上的分支名

⚠️ 如果你不是 clone 而是直接本地 init 一个仓库再去远程仓库关联的话, 直接 push 会遇到 
![rejected] master -> master (fetch first)
这是因为在远程仓库上创建新的项目时勾选了自动生成 readme/.gitignore, 它们就作为远程仓库的第一次 commit, 而我们本地 git init 的仓库没有远程仓库的 readme/.gitignore 文件, 所以无法提交成功. 所以要先拉取一下远端的代码同步到本地, 再把我们的改动提交上去. 先 pull 再 push. 当然如果你创建远程仓库时不勾选 readme/.gitignore 就可以直接 push 上去.
但是光执行 pull 也是不够的, 远程仓库有一个提交, 我们本地仓库也有一个提交, 直接拉取远端的代码, 这两个提交谁先谁后呢? 没有操作相同文件时可能无所谓, 但是一旦都修改了同一个文件, 就涉及到哪次提交在后, 覆盖的问题, 所以要执行:
git pull --rebase origin master
这条指令的意思是把远程库中的更新合并到本地库中, --rebase 的作用是取消掉本地库中刚刚的 commit, 并把他们接到更新后的版本库之中.



```

### rebase, merge, cherry-pick

```shell
修改基础点
站在分支1点上执行 git rebase master, 那么是将分支1合并(迁移到)到master上, 也就是将master作为基础点

rebase: 将站在的位置迁到命中的分支点
交互式 rebase, -i

merge: 将命令中分支点迁到站在的位置

git cherry-pick C2 C4: 同merge, 将站在以外的迁过来
```



git merge --abort //取消merge操作, 执行之后回去看冲突文件，就变回了merge之前的状态。

fast-forward 快速前移, 当**我们所在分支落后于目标分支**时（目标分支包含当前分支所有的提交记录），在当前分支对目标分支执行merge，就是直接把HEAD和所在分支都指向目标分支最新的commit，也称为

关于rebase，只要记住，它是修改需要**被合并**的分支的基础点，同时与merge相反，需要在**被合并**的分支上操作的指令



#### branch

```shell
git branch -f master HEAD~4
强制修改分支位置, 强制移动分支到某个位置, 一次后退四步
-f 选项让分支指向另一个提交
某个位置有2个标记, 一个是分支标记, 一个是 HEAD. 分支标记并不是标记整颗tree, 而是标记某个节点.

git branch -f master C6
git branch -f bugFix HEAD~1
```

#### describe

```shell
在 tree 茫茫提交记录点中, 站在所在地查看距离最近的 tag(路标).
git describe

$ git describe HEAD
v0.1.3-alpha

<tag>_<numCommits>_g<hash>
tag 表示的是离 ref 最近的标签, 
numCommits 是表示这个 ref 与 tag 相差有多少个提交记录, 
hash 表示的是你所给定的 ref 所表示的提交记录哈希值的前几位.
当 ref 提交记录上有某个标签时, 则只输出标签名称
```

#### bisect

```shell
```

#### ref

```shell
ref 可引用的一切都称为 ref, refs
提交记录的id, 分支标记, tag, HEAD, 都是 ref
```







#### git remote

```shell
https://blog.csdn.net/HaveFun_Wine/article/details/127074637
前提说明:
本地安装完 git 客户端之后就默认带有了 本地 git 仓库.
$ git init
站在当前目录创建一个默认 master 分支和暂存区index/stage, 也就是初始化当前目录为一个git仓库. 在文件夹下看到隐藏的.git目录
$ git checkout -b 本地新分支
checkout 是从本地仓库向工作区迁出一个名为‘本地新分支’的分支, 即创建一个新分支, 本地仓库里也有了


$ git remote -v
查看远程仓库, 未关联为空

$ git remote add <自定义仓库名><仓库地址>

$ git remote show
查看远程仓库信息

$ git fetch <远程仓库名>
$ git remote update
$ git remote update --prune

$ git branch --set-upstream-to=<远程仓库名><远程分支名>
站在本地某个分支上

$ git branch -vv
查看分支关联情况

$ git remote rm <远程仓库名>
$ git remote remove <远程仓库名>
删除远程仓库

$ git remote rename <远程仓库旧名字><远程仓库新名字>
重命名远程仓库
```



![2E69B0B5-EE02-4793-88C1-7211E70F3102](/var/folders/g1/6bv__wz96ps83psq19r2fy_w0000gn/T/com.yinxiang.Mac/WebKitDnD.MptN3E/2E69B0B5-EE02-4793-88C1-7211E70F3102.png)



## Troubleshooting

```sh
 报错:
 ! [rejected]        master     -> chenchen  (non-fast-forward)
 
 场景:
 在 chenchen 分支上, 由于没有先 git pull origin master:chenchen, 而是先 git add ., git commit
  -m''; 这是 chenchen 分支上的代码已经提交到 stage 区, 现在再去 git pull origin master:chenchen 时, 报错这个错误.
 
 原因:
 从操作的过程就可以看出：git pull origin master，相当于git fetch + git merge。即从远程库中获取最新版本并merge到本地。
 
 解决:
 
 
 
```

```sh
报错: fatal: refusing to merge unrelated histories 
[root@9fdcb516b951 fr]# git merge 
fatal: refusing to merge unrelated histories
[root@9fdcb516b951 fr]# git pull origin master --allow-unrelated-histories
From ssh://git.intra.weibo.com:2222/weibo_sports/front-cassi.sports.weibo.cn
 * branch            master     -> FETCH_HEAD
CONFLICT (add/add): Merge conflict in README.md
Auto-merging README.md
Automatic merge failed; fix conflicts and then commit the result.

原因:
首次创建远程仓库后本地仓库和远程仓库没有关联. 

解决:
通过 --allow-unrelated-histories 参数强制关联, 然后解决冲突, 然后重新提交
```



```sh
[chenchen@dev3_10.211.21.18 cassi.sports.weibo.cn]$ git co 0ef91149028bc23168dc application/modules/Game/controllers/Start.php
[chenchen@dev3_10.211.21.18 cassi.sports.weibo.cn]$ git co 0ef91149028bc23168dc application/models/ActorStartGame.php

将某个log的文件迁出覆盖本地分支文件
```



```sh
提交代码三连：

git add file
git commit -m '修改原因'
git push
1
2
3
执行完了commit后，还没有执行push，想要撤销这次的commit

解决方案（使用命令）：

git reset --soft HEAD^
1
这样就成功撤销了commit，如果想要连着add也撤销的话，–soft改为–hard（删除工作空间的改动代码）

git reset --hard HEAD^
1
命令详解：
HEAD^ 表示上一个版本，即上一次的commit，也可以写成HEAD~1
如果进行两次的commit，想要都撤回，可以使用HEAD~2

-soft
不删除工作空间的改动代码 ，撤销commit，不撤销git add file

-hard
删除工作空间的改动代码，撤销commit且撤销add

另外一点，commit注释写错了，想要改一下注释，当然也是可以的（详情请看这篇文章）：

git commit --amend
1

```

```sh
一、理解 git fetch, git pull 

要讲清楚git fetch、git pull,必须要附加讲清楚git remote，git merge 、远程repo， branch 、 commit-id 以及 FETCH_HEAD。

1. git remote

 git是一个分布式的结构，这意味着本地和远程是一个相对的名称。
本地仓库（repository）要与远程仓库（repository）配合完成，版本对应必须要有 git remote子命令，通过git remote add来添加当前本地仓库的远程仓库， 有了这个动作本地仓库（repository）就知道了当遇到git push 的时候应该往哪里提交代码
（git push 后不加参数的时候，默认就是git push origin 当前的分支名，比如对本地的master分支执行git push，其实就是git push origin master，当然，如果远程仓库没有master这个分支的话，肯定会报错）。

2. git branch

git天生就是为了多版本分支管理而创造的，因此分支一说，不得不提， 分支就相当于是为了单独记录软件的某一个发布版本而存在的，既然git是分布式的，便有了本地分支和远程分支一说，git branch 可以查看本地分支， git branch -r  可以用来查看远程分支。 本地分支和远程分支在git push 的时候可以随意指定，交错对应，只要不出现版本冲突即可。

3. git merge

git的分布式结构也非常适合多人合作开发不同的功能模块，此时如果每个人都在其各自的分支上开发一个相对独立的模块的话，在每次release制作时都需先将各成员的模块做一个合并操作，用于合并各成员的工作成果，完成集成。 此时需要的就是git merge.

4.git push 和 commit-id

在每次本地工作完成后，都会做一个git commit 操作来保存当前工作到本地仓库(repository)， 此时会产生一个commit-id，这是一个能唯一标识一个版本的序列号。 在使用git push后，这个序列号还会同步到远程仓库(repository)。
在理解了以上git要素之后，分析git fetch 和 git pull 就不再困难了。 

二、git fetch 四种基本用法

1. git fetch 

 这将更新git remote 中所有的远程仓库(repository) 所包含分支的最新commit-id, 将其记录到.git/FETCH_HEAD文件中

2. git fetch remote_repository

 这将更新名称为remote_repository 的远程repository上的所有branch的最新commit-id，将其记录。 

3. git fetch remote_repository remote_branch_name

 这将更新名称为remote_repository 的远程repository上的分支： remote_branch_name

4. git fetch remote_repository remote_branch_name:local_branch_name 

这将更新名称为remote_repository 的远程repository上的分支： remote_branch_name ，并在本地创建local_branch_name 本地分支保存远端分支的所有数据。

FETCH_HEAD： 是一个版本链接，记录在本地的一个文件中，指向目前已经从远程仓库取下来的分支的末端版本。

三、git pull 的运行过程

1. git pull 

首先，基于本地的FETCH_HEAD记录，比对本地的FETCH_HEAD记录与远程仓库的版本号，然后git fetch 获得当前指向的远程分支的后续版本的数据，然后再利用git merge将其与本地的当前分支合并。

git pull 后不加参数的时候，跟git push 一样，默认就是git pull origin 当前分支名，当然远程仓库没有跟本地当前分支名一样的分支的话，肯定会报错。
本地master分支执行git pull的时候，其实就是git pull origin master。

2. 拆解git pull 操作
git pull操作其实是git fetch 与 git merge 两个命令的集合。
git pull  等效于先执行 git fetch origin 当前分支名, 再执行 git merge FETCH_HEAD.

通过上述分析，可以知道，如果要合并代码就并不一定要用git merge命令了，也可以用git pull命名，比如要把远程origin仓库的xx分支合并到本地的yy分支，可以有如下两种做法。
第一种，传统标准的做法：
git fetch origin 目标分支名  // fetch到远程仓库目标分支的最新commit记录到  ./git/FETCH_HEAD文件中
git checkout 要被合并的分支名  // 切换到要合并的分支
git merge FETCH_HEAD  // 将目标分支最新的commit记录合并到当前分支

举例说明：将远程origin仓库的xx分支合并到本地的yy分支。
git fetch origin xx
git checkout yy
git merge FETCH_HEAD
完成。

第二种，直接使用pull命令，将远程仓库的目标分支合并到本地的分支：
git pull <remote_repository_name> <branch_name> 

举例说明：将远程origin仓库的xx分支合并到本地的yy分支

git checkout yy

git pull origin xx
完成。

其实还有一种思路，在前面第一种，第二种方式的基础上，可以这样来思考。

是否可以先本地checkout远程目标分支，或者本地已经有了，先pull更新下来，然后将本地的两个分支进行merge不也可以吗？

答案是肯定的。

举个例子：

将远程origin仓库的xx分支合并到本地的yy分支。

git checkout xx

git pull   // 如果本地没有xx分支的，这一步都可以不执行。

git checkout yy   // 切换到yy分支

git merge xx  //  将xx分支合并到yy分支  这一步可以加上 --no-ff 参数，即 git merge --no-ff

原文地址：git fetch 、git pull、git merge 的理解_Json159的博客-CSDN博客

详解git pull和git fetch的区别：_马恩光的博客-CSDN博客_gitpull和gitfetch


```

```sh
git remote show origin
git  log -5 --stat
```



### git pull origin chenchen:chenchen

```sh
[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git lg -20
* 2f7155d - (69 seconds ago) 060101 — chenchen (HEAD, chenchen)
* 7cdaf2f - (24 hours ago) 053107 — chenchen (origin/chenchen)
* 9f65ff5 - (25 hours ago) 053106 — chenchen
* 3205272 - (25 hours ago) 053105 — chenchen
* 6ab8eb1 - (26 hours ago) 053102 — chenchen

[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git pull
From ssh://git.intra.weibo.com:2222/weibo_sports/szbr.sports.weibo.cn
   7cdaf2f..2f7155d  chenchen   -> origin/chenchen
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> chenchen

[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git branch --set-upstream-to=origin/chenchen chenchen
Branch chenchen set up to track remote branch chenchen from origin.

[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git lg -20
* 2f7155d - (80 seconds ago) 060101 — chenchen (HEAD, origin/chenchen, chenchen)
* 7cdaf2f - (24 hours ago) 053107 — chenchen
* 9f65ff5 - (25 hours ago) 053106 — chenchen
* 3205272 - (25 hours ago) 053105 — chenchen

[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git pull
From ssh://git.intra.weibo.com:2222/weibo_sports/szbr.sports.weibo.cn
   2f7155d..e0ee63c  chenchen   -> origin/chenchen
Already up-to-date.
```





## See Also

https://zhuanlan.zhihu.com/p/51844762 index/stage















