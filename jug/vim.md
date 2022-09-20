# vim

---



### 目录


[TOC]



### 背景和原因

如果打算在开发时使用 vim 作为 Golang 的编辑器, 那么这里有一些过程和说明

Golang 需要 vim 8+, 但 CentOS 7 的 yum 是 vim 7+, 需要更高版本



### 升级和环境

实现更高版本的方法:

1. 通过 rpm 包的 yum 升级
2. 卸载老版本, 源码安装新版本
3. 新老版本共存, 老版本是全局, 新版本是自己环境使用 (采用)

结合目前运维 dpool 5.0 整体环境和本着开发环境与生产环境的高度一致原则, 选择采用第三种方法实现



### Vim 安装

##### 总览

1. 保持老版本不动, 包括 vim环境配置, bin, libs 等等

2. 重新源码安装 vim 8+, 保证与现有老版本无交集

   1. 全局安装位置, /usr/local/vim8

   2. 本地安装位置, /data1/www/htdocs/chenchen


##### 步骤

1. 查看清楚老版本

   ```shell
   [chenchen@dev3_10.211.21.18 chenchen]$ rpm -qa | grep vim			# Query all installed packages
   vim-minimal-7.4.160-1.el7.x86_64
   vim-common-7.4.160-1.el7.x86_64
   vim-filesystem-7.4.160-1.el7.x86_64
   vim-enhanced-7.4.160-1.el7.x86_64
   [chenchen@dev3_10.211.21.18 chenchen]$ rpm -ql vim-enhanced-7.4.160-1.el7.x86_64
   /etc/profile.d/vim.csh
   /etc/profile.d/vim.sh
   /usr/bin/rvim
   /usr/bin/vim
   /usr/bin/vimdiff
   /usr/bin/vimtutor
   [chenchen@dev3_10.211.21.18 chenchen]$ /usr/bin/vim --version			# vim 7.4
   VIM - Vi IMproved 7.4 (2013 Aug 10, compiled Jun 10 2014 06:55:55)
   Included patches: 1-160
   Modified by <bugzilla@redhat.com>
   Compiled by <bugzilla@redhat.com>
   Huge version without GUI.  Features included (+) or not (-):
   +acl             +farsi           +mouse_netterm   +syntax
   ...
   ...
   ...
   +extra_search    -mouse_jsbterm   -sun_workshop    -xpm
      system vimrc file: "/etc/vimrc"
        user vimrc file: "$HOME/.vimrc"
    2nd user vimrc file: "~/.vim/vimrc"
         user exrc file: "$HOME/.exrc"
     fall-back for $VIM: "/etc"
    f-b for $VIMRUNTIME: "/usr/share/vim/vim74"
   Compilation: gcc -c -I. -Iproto -DHAVE_CONFIG_H     -O2 -g -pipe -Wall -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches   -m64 -mtune=generic -D_GNU_SOURCE -D_FILE_OFFSET_BITS=64 -U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=1      
   Linking: gcc   -L. -Wl,-z,relro -fstack-protector -rdynamic -Wl,-export-dynamic -Wl,--enable-new-dtags -Wl,-rpath,/usr/lib64/perl5/CORE  -Wl,-z,relro  -L/usr/local/lib -Wl,--as-needed -o vim        -lm -lnsl  -lselinux  -lncurses -lacl -lattr -lgpm -ldl   -Wl,--enable-new-dtags -Wl,-rpath,/usr/lib64/perl5/CORE  -fstack-protector  -L/usr/lib64/perl5/CORE -lperl -lresolv -lnsl -ldl -lm -lcrypt -lutil -lpthread -lc       
   [chenchen@dev3_10.211.21.18 chenchen]$ 
   ```


2. 在指定位置安装新版本

      ```shell
      # $ sudo yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel (安装依赖)
      # $ sudo yum install -y gcc make ncurses ncurses-devel (安装依赖)
      # $ sudo yum install -y  git-core
      
      [chenchen@localhost ~]$ git --version
      git version 1.8.3.1
      [chenchen@localhost ~]$ git clone https://github.com/vim/vim.git
      Cloning into 'vim'...
      remote: Enumerating objects: 137529, done.
      remote: Total 137529 (delta 0), reused 0 (delta 0), pack-reused 137529
      Receiving objects: 100% (137529/137529), 120.60 MiB | 3.10 MiB/s, done.
      Resolving deltas: 100% (116591/116591), done.
      [chenchen@localhost ~]$ l
      [chenchen@localhost ~]$ cd vim
      [chenchen@localhost vim]$ make distclean			# 清除 configure
      [chenchen@localhost vim]$ ./configure --help
      [chenchen@localhost vim]$ ./configure --prefix=/usr/local/vim8
      [chenchen@localhost vim]$ make clean
      [chenchen@localhost vim]$ make
      [chenchen@localhost vim]$ sudo make install
      
      $ make distclean, 类似 make clean, 但同时也将 configure 生成的文件全部删除掉, 包括 Makefile.
      $ make clean, 清除上次的 make 命令所产生的 object 文件(后缀为 ".o" 的文件)及可执行文件.
      
      
      # 修改配置 ~/.bashrc or ~/.bash_profile
      # vim8, 位置要在 vim7 之前
      export PATH=/usr/local/vim8/bin:$PATH
      [chenchen@localhost ~]$ . ~/.bash_profile
      
      [chenchen@localhost ~]$ vim --version
      VIM - Vi IMproved 8.2 (2019 Dec 12, compiled Nov 8 2021 21:13:02)
      Included patches: 1-3582
      Compiled by chenchen@localhost.localdomain
      Huge version without GUI. Features included (+) or not (-):
      +acl +file_in_path +mouse_urxvt -tag_any_white
      ...
      ...
      ...
      +extra_search +mouse_sgr +tag_binary
      -farsi -mouse_sysmouse -tag_old_static
      system vimrc file: "$VIM/vimrc"
      user vimrc file: "$HOME/.vimrc"
      2nd user vimrc file: "~/.vim/vimrc"
      user exrc file: "$HOME/.exrc"
      defaults file: "$VIMRUNTIME/defaults.vim"
      fall-back for $VIM: "/usr/local/vim8/share/vim"
      Compilation: gcc -std=gnu99 -c -I. -Iproto -DHAVE_CONFIG_H -g -O2 -D_REENTRANT -U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=1
      Linking: gcc -std=gnu99 -L/usr/local/lib -Wl,--as-needed -o vim -lm -ltinfo -lselinux -ldl
      [chenchen@localhost ~]$
      
      # vimrc 配置文件加载顺序:
      system vimrc file: "$VIM/vimrc"
      user vimrc file: "$HOME/.vimrc"
      2nd user vimrc file: "~/.vim/vimrc"
      user exrc file: "$HOME/.exrc"
      defaults file: "$VIMRUNTIME/defaults.vim"
      fall-back for $VIM: "/usr/local/vim8/share/vim"
      通过实验理解, 以上 vim --version 的加载顺序中, 命中一个就立即返回, 不会累加
      
      # 在 vim 运行中, :scriptnames 查看插件和插件加载顺序
      
      ```



### Golang 插件相关

##### 总览

Golang 相关的配置对后面 vim-go 插件的安装有影响, 需要特别注意.


vim 插件 (plugins) 安装之前要先安装插件管理器 (package managers), 而且插件管理器也有很多品牌(比如: Vim 8 packages, Neovim packages, Vundle, etc.), 根据 vim-go 的 git 提示, 使用了第一个 Vim 8 packages, 事后证明 Vim 8 packages 是明智的, 他是最新的 Vim 推荐的配置最简单的 ... 

通过 Vim 8 packages 插件管理器安装的插件按照惯例都会安装(拷贝)到 `~/.vim` 目录下, 按各个插件的安装文档操作.

```shell
# 在 vim 运行中, :scriptnames 查看所有已经安装的插件和插件加载顺序
:scriptnames
```

##### Go-lang 配置
```shell
# Golang 运行时配置移到 ~/.config/go/env
# GO111MODULE=on
GOPROXY=https://goproxy.cn,direct


# ~/.bashrc or ~/.bash_profile
# proxy
# export http_proxy="http://10.81.254.21:3128"
# export https_proxy="http://10.81.254.21:3128"

# vim8
export PATH=/usr/home/chenchen/htdocs/vim8/bin:$PATH

# go 
export PATH=$PATH:/usr/home/chenchen/htdocs/go1173/bin

# Golang, GOPATH, GOROOT, GOBIN
export GOROOT=/usr/home/chenchen/htdocs/go1173
export GOPATH=/usr/home/chenchen/htdocs/tmp/go1173/gopath
export GOBIN=/usr/home/chenchen/htdocs/tmp/go1173/gobin

# upx
export PATH=$PATH:/home/chenchen/tmp/upx-3.96-amd64_linux
```


##### vim-go plugin install

1. 采用 `Vim 8 packages` 插件管理器安装

   ```shell
   * Vim 8 packages
       * git clone https://github.com/fatih/vim-go.git ~/.vim/pack/plugins/start/vim-go
   ```

   > ⚠️
   >
   > 由于公司服务可能禁掉了对 github.com 的访问, 所以通过 `Vim 8 packages` 插件管理器安装插件时会遇到问题, 此时采取代理方式: 
   >
   > 网上找了一个用 "hub.fastgit.org" 替换 "github.com", 后文不在重复说明.
   >
   > ```shell
   > # 用 "hub.fastgit.org" 替换 "github.com"
   > $ git clone https://hub.fastgit.org/vim/vim.git
   > $ git clone https://hub.fastgit.org/fatih/vim-go.git ~/.vim/pack/plugins/start/vim-go
   > $ git clone https://hub.fastgit.org/preservim/nerdtree.git ~/.vim/pack/vendor/start/nerdtree
   > $ git clone https://hub.fastgit.org/vim-airline/vim-airline ~/.vim/pack/dist/start/vim-airline
   > $ git clone https://hub.fastgit.org/vim-airline/vim-airline-themes ~/.vim/pack/dist/start/vim-airline-themes
   > ```

2. 继续安装各种二进制文件

   启动 vim 进入 vim 编辑环境, 执行 `:GoInstallBinaries` 完成安装.

   这里会使用到环境变量 `GOBIN`, `GOPATH` 等, 需要事先提前配置正确.

   > 由于禁用 github.com, 同时在  `:GoInstallBinaries` 时无法修改, 所以这部分没有更好办法, 只能从另外机器上拷贝打包放入 `GOBIN` 下面. 😦
   >
   > ```shell
   > [chenchen@localhost go1173]$ rsync -avzP ./gobin1173 10.211.21.18::chenchen/ --port=8873
   > ```
   >
   > ❓ 继续寻找一种解决办法...

3. 继续设置 vim 环境下的 vim-go 插件帮助文档

   ```shell
   # vim 编辑环境下通过 :helptags 设置
   :help vim-go
   :helptags ~/.vim/pack/plugins/start/vim-go/doc
   :help vim-go
   ```

4. ~/.vimrc 配置文件

   出于方便考虑, 使用 `~/.vimrc` 这个配置文件 (上文).

   vim-go 插件似乎没有什么需要配置的.
   
5. Autocomplete

   vim-go 自带代码自动完成, 默认使用的是 gopls 工具支持的 omnifunc, 并且是开启状态.

   ```shell
   # 查看 autocompletion 是否配置正确
   # 在 vim 运行环境下
   :verbose setlocal omnifunc?
     omnifunc=go#complete#Complete
           Last set from ~/.vim/pack/plugins/start/vim-go/ftplugin/go.vim line 30
   
   # 在代码中连续 ctrl+xo 呼出提示后通过 ctrl+n/p 上下选择
   ```

### vim-go uninstall

```shell
$ rm -r ~/.vim/pack/plugins/start/vim-go
```

##### NERDTree plugin

1. The NERDTree is a file system explorer for the Vim editor. 
2. Vim 8+ packages

```shell
$ git clone https://github.com/preservim/nerdtree.git ~/.vim/pack/vendor/start/nerdtree
$ vim -u NONE -c "helptags ~/.vim/pack/vendor/start/nerdtree/doc" -c q			# 设置帮助文档的另一种方式
```

```shell
# vim 编辑环境下
:NERDTree			# 开启 NERDTree, Press ? for help
:help NERDTree
```

3. 常用命令

- 目录前面有"+"号, 按 `Enter` 会展开目录，文件前面是"-"号, 按 `Enter` 会在右侧窗口展现该文件的内容, 并光标的焦点focus右侧

- ctr+w

- 和编辑文件一样, 通过 `h j k l` 移动光标定位

- i 和 s 可以水平分割或纵向分割窗口打开文件

- t 在标签页中打开

- T 在后台标签页中打开

- p 到上层目录

- P 到根目录

- K 到同目录第一个节点

- J 到同目录最后一个节点

- m 显示文件系统菜单 (添加, 删除, 移动操作)

- ? 帮助

- q 关闭

  ```
  和编辑文件一样，通过h j k l移动光标定位
  打开关闭文件或者目录，如果是文件的话，光标出现在打开的文件中
  go 效果同上，不过光标保持在文件目录里，类似预览文件内容的功能
  i和s可以水平分割或纵向分割窗口打开文件，前面加g类似go的功能
  t 在标签页中打开
  T 在后台标签页中打开
  ctrl+w+w 光标在左右窗口切换
  ctrl+w+r 切换当前窗口左右布局
  ctrl+p 模糊搜索文件
  gT 切换到前一个tab
  g t 切换到后一个tab
  p 到上层目录
  P 到根目录
  K 到同目录第一个节点
  J 到同目录最后一个节点
  m 显示文件系统菜单（添加、删除、移动操作）
  r: 刷新光标所在的目录
  R: 刷新当前根路径
  ? 帮助
  q 关闭
  ```

  

### vim-airline plugin

1. [vim-airline](https://github.com/vim-airline/vim-airline): Lean & mean status/tabline for vim that's light as air. `powerline` 替代品, 100% vimscript, no python needed. 

2. pack feature (native Vim 8 package feature)

   ```shell
   $ git clone https://github.com/vim-airline/vim-airline ~/.vim/pack/dist/start/vim-airline
   ```

   ```shell
   # vim 编辑环境下通过 :helptags 设置
   :helptags ~/.vim/pack/dist/start/vim-airline/doc
   ```

3. Change the vim-airline theme

   `:AirlineTheme {theme-name}` Displays or changes the current theme.

   [Screenshots](https://github.com/vim-airline/vim-airline/wiki/Screenshots)

### vim-airline-themes plugin

1. [vim-airline-themes](https://github.com/vim-airline/vim-airline-themes): This is the official theme repository for [vim-airline](https://github.com/vim-airline/vim-airline)

2. pack feature (native Vim 8 package feature)

   ```shell
   $ git clone https://github.com/vim-airline/vim-airline-themes ~/.vim/pack/dist/start/vim-airline-themes
   ```

   ```shell
   # vim 编辑环境下通过 :helptags 设置
   :helptags ~/.vim/pack/dist/start/vim-airline-themes/doc
   ```

3. ~/.vimrc 设置

   ```shell
   " airline_theme
   let g:airline_theme='dark'
   " 使用 powerline 字体, 默认0, 需要显式开启
   let g:airline_powerline_fonts = 1
   let g:airline#extensions#tabline#enabled = 1
   let g:airline#extensions#tabline#buffer_nr_show = 1
   let g:airline#extensions#tabline#left_sep = ' '
   let g:airline#extensions#tabline#left_alt_sep = '|'
   let g:airline#extensions#tabline#formatter = 'default'
   ```

### Powerline fonts

1. [Powerline fonts](https://github.com/powerline/fonts): This repository contains pre-patched and adjusted fonts for usage with the [Powerline](https://github.com/powerline/powerline) statusline plugin.

2. installation

   ```shell
   # clone
   $ git clone https://github.com/powerline/fonts.git --depth=1
   # install
   cd 各个字体目录
   然后按照本地系统的字体安装方法安装, macOS, Windows, Linux 等等均不同
   ```

3. 终端软件设置字体

### Vim 使用

- `tmux`
- `:terminal`, `:term`
- buffer, window, tab, `:help window`
  - A buffer is the in-memory text of a file. 载入到内存的文件内容
  - A window is a viewport on a buffer. 显示 buffer 内容, 和布局
  - A tab page is a collection of windows. tab 页是用来管理 windows
  - `:ls` 可查看当前已打开的 buffer


### See Also

[rpm]: https://rpm-software-management.github.io/rpm/man/rpm.8.html
[vim]: https://www.vim.org/docs.php
[vim install]: https://github.com/vim/vim/blob/master/src/INSTALL
[Vim packages]: https://vimhelp.org/repeat.txt.html#packages
[vim git]: https://github.com/vim/vim.git
[vim-go plugin]: https://github.com/fatih/vim-go
[Vim 8 packages]: http://vimhelp.appspot.com/repeat.txt.html#packages

[NERDTree git]: https://github.com/preservim/nerdtree
[vim-airline]: https://github.com/vim-airline/vim-airline
[vim-airline-themes]: https://github.com/vim-airline/vim-airline-themes
[vim-airline theme Screenshots]: https://github.com/vim-airline/vim-airline/wiki/Screenshots
[powerline fonts]: https://github.com/powerline/fonts
[powerline fonts-installation]: https://powerline.readthedocs.io/en/master/installation/osx.html#fonts-installation

[tabby]: https://tabby.sh/
[vim8原生内置(naive)插件管理器安装]: https://blog.csdn.net/qq_27825451/article/details/100557133

[nerdtree]: https://vimawesome.com/plugin/nerdtree-red

