# Git

----

[toc]



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
```



## git config

```shell
[chenchen@dev3_10.211.21.18 ~]$ git config --list
user.email=chenchen@staff.sina.com.cn
user.name=chenchen
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

## tag

```shell
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





## See Also

















