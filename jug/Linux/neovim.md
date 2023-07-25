# neovim

[toc]



## Overview

### install

```sh
# 参考: https://github.com/neovim/neovim/wiki/Installing-Neovim 略有改动
[chenchen@dev3_10.211.21.18 htdocs]$ mkdir neovim
[chenchen@dev3_10.211.21.18 htdocs]$ cd neovim
[chenchen@dev3_10.211.21.18 neovim]$ l
total 16K
23330989 drwxr-xr-x 88 chenchen www       12K Jul 24 19:17 ..
23446284 drwxrwxr-x  2 chenchen chenchen 4.0K Jul 24 19:17 .
[chenchen@dev3_10.211.21.18 neovim]$ curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim.appimage
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:00:01 --:--:--     0
  0     0    0     0    0     0      0      0 --:--:--  0:00:01 --:--:--     0
100 10.5M  100 10.5M    0     0  2451k      0  0:00:04  0:00:04 --:--:-- 4024k
[chenchen@dev3_10.211.21.18 neovim]$ l
total 11M
23330989 drwxr-xr-x 88 chenchen www       12K Jul 24 19:17 ..
23446284 drwxrwxr-x  2 chenchen chenchen 4.0K Jul 24 19:17 .
23439489 -rw-rw-r--  1 chenchen chenchen  11M Jul 24 19:17 nvim.appimage
[chenchen@dev3_10.211.21.18 neovim]$ chmod u+x nvim.appimage
[chenchen@dev3_10.211.21.18 neovim]$ l
total 11M
23330989 drwxr-xr-x 88 chenchen www       12K Jul 24 19:17 ..
23446284 drwxrwxr-x  2 chenchen chenchen 4.0K Jul 24 19:17 .
23439489 -rwxrw-r--  1 chenchen chenchen  11M Jul 24 19:17 nvim.appimage
[chenchen@dev3_10.211.21.18 neovim]$ ./nvim.appimage
fuse: failed to exec fusermount: No such file or directory

Cannot mount AppImage, please check your FUSE setup.
You might still be able to extract the contents of this AppImage 
if you run it with the --appimage-extract option. 
See https://github.com/AppImage/AppImageKit/wiki/FUSE 
for more information
open dir error: No such file or directory
[chenchen@dev3_10.211.21.18 neovim]$ ./nvim.appimage --appimage-extract
......
......
[chenchen@dev3_10.211.21.18 neovim]$ l
total 11M
23330989 drwxr-xr-x 88 chenchen www       12K Jul 24 19:17 ..
23439489 -rwxrw-r--  1 chenchen chenchen  11M Jul 24 19:17 nvim.appimage
23446284 drwxrwxr-x  3 chenchen chenchen 4.0K Jul 24 19:18 .
23446285 drwxr-xr-x  3 chenchen chenchen 4.0K Jul 24 19:18 squashfs-root
[chenchen@dev3_10.211.21.18 neovim]$ ./squashfs-root/AppRun --version
NVIM v0.9.1
Build type: Release
LuaJIT 2.1.0-beta3

   system vimrc file: "$VIM/sysinit.vim"
  fall-back for $VIM: "/__w/neovim/neovim/build/nvim.AppDir/usr/share/nvim"

Run :checkhealth for more info
[chenchen@dev3_10.211.21.18 neovim]$ cd squashfs-root
[chenchen@dev3_10.211.21.18 squashfs-root]$ ln -s AppRun nvim
[chenchen@dev3_10.211.21.18 neovim]$

# ~/.bash_profile 增加 PATH, 为了可以直接用, 并且在自己的环境里
# nvim
export PATH=/usr/home/chenchen/htdocs/neovim/squashfs-root:$PATH

[chenchen@dev3_10.211.21.18 squashfs-root]$ . ~/.bash_profile
[chenchen@dev3_10.211.21.18 squashfs-root]$ nvim --version
NVIM v0.9.1
Build type: Release
LuaJIT 2.1.0-beta3

   system vimrc file: "$VIM/sysinit.vim"
  fall-back for $VIM: "/__w/neovim/neovim/build/nvim.AppDir/usr/share/nvim"

Run :checkhealth for more info
[chenchen@dev3_10.211.21.18 squashfs-root]$ 
[chenchen@dev3_10.211.21.18 squashfs-root]$ nvim -V
:checkhealth<Enter>     to optimize Nvim
```

```sh
环境变量清单：用户层面变量(User-Level Variables)
$XDG_DATA_HOME 定义了应存储用户特定的数据文件的基准目录. 默认值是 $HOME/.local/share
$XDG_CONFIG_HOME 定义了应存储用户特定的配置文件的基准目录. 默认值是 $HOME/.config
$XDG_CACHE_HOME 定义了应存储用户特定的非重要性数据文件的基准目录. 默认值是 $HOME/.cache
$XDG_RUNTIME_DIR 定义了应存储用户特定的非重要性运行时文件和一些其他文件对象.
$XDG_CONFIG_DIRS 定义了一套按照偏好顺序的基准目录集, 用来搜索除了 $XDG_CONFIG_HOME 目录之外的配置文件. 该目录中的文件夹应该用冒号(:)隔开. 默认值是 /etc/xdg
$XDG_DATA_DIRS 定义了一套按照偏好顺序的基准目录集, 用来搜索除了 $XDG_DATA_HOME 目录之外的数据文件. 该目录中的文件夹应该用冒号(:)隔开. 默认值是 /usr/local/share/:/usr/share/
```







## FAQ





## See Also

https://github.com/neovim/neovim/wiki/Installing-Neovim

https://baijiahao.baidu.com/s?id=1716081393364875233&wfr=spider&for=pc





