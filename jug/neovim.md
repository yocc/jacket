# neovim

----

[toc]



## Install, binary

```shell
# https://github.com/neovim/neovim
在 https://github.com/neovim/neovim/releases, releases 里下载稳定二进制版本, Download nvim-linux64.tar.gz
解压 Extract: tar xzvf nvim-linux64.tar.gz
将 nvim-linux64 目录拷贝到 /usr/local/ 下, 并改名字为 nvim, 即, /usr/local/nvim
看一下 vim 在什么位置上, 将 nvim 放到一起吧.
[chenchen@grpc01 bin]$ whereis vim
vim: /usr/bin/vim /usr/local/vim8.bak /usr/share/vim /usr/local/vim8/bin/vim /usr/share/man/man1/vim.1.gz

# shortcut
$ sudo ln -s /usr/local/nvim/bin/nvim /usr/bin/nvim

sudo ln -s /squashfs-root/AppRun /usr/bin/nvim
nvim
```

## Linux

### AppImage ("universal" Linux package)

The [Releases](https://github.com/neovim/neovim/releases) page provides an [AppImage](https://appimage.org/) that runs on most Linux systems. No installation is needed, just download `nvim.appimage` and run it. (It might not work if your Linux distribution is more than 4 years old.)

```
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim.appimage
chmod u+x nvim.appimage
./nvim.appimage
```

If the `./nvim.appimage` command fails, try:

```
./nvim.appimage --appimage-extract
./squashfs-root/AppRun --version

# Optional: exposing nvim globally.
sudo mv squashfs-root /
sudo ln -s /squashfs-root/AppRun /usr/bin/nvim
nvim
```

### 

## Uninstall

To *uninstall* after `make install`, just delete the `CMAKE_INSTALL_PREFIX` artifacts:

```
sudo rm /usr/local/bin/nvim
sudo rm -r /usr/local/share/nvim/
```



## 配置文件

### 结构

配置文件
Neovim 配置文件不是 .vimrc

而是保存在 ~/.config/nvim/init.vim 也可以直接是 init.lua

但是 `init.vim` 只作为入口，真正的配置，是加载的其他的 `lua` 配置文件

### lua

.vim 中调用 lua

从 `init.vim` 里可以直接写 `lua`代码，这样

```text
lua print('单行 lua')
```

多行调用

```text
lua <<EOF
print('多行 lua')
print('多行 lua')
EOF
```







## Usage

```shell
```



## FAQ and troubleshooting

```shell

```



## See Also

https://neovim.io/

https://github.com/neovim/neovim/wiki

https://github.com/neovim/neovim

https://github.com/neovim/neovim/wiki/Installing-Neovim#install-from-package

https://zhuanlan.zhihu.com/p/434731678