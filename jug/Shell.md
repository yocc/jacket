# Shell

----

[toc]





## Overview

### 符号

$, !$, ~

& 丢到后台执行

```shell
$ sleep 200 &
[1] 2965
$ jobs
[1]+  运行中               sleep 200 &
```

; 多条命令写一行，用分号连接

```shell
$ mkdir awen ; cd awen ; pwd
/tmp/awen
```

|| 或者, 前面的代码正确, 就不执行后面的程序了.

```shell
$ ls
a.txt  awen  c.txt
$ ls d.txt || echo "cant't find d"
ls: 无法访问'd.txt': No such file or directory
cant't find d
```

&& 和 || 相反, 它是如果前面的命令成功才执行后面的命令.

```shell
$ ls a.txt && echo "a.txt exist"
a.txt
a.txt exist
```

crontab, 小于粒度1秒

```sh
# 格式:
* * * * * sleep 5  && /解析器/php /data1/www/htdocs/脚本.php > /data1/www/applogs/日日志.log 2>&1

## 女足世界杯, nami match 刷到 livcast match 直播大厅
* * * * *             /usr/local/sinasrv5/bin/php /data1/www/htdocs/ore.sports.sina.com.cn/system/match/nmaz.livecast.update.php > /data1/www/applogs/ore.sports.sina.com.cn/nmaz.livecast.update.cron.log 2>&1
* * * * * sleep 5  && /usr/local/sinasrv5/bin/php /data1/www/htdocs/ore.sports.sina.com.cn/system/match/nmaz.livecast.update.php > /data1/www/applogs/ore.sports.sina.com.cn/nmaz.livecast.update.cron.log 2>&1
* * * * * sleep 10 && /usr/local/sinasrv5/bin/php /data1/www/htdocs/ore.sports.sina.com.cn/system/match/nmaz.livecast.update.php > /data1/www/applogs/ore.sports.sina.com.cn/nmaz.livecast.update.cron.log 2>&1
* * * * * sleep 15 && /usr/local/sinasrv5/bin/php /data1/www/htdocs/ore.sports.sina.com.cn/system/match/nmaz.livecast.update.php > /data1/www/applogs/ore.sports.sina.com.cn/nmaz.livecast.update.cron.log 2>&1
* * * * * sleep 20 && /usr/local/sinasrv5/bin/php /data1/www/htdocs/ore.sports.sina.com.cn/system/match/nmaz.livecast.update.php > /data1/www/applogs/ore.sports.sina.com.cn/nmaz.livecast.update.cron.log 2>&1
* * * * * sleep 25 && /usr/local/sinasrv5/bin/php /data1/www/htdocs/ore.sports.sina.com.cn/system/match/nmaz.livecast.update.php > /data1/www/applogs/ore.sports.sina.com.cn/nmaz.livecast.update.cron.log 2>&1
* * * * * sleep 30 && /usr/local/sinasrv5/bin/php /data1/www/htdocs/ore.sports.sina.com.cn/system/match/nmaz.livecast.update.php > /data1/www/applogs/ore.sports.sina.com.cn/nmaz.livecast.update.cron.log 2>&1
* * * * * sleep 35 && /usr/local/sinasrv5/bin/php /data1/www/htdocs/ore.sports.sina.com.cn/system/match/nmaz.livecast.update.php > /data1/www/applogs/ore.sports.sina.com.cn/nmaz.livecast.update.cron.log 2>&1
* * * * * sleep 40 && /usr/local/sinasrv5/bin/php /data1/www/htdocs/ore.sports.sina.com.cn/system/match/nmaz.livecast.update.php > /data1/www/applogs/ore.sports.sina.com.cn/nmaz.livecast.update.cron.log 2>&1
* * * * * sleep 45 && /usr/local/sinasrv5/bin/php /data1/www/htdocs/ore.sports.sina.com.cn/system/match/nmaz.livecast.update.php > /data1/www/applogs/ore.sports.sina.com.cn/nmaz.livecast.update.cron.log 2>&1
* * * * * sleep 50 && /usr/local/sinasrv5/bin/php /data1/www/htdocs/ore.sports.sina.com.cn/system/match/nmaz.livecast.update.php > /data1/www/applogs/ore.sports.sina.com.cn/nmaz.livecast.update.cron.log 2>&1
* * * * * sleep 55 && /usr/local/sinasrv5/bin/php /data1/www/htdocs/ore.sports.sina.com.cn/system/match/nmaz.livecast.update.php > /data1/www/applogs/ore.sports.sina.com.cn/nmaz.livecast.update.cron.log 2>&1
```







## FAQ





## See Also





