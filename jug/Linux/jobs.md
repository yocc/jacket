# Bg, Fg, &, Jobs, Nohup And Disown

[toc]



## Overview

UNIX 命令行中的与号“&”用于从 shell 或前台启动命令或脚本到后台。(fg启动 -> bg)

当我们将一个进程发送到后台bg时，我们无法通过 stdin 与它进行交互，但 stdout 仍然会发送到屏幕，除非转移到其他文件。此外，终止我们的 shell 会话将终止仍在后台bg运行的作业。

(bg 不能 stdin, 但能 stdout, 或者 > 重定向到文件.  logout shell, bg也退出)

```sh
Control+Z: 停止(挂起)当前进程, 暂停运行.
jobs: 列出所有工作进程列表
jobs -l: 列出带PIDs的工作进程列表
jobs -r: 列出正在运行的工作进程列表
jobs -s: 列出已经停止的工作进程列表
bg 1: 重新将 job 1 放到后台
fg 1: 重新将 job 1 放到前台
Control+C: 退出进程
```

大多数情况下，当我们使用 bg 或在启动时使用 & 将作业发送到后台时，我们会将 stdout 转移到某个文件，以免终端被输出弄乱。如果我们想知道任何错误，我们可能仍然将 stderr 发送到终端。让我们看两个例子：

```sh
root:/tmp> ./sleeptalk.sh > /tmp/sleeptalk.log &
.
root:/tmp> ./sleeptalk.sh > /tmp/sleeptalk.log 2>&1
```

第一个示例将 stdout 转移到日志文件并在后台启动脚本，但是由于 stderr 仍然转到终端，因此如果发生任何错误，我们应该看到它们。

第二个示例将 stdout 和 stderr 转移到同一个日志文件，但脚本在前台启动。如果我们想在停止脚本 (Control+Z) 并将其发送到后台 (bg) 之前检查脚本是否平稳运行（没有错误）几秒钟，我们可能需要这样做。

这两个示例中的方法对于短期运行的作业来说是完全合理的。但对于长时间运行的作业，存在这样的危险：我们的 shell 会话可能会终止（网络中断、不活动超时等），并且其所有子项都将被标记为终止。在这种情况下，我们需要的是子进程......

1. 忽略收到的任何 SIGHUP 信号（SIGHUP = 控制终端关闭/死机）
2. 将 stdin 从键盘转移到 /dev/null
3. 将 stdout 从屏幕转移到 /dev/null 除非它已经被转移
4. 将 stderr 从屏幕转移到 stdout
  我们可以通过两种方式做到这一点：

```sh
root:~> nohup /tmp/sleeptalk.sh > /tmp/sleeptalk.log 2>&1 &
root:~> /tmp/sleeptalk.sh > /tmp/sleeptalk.log 2>&1
^Z
root:~> jobs -l
[1]+ 3034 Stopped /tmp/sleeptalk.sh > /tmp/sleeptalk.log 2>&1
root:~> bg 1
[1]+ 3034 Stopped /tmp/sleeptalk.sh > /tmp/sleeptalk.log 2>&1
root:~> disown -h -a
```

我们运行的第一个命令是在后台启动脚本（“&”），将 stdout 和 stderr 转移到日志文件，stdin 被阻止，并将 SIGHUP 信号设置为忽略（“nohup”）。

第二个命令启动脚本并转移 stdout 和 stderr，但脚本在前台运行，任何 SIGHUP 信号都会杀死它。所以我们停止它（Control+Z），在后台重新启动它（bg），并使用标志-a（所有作业）和-h（忽略SIGHUP）来否认它。我们也可以使用标志 -r（仅放弃正在运行的作业）或作业编号（例如，放弃 -h %1）。

## example

```sh
# cat /tmp/sleeptalk.sh
#!/bin/bash
.
while [ true ]
do
__echo “Talk talk at `date`”
__sleep 2
done
.
exit 0
```

```sh
root:/tmp> /tmp/sleeptalk.sh                             -> we launch the process …
Talk talk at Tue Oct 20 21:17:14 BST 2015
Talk talk at Tue Oct 20 21:17:16 BST 2015
Talk talk at Tue Oct 20 21:17:18 BST 2015
^Z
[1]+ Stopped ./sleeptalk.sh                              -> … and stop it with Control+Z
.
root:/tmp> /tmp/sleeptalk.sh                             -> we launch a second process …
Talk talk at Tue Oct 20 21:17:14 BST 2015
Talk talk at Tue Oct 20 21:17:16 BST 2015
Talk talk at Tue Oct 20 21:17:18 BST 2015
^Z
[2]+ Stopped ./sleeptalk.sh                              -> … and stop it again with Control+Z
.
root:/tmp> jobs                                          -> list the jobs in the background
[1]­- Stopped ./sleeptalk.sh
[2]+ Stopped ./sleeptalk.sh
.
root:/tmp> jobs -­l                                       -> list the jobs in the background with PIDs
[1]­- 31846 Stopped (tty input) ./sleeptalk.sh
[2]+ 31873 Stopped ./sleeptalk.sh
.
root:/tmp> jobs ­-r                                       -> list running jobs
root:/tmp> jobs -s                                       -> list stopped jobs
[1]-­ Stopped ./sleeptalk.sh
[2]+ Stopped ./sleeptalk.sh
.
root:/tmp> bg 1                                          -> restart job 1 in background
[1]­- ./sleeptalk.sh &
root:/tmp> Talk talk at Tue Oct 20 21:29:50 BST 2015
Talk talk at Tue Oct 20 21:29:52 BST 2015
Talk talk at Tue Oct 20 21:29:54 BST 2015
root:/tmp> fg 1                                          -> bring job 1 to foreground
Talk talk at Tue Oct 20 21:29:56 BST 2015
Talk talk at Tue Oct 20 21:29:58 BST 2015
^C     
```









## See Also

http://www.lostpenguin.net/index.php/bg-fg-jobs-nohup-and-disown/

http://www.lostpenguin.net/index.php/stty-tty-clear-reset/

http://www.lostpenguin.net/index.php/systemd/