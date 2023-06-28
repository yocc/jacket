# Git

----



[toc]





## Overview

```sh
git st
git br
git lg -20

```

```sh
vim .git/config
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true
[remote "origin"]
    url = ssh://git@git.intra.weibo.com:2222/weibo_sports/szbr.sports.weibo.cn.git
    fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
    remote = origin
    merge = refs/heads/master
// 本地仓库配置,
// 通过 git config --local 查看或者设置,
// 该文件的路径也受环境变量 GIT_COMMON_DIR 控制.

# 以上在本地项目根目录中 .git 目录下的 config 文件内容, 同 $ git config --local -l 展示结果一样
# 也就是 $ git config --local -l 用于查看 .git/config 文件
[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git config --local -l
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
remote.origin.url=ssh://git@git.intra.weibo.com:2222/weibo_sports/szbr.sports.weibo.cn.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=origin
branch.master.merge=refs/heads/master


vim .git/HEAD
ref: refs/heads/chenchen
// 指明当前激活的分支

vim .git/ORIG_HEAD
bce16ee1cc652848463d00494349017a97708927

vim .git/FETCH_HEAD
988fef6e3220b1e0605675c10890ab5b1bef5cd7        branch 'chenchen' of ssh://git.intra.weibo.com:2222/weibo_sports/szbr.sports.weibo.cn

tree .git/refs
└── refs
    ├── heads
    │   ├── chenchen
    │   ├── master
    │   └── test
    ├── remotes
    │   └── origin
    │       ├── chenchen
    │       ├── HEAD
    │       ├── master
    │       └── test
    └── tags

vim .git/description
Unnamed repository; edit this file 'description' to name the repository.
// 供 gitweb (github 的一种前身) 使用, 显示仓库的描述.

vim .git/COMMIT_EDITMSG
061302
// 最后一次提交的注释内容.

vim .git/packed-refs
# pack-refs with: peeled fully-peeled 
ec0cf3cff912b45bdcbdc14ff28cc29333e03edb refs/remotes/origin/master


```

```sh
* ef4b354 - (23 minutes ago) 061302 — chenchen (HEAD, origin/master, origin/chenchen, origin/HEAD, master, chenchen)

HEAD, 当前激活的分支
origin/master, 
origin/chenchen, 
origin/HEAD, 
master, 
chenchen, 
```

```sh
# 查看远程仓库地址
[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git remote -vv
origin  ssh://git@git.intra.weibo.com:2222/weibo_sports/szbr.sports.weibo.cn.git (fetch)
origin  ssh://git@git.intra.weibo.com:2222/weibo_sports/szbr.sports.weibo.cn.git (push)
// 第一列, origin 是仓库名称, 这里是 ‘origin’
// 第二列, 仓库的地址, 一般有两个, 一个是上行地址push, 一个是下行地址fetch

# 和远程仓库建立关联
[chenchen@dev3_10.211.21.18 trim.sports.weibo.cn]$ git remote add origin(这是新起的仓库名字, 在本地的叫法, 不同账号可以起不同的名字在本地) ssh://git@git.intra.weibo.com:2222/weibo_sports/trim.sports.weibo.cn.git(这是远程仓库地址, 唯一)
// 在本地项目中给 远程仓库地址 起一个别名叫 origin
```

```sh
# 远程引用的完整列表信息
[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git ls-remote
From ssh://git@git.intra.weibo.com:2222/weibo_sports/szbr.sports.weibo.cn.git
ef4b354843f2884aa322507cc0ac586ffcf71419        HEAD
ef4b354843f2884aa322507cc0ac586ffcf71419        refs/heads/chenchen
ef4b354843f2884aa322507cc0ac586ffcf71419        refs/heads/master
de46637a672a94cd150e6efcd27dc2681f331010        refs/heads/test
# 远程分支的更多信息
[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git remote show
origin
# kkk
[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git remote show origin
* remote origin
  Fetch URL: ssh://git@git.intra.weibo.com:2222/weibo_sports/szbr.sports.weibo.cn.git
  Push  URL: ssh://git@git.intra.weibo.com:2222/weibo_sports/szbr.sports.weibo.cn.git
  HEAD branch (remote HEAD is ambiguous, may be one of the following):
    chenchen
    master
  Remote branches:
    chenchen tracked
    master   tracked
    test     tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local refs configured for 'git push':
    chenchen pushes to chenchen (up to date)
    master   pushes to master   (up to date)
    test     pushes to test     (up to date)
   
```

```sh
# 远程分支

# 远程跟踪分支
远程跟踪分支是远程分支状态的引用. 它们是你无法移动的本地引用.
它们以 / 的形式命名. origin/master
例如, 如果你想要查看最后一次与远程仓库 origin 通信时 master 分支的状态, 你可以查看 origin/master 分支.
你与同事合作解决一个问题并且他们推送了一个 iss53 分支, 你可能有自己的本地 iss53 分支, 然而在服务器上的分支会以 origin/iss53 来表示.
```

## example

```sh
# 当前在 chenchen 分支上
[chenchen@dev3_10.211.21.18 dollaz2023.sports.weibo.cn]$ git pull
remote: Enumerating objects: 828, done.
remote: Counting objects: 100% (828/828), done.
remote: Compressing objects: 100% (467/467), done.
remote: Total 812 (delta 342), reused 775 (delta 305), pack-reused 0
Receiving objects: 100% (812/812), 452.83 KiB | 0 bytes/s, done.
Resolving deltas: 100% (342/342), completed with 9 local objects.
From ssh://git.intra.weibo.com:2222/weibo_sports/dollaz2023.sports.weibo.cn
   925a4cb..e58ee83  master     -> origin/master
There is no tracking information for the current branch.	// 对于当前分支(chenchen)上没有跟踪信息
Please specify which branch you want to merge with. // 请指明哪个分支想要merge
See git-pull(1) for details

    git pull <remote> <branch>	// 从一个远程分支上拉

If you wish to set tracking information for this branch you can do so with:	// 对于当前分支你可以设置跟踪信息

    git branch --set-upstream-to=origin/<branch> chenchen		// 设置跟踪信息
```
```sh
# 站在 chenchen 分支上
# 拉 远程 master 到本地 chenchen
[chenchen@dev3_10.211.21.18 dollaz2023.sports.weibo.cn]$ git pull origin master
From ssh://git.intra.weibo.com:2222/weibo_sports/dollaz2023.sports.weibo.cn
 * branch            master     -> FETCH_HEAD
Auto-merging config/DictConfig.php
CONFLICT (content): Merge conflict in config/DictConfig.php
Auto-merging conf/application.ini
CONFLICT (content): Merge conflict in conf/application.ini
Removing application/modules/Tell/controllers/Toin.php
Auto-merging application/modules/Game/controllers/Start.php
CONFLICT (add/add): Merge conflict in application/modules/Game/controllers/Start.php
CONFLICT (modify/delete): application/models/Weibo.php deleted in e58ee83f1b405696e5f6d30a2bafd61af3031d90 and modified in HEAD. Version HEAD of application/models/Weibo.php left in tree.
Auto-merging application/models/Crys.php
CONFLICT (add/add): Merge conflict in application/models/Crys.php
Auto-merging application/library/Utils.php
CONFLICT (add/add): Merge conflict in application/library/Utils.php
Auto-merging application/library/Auth.php
CONFLICT (content): Merge conflict in application/library/Auth.php
Automatic merge failed; fix conflicts and then commit the result.
[chenchen@dev3_10.211.21.18 dollaz2023.sports.weibo.cn]$ 

# 问题描述: chenchen 分支长期不用, 导致和 master 分支差距巨大, 冲突很多
# 目标打算: 打算直接将 master 内容替换 到 chenchen 上, 之后再按各自开发.
$ git pull origin master:chenchen -f	// 当前master分支, 先强行将远程master拉回到chenchen分支
$ git co chenchen
$ git push origin chenchen:chenchen -f	// 强行将本地chenchen分支推到远程chenchen分支上


[chenchen@dev3_10.211.21.18 dollaz2023.sports.weibo.cn]$ git co master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 3 commits.		// 当前分支比 远程跟踪分支 领先3个提交
  (use "git push" to publish your local commits)
[chenchen@dev3_10.211.21.18 dollaz2023.sports.weibo.cn]$ git br
  chenchen                eddd136 改字段
* master                  eddd136 [origin/master: ahead 3] 改字段
  remotes/origin/HEAD     -> origin/master
  remotes/origin/chenchen eddd136 改字段
  remotes/origin/master   e58ee83 加字段
  
# 解决直接 push 出现 warning 的问题, 需要手动指明 push 的远程分支
[chenchen@dev3_10.211.21.18 dollaz2023.sports.weibo.cn]$ git push
warning: push.default is unset; its implicit value is changing in
Git 2.0 from 'matching' to 'simple'. To squelch this message
and maintain the current behavior after the default changes, use:

  git config --global push.default matching

To squelch this message and adopt the new behavior now, use:

  git config --global push.default simple

See 'git help config' and search for 'push.default' for further information.
(the 'simple' mode was introduced in Git 1.7.11. Use the similar mode
'current' instead of 'simple' if you sometimes use older versions of Git)

Everything up-to-date
# 解决办法
[chenchen@dev3_10.211.21.18 dollaz2023.sports.weibo.cn]$ git push origin master
Everything up-to-date
```

```sh
[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git pull
remote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 5 (delta 4), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (5/5), done.
From ssh://git.intra.weibo.com:2222/weibo_sports/szbr.sports.weibo.cn
   ef4b354..6f75aac  chenchen   -> origin/chenchen
   ef4b354..6f75aac  master     -> origin/master
Updating ef4b354..6f75aac
Fast-forward
 application/models/GetMklgFmbs.php | 48 ++++++++++++++++++++++++------------------------
 1 file changed, 24 insertions(+), 24 deletions(-)
[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git br
* chenchen                6f75aac [origin/chenchen] 061401
  master                  ef4b354 [origin/master: behind 1] 061302	// 本地master分支落后于远程跟踪1个提交
  remotes/origin/HEAD     -> origin/master
  remotes/origin/chenchen 6f75aac 061401
  remotes/origin/master   6f75aac 061401
  remotes/origin/test     ef4b354 061302
[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git co master
Switched to branch 'master'
Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)
[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git pull
Updating ef4b354..6f75aac
Fast-forward
 application/models/GetMklgFmbs.php | 48 ++++++++++++++++++++++++------------------------
 1 file changed, 24 insertions(+), 24 deletions(-)
[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git co chenchen
Switched to branch 'chenchen'
[chenchen@dev3_10.211.21.18 szbr.sports.weibo.cn]$ git br
* chenchen                6f75aac [origin/chenchen] 061401
  master                  6f75aac [origin/master] 061401
  remotes/origin/HEAD     -> origin/master
  remotes/origin/chenchen 6f75aac 061401
  remotes/origin/master   6f75aac 061401
  remotes/origin/test     ef4b354 061302
```





## Troubleshooting





## FAQ





## See Also

https://www.jianshu.com/p/9701d49810f7





