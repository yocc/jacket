# vim9

----

[toc]



## 源码安装

```shell
###
[chenchen@grpc01 tmp]$ git clone https://github.com/vim/vim.git
Cloning into 'vim'...
remote: Enumerating objects: 153578, done.
remote: Counting objects: 100% (264/264), done.
remote: Compressing objects: 100% (162/162), done.
remote: Total 153578 (delta 139), reused 206 (delta 101), pack-reused 153314
Receiving objects: 100% (153578/153578), 176.58 MiB | 1.63 MiB/s, done.
Resolving deltas: 100% (130408/130408), done.

###
[chenchen@grpc01 tmp]$ l
total 4.0K
   597693 drwxrwxr-x.  4 chenchen chenchen   46 Jul  6 21:06 ..
608174180 drwxrwxr-x.  3 chenchen chenchen   17 Jul  6 21:06 .
    27195 drwxrwxr-x. 11 chenchen chenchen 4.0K Jul  6 21:09 vim
[chenchen@grpc01 tmp]$ cd vim
[chenchen@grpc01 vim]$ git pull
Already up-to-date.
[chenchen@grpc01 vim]$ cd src
[chenchen@grpc01 src]$ make distclean													# if you build Vim before
[chenchen@grpc01 src]$ ./configure --help
[chenchen@grpc01 src]$ ./configure --prefix=/usr/local/vim9
[chenchen@grpc01 src]$ make clean
[chenchen@grpc01 src]$ make
[chenchen@grpc01 src]$ sudo make install											# 编译不用 sudo, 但安装一定 sudo
[chenchen@grpc01 src]$ cd /usr/local/vim9/bin
[chenchen@grpc01 bin]$ sudo ln -s /usr/local/vim9/bin/vim /usr/bin/vim9
```

```shell
$ make distclean, 类似 make clean, 但同时也将 configure 生成的文件全部删除掉, 包括 Makefile.
$ make clean, 清除上次的 make 命令所产生的 object 文件(后缀为 ".o" 的文件)及可执行文件.
```

### +python3

```sh
# 细节参考, vim.md. (python3 的源码安装包不能删)
[chenchen@localhost ~]$ cd vim
[chenchen@localhost vim]$ make distclean			# 清除 configure
[chenchen@localhost vim]$ ./configure --help

[chenchen@localhost vim]$ ./configure --prefix=/data1/www/htdocs/chenchen/vim8 --with-features=huge --enable-python3interp=yes --with-python3-command=/data1/www/htdocs/chenchen/python31011/bin/python3 --with-python3-config-dir=/usr/home/chenchen/htdocs/python31011/Python-3.10.11
[chenchen@localhost vim]$ make
[chenchen@localhost vim]$ sudo make install
```



## ~/.bashrc 和 ~/.bash_profile

```shell
# vim9
export PATH=/usr/local/vim9/bin:$PATH
```



## --version

```shell
[chenchen@grpc01 ~]$ vim9 --version
VIM - Vi IMproved 9.0 (2022 Jun 28, compiled Jul  6 2022 21:25:04)
Included patches: 1-44
Compiled by chenchen@grpc01
Huge version without GUI.  Features included (+) or not (-):
+acl               +file_in_path      +mouse_urxvt       -tag_any_white
+arabic            +find_in_path      +mouse_xterm       -tcl
+autocmd           +float             +multi_byte        +termguicolors
+autochdir         +folding           +multi_lang        +terminal
-autoservername    -footer            -mzscheme          +terminfo
-balloon_eval      +fork()            +netbeans_intg     +termresponse
+balloon_eval_term +gettext           +num64             +textobjects
-browse            -hangul_input      +packages          +textprop
++builtin_terms    +iconv             +path_extra        +timers
+byte_offset       +insert_expand     -perl              +title
+channel           +ipv6              +persistent_undo   -toolbar
+cindent           +job               +popupwin          +user_commands
-clientserver      +jumplist          +postscript        +vartabs
-clipboard         +keymap            +printer           +vertsplit
+cmdline_compl     +lambda            +profile           +vim9script
+cmdline_hist      +langmap           -python            +viminfo
+cmdline_info      +libcall           -python3           +virtualedit
+comments          +linebreak         +quickfix          +visual
+conceal           +lispindent        +reltime           +visualextra
+cryptv            +listcmds          +rightleft         +vreplace
+cscope            +localmap          -ruby              +wildignore
+cursorbind        -lua               +scrollbind        +wildmenu
+cursorshape       +menu              +signs             +windows
+dialog_con        +mksession         +smartindent       +writebackup
+diff              +modify_fname      -sodium            -X11
+digraphs          +mouse             -sound             -xfontset
-dnd               -mouseshape        +spell             -xim
-ebcdic            +mouse_dec         +startuptime       -xpm
+emacs_tags        -mouse_gpm         +statusline        -xsmp
+eval              -mouse_jsbterm     -sun_workshop      -xterm_clipboard
+ex_extra          +mouse_netterm     +syntax            -xterm_save
+extra_search      +mouse_sgr         +tag_binary        
-farsi             -mouse_sysmouse    -tag_old_static    
   system vimrc file: "$VIM/vimrc"
     user vimrc file: "$HOME/.vimrc"
 2nd user vimrc file: "~/.vim/vimrc"
      user exrc file: "$HOME/.exrc"
       defaults file: "$VIMRUNTIME/defaults.vim"
  fall-back for $VIM: "/usr/local/vim9/share/vim"
Compilation: gcc -std=gnu99 -c -I. -Iproto -DHAVE_CONFIG_H -g -O2 -D_REENTRANT -U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=1 
Linking: gcc -std=gnu99 -L/usr/local/lib -Wl,--as-needed -o vim -lm -ltinfo -lselinux -lrt -ldl 
[chenchen@grpc01 ~]$ 
```

```shell
# 环境变量
[chenchen@grpc01 ~]$ echo $VIMRUNTIME					# 空

[chenchen@grpc01 ~]$ echo $HOME
/home/chenchen
[chenchen@grpc01 ~]$ echo $VIM								# 空

[chenchen@grpc01 ~]$ 

# 配置文件
   system vimrc file: "$VIM/vimrc"
     user vimrc file: "$HOME/.vimrc"					# 采用
 2nd user vimrc file: "~/.vim/vimrc"
      user exrc file: "$HOME/.exrc"
       defaults file: "$VIMRUNTIME/defaults.vim"
  fall-back for $VIM: "/usr/local/vim9/share/vim"
  
# 配置文件说明:
一般我只用 `user vimrc file: "$HOME/.vimrc"` 这一个配置文件, 备份也只备份这一个配置文件.
```



## Golang 插件

### Overview

由于本次安装是在原有安装过 vim 8 的机器上, 再安装了 vim 9, 所以, ~/.vim/ 目录下已经有各种插件, 所以进入 vim 后风格样式都有现成的. 并且 `:scriptnames` 也有插件. 唯一差别是



### Vim 8 packages

参考: https://vimhelp.org/repeat.txt.html#packages

vim 插件 (plugins) 安装之前要先安装插件管理器 (package managers), 而且插件管理器也有很多品牌(比如: Vim 8 packages, Neovim packages, Vundle, etc.), 根据 vim-go 的 git 提示, 使用了第一个 Vim 8 packages, 事后证明 Vim 8 packages 是明智的, 他是最新的 Vim 推荐的配置最简单的 ... 

> 何谓 Vim 8 packages (package manager)? 
>
> 其实并不是什么软件, 而是 vim 自带的安装插件的规定, 这是 vim 自带的, 无需配置, 只需按规则操作. 
>
> 操作方法很简单, 就是将插件按规则拷贝到~/.vim 目录下. 拷贝方式通常是通过 git clone: 
>
> `git clone https://github.com/fatih/vim-go.git ~/.vim/pack/plugins/start/vim-go`

### plugin

##### plugin list

通过 Vim 8 packages 插件管理器安装的插件按照惯例都会安装(拷贝)到 `~/.vim` 目录下, 按各个插件的安装文档操作.

```shell
# 在 vim 运行中, `:scriptnames` 查看所有已经安装的插件和插件加载顺序
:scriptnames
```

##### vim-go plugin install

1. 采用 `Vim 8 packages` 插件管理器安装

   ```shell
   * Vim 8 packages
       $ git clone https://github.com/fatih/vim-go.git ~/.vim/pack/plugins/start/vim-go
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
   >
   > ```shell
   > 通过 clash, 让 Linux 主机系统翻墙, 另参见: 
   > ```

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

##### vim-go uninstall

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

```sh
" ~/.vimrc 配置文件, NERDTree 的配置

" NERDTree
" nmap '' :NERDTreeToggle<CR>         " NerdTree 将在没有文件的终端中启动 vim 时自动打开
nnoremap <C-t> :NERDTreeToggle<CR>    " ctrl+t, 启闭
nnoremap <C-f> :NERDTreeFocus<CR>     " 把侧边栏定位到当前的文件
" nnoremap <C-n> :NERDTree<CR>          " 在当前文件上打开 文件管理
" nnoremap <C-l> :call CocActionAsync('jumpDefinition')<CR>

" let g:NERDTreeDirArrowExpandable="+"
" let g:NERDTreeDirArrowCollapsible="~"
let NERDTreeDirArrowExpandable="+"
let NERDTreeDirArrowCollapsible="~"

" NERDTree 键入 展开 侧边栏树形结构
" NERDTreeToggle 触发打开和关闭
" NERDTreeFind 把侧边栏定位到当前的文件

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

  

##### vim-airline plugin

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



##### vim-airline-themes plugin

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

##### Powerline fonts

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

##### vim-vue

1. 

2. installation

   ```sh
   * Vim 8 packages
   # clone
   $ git clone https://github.com/posva/vim-vue.git ~/.vim/pack/plugins/start/vim-vue
   # install
   
   ```
   
   



### Vim 使用

- `tmux`

- `:terminal`, `:term`

- buffer, window, tab, `:help window`
  - A buffer is the in-memory text of a file. 载入到内存的文件内容
  - A window is a viewport on a buffer. 显示 buffer 内容, 和布局
  - A tab page is a collection of windows. tab 页是用来管理 windows
  - `:ls` 可查看当前已打开的 buffer
  
   

### Golang 本身

```shell
# 参见, go1.18.3.md
```



### Git

```shell
# ~/.gitconfig

[https]
    proxy = http://127.0.0.1:7890
[http]
    proxy = http://127.0.0.1:7890
[all]
    proxy = socks5://127.0.0.1:7890
```





## FAQ

```shell
```



## Plugin

### NERDTree

```shell
# 以下代码块均发生在 ~/.vimrc 文件中
```

### 从缓冲区写文件到磁盘后, 目录树中没有此文件, 需要刷新/重新加载

```shell
在 NERDTree 选中下, 按 `r` 键
```

### How can I map a specific key or shortcut to open NERDTree?

NERDTree doesn't create any shortcuts outside of the NERDTree window, so as not to overwrite any of your other shortcuts. Use the `nnoremap` command in your `vimrc`. You, of course, have many keys and NERDTree commands to choose from. Here are but a few examples.

```vim
let mapleader = "\"												# 默认是反斜线`\`
nnoremap <leader>n :NERDTreeFocus<CR>			# 通过 leader 快捷, 迅速将焦点回到 NERDTree

nnoremap <C-n> :NERDTree<CR>
nnoremap <C-t> :NERDTreeToggle<CR>
nnoremap <C-f> :NERDTreeFind<CR>
```

### How do I open NERDTree automatically when Vim starts?

Each code block below is slightly different, as described in the `" Comment lines`.

```vim
" 启动 NERDTree 并将光标留在其中
autocmd VimEnter * NERDTree
```

------

```vim
" 启动 NERDTree 并将光标放回另一个窗口中
autocmd VimEnter * NERDTree | wincmd p
```

------

```vim
" Start NERDTree when Vim is started without file arguments.
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 0 && !exists('s:std_in') | NERDTree | endif
```

------

```vim
" Start NERDTree. If a file is specified, move the cursor to its window.
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * NERDTree | if argc() > 0 || exists("s:std_in") | wincmd p | endif
```

------

```vim
" Start NERDTree, unless a file or session is specified, eg. vim -S session_file.vim.
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 0 && !exists('s:std_in') && v:this_session == '' | NERDTree | endif
```

------

```vim
" Start NERDTree when Vim starts with a directory argument.
autocmd StdinReadPre * let s:std_in=1
autocmd VimEnter * if argc() == 1 && isdirectory(argv()[0]) && !exists('s:std_in') |
    \ execute 'NERDTree' argv()[0] | wincmd p | enew | execute 'cd '.argv()[0] | endif
```

### How can I close Vim or a tab automatically when NERDTree is the last window?

```vim
" 如果 NERDTree 是唯一 tab 中剩下的唯一窗口，则退出 Vim
autocmd BufEnter * if tabpagenr('$') == 1 && winnr('$') == 1 && exists('b:NERDTree') && b:NERDTree.isTabTree() | quit | endif
```

------

```vim
" 如果 NERDTree 是其中唯一剩下的窗口, 请关闭该 tab
autocmd BufEnter * if winnr('$') == 1 && exists('b:NERDTree') && b:NERDTree.isTabTree() | quit | endif
```

### How can I prevent other buffers replacing NERDTree in its window?

```vim
" 如果另一个 buffers 试图替换 NERDTree, 请将其放在另一个 window 中, 然后带回 NERDTree
autocmd BufEnter * if bufname('#') =~ 'NERD_tree_\d\+' && bufname('%') !~ 'NERD_tree_\d\+' && winnr('$') > 1 |
    \ let buf=bufnr() | buffer# | execute "normal! \<C-W>w" | execute 'buffer'.buf | endif
```

### Can I have the same NERDTree on every tab automatically?

```vim
" 在每个新 tab 上打开现有的 NERDTree
autocmd BufWinEnter * if getcmdwintype() == '' | silent NERDTreeMirror | endif
```

or change your NERDTree-launching shortcut key like so:

```vim
" 在显示之前镜像 NERDTree. 这使得它在所有 tab 上都相同
nnoremap <C-n> :NERDTreeMirror<CR>:NERDTreeFocus<CR>
```

### How can I change the default arrows?

```vim
let g:NERDTreeDirArrowExpandable = '▸'
let g:NERDTreeDirArrowCollapsible = '▾'
```

The preceding values are the non-Windows default arrow symbols. Setting these variables to empty strings will remove the arrows completely and shift the entire tree two character positions to the left. See `:h NERDTreeDirArrowExpandable` for more details.

## Installation

Unzip the archive into your ~/.vim directory. That should put NERD_tree.vim in ~/.vim/plugin and NERD_tree.txt in ~/.vim/doc.  Run :helptags. Go :help NERD_tree.txt for the help page.

1. 解压存档到 ~/.vim 目录下
2. 将 NERD_tree.vim 文件放到 ~/.vim/plugin 目录下
3. 将 NERD_tree.txt 文件放到 ~/.vim/doc 目录下



## Vim 使用

- sudo

  ```shell
  :w !sudo tee %
  :w !sudo tee  /var/aa.txt
  ```

- `tmux`
- `:!python3 %`
- `:terminal`, `:term`
- buffer, window, tab, `:help window`
  - A buffer is the in-memory text of a file. 载入到内存的文件内容
  - A window is a viewport on a buffer. 显示 buffer 内容, 和布局的视口
  - A tab page is a collection of windows. tab 页是 windows 集合
  
    > 一个 tab 下面含有 n 个 window, 一个 window 下面含有多个 buffer
    >
    > tab 切换: 		ctrl-n, ctrl-p, :tabs, 2gt
    >
    > window 切换: ctrl-w-w, ctrl-w-j, 
    >
    > buffer 切换: 	ctrl-b-n, ctrl-b-p, :ls, :3b, :b2

```shell
# buffers
:ls
:badd xx.yy			# buffer add = badd 文件名
:bd1 						# buffer delete = bd1
% 正在当前激活 window 中打开的 buffer
a 这个 buffer 是 active 的
:b2 						# 加载 2 号 buffer 到当前窗口


- （非活动的缓冲区）
a （当前被**缓冲区）    #指光标所在的缓冲区
h （隐藏的缓冲区）
% （当前的缓冲区）          #是指当前窗口可见的缓冲区，因为可以分割窗口，可能有多个
# （交换缓冲区）             # 代表轮换文件，按 <C - ^> 可以在当前，怎么指定轮换？
= （只读缓冲区）
+ （已经更改的缓冲区）


命令 :ls 可查看当前已打开的buffer
命令 :b num 可切换buffer (num为buffer list中的编号)

其它命令:
:bn -- buffer列表中下一个 buffer
:bp -- buffer列表中前一个 buffer
:b# -- 你之前所在的前一个 buffer

# nmap, noremap, nnoremap, <leader>
nmap <C-b>n  :bnext<CR>;
nmap <C-b>p  :bprev<CR>;
nnoremap <C-t> :NERDTreeToggle<CR>
noremap <S-space> <C-b>
noremap <leader>t :NERDTreeToggle<CR>

map, 作用于 normal, visual 模式
nmap, 仅在 normal 模式, 
vmap, 仅在 visual 模式, 例如: vmap \ U, 按\切换就是U的意思, 就是切换大小写
imap, 仅在 insert 模式, 例如: 

例如: 
:map td :tabnew .<cr>, 在其作用模式(普通、可视、操作符)下, 输入td等价于输入 :tabnew .回车

递归
nmap - dd
nmap \ -
以上是一组, 在 normal 模式下, 按-就是dd, 按\就是-, -又是dd, 可以自动找到最后

非递归(norecursion)
nnoremap, normal 模式
vnoremap, visual 模式
inoremap, insert 模式
任何时候都建议用非递归映射
nnoremap py :!python %

<leader>
键位一共就那么多, 总会不够用, 所以设置一个<leader>会是一个不错的选择.
:let mapleader = ","		"我习惯用逗号作为leader
然后就可以使用 :map <leader>- dd这样子设置映射了.
这个映射的意思是, 先按下逗号, 再按下-, 会删除本行(dd).

# let l:, g:, b:, ....
See :help internal-variables
内部变量
It lists the following types:
                (nothing) In a function: local to a function; otherwise: global 
                如果不写, 在函数内就是局部, 否则就是全局
buffer-variable    b:     Local to the current buffer.                          
window-variable    w:     Local to the current window.                          
tabpage-variable   t:     Local to the current tab page.                        
global-variable    g:     Global.                                         # 函数外都是全局      
local-variable     l:     Local to a function.                            # 函数内都是局部      
script-variable    s:     Local to a :source'ed Vim script.                     
function-argument  a:     Function argument (only inside a function).           
vim-variable       v:     Global, predefined by Vim.                       # vim 保留预定义的全局


跳跃指令 jumps
类似浏览器中的 前进, 后退
ctrl-]
ctrl-o
ctrl-i
ctrl-[
:ju, 显示所有可以跳跃的地方
```

```shell
1. buffer

vim 中 buffer 的概念对应 UE 等编辑器中 tab 的概念, 我们使用 vim a.py 打开一个文件后, 使用 :ls 命令可以看到当前所有的buffer, 比如:
:ls
  1 %a   "a.py"                         line 1

使用: badd b.py 可以打开 b.py 的 buffer(badd 代表 buffer add, 也可以简写成 bad), 只不过这个时候 b.py 的 buffer 是hidden 的, 使用 :ls 可以看到如下输出:
:ls                                                                         
  1 %a   "a.py"                         line 1
  2      "b.py"                         line 1

输出中，左侧是buffer的编号，有%的表示是当前激活的window中打开的buffer，a表示这个buffer是active的，双引号中的字符串表示了buffer对应的文件名字，line n表示当前cursor处于该buffer的哪一行。
我们使用:b2命令将b.py的buffer加载到当前窗口，然后使用:ls命令查看如下：

:ls
  1 #    "a.py"                         line 1
  2 %a   "b.py"                         line 1

说明我们目前确实是在b.y的buffer中。我们可以使用:bd1删除a.py的buffer（bd代表buffer delete，当然删除前需要保存buffer 1的更改），然后使用:ls命令查看如下：

:ls
  2 %a   "b.py"                         line 1

熟悉了buffer的创建、切换、删除、查看buffer列表之后，有人可能会问，我能够方便的使用:sp及:vsp分割窗口，为什么要用buffer呢？不要忘了，能使用:sp及:vsp分割窗口在大型项目中工作的人，显示器一定很大，buffer在显示器较小的时候就很有优势了，它能够实现在文件间的快速切换。

```

```shell
2. windows

# 左右
:100winc>
:100winc<

# 上下
:50winc+
:50winc-

#
ctrl+w+=+：让左右上下各个分屏宽度，高度均等。
ctrl+w+_(shift + -) :当前屏幕高度扩展到最大
ctrl+w+|(shift + \) :当前屏幕宽度扩展到最大
ctrl+w+x: 交换窗口

ctrl+w+20<		横向缩小20
ctrl+w+20-		纵向缩小20


vim -o file1 file2 file3, 打开三个文件每个文件对应一个 window, 窗口之间水平分割
vim -O file1 file2 file3, 打开三个文件每个文件对应一个 window, 窗口之间垂直分割

:term
:vert term
:tab term
:leftabove vert term
:rightbelow vert term
:rightbelow term
:bo term									# 最下
:term ++rows=20						# 以特定高度/行数打开
:bo term ++rows=20				# 以特定高度/行数打开

:leftabove vert term			# 在左侧打开终端
:abo vert term

:rightbelow vert term			# 在右侧打开终端
:bel vert term
:help rightbelow

关闭这个terminal：[Ctrl+w]:quit!，要加!，因为vim认为这个工作没有结束，不能这么轻易地关了。
```

```shell
3. tabs
tabe xxx.yy = tab edit xxx.yy 				# 打开文件
tab 的标题显示的含义:
3 9: hell1
本 tab 含有 3 个window, 其中在第 1 个窗口里显示的是 9 号 buffer 是当前焦点, ":" 冒号后面是第 1 个窗口里当前焦点文件名

# :tabs 查看 tabs 列表
:tabs
Tab page 1
    NERD_tree_1
    hello.go
    hello.go
Tab page 2
    NERD_tree_1
>   ~/.vimrc
Tab page 3
    NERD_tree_1
    hell1
    saas2
    
'>' 表示 cursor 当前在 tab 2 的 samp.py 上, 使用 1gt 即可快速切换到 tab 1
1gt, 2gt 可以在任何时候直接敲入切换 tab, 不用:冒号起步.
如果当前 tab 分隔的 window 比较多, 可以使用 :tabc 命令关闭当前 tab (tabc 代表 tab close)
:tabm2, 将当前 tab 交换到左起第三位, 从0开始计数, 第一位是0.

vim -p file1 file2 file3, 打开三个文件, 每个文件对应一个 tab
```



## map, remap noremap, inoremap, vnoremap, nnorremap

```shell
# Recursive Mapping
map a b
map c a
默认 map 是递归的, 等价于:
map c b

# 由于 map 默认是 递归的, 所以, 有时候需要声明非递归来克制
noremap = no recursion map 的意思
noremap Y y

# 又由于 vim 分三种模式, insert, visual, normal, 所以针对细分三模式
inoremap = insert no recursion map
vnoremap = visual no recursion map
nnoremap = normal no recursion map
```



```shell

:1,90s/redis/Redis/g ;把1-90行的redis替换为Redis。语法n1,n2s/原关键字/新关键字/g，n1
代表其实行,n2代表结尾行,g是必须要的

```



## Fold

```shell
展开:
zo - 打开一个光标折叠
zO - 递归打开所有光标这叠
折叠:
zc - 关闭一个光标折叠
zC - 递归关闭所有光标这叠


全局打开
zr - 打开一层全局折叠: 在'foldlevel'中加1
zR - 打开所有全局折叠: 'foldlevel' 设置为最高
全局折叠
zm - 关闭一层全局折叠: 在'foldlevel'中加1
zM - 关闭所有全局折叠: 'foldlevel' 设置为0


zx - 
zX - 

zn - 重置'foldenable', 全打开
zN - 全关闭 'foldenable'

za - 循环切换打开和关闭一个折叠
zA - 循环切换递归打开和关闭一个折叠

zv - 查看光标线; 打开足够的折叠, 使光标所在的线条不折叠. 仅从折叠线上看不折叠
```



## See Also

https://www.vim.org/git.php

https://vimhelp.org/repeat.txt.html#packages

https://vimawesome.com/plugin/nerdtree-red

`:help window`

`:help CTRL-W`

`:help :map`
`:help :noremap`
`:help recursive_mapping`
`:help :map-modes`

https://www.mianshigee.com/project/vim-vue



