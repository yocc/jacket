# Compile and install the application

https://golang.google.cn/doc/tutorial/compile-install





```shell
在最后一个主题中, 您将学习几个新的 go 命令. 虽然 go run 命令是在您进行频繁更改时编译和运行程序的有用快捷方式, 但它不会生成二进制可执行文件. 
本主题介绍了两个用于构建代码的附加命令: 
. go build 命令编译包及其依赖项, 但不安装结果. 
. go install 命令编译并安装软件包. 
注意: 本主题是从创建 Go 模块开始的多部分教程的一部分. 

1. 从 hello 目录中的命令行, 运行 go build 命令将代码编译为可执行文件. 
$ go build

2. 从 hello 目录中的命令行, 运行新的 hello 可执行文件以确认代码有效. 
请注意, 根据您在测试后是否更改了 greetings.go 代码, 您的结果可能会有所不同. 
在 Linux 或 Mac 上: 
$ ./hello
map[Darrin:Great to see you, Darrin! Gladys:Hail, Gladys! Well met! Samantha:Hail, Samantha! Well met!]
在 Windows 上: 
$ hello.exe
map[Darrin:Great to see you, Darrin! Gladys:Hail, Gladys! Well met! Samantha:Hail, Samantha! Well met!]
您已将应用程序编译为可执行文件, 以便可以运行它. 但是要当前运行它, 您的提示需要位于可执行文件的目录中, 或者指定可执行文件的路径. 
接下来, 您将安装可执行文件, 以便无需指定其路径即可运行它. 

3. 发现 Go 安装路径, go 命令将在其中安装当前包. 
您可以通过运行 go list 命令来发现安装路径, 如下例所示: 
$ go list -f '{{.Target}}'
例如, 命令的输出可能是 /home/gopher/bin/hello, 这意味着二进制文件安装到 /home/gopher/bin. 在下一步中您将需要此安装目录. 

4. 将 Go 安装目录添加到系统的 shell 路径. 
这样, 您就可以运行程序的可执行文件, 而无需指定可执行文件的位置. 
在 Linux 或 Mac 上, 运行以下命令: 
$ export PATH=$PATH:/path/to/your/install/directory
在 Windows 上, 运行以下命令: 
$ set PATH=%PATH%;C:\path\to\your\install\directory
或者, 如果你的 shell 路径中已经有一个像 $HOME/bin 这样的目录, 并且你想在那里安装你的 Go 程序, 你可以通过使用 go env 命令设置 GOBIN 变量来更改安装目标: 
$ go env -w GOBIN=/path/to/your/bin
要么
$ go env -w GOBIN=C:\path\to\your\bin

5. 更新 shell 路径后, 运行 go install 命令来编译和安装包. 
$ go install
只需键入其名称即可运行您的应用程序. 为了使这个有趣, 打开一个新的命令提示符并在其他目录中运行 hello 可执行文件名称. 
$ hello
map[Darrin:Hail, Darrin! Well met! Gladys:Great to see you, Gladys! Samantha:Hail, Samantha! Well met!]

本 Go 教程到此结束！
```



```shell
结论
在本教程中，您编写了打包成两个模块的函数: 一个具有发送问候的逻辑; 另一个作为第一个消费者.

注意: 本主题是从创建 Go 模块开始的多部分教程的一部分.
有关在代码中管理依赖项的更多信息, 请参阅管理依赖项. 有关开发供他人使用的模块的更多信息, 请参阅开发和发布模块.

有关 Go 语言的更多功能, 请查看 Go 之旅.
```

