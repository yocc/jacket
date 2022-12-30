# Redis

----

[toc]



## Overview



服务安装

```shell
[chenchen@grpc01 tmp]$ mkdir redis
[chenchen@grpc01 tmp]$ cd redis
[chenchen@grpc01 redis]$ wget https://download.redis.io/redis-stable.tar.gz
--2022-09-27 01:30:16--  https://download.redis.io/redis-stable.tar.gz
Connecting to 127.0.0.1:7890... connected.
Proxy request sent, awaiting response... 200 OK
Length: 3047785 (2.9M) [application/octet-stream]
Saving to: ‘redis-stable.tar.gz’

100%[===================================================================================================================================================================================================================>] 3,047,785   45.6KB/s   in 24s    

2022-09-27 01:30:42 (122 KB/s) - ‘redis-stable.tar.gz’ saved [3047785/3047785]

[chenchen@grpc01 redis]$ l
total 3.0M
134282719 -rw-rw-r--.  1 chenchen chenchen 3.0M Sep 22 03:45 redis-stable.tar.gz
   595690 drwxrwxr-x. 34 chenchen chenchen 4.0K Sep 27 01:29 ..
134282718 drwxrwxr-x.  2 chenchen chenchen   33 Sep 27 01:30 .
[chenchen@grpc01 redis]$ tar -xzvf redis-stable.tar.gz 
[chenchen@grpc01 redis]$ l
total 3.0M
138502889 drwxrwxr-x.  8 chenchen chenchen 4.0K Sep 22 03:42 redis-stable
134282719 -rw-rw-r--.  1 chenchen chenchen 3.0M Sep 22 03:45 redis-stable.tar.gz
   595690 drwxrwxr-x. 34 chenchen chenchen 4.0K Sep 27 01:29 ..
134282718 drwxrwxr-x.  3 chenchen chenchen   53 Sep 27 01:31 .
[chenchen@grpc01 redis]$ cd redis-stable
[chenchen@grpc01 redis-stable]$ l
total 276K
167900232 drwxrwxr-x.  8 chenchen chenchen 4.0K Sep 22 03:42 utils
138502895 -rw-rw-r--.  1 chenchen chenchen 3.0K Sep 22 03:42 TLS.md
381720774 drwxrwxr-x. 11 chenchen chenchen  199 Sep 22 03:42 tests
142669353 drwxrwxr-x.  4 chenchen chenchen 8.0K Sep 22 03:42 src
138502907 -rw-rw-r--.  1 chenchen chenchen  14K Sep 22 03:42 sentinel.conf
138502904 -rw-rw-r--.  1 chenchen chenchen 1.7K Sep 22 03:42 SECURITY.md
138502899 -rwxrwxr-x.  1 chenchen chenchen  285 Sep 22 03:42 runtest-sentinel
138502908 -rwxrwxr-x.  1 chenchen chenchen 1.6K Sep 22 03:42 runtest-moduleapi
138502900 -rwxrwxr-x.  1 chenchen chenchen  283 Sep 22 03:42 runtest-cluster
138502902 -rwxrwxr-x.  1 chenchen chenchen  279 Sep 22 03:42 runtest
138502897 -rw-rw-r--.  1 chenchen chenchen 105K Sep 22 03:42 redis.conf
138502891 -rw-rw-r--.  1 chenchen chenchen  22K Sep 22 03:42 README.md
138502903 -rw-rw-r--.  1 chenchen chenchen 6.8K Sep 22 03:42 MANIFESTO
138502898 -rw-rw-r--.  1 chenchen chenchen  151 Sep 22 03:42 Makefile
138502896 -rw-rw-r--.  1 chenchen chenchen   11 Sep 22 03:42 INSTALL
138502893 -rw-rw-r--.  1 chenchen chenchen  535 Sep 22 03:42 .gitignore
155268919 drwxrwxr-x.  4 chenchen chenchen   67 Sep 22 03:42 .github
138502905 -rw-rw-r--.  1 chenchen chenchen  405 Sep 22 03:42 .gitattributes
201391230 drwxrwxr-x.  7 chenchen chenchen  119 Sep 22 03:42 deps
138502890 -rw-rw-r--.  1 chenchen chenchen 1.5K Sep 22 03:42 COPYING
138502901 -rw-rw-r--.  1 chenchen chenchen 2.6K Sep 22 03:42 CONTRIBUTING.md
469824554 drwxrwxr-x.  2 chenchen chenchen   70 Sep 22 03:42 .codespell
138502906 -rw-rw-r--.  1 chenchen chenchen 5.0K Sep 22 03:42 CODE_OF_CONDUCT.md
138502892 -rw-rw-r--.  1 chenchen chenchen   51 Sep 22 03:42 BUGS
138502894 -rw-rw-r--.  1 chenchen chenchen  37K Sep 22 03:42 00-RELEASENOTES
138502889 drwxrwxr-x.  8 chenchen chenchen 4.0K Sep 22 03:42 .
134282718 drwxrwxr-x.  3 chenchen chenchen   53 Sep 27 01:31 ..
[chenchen@grpc01 redis-stable]$ make
cd src && make all
make[1]: Entering directory `/home/chenchen/tmp/redis/redis-stable/src'
    CC Makefile.dep
make[1]: Leaving directory `/home/chenchen/tmp/redis/redis-stable/src'
make[1]: Entering directory `/home/chenchen/tmp/redis/redis-stable/src'
...
    CC function_lua.o
    CC commands.o
    LINK redis-server
    INSTALL redis-sentinel
    CC redis-cli.o
    CC redisassert.o
    CC cli_common.o
    LINK redis-cli
    CC redis-benchmark.o
    LINK redis-benchmark
    INSTALL redis-check-rdb
    INSTALL redis-check-aof

Hint: It's a good idea to run 'make test' ;)

make[1]: Leaving directory `/home/chenchen/tmp/redis/redis-stable/src'
[chenchen@grpc01 redis-stable]$ l
total 280K
167900232 drwxrwxr-x.  8 chenchen chenchen 4.0K Sep 22 03:42 utils
138502895 -rw-rw-r--.  1 chenchen chenchen 3.0K Sep 22 03:42 TLS.md
381720774 drwxrwxr-x. 11 chenchen chenchen  199 Sep 22 03:42 tests
138502907 -rw-rw-r--.  1 chenchen chenchen  14K Sep 22 03:42 sentinel.conf
138502904 -rw-rw-r--.  1 chenchen chenchen 1.7K Sep 22 03:42 SECURITY.md
138502899 -rwxrwxr-x.  1 chenchen chenchen  285 Sep 22 03:42 runtest-sentinel
138502908 -rwxrwxr-x.  1 chenchen chenchen 1.6K Sep 22 03:42 runtest-moduleapi
138502900 -rwxrwxr-x.  1 chenchen chenchen  283 Sep 22 03:42 runtest-cluster
138502902 -rwxrwxr-x.  1 chenchen chenchen  279 Sep 22 03:42 runtest
138502897 -rw-rw-r--.  1 chenchen chenchen 105K Sep 22 03:42 redis.conf
138502891 -rw-rw-r--.  1 chenchen chenchen  22K Sep 22 03:42 README.md
138502903 -rw-rw-r--.  1 chenchen chenchen 6.8K Sep 22 03:42 MANIFESTO
138502898 -rw-rw-r--.  1 chenchen chenchen  151 Sep 22 03:42 Makefile
138502896 -rw-rw-r--.  1 chenchen chenchen   11 Sep 22 03:42 INSTALL
138502893 -rw-rw-r--.  1 chenchen chenchen  535 Sep 22 03:42 .gitignore
155268919 drwxrwxr-x.  4 chenchen chenchen   67 Sep 22 03:42 .github
138502905 -rw-rw-r--.  1 chenchen chenchen  405 Sep 22 03:42 .gitattributes
138502890 -rw-rw-r--.  1 chenchen chenchen 1.5K Sep 22 03:42 COPYING
138502901 -rw-rw-r--.  1 chenchen chenchen 2.6K Sep 22 03:42 CONTRIBUTING.md
469824554 drwxrwxr-x.  2 chenchen chenchen   70 Sep 22 03:42 .codespell
138502906 -rw-rw-r--.  1 chenchen chenchen 5.0K Sep 22 03:42 CODE_OF_CONDUCT.md
138502892 -rw-rw-r--.  1 chenchen chenchen   51 Sep 22 03:42 BUGS
138502894 -rw-rw-r--.  1 chenchen chenchen  37K Sep 22 03:42 00-RELEASENOTES
138502889 drwxrwxr-x.  8 chenchen chenchen 4.0K Sep 22 03:42 .
134282718 drwxrwxr-x.  3 chenchen chenchen   53 Sep 27 01:31 ..
201391230 drwxrwxr-x.  7 chenchen chenchen  187 Sep 27 01:32 deps
142669353 drwxrwxr-x.  4 chenchen chenchen  12K Sep 27 01:33 src
[chenchen@grpc01 redis-stable]$ cd src
[chenchen@grpc01 src]$ l
[chenchen@grpc01 src]$ l /usr/local/bin
total 8.8M
  645208 -r-xr-xr-x.  1 root root  16K Jun 22 03:05 corelist
     122 drwxr-xr-x.  2 root root   35 Jul  6 04:23 .
12583335 drwxr-xr-x. 24 root root 4.0K Jul  6 21:25 ..
   11160 -r-xr-xr-x.  1 root root 8.8M Sep 26 10:52 clash
[chenchen@grpc01 src]$ cd ..
[chenchen@grpc01 redis-stable]$ l
total 280K
167900232 drwxrwxr-x.  8 chenchen chenchen 4.0K Sep 22 03:42 utils
138502895 -rw-rw-r--.  1 chenchen chenchen 3.0K Sep 22 03:42 TLS.md
381720774 drwxrwxr-x. 11 chenchen chenchen  199 Sep 22 03:42 tests
138502907 -rw-rw-r--.  1 chenchen chenchen  14K Sep 22 03:42 sentinel.conf
138502904 -rw-rw-r--.  1 chenchen chenchen 1.7K Sep 22 03:42 SECURITY.md
138502899 -rwxrwxr-x.  1 chenchen chenchen  285 Sep 22 03:42 runtest-sentinel
138502908 -rwxrwxr-x.  1 chenchen chenchen 1.6K Sep 22 03:42 runtest-moduleapi
138502900 -rwxrwxr-x.  1 chenchen chenchen  283 Sep 22 03:42 runtest-cluster
138502902 -rwxrwxr-x.  1 chenchen chenchen  279 Sep 22 03:42 runtest
138502897 -rw-rw-r--.  1 chenchen chenchen 105K Sep 22 03:42 redis.conf
138502891 -rw-rw-r--.  1 chenchen chenchen  22K Sep 22 03:42 README.md
138502903 -rw-rw-r--.  1 chenchen chenchen 6.8K Sep 22 03:42 MANIFESTO
138502898 -rw-rw-r--.  1 chenchen chenchen  151 Sep 22 03:42 Makefile
138502896 -rw-rw-r--.  1 chenchen chenchen   11 Sep 22 03:42 INSTALL
138502893 -rw-rw-r--.  1 chenchen chenchen  535 Sep 22 03:42 .gitignore
155268919 drwxrwxr-x.  4 chenchen chenchen   67 Sep 22 03:42 .github
138502905 -rw-rw-r--.  1 chenchen chenchen  405 Sep 22 03:42 .gitattributes
138502890 -rw-rw-r--.  1 chenchen chenchen 1.5K Sep 22 03:42 COPYING
138502901 -rw-rw-r--.  1 chenchen chenchen 2.6K Sep 22 03:42 CONTRIBUTING.md
469824554 drwxrwxr-x.  2 chenchen chenchen   70 Sep 22 03:42 .codespell
138502906 -rw-rw-r--.  1 chenchen chenchen 5.0K Sep 22 03:42 CODE_OF_CONDUCT.md
138502892 -rw-rw-r--.  1 chenchen chenchen   51 Sep 22 03:42 BUGS
138502894 -rw-rw-r--.  1 chenchen chenchen  37K Sep 22 03:42 00-RELEASENOTES
138502889 drwxrwxr-x.  8 chenchen chenchen 4.0K Sep 22 03:42 .
134282718 drwxrwxr-x.  3 chenchen chenchen   53 Sep 27 01:31 ..
201391230 drwxrwxr-x.  7 chenchen chenchen  187 Sep 27 01:32 deps
142669353 drwxrwxr-x.  4 chenchen chenchen  12K Sep 27 01:33 src
[chenchen@grpc01 redis-stable]$ make test
cd src && make test
make[1]: Entering directory `/home/chenchen/tmp/redis/redis-stable/src'
    CC Makefile.dep
make[1]: Leaving directory `/home/chenchen/tmp/redis/redis-stable/src'
make[1]: Entering directory `/home/chenchen/tmp/redis/redis-stable/src'
Cleanup: may take some time... OK
Starting test server at port 21079
[ready]: 20289
Testing unit/printver
[ready]: 20292
Testing unit/dump
[ready]: 20295
Testing unit/auth
[ready]: 20298
Testing unit/protocol
...
  109 seconds - unit/hyperloglog
  198 seconds - integration/corrupt-dump
  141 seconds - unit/obuf-limits
  273 seconds - integration/replication-psync
  148 seconds - unit/geo
  182 seconds - unit/maxmemory
  379 seconds - integration/replication
  0 seconds - list-large-memory
  0 seconds - set-large-memory
  0 seconds - bitops-large-memory
  97 seconds - defrag
  1 seconds - violations

\o/ All tests passed without errors!

Cleanup: may take some time... OK
make[1]: Leaving directory `/home/chenchen/tmp/redis/redis-stable/src'
[chenchen@grpc01 redis-stable]$ 
[chenchen@grpc01 redis-stable]$ make install
cd src && make install
make[1]: Entering directory `/home/chenchen/tmp/redis/redis-stable/src'

Hint: It's a good idea to run 'make test' ;)

    INSTALL redis-server
install: cannot create regular file ‘/usr/local/bin/redis-server’: Permission denied
make[1]: *** [install] Error 1
make[1]: Leaving directory `/home/chenchen/tmp/redis/redis-stable/src'
make: *** [install] Error 2
[chenchen@grpc01 redis-stable]$ 
[chenchen@grpc01 redis-stable]$ sudo make install
cd src && make install
make[1]: Entering directory `/home/chenchen/tmp/redis/redis-stable/src'

Hint: It's a good idea to run 'make test' ;)

    INSTALL redis-server
    INSTALL redis-benchmark
    INSTALL redis-cli
make[1]: Leaving directory `/home/chenchen/tmp/redis/redis-stable/src'
[chenchen@grpc01 redis-stable]$ 
[chenchen@grpc01 redis-stable]$ l /usr/local/bin/
total 30M
  645208 -r-xr-xr-x.  1 root root  16K Jun 22 03:05 corelist
12583335 drwxr-xr-x. 24 root root 4.0K Jul  6 21:25 ..
   11160 -r-xr-xr-x.  1 root root 8.8M Sep 26 10:52 clash
 2453766 -rwxr-xr-x.  1 root root  11M Sep 27 02:29 redis-server
  807855 -rwxr-xr-x.  1 root root 5.0M Sep 27 02:29 redis-benchmark
  807856 -rwxr-xr-x.  1 root root 5.2M Sep 27 02:29 redis-cli
  807857 lrwxrwxrwx.  1 root root   12 Sep 27 02:29 redis-check-rdb -> redis-server
  807859 lrwxrwxrwx.  1 root root   12 Sep 27 02:29 redis-sentinel -> redis-server
  807858 lrwxrwxrwx.  1 root root   12 Sep 27 02:29 redis-check-aof -> redis-server
     122 drwxr-xr-x.  2 root root  163 Sep 27 02:29 .
[chenchen@grpc01 redis-stable]$ 
[chenchen@grpc01 redis-stable]$ pwd
/home/chenchen/tmp/redis/redis-stable

redis-benchmark：redis性能测试工具
redis-check-aof：检查aof日志的工具
redis-check-dump：检查rdb日志的工具
redis-cli：连接用的客户端
redis-server：redis服务进程


[chenchen@grpc01 src]$ l /usr/local/bin
total 8.8M
  645208 -r-xr-xr-x.  1 root root  16K Jun 22 03:05 corelist
     122 drwxr-xr-x.  2 root root   35 Jul  6 04:23 .
12583335 drwxr-xr-x. 24 root root 4.0K Jul  6 21:25 ..
   11160 -r-xr-xr-x.  1 root root 8.8M Sep 26 10:52 clash
[chenchen@grpc01 src]$ cd ..
[chenchen@grpc01 redis-stable]$ l
```







测试

```shell
[chenchen@grpc01 redis-stable]$ redis-server --help
Usage: ./redis-server [/path/to/redis.conf] [options] [-]
       ./redis-server - (read config from stdin)
       ./redis-server -v or --version
       ./redis-server -h or --help
       ./redis-server --test-memory <megabytes>
       ./redis-server --check-system

Examples:
       ./redis-server (run the server with default conf)
       echo 'maxmemory 128mb' | ./redis-server -
       ./redis-server /etc/redis/6379.conf
       ./redis-server --port 7777
       ./redis-server --port 7777 --replicaof 127.0.0.1 8888
       ./redis-server /etc/myredis.conf --loglevel verbose -
       ./redis-server /etc/myredis.conf --loglevel verbose

Sentinel mode: # 哨兵模式
       ./redis-server /etc/sentinel.conf --sentinel
[chenchen@grpc01 redis-stable]$ 

# 默认配置前台运行服务器
# If successful, you'll see the startup logs for Redis, and Redis will be running in the foreground.
# To stop Redis, enter Ctrl-C.
# Redis is able to start without a configuration file using a built-in default configuration, however this setup is only recommended for testing and development purposes.
[chenchen@grpc01 redis-stable]$ redis-server
11132:C 27 Sep 2022 02:36:30.092 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
11132:C 27 Sep 2022 02:36:30.092 # Redis version=7.0.5, bits=64, commit=00000000, modified=0, pid=11132, just started
11132:C 27 Sep 2022 02:36:30.092 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
11132:M 27 Sep 2022 02:36:30.093 * monotonic clock: POSIX clock_gettime
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 7.0.5 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                  
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 11132
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           https://redis.io       
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

11132:M 27 Sep 2022 02:36:30.094 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
11132:M 27 Sep 2022 02:36:30.094 # Server initialized
11132:M 27 Sep 2022 02:36:30.095 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
11132:M 27 Sep 2022 02:36:30.095 * Ready to accept connections


# To stop Redis, enter Ctrl-C.
^C11132:signal-handler (1664217660) Received SIGINT scheduling shutdown...
11132:M 27 Sep 2022 02:41:00.651 # User requested shutdown...
11132:M 27 Sep 2022 02:41:00.651 * Saving the final RDB snapshot before exiting.
11132:M 27 Sep 2022 02:41:00.654 * DB saved on disk
11132:M 27 Sep 2022 02:41:00.654 # Redis is now ready to exit, bye bye...
[chenchen@grpc01 redis-stable]$ 

# 客户端
[chenchen@grpc01 tmp]$ ./python37/bin/iredis
iredis  1.11.1 (Python 3.7.12)
redis-server  7.0.5 
Home:   https://iredis.io
Issues: https://iredis.io/issues
127.0.0.1:6379> exit
Goodbye!
[chenchen@grpc01 tmp]$ 


```



./iredis -h rm22397.hebe.grid.sina.com.cn -p 22397

[chenchen@grpc01 tmp]$ ./python37/bin/iredis -h rm33907.eos.grid.sina.com.cn -p 33907





服务配置

https://redis.io/docs/manual/config/
```shell
Redis configuration
Overview of redis.conf, the Redis configuration file
The proper way to configure Redis is by providing a Redis configuration file, usually called redis.conf.
The redis.conf file contains a number of directives that have a very simple format:

keyword argument1 argument2 ... argumentN
This is an example of a configuration directive:

replicaof 127.0.0.1 6380
It is possible to provide strings containing spaces as arguments using (double or single) quotes, as in the following example:

requirepass "hello world"
Single-quoted string can contain characters escaped by backslashes, and double-quoted strings can additionally include any ASCII symbols encoded using backslashed hexadecimal notation "\xff".

The list of configuration directives, and their meaning and intended usage is available in the self documented example redis.conf shipped into the Redis distribution.
The self documented redis.conf for Redis 7.0.
https://raw.githubusercontent.com/redis/redis/7.0/redis.conf
The self documented redis.conf for Redis 6.2.
...

# 推荐使用配置文件来配置服务, 并在启动时指定配置文件
[chenchen@grpc01 bin]$ sudo curl https://raw.githubusercontent.com/redis/redis/7.0/redis.conf -O
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  104k  100  104k    0     0  45337      0  0:00:02  0:00:02 --:--:-- 45357
[chenchen@grpc01 bin]$ l
total 30M
  645208 -r-xr-xr-x.  1 root root  16K Jun 22 03:05 corelist
12583335 drwxr-xr-x. 24 root root 4.0K Jul  6 21:25 ..
   11160 -r-xr-xr-x.  1 root root 8.8M Sep 26 10:52 clash
 2453766 -rwxr-xr-x.  1 root root  11M Sep 27 02:29 redis-server
  807855 -rwxr-xr-x.  1 root root 5.0M Sep 27 02:29 redis-benchmark
  807856 -rwxr-xr-x.  1 root root 5.2M Sep 27 02:29 redis-cli
  807857 lrwxrwxrwx.  1 root root   12 Sep 27 02:29 redis-check-rdb -> redis-server
  807859 lrwxrwxrwx.  1 root root   12 Sep 27 02:29 redis-sentinel -> redis-server
  807858 lrwxrwxrwx.  1 root root   12 Sep 27 02:29 redis-check-aof -> redis-server
     122 drwxr-xr-x.  2 root root  181 Sep 27 02:56 .
  807860 -rw-r--r--.  1 root root 105K Sep 27 02:56 redis.conf
[chenchen@grpc01 bin]$ 
# ./redis-server /etc/redis/6379.conf


# 其他: (不推荐, 非主流服务配置启动方法)
通过命令行参数启动服务等
Passing arguments via the command line
You can also pass Redis configuration parameters using the command line directly. This is very useful for testing purposes. The following is an example that starts a new Redis instance using port 6380 as a replica of the instance running at 127.0.0.1 port 6379.

./redis-server --port 6380 --replicaof 127.0.0.1 6379
The format of the arguments passed via the command line is exactly the same as the one used in the redis.conf file, with the exception that the keyword is prefixed with --.

Note that internally this generates an in-memory temporary config file (possibly concatenating the config file passed by the user, if any) where arguments are translated into the format of redis.conf.

# 当服务器正在运行时, 此时修改服务器配置
Changing Redis configuration while the server is running
It is possible to reconfigure Redis on the fly without stopping and restarting the service, or querying the current configuration programmatically using the special commands CONFIG SET and CONFIG GET.

Not all of the configuration directives are supported in this way, but most are supported as expected. Please refer to the CONFIG SET and CONFIG GET pages for more information.

Note that modifying the configuration on the fly has no effects on the redis.conf file so at the next restart of Redis the old configuration will be used instead.

Make sure to also modify the redis.conf file accordingly to the configuration you set using CONFIG SET. You can do it manually, or you can use CONFIG REWRITE, which will automatically scan your redis.conf file and update the fields which don't match the current configuration value. Fields non existing but set to the default value are not added. Comments inside your configuration file are retained.


# 将 Redis 配置成缓存服务器, 纯当缓存使用.
Configuring Redis as a cache
If you plan to use Redis as a cache where every key will have an expire set, you may consider using the following configuration instead (assuming a max memory limit of 2 megabytes as an example):

maxmemory 2mb
maxmemory-policy allkeys-lru
In this configuration there is no need for the application to set a time to live for keys using the EXPIRE command (or equivalent) since all the keys will be evicted using an approximated LRU algorithm as long as we hit the 2 megabyte memory limit.

Basically, in this configuration Redis acts in a similar way to memcached. We have more extensive documentation about using Redis as an LRU cache here.

# 近似 LRU 算法
Approximated LRU algorithm
Redis LRU algorithm is not an exact implementation. This means that Redis is not able to pick the best candidate for eviction, that is, the access that was accessed the furthest in the past. Instead it will try to run an approximation of the LRU algorithm, by sampling a small number of keys, and evicting the one that is the best (with the oldest access time) among the sampled keys.

However, since Redis 3.0 the algorithm was improved to also take a pool of good candidates for eviction. This improved the performance of the algorithm, making it able to approximate more closely the behavior of a real LRU algorithm.

What is important about the Redis LRU algorithm is that you are able to tune the precision of the algorithm by changing the number of samples to check for every eviction. This parameter is controlled by the following configuration directive:

maxmemory-samples 5
The reason Redis does not use a true LRU implementation is because it costs more memory. However, the approximation is virtually equivalent for an application using Redis. This figure compares the LRU approximation used by Redis with true LRU.

近似 LRU 算法 
雷迪斯 LRU 算法不是一个精确的实现。这意味着 Redis 无法选择驱逐的最佳候选项，即过去访问过最远的访问。相反，它将尝试运行LRU算法的近似值，方法是对少量密钥进行采样，并逐出采样密钥中最好的密钥（具有最晚的访问时间）。
 
但是，自 Redis 3.0 以来，该算法得到了改进，也采用了一组很好的候选者进行逐出。这提高了算法的性能，使其能够更接近真实LRU算法的行为。
 
Redis LRU 算法的重要之处在于，您可以通过更改要检查每个逐出的样本数来调整算法的精度。此参数由以下配置指令控制： 
 
maxmemory-samples 5
Redis 不使用真正的 LRU 实现的原因是因为它会消耗更多的内存。但是，对于使用 Redis 的应用程序，近似值实际上是等效的。该图将雷迪斯使用的 LRU 近似值与真 LRU 进行了比较。
```



新的 LFU 模型

http://antirez.com/news/109

```shell
# https://redis.io/docs/manual/eviction/#approximated-lru-algorithm

The new LFU mode
Starting with Redis 4.0, the Least Frequently Used eviction mode is available. This mode may work better (provide a better hits/misses ratio) in certain cases. In LFU mode, Redis will try to track the frequency of access of items, so the ones used rarely are evicted. This means the keys used often have a higher chance of remaining in memory.
```



在生产环境中配置和管理 Redis 的建议

https://redis.io/docs/manual/admin/



后台运行服务

默认情况，Redis不是在后台运行，我们需要把redis放在后台运行
　　vim /usr/local/redis/etc/redis.conf
　　将daemonize的值改为yes



停止redis实例
　　/usr/local/redis/bin/redis-cli shutdown
　　或者
　　pkill redis-server

10、让redis开机自启
　　vim /etc/rc.local
　　加入
　　/usr/local/redis/bin/redis-server /usr/local/redis/etc/redis-conf






## 关闭Redis

强制结束Redis程序，可能会导致redis持久化数据丢失！企业中禁止使用！

查看redis进程端口占用

```bash
ps -ef | grep -i redis
1
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/79d43b9dcb514172829cb56ed6736682.png)
强制杀死进程：kill -9 5718

以上是错误的关闭服务器的方法. 禁止使用.

以下是正确停止服务器的方法.

**正确停止Redis的方式应该是向Redis发送shutdown命令**
方式一：

```bash
cd /usr/sfotware/redis/bin

./redis-cli shutdown
123
```

方式二：
![在这里插入图片描述](https://img-blog.csdnimg.cn/1a196ea5910947cebe9bf720d713e08f.png)



使用 Redis

https://blog.csdn.net/weixin_45193791/article/details/124691797

3、redis的简单使用
3.1、ping
ping [message]：测试客户端与Redis服务的连接是否正常，如果连接正常会收到回复PONG（如果指定message，则原样回复）。

![在这里插入图片描述](https://img-blog.csdnimg.cn/cbfe3c581796414b8c85b23032c20c8d.png)

3.2、set/get：存储值。（get命令只能用于获取String value）

set value为中文，get取值时乱码。

![在这里插入图片描述](https://img-blog.csdnimg.cn/3e3e789be71b446bb84dbe2384e5b706.png)

解决方法：连接redis服务器时使用命令：./redis-cli --raw

![在这里插入图片描述](https://img-blog.csdnimg.cn/7d8716e8784f4959ba7e816a8d452e2d.png)

3.3、echo
echo message：在命令行打印一些内容


3.4、info
info：获取服务器的信息和相关统计。


3.5、操作key
keys pattern：获取所有与pattern匹配的key。pattern中 * 表示任意多个字符，?表示任意一个字符

del key1 ke2 key3：删除key对应的数据

exists key：判断指定的key是否存在，0代表不存在，大于0表示存在

rename oldkey newkey：为当前的key重命名

expire key ：设置超时时间，单位：秒(超时后，会删除设置超时的key的数据）

ttl key：获取该key所剩的超时时间，如果没有设置，返回-1，返回-2表示key本身不存在。

type key：获取指定的key对应数据的类型：string、hash、list、set或zset


3.5、操作value
incr key：将指定的key对应的value递增1。
decr key：将指定的key的value递减1。

incrby key increment：将指定的key的value增加increment。
decrby key decrment：将指定的key的value减少decrement。

如果希望操作小数，可以使用命令：incrbyfloat key increment，没有decrbyfloat，可以使用正数表示增加，负数表示减少。


3.6、字符串拼接
append key value：如果该key存在，则在原有的value后追加该值；如果该key不存在，则重新创建一个key/value，返回值为追加后的长度！


4、redis的五种数据类型
value支持五种基本数据类型：

字符串（String）
哈希（hash）
列表（list）
集合（set）
有序集合（sorted set）
5、hash常用命令
5.1、hash赋值
hset key field value：为指定的key设定field-value对（键值对）。


5.2、hash取值
hget key field：获取key中的单个field的值（单个）
hmget key fileds：获取key中的多个filed的值（多个）
hgetall key：获取key中的所有filed-value（所有）


5.3、hash删除
hdel key field [field … ] ：可以删除一个或多个字段，返回值是被删除的字段个数。

del key ：删除整个hash


5.4、其它命令
hexists key field：判断指定的key中的filed是否存在（只能单个）

hlen key：获取key所包含的field的数量
hkeys key ：获得key所有field
hvals key：获得key所有的value


6、list常用命令
6.1、两端插入（列表插入值）
lpush key value…：在list的头部插入所有给定的值，如果该key不存在，则自动创建新list再操作，返回所有元素的个数。（前插法）

rpush key value…：在list的尾部插入所有给定的值，如果该key不存在，则自动创建新list再操作，返回所有元素的个数。（后插法）

lpushx key value [value…]：将给定的值插入到已存在的列表头部，如果列表不存在，则不插入。
rpushx key value [value…]：将给定的值插入到已存在的列表尾部，如果列表不存在，则不插入。


6.2、两端弹出（删除）
lpop key [count]：弹出并返回list中第一个元素，如果该key不存在，返回(nil)。count表示弹出几个，默认为1（左→右）

rpop key [count]：弹出并返回list中最后一个元素，如果该key不存在，返回(nil)。count表示弹出几个，默认为1（右→左）


6.3、获取长度
llen key：返回指定的key关联的链表中的元素的数量，如果list不存在，返回0。


6.4、删除多个相同的value
lrem key count value：删除count个值为value的元素，count 的值可以是以下几种：
count > 0 : 从表头开始向表尾搜索，移除与 value 相等的元素，数量为 count。
count < 0 : 从表尾开始向表头搜索，移除与 value 相等的元素，数量为 count 的绝对值。
count = 0 : 移除表中所有与 value 相等的值。


6.5、根据index修改值
lset key index value：设置链表中索引为index的元素值，0表示第一个，-1表示最后一个，索引不存在则抛异常。


6.6、在指定value前后插入
linsert key before|after pivot value：在pivot元素前或者后插入value。


6.7、操作两个list
rpoplpush resource destination：移除resource列表的最后一个元素，并将该元素添加到destination列表的头部。


7、set（Set集合中不允许出现重复的元素）
7.1、存值和取值
sadd key member… ：向Set中添加值，如果值已存在则不添加，返回添加成功的元素个数。
smembers key：获得集合中的所有元素。

srem key value… ：删除Set中指定的值，返回移除元素的个数。


7.2、判断某个值是否存在
sismember key member：判断Set中是否存在指定的值，1表示存在，0表示不存在或者该key本身就不存在。
如果希望准确判断到底是key本身不存在，还是key对应的set中的值不存在，可以结合exists key先判断key对应的set是否存在（返回0表示key对应的set不存在），从而得出准确结论。


7.3 、差集、并集、交集
7.3.1、差集
sdiff key1 key2…：只在key1中存在的元素，称为“差集”。


7.3.2、交集
sinter key1 key2…：在key1和key2中都存在的元素，称为“交集”。


7.3.3、并集
sunion key1 key2…：在key1和key2中的所有元素（去重），称为“并集”。


7.3.4、获取差集、并集、交集的结果
sdiffstore destination key1 key2…：将返回的差集存储在destination上
sinterstore destination key1 key2：将返回的交集存储在destination上
sunionstore destination key1 key2：将返回的并集存储在destination上

7.4、获取随机成员
scard key：获取set中成员的数量

srandmember key：随机返回set中的一个成员，可以用于抽取幸运用户！


8、 zset（有序集合）
set和zset主要差别是Sorted-Set中的每一个成员都会有一个分数(score)与之关联，Redis正是通过分数来为集合中的成员进行从小到大的排序。然而需要额外指出的是，尽管Sorted-Set中的成员必须是唯一的，但是分数(score)却是可以重复的。

8.1 添加元素
zadd key score member score2 member2 …：将所有成员以及该成员的分数存放到sorted-set中。如果该元素已经存在则会用新的分数替换原有的分数。返回值是新加入到集合中的元素个数，不包含之前已经存在的元素。

8.2 获取元素
zscore key member：返回指定成员的分数

zrange key start stop [withscores]：按分数从小到大的顺序返回索引从start到stop之间的所有元素（包含两端的元素），[withscores]参数表明返回的成员包含其分数。（正序排列）

zrevrange key start stop [withscores]：按分数从大到小的顺序返回索引从start到stop之间的所有元素（包含两端的元素），（反序排列）

zrangebyscore key min max [withscores] [limit offset count]：返回分数在[min,max]的成员并按照分数从低到高排序。[withscores]：显示分数；
[limit offset count]：offset，索引从offset开始的count个元素。


8.2 、删除元素
zrem key member [member…]：移除集合中指定的成员，可以指定多个成员。

zremrangebyrank key start stop：删除索引在start-stop之间的成员，包括两端的成员。


zremrangebyscore key min max：按照分数范围删除元素

zremrangebylex key min max：按照字典（元素）范围删除元素
该命令的使用前提是所有元素的分数必须一致！


8.3、改变数值
zincrby key increment member：给指定的成员增加指定的分数。返回值是更改后的分数。
该命令常用于投票，或者点击某个微博标题增加热度的时候使用！


8.4、 其他命令
zcount key min max：获取分数在[min,max]之间的成员个数

zrank key member：返回成员在集合中的排名。（从小到大）
zrevrank key member：返回成员在集合中的排名。（从大到小）


9、三种特殊数据类型
9.1、bitmaps（打卡计数）
位图：一般用于完成类似打卡这样的操作。例如：记录一周的打卡状态。


9.2、hyperloglogs
统计集合中不重复元素的个数。特点：不管有多少个元素，只需要占用12KB的内存！

#往集合添加数据
pfadd key element...
#统计集合中不重复元素的个数
pfcount key
#将多个 HyperLogLog 合并为一个 HyperLogLog 
pfmerge destkey sourcekey [sourcekey ...]



9.3、geospatial
geospatial可以用于地理空间位置的计算，例如计算两地之间的距离、某一地理位置指定半径范围内的所有地理位置等。

#添加北京和深圳的地理位置：
geoadd citys 114.085947 22.547 shenzheng 116.405285 39.904989 beijing
#获取北京到深圳的直线距离：
geodist citys shenzheng beijing km
#返回指定地理位置的经纬度（和添加时的经纬度有细微的区别）
geopos citys shenzheng
#以给定的经纬度为中心， 返回与中心的距离不超过给定最大距离的所有位置元素。
georadius citys 114.085947 22.547 2000 km
#获取指定的地理位置指定半径内的所有元素
georadiusbymember citys shenzheng 2000 km





10、redis高级
10.1、多数据库
一个redis实例（连接）包括16个数据库，下标从0到15，客户端默认使用第0号数据库。

select index：切换到指定的库，索引范围为0-15
move key index：将当前库指定的key移植到指定的库（剪切）
dbsize：返回当前数据库中 key 的数目。
flushdb：删除当前选择数据库中的所有key。
flushall：删除所有数据库中的所有key。



10.2、事务
10.2.1、事务中的命令
multi：开启事务用于标记事务的开始，其后执行的命令都将被存入命令队列，直到执行exec时，这些命令才会被执行。
exec：事务提交。
discard：事务回滚。
watch：监视某个数据的变化。

10.2.3、事务中的异常


10.2.3、回滚事务


10.2.4、redis实现乐观锁
悲观锁：悲观锁在操作数据时是直接把数据上锁，直到操作完成之后才会释放锁，在上锁期间其他人不能操作数据。
乐观锁：只有提交数据时，才会去判断在此期间别人是否修改了数据，如果别人修改了数据则放弃操作，否则执行操作。

redis中，通过watch命令，来对某个要在事务中操作的数据进行监视，如果在最终执行的时候，发现监视的数据已经被修改了，则放弃事务对监视数据的改变。（类似乐观锁）


10.3、消息订阅与发布
subscribe channel：订阅频道。此时如果没有人“发布”消息，第一个窗口处于等待状态。

publish channel content：在指定的频道中发布消息。打开第二个窗口，在mychannel频道中发布消息"hello everyone"。

第一个窗口收到第二个窗口发布的消息。

psubscribe channel*：订阅的频道支持模式（pattern），可以进行批量订阅频道。

10.4、 redis持久化
10.4.1、两种方式
Redis支持两种方式的持久化

RDB方式（默认支持，无需配置）
该机制是以指定的时间间隔内将内存中的数据集快照写入磁盘。
优势：备份恢复
劣势：数据丢失
当客户端调用shutdown命令时，在redis服务器在关闭之前，会自动持久化内存中的数据到磁盘，除非在关闭redis时不进行持久化：shutdown nosave，或者强制杀死redis进程：kill -9 进程id。
AOF方式
该机制将以日志的形式记录服务器所处理的每一个写操作(对数据产生改变的操作)，在Redis服务器启动之初会读取该文件来重新构建数据库，以保证启动后数据库中的数据是完整的。
优势：高数据持久性，相较于RDB
劣势：AOF在运行效率上往往会慢于RDB
10.4.2、AOF的使用
1.修改redis.conf的appendonly no为appendonly yes，开启AOF
2.清空数据库：flushall
3.设置一些数据
4.关闭redis：shutdown nosave
5.启动redis，并查看数据


10.4、 启动多个redis
方式一：

#开启默认redis服务器
./redis-server redis.conf
#开启指定端口号的redis服务器
./redis-server redis.conf --port 6380


关闭默认redis服务器：./redis-cli shutdown
关闭指定端口号redis服务器：./redis-cli -p 端口号 shutdown

方式二：复制redis.conf文件，修改端口【推荐使用】
1）cp redis.conf redis6380.conf

2）修改redis6380.conf中的启动端口为6380

3）启动redis时，指定配置文件redis6380.conf


10.5、主从复制（主服务器写入数据，从服务器只读）
1、创建两个空文本 redis6380.conf和redis6381.conf
命令：touch redis6380.conf redis6381.conf

2、编辑从服务器的配置文件
vim redis6380.conf

include redis.conf
#后端启动
daemonize yes
#端口号
port 6380
#指定主服务器的IP和端口
slaveof 127.0.0.1 6379
vim redis6381.conf

include redis.conf
#后端启动
daemonize yes
#端口号
port 6381
#指定主服务器的IP和端口
slaveof 127.0.0.1 6379

3、启动主从服务器
端口号为6379的redis为主服务器（Master）
端口号为6380和6381的redis为从服务器（Salve）

4、登录master服务器和slave服务器
info replication：查看服务器信息


测试主写数据，从读数据
登录master服务器，写入数据

登录slave服务器，读取数据

slave服务器写数据失败

slaveof no one：将当前slave服务器升为master服务器。
slaveof host port：将当前服务器添加到host port的从服务器中。

10.6、高可用哨兵Sentinel
Sentinel 系统有三个主要任务：
监控：Sentinel 不断的检查主服务和从服务器是否按照预期正常工作。
提醒：被监控的 Redis 出现问题时，Sentinel 会通知管理员或其他应用程序。
自动故障转移：监控的主 Redis 不能正常工作，Sentinel 会开始进行故障迁移操作。将一个从服务器升级新的主服务器。让其他从服务器挂到新的主服务器。同时向客户端提供新的主服务器地址。

1）在/usr/software/redis/bin下新建3个哨兵配置文件：
touch sentinel26379.conf sentinel26380.conf sentinel26381.conf

2）vim sentinel26379.conf：修改文件
mater服务器的ip：127.0.0.1，port：9379

#导入redis-7.0.0下的sentinel.conf
include /usr/software/redis-7.0.0/sentinel.conf
#需要改变的从这开始（覆盖）后端启动
daemonize yes
#master服务器的信息，IP，port，对mater服务器监视
sentinel monitor m6379 127.0.0.1 6379 2

vim sentinel26380.conf：修改文件

#导入redis-7.0.0下的sentinel.conf
include /usr/software/redis-7.0.0/sentinel.conf
#需要改变的从这开始（覆盖）后端启动
daemonize yes
port 26380
#master服务器的信息，IP，port，对mater服务器监视
sentinel monitor m6379 127.0.0.1 6379 2

vim sentinel26381.conf：修改文件

#导入redis-7.0.0下的sentinel.conf
include /usr/software/redis-7.0.0/sentinel.conf
#需要改变的从这开始（覆盖）后端启动
daemonize yes
port 26381
#master服务器的信息，IP，port，对mater服务器监视
sentinel monitor m6379 127.0.0.1 6379 2

3）修改完成之后，启动3个哨兵进程

./redis- sentinel sentinel26379.conf
./redis- sentinel sentinel26380.conf
./redis- sentinel sentinel26381.conf

4）手动干掉master，稍等1分钟，再次查看当前master/slave情况。

发现127.0.0.1：6380由slave升为master服务器，127.0.0.1：6381变为新master的slave。（这就是哨兵，发现master故障，从master的slave选择一个作为新master，把其他slave改为新master的slave，保证注册服务器架构）


10.7、redis设置密码
1、vim redis.conf：修改配置文件
修改前

修改后

2、.redis-server redis.conf：启动redis。./redis-cli：连接redis服务器。发现redis命令失效，没有权限。

3、访问有密码的 Redis 两种方式：
①：在连接到客户端后，使用命令：auth 密码，命令执行成功后，可以正常使用 Redis

②：在连接客户端时使用 -a 密码。例如 ./redis-cli -h ip -p port -a password



```shell
# 服务启动
[root@grpc01 bin]# ./redis-server /usr/local/bin/6379.conf
[root@grpc01 bin]# /usr/local/bin/redis-server /usr/local/bin/6379.conf
[root@grpc01 bin]# ps -ef f | grep redis
root     14450 12969  0 04:31 pts/0    S+     0:00                      \_ grep --color=auto redis
root     14444     1  0 04:30 ?        Ssl    0:00 ./redis-server 127.0.0.1:6379
[root@grpc01 bin]# vim redis.conf
[root@grpc01 bin]# tail -f /var/run/redis_6379.pid
14444

# 客户端连接
./iredis -h rm22397.hebe.grid.sina.com.cn -p 22397
./iredis -h 127.0.0.1 -p 6379
~/tmp/python37/bin/iredis -h 127.0.0.1 -p 6379
~/tmp/python37/bin/iredis -h 192.168.56.5 -p 6379
~/tmp/python37/bin/iredis -h rm33907.eos.grid.sina.com.cn -p 33907				# 测试运维
~/tmp/python37/bin/iredis -h rm22397.hebe.grid.sina.com.cn -p 22397

# 停服务(一)
[chenchen@grpc01 tmp]$ ~/tmp/python37/bin/iredis -h 127.0.0.1 -p 6379
iredis  1.11.1 (Python 3.7.12)
redis-server  7.0.5 
Home:   https://iredis.io
Issues: https://iredis.io/issues
127.0.0.1:6379> shutdown
SHUTDOWN will shutdown the server.
# 停服务(二)
[chenchen@grpc01 tmp]$ ~/tmp/python37/bin/redis shutdown
[chenchen@grpc01 tmp]$ ~/tmp/python37/bin/iredis shutdown
[root@grpc01 bin]# ~/tmp/python37/bin/iredis shutdown
SHUTDOWN will shutdown the server.
Do you want to proceed? (y/n): y
Your Call!!
Connection closed by server. retrying... retry left: 2
Error 111 connecting to 127.0.0.1:6379. Connection refused. retrying... retry left: 1
(error) ERROR Error 111 connecting to 127.0.0.1:6379. Connection refused.

```



```shell
rm33907.eos.grid.sina.com.cn:33907> get yocc
(nil)
rm33907.eos.grid.sina.com.cn:33907> EXISTS yocc
(integer) 0
rm33907.eos.grid.sina.com.cn:33907> SET yocc jeffchen NX
OK
rm33907.eos.grid.sina.com.cn:33907> SET yocc jeffchen NX
(nil)
rm33907.eos.grid.sina.com.cn:33907> GET yocc
"jeffchen"
rm33907.eos.grid.sina.com.cn:33907>
```



```shell
选项
SET 命令支持一组修改其行为的选项:

EX 秒 -- 设置指定的过期时间(以秒为单位)
PX 毫秒 -- 设置指定的过期时间(以毫秒为单位)
EXAT 时间戳-秒 -- 设置 key 过期的指定 Unix 时间戳(以秒为单位)
PXAT 时间戳-毫秒 -- 设置 key 过期的指定 Unix 时间戳(以毫秒为单位)
NX -- 仅当 key 尚不存在时才设置该 key
XX -- 仅当 key 已存在时才设置该 key
KEEPTTL -- 保留与 key 相关的生存时间
GET -- 返回存储在 key 里的旧字符串, 如果 key 不存在, 则返回 nil. 如果存储在 key 里的值不是字符串, 则返回错误并中止 SET.
注意： 由于 SET 命令选项可以替换 SETNX、SETEX、PSETEX、GETSET，因此在 Redis 的未来版本中，这些命令可能会被弃用并最终删除。
```





### bitfield

```shell
Bitmaps 本身不是一种数据结构, 实际上它就是字符串, 但是它可以对字符串的位进行操作.
Bitmaps 单独提供了一套命令, 所以在 Redis 中使用 Bitmaps 和使用字符串的方法不太相同.
可以把 Bitmaps 想象成一个以位为单位的数组, 数组的每个单元只能存储0和1, 数组的下标在 Bitmaps 中叫做 偏移量.

一个 bitfield 命令, 可以一次对多个位进行操作. 
这个指令有三个子指令, get, set, incrby, 都可以对指定位片段进行读写, 但最多只能处理64个连续的位, 如超过64位, 则要使用多个子指令, bitfield可以一次执行多个子指令.

```





### 配置文件中文翻译

```shell
# Redis 配置文件样本
#
# 注意：如果想要读取配置文件的参数，必须将配置文件以第一参数的形式启动，如下启动示例:
#
# ./redis-server /path/to/redis.conf

# 单位说明：当需要设置内存大小时，可以用1K 5GB 4M等常用格式指定：
#
# 1k => 1000 bytes
# 1kb => 1024 bytes
# 1m => 1000000 bytes
# 1mb => 1024*1024 bytes
# 1g => 1000000000 bytes
# 1gb => 1024*1024*1024 bytes
#
# 单位不区分大小写 所以 1GB 1Gb 1gB 都是一样的.

################################## 多包含 ###################################

# 在此处包含一个或多个其他配置文件。
# 如果您有一个标准模板，该模板可用于所有Redis服务器，
# 但还需要自定义每个服务器的一些设置，则此模板非常有用。
# include文件可以包括其他文件，因此请聪明地使用它。
#
# 注意选项“include”不会被来自admin或redis sentinel的命令“config rewrite”重写。
# 因为redis总是使用最后处理的行作为配置指令的值，所以最好将include放在该文件的开头，
# 以避免在运行时覆盖配置更改。
#
# 反之如果您对使用include覆盖配置选项感兴趣，最好使用include作为最后一行。
#
# include /path/to/local.conf
# include /path/to/other.conf

################################## 多模块 #####################################

# 启动时加载模块。如果服务器无法加载模块，它将中止。可以使用多个loadmodule指令。
#
# loadmodule /path/to/my_module.so
# loadmodule /path/to/other_module.so

################################## 网络 #####################################

# 默认情况下如果没有指定bind配置
# Redis侦听服务器上所有可用网络接口的连接（指的是网卡）
# 可以使用“bind”配置指令只侦听一个或多个选定的接口，后跟一个或多个IP地址。
#
# Examples:
#
# bind 192.168.1.100 10.0.0.1
# bind 127.0.0.1 ::1
#
# ~~~ 警告 ~~~ 如果运行Redis的计算机直接暴露在Internet上，
# 绑定到所有接口是危险的，并且会将实例暴露给互联网上的每个人。
# 因此，默认情况下，设置了bind 127.0.0.1，
# 这意味着Redis只能接受运行在同一台计算机上的客户端的连接。
#
# 如果您确定希望实例监听所有接口
# 只需要注释下面这一行.
# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bind 127.0.0.1

# 保护模式是一个安全保护层，为了避免在Internet上打开的Redis实例被访问和利用
#
# 当保护模式打开时，如果:
#
# 1) 服务器没有使用“bind”指令显式绑定到一组地址。
# 2) 未配置密码。
#
# 服务器只接受来自从IPv4和IPv6内网地址
# 127.0.0.1和：：1连接的客户端以及来自Unix域socket的连接。
#
# 默认情况下启用保护模式。 只有当您确定希望来自其他主机的客户机连接到Redis时，
# 您才应该禁用它，即使没有配置身份验证，也没有使用“bind”指令显式列出特定的接口集。
protected-mode yes

# 指定端口上的连接，默认值为6379
# 如果指定了端口0，Redis将不会侦听TCP连接。
port 6379

# TCP listen() backlog.
#
# 在每秒高请求数的环境中，为了避免客户机连接速度慢的问题，您需要大量backlog。
# 请注意，Linux内核将自动将其截断为/proc/sys/net/core/somaxconn的值，
# 因此请确保同时提高somaxconn和tcp-max-syn-u backlog的值，以获得所需的效果。
tcp-backlog 511

# Unix socket.
#
# 指定将用于侦听传入连接的Unix socket的路径。
# 没有默认值，因此未指定时，redis不会在UNIX socket上侦听。
#
# unixsocket /tmp/redis.sock
# unixsocketperm 700

# 客户端空闲n秒后关闭连接（0表示禁用）
timeout 0

# TCP keepalive.
#
# 如果非零，则使用so-keepalive在没有通信的情况下向客户机发送TCP ACK。这有两个原因:
#
# 1) 能够检测无响应的服务
# 2) 让该连接中间的网络设备知道这个连接还存活
#
# 在Linux上，这个指定的值(单位秒)就是发送ACK的时间间隔。
# 注意：要关闭这个连接需要两倍的这个时间值。
# 在其他内核上这个时间间隔由内核配置决定
tcp-keepalive 300

################################# 常用 #####################################

# 默认情况下，redis不作为守护进程运行。如果需要，请使用“是”。请注意，当后台监控时，
# redis将在/var/run/redis.pid中写入一个pid文件。
daemonize no

# 是否通过upstart或systemd管理守护进程。
# 默认no没有服务监控，其它选项有upstart, systemd, auto。
supervised no

# 生成的pid文件位置
pidfile /var/run/redis_6379.pid

# 指定服务器日志级别
# 可以配置为其中一个参数:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel notice

# 指定日志文件名。空字符串还可以用于强制redis记录标准输出
# 请注意，如果使用标准输出进行日志记录，但使用后台监控，则日志将发送到/dev/null
logfile ""

# 启用系统日志记录, 只需要设置 'syslog-enabled' 为 yes,
# 还可以根据需要更新其他系统日志参数
# syslog-enabled no

# 指定系统日志标识
# syslog-ident redis

# 指定系统日志功能。必须是用户或介于 LOCAL0-LOCAL7.
# syslog-facility local0

# 设置数据库数。默认数据库为db0,
# 您可以使用select<dbid>在每个连接上选择不同的连接
databases 16

# redis启动时是否显示Logo
always-show-logo yes

################################  快照  ################################
#
# 保存数据到硬盘（持久化）:
#
#   save <seconds> <changes>
#
#   如果在<seconds>秒后执行过<changes>次改变将会保存到硬盘
#
#   save 900 1 表示如果在900秒后至少发生了1次改变就会保存
#   save 300 10 表示如果在300秒后至少发生了10次改变就会保存
#   save save 60 10000 表示如果在60秒后至少发生了10000次改变就会保存
#
#   注意：你可以注释掉所有的 save 行来停用保存功能。
#   也可以直接一个空字符串来实现停用：
#   save ""

save 900 1
save 300 10
save 60 10000

# 默认情况下，如果 redis 最后一次的后台保存失败，redis 将停止接受写操作，
# 这样以一种强硬的方式让用户知道数据不能正确的持久化到磁盘，
# 否则就会没人注意到灾难的发生。
#
# 如果后台保存进程重新启动工作了，redis 也将自动的允许写操作。
#
# 然而你要是安装了靠谱的监控，你可能不希望 redis 这样做，那你就改成 no 好了。
stop-writes-on-bgsave-error yes

# 是否在 dump .rdb 数据库的时候使用 LZF 压缩字符串
# 默认都设为 yes
# 如果你希望保存子进程节省点 cpu ，你就设置它为 no ，
# 不过这个数据集可能就会比较大
rdbcompression yes

# 是否校验rdb文件
rdbchecksum yes

# dump文件名
dbfilename dump.rdb

# 工作目录
# 例如上面的 dbfilename 只指定了文件名，
# 但是它会写入到这个目录下。这个配置项一定是个目录，而不能是文件名。
dir ./

################################# 复制 #################################

# 主从复制. 使用replicoaf将一个redis实例作为另一个redis服务器的副本，
# 只需要在从服务器配置
# A few things to understand ASAP about Redis replication.
#
#   +------------------+      +---------------+
#   |      Master      | ---> |    Replica    |
#   | (receive writes) |      |  (exact copy) |
#   +------------------+      +---------------+
#
# 1) Redis复制是异步的, but you can configure a master to
#    stop accepting writes if it appears to be not connected with at least
#    a given number of replicas.
# 2) 如果复制链接丢失的时间相对较短，Redis副本可以执行与主服务器的部分重新同步。
#    根据需要，您可能需要使用合理的值配置复制积压工作的大小（请参阅此文件的下一节）。
# 3) 复制是自动的，不需要用户干预。
#    在网络分区复制副本自动尝试重新连接到主服务器并与主服务器重新同步之后。
#
# replicaof <masterip> <masterport>

# 同步的主服务的密码
# masterauth <master-password>

# 当一个从服务失去和主服务的连接，或者同步正在进行中，副本可以以两种不同的方式进行操作：
# 1) 如果 replica-serve-stale-data 设置为 "yes" (默认值)，
#    从服务器会继续响应客户端请求，可能是正常数据，也可能是还没获得值的空数据。
# 2) 如果 replica-serve-stale-data 设置为 "no"，
#    从服务会回复"正在从主服务同步（SYNC with master in progress）"来处理各种请求，
#    除了 INFO, replicaOF, AUTH, PING, SHUTDOWN, REPLCONF, ROLE, CONFIG,
#    SUBSCRIBE, UNSUBSCRIBE, PSUBSCRIBE, PUNSUBSCRIBE, PUBLISH, PUBSUB,
#    COMMAND, POST, HOST: and LATENCY。
replica-serve-stale-data yes

# 你可以配置从服务实例是否接受写操作。可写的从服务实例可能对存储临时数据可能很有用(因为
# 写入从服务的数据在同主服务同步之后将很容被删除)，
# 但是如果客户端由于配置错误在写入时也可能导致问题。
# 从Redis2.6默认所有的从服务为只读
# 注意:只读的slave不是为了暴露给互联网上不可信的客户端而设计的。
# 它只是一个防止实例误用的保护层。
# 一个只读的slave支持所有的管理命令比如config,debug等。
# 为了限制你可以用'rename-command'来隐藏所有的管理和危险命令来增强只读slave的安全性。
replica-read-only yes

# 复制同步策略：磁盘或socket。
#
# -------------------------------------------------------
# WARNING: DISKLESS REPLICATION IS EXPERIMENTAL CURRENTLY
# -------------------------------------------------------
#
# 新的从服务和重新连接的从服务在复制的过程中将会无法接收差异数据，
# 需要做完全同步，RDB文件从主服务传输到从服务

# 传输有两种不同的方式：
#
# 1) 磁盘备份: redis主机创建了一个新进程，该进程将RDB文件写入磁盘。
#    随后，父进程将文件增量传输到副本。
# 2) 无盘备份: redis master创建了一个新的进程，
#    该进程直接将RDB文件写入从服务sockets，而根本不接触磁盘。
#
# 使用磁盘备份复制，在生成RDB文件的同时，
# 可以在当前生成子级时将更多从服务排队并与RDB文件一起提供服务。
# RDB文件完成其工作. 在无盘复制中，一旦传输开始，
# 新的从服务将会排队等到当前从服务终止才会开始传输
#
# 当使用无盘复制时, 服务器在开始传输之前等待一段可配置的时间（以秒为单位），
# 希望多个从服务能够到达，并且传输可以并行。
#
# 使用低速磁盘和快速（大带宽）网络，无盘复制会更好。
repl-diskless-sync no

# When diskless replication is enabled, it is possible to configure the delay the server waits in order to spawn the child that transfers the RDB via socket to the replicas.
# 当启用无盘复制时，可以配置服务器等待的延迟，以便生成通过socket将RDB传输到副本的子级。
#
# 这很重要，因为一旦转移开始，可能将不会为新的从服务提供服务,
# 那将排队等待下一次RDB传输，因此，服务器会等待一段延迟，以便让更多的从服务到达
#
# 延迟以秒为单位，默认为5秒。要完全禁用它，只需将其设置为0秒，传输将尽快开始。
repl-diskless-sync-delay 5

# 副本以预先定义的间隔向服务器发送ping。
# 可以使用repl-ping-u replica-period选项更改此间隔。默认值为10秒。
#
# repl-ping-replica-period 10

# 以下选项设置复制超时：
#
# 1) 从复制副本的角度看，同步期间的批量传输I/O。
# 2) 从从服务的角度看主服务的超时 (data, pings)。
# 3) 从主服务器的角度来看，复制超时 (REPLCONF ACK pings)。
#
# 必须确保该值大于为repl-ping-replica-period指定的值，
# 否则每次主服务器和副本之间的通信量低时都会检测到超时。
#
# repl-timeout 60

# 同步后在从服务socket上禁用TCP_NODELAY？
#
# 如果选择“yes”，Redis将使用较少的TCP数据包和较少的带宽向从服务发送数据。
# 但这可能会增加数据在从服务端出现的延迟，对于使用默认配置的Linux内核，延迟最长可达40毫秒。
#
# 如果选择“no”，将减少数据出现在从服务端的延迟，但将使用更多带宽进行复制。
#
# 默认情况下，我们会针对低延迟进行优化，但在非常高的流量条件下，
# 或者当主服务器和从哪个服务器距离很多跃点时，将其设置为“yes”会更好
repl-disable-tcp-nodelay no

# 设置从服务积压量backlog大小。backlog是一个缓冲区，当从服务器断开连接一段时间后，
# 它会累积从服务的数据，因此当从服务希望再次重新连接时，通常不需要完全重新同步，
# 只部分重新同步就足够了，只需传递断开连接时从服务丢失的数据部分。
#
# 从服务积压量backlog越大，从服务可断联时间可以越长，之后就可以执行部分重新同步。
#
# 只有在至少连接了一个从服务后，才会分配积压空间backlog。
#
# repl-backlog-size 1mb

# 在主服务器一段时间内不再连接副本后，将释放backlog空间。
# 以下选项配置从最后一个从服务断开连接开始，需要经过的秒数，以便释放backlog缓冲区。
#
# 注意，从服务永远不会释放积压的超时时间，因为它们可能在主服务挂掉之后被提升为主服务，
# 并且应该能够与从服务正确地“部分重新同步”：因此它们应该总是积累backlog。
#
# 值为0表示永不释放backlog。
#
# repl-backlog-ttl 3600

# 从服务优先级是整数类型的参数
# Redis Sentinel使用它来选择一个从服务，以便在主服务器不再正常工作时升级为主服务器
#
# 设置值越小优先级越高
#
# 但是，特殊优先级0将副本标记为无法执行master角色，
# 因此redis sentinel将永远不会选择优先级为0的副本进行升级。
#
# 默认配置为100
replica-priority 100

# 如果连接的从服务少于n个，且延迟小于或等于m秒，则主机可以停止接受写入。
#
# n个副本需要处于“oneline”状态。
#
# 延时是以秒为单位，并且必须小于等于指定值，
# 是从最后一个从slave接收到的ping（通常每秒发送）开始计数。
# 该选项不保证N个slave正确同步写操作，但是限制数据丢失的窗口期。
#
# 例如至少需要3个延时小于等于10秒的从服务用下面的指令：
#
# min-replicas-to-write 3
# min-replicas-max-lag 10
#
# 将其中一个设置为0将禁用该功能。
# 默认min-replicas-to-write是0，min-replicas-max-lag是10.

# redis主服务可以以不同的方式列出从服务的地址和端口。
# 比如"INFO replication"命令就可以查看这些信息，
# Redis Sentinel使用它和其他工具来发现副本实例。
# 另一个可以获得此信息的地方是在主服务使用“role”命令查看这些信息
#
# 从服务通常报告的列出的IP和地址是通过以下方式获得的：
#
#   IP: 通过检查从服务用于与主服务器连接的socket的对等地址，可以自动检测该地址。
#
#   Port: 在复制握手期间，该端口由从服务通信，通常是在从服务用来侦听连接的端口。
#
# 但是，当使用端口转发或网络地址转换（NAT）时，从服务实际上可以通过不同的IP和端口对访问。
# 下面两个参数就是为了解决这个问题的，可以自行设置，从节点上报给master的自己ip和端口
#
# replica-announce-ip 5.5.5.5
# replica-announce-port 1234

################################## 安全 ###################################

# 要求客户端在处理任何其他命令之前发出auth<password>。
# 在您不信任其他人访问运行redis服务器的主机的环境中，这可能很有用。
#
# 为了向后兼容，并且因为大多数人不需要身份验证（例如，他们运行自己的服务器），
# 所以应该将其注释掉。
#
# 警告: 由于redis速度非常快，外部用户尝试破解密码速度会达到每秒多达150k次。
# 这意味着您应该使用一个非常强的密码，否则很容易破解。
#
# requirepass foobared

# 命令重命名。
#
# 在共享环境下，可以为危险命令改变名字。
# 比如，你可以为 CONFIG 改个其他不太容易猜到的名字，这样内部的工具仍然可以使用
#
# 比如:
#
# rename-command CONFIG b840fc02d524045429941cc15f59e41cb7be6c52
#
# 也可以通过将命令重命名为空字符串来完全禁用它。
#
# rename-command CONFIG ""
#
# 请注意：改变命令名字被记录到AOF文件或被传送到从服务器可能产生问题

################################### 客户端 ####################################

# 同时设置最大连接客户端数。 默认情况下，此限制设置为10000个客户端，
# 但是如果Redis服务器无法配置进程文件限制以允许指定的限制，
# 则允许的最大客户端数设置为当前文件限制减去32
#（因为Redis保留一些文件描述符供内部使用）。
#
# 达到限制后，Redis将关闭所有发送“max number of clients reached”错误的新连接。
#
# maxclients 10000

############################## 内存管理 ################################

# 设置使用内存上限，当达到上限，Redis会尝试根据maxmemory-policy的删除策略删除keys
#
# 如果redis不能根据策略删除keys,或者如果策略设置为“noevicetion”, 
# Redis会回复需要更多内存的错误信息给命令。
# 例如，SET,LPUSH等等，但是会继续响应像Get这样的只读命令。
#
# 当将redis用作LRU或LFU缓存
# 或者为实例设置了硬性内存限制的时候（使用“noevicetion”策略）时，此选项很有用。
#
# 警告：当有多个slave连上达到内存上限时，
# 主服务为同步从服务的输出缓冲区所需内存不计算在使用内存中。
# 这样当移除key时，就不会因网络问题 / 重新同步事件触发移除key的循环，
# 反过来从服务的输出缓冲区充满了key被移除的DEL命令，这将触发删除更多的key，
# 直到这个数据库完全被清空为止。
#
# 总之：如果您连接多个从服务，建议您为maxmemory设置一个下限，
# 以便系统上有一些用于副本输出缓冲区的可用RAM（但如果策略为“noevection”，则不需要这样做）。
#
# maxmemory <bytes>

# MAXMEMORY POLICY:当达到最大内存时，Redis如何选择要删除的内容。你可以选择下面五种之一:
#
# volatile-lru -> 根据LRU算法删除设置过期时间的key
# allkeys-lru -> 根据LRU算法删除任何key
# volatile-random -> 随机删除设置了过期时间的key
# allkeys-random -> 随机删除任何key
# volatile-ttl -> 删除即将过期的key(minor TTL)
# noeviction -> 不删除任何key，只返回一个写错误信息
#
# LRU means Least Recently Used
# LFU means Least Frequently Used
#
# Both LRU, LFU and volatile-ttl are implemented using approximated
# randomized algorithms.
#
# 注意: 以上任何一项政策，当没有腾出足够的空间执行写命令前，redis都会报一个写错误
#       可能受影响的命令: set setnx setex append
#       incr decr rpush lpush rpushx lpushx linsert lset rpoplpush sadd
#       sinter sinterstore sunion sunionstore sdiff sdiffstore zadd zincrby
#       zunionstore zinterstore hset hsetnx hmset hincrby incrby decrby
#       getset mset msetnx exec sort
#
# 默认配置如下（不做删除操作）：
#
# maxmemory-policy noeviction

# LRU、LFU和最小TTL算法不是精确算法，而是近似算法（为了节省内存）,
# 所以你可以调整它的速度或精度。对于默认的redis将检查五个键并选择最近使用较少的键，
# 您可以使用以下配置指令更改样本大小。
#
# 默认值5会产生足够好的结果。10非常接近真实的LRU，但消耗更多的CPU。3速度更快，但不太准确。
#
# maxmemory-samples 5

# 从redis 5开始，默认情况下从服务将忽略其maxmemory设置（除非在故障是Redis cluster转移
# 主从或手动将从服务升级为主服务）这意味着删除策略只有主服务处理, 只是发送del命令给从服务
#
# 这样可以保证主从数据的一致性
# 但是，如果您的从服务是可写的，或者您希望从服务具有不同的内存设置，
# 并且您确定对从服务执行的所有写入都是等幂的，则可以更改此默认值(但一定要明白你在做什么)。
#
# replica-ignore-maxmemory yes

############################# LAZY FREEING ####################################

# Redis has two primitives to delete keys. One is called DEL and is a blocking
# deletion of the object. It means that the server stops processing new commands
# in order to reclaim all the memory associated with an object in a synchronous
# way. If the key deleted is associated with a small object, the time needed
# in order to execute the DEL command is very small and comparable to most other
# O(1) or O(log_N) commands in Redis. However if the key is associated with an
# aggregated value containing millions of elements, the server can block for
# a long time (even seconds) in order to complete the operation.
#
# For the above reasons Redis also offers non blocking deletion primitives
# such as UNLINK (non blocking DEL) and the ASYNC option of FLUSHALL and
# FLUSHDB commands, in order to reclaim memory in background. Those commands
# are executed in constant time. Another thread will incrementally free the
# object in the background as fast as possible.
#
# DEL, UNLINK and ASYNC option of FLUSHALL and FLUSHDB are user-controlled.
# It's up to the design of the application to understand when it is a good
# idea to use one or the other. However the Redis server sometimes has to
# delete keys or flush the whole database as a side effect of other operations.
# Specifically Redis deletes objects independently of a user call in the
# following scenarios:
#
# 1) On eviction, because of the maxmemory and maxmemory policy configurations,
#    in order to make room for new data, without going over the specified
#    memory limit.
# 2) Because of expire: when a key with an associated time to live (see the
#    EXPIRE command) must be deleted from memory.
# 3) Because of a side effect of a command that stores data on a key that may
#    already exist. For example the RENAME command may delete the old key
#    content when it is replaced with another one. Similarly SUNIONSTORE
#    or SORT with STORE option may delete existing keys. The SET command
#    itself removes any old content of the specified key in order to replace
#    it with the specified string.
# 4) During replication, when a replica performs a full resynchronization with
#    its master, the content of the whole database is removed in order to
#    load the RDB file just transferred.
#
# In all the above cases the default is to delete objects in a blocking way,
# like if DEL was called. However you can configure each case specifically
# in order to instead release memory in a non-blocking way like if UNLINK
# was called, using the following configuration directives:

lazyfree-lazy-eviction no
lazyfree-lazy-expire no
lazyfree-lazy-server-del no
replica-lazy-flush no

############################## APPEND ONLY MODE ###############################

# 默认情况下，redis异步转储磁盘上的数据集。在许多应用程序中，这种模式已经足够好了，
# 但是Redis进程的问题或断电可能会导致几分钟的写入丢失（取决于配置的保存点）。
#
# AOF是另一种持久性模式，可提供更好的耐久性。例如，如果使用RDB策略,
# Redis挂掉（如服务器断电）时只会丢失一秒钟的写入时间
#
# AOF和RDB持久性可以同时启用，不会出现问题。
# Redis会优先使用AOF策略，因为更好地保证了数据持久化
#
# 请访问 http://redis.io/topics/persistence 获取更多信息

appendonly no

# AOF文件名（默认："appendonly.aof"）
appendfilename "appendonly.aof"

# fsync() 系统调用告诉操作系统把数据写到磁盘上，而不是等更多的数据进入输出缓冲区。
# 有些操作系统会真的把数据马上刷到磁盘上；有些则会尽快去尝试这么做。
# Redis支持三种不同的模式：
# no：不要立刻刷，只有在操作系统需要刷的时候再刷。比较快。
# always：每次写操作都立刻写入到aof文件。慢，但是最安全。
# everysec：每秒写一次。折中方案。
# 默认的 "everysec" 通常来说能在速度和数据安全性之间取得比较好的平衡。
#
# 更多细节信息访问下面链接：
# http://antirez.com/post/redis-persistence-demystified.html
#
# 如果不确定就使用everysec

# appendfsync always
appendfsync everysec
# appendfsync no

# 当AOF同步策略设置为always或everysec时，后台保存进程(后台保存或写入AOF日志)正在对
# 磁盘执行大量I/O，在某些Linux配置中，redis可能会在fsync（）调用上阻塞太长时间。
# 注意，目前对这个情况还没有完美修正，甚至不同线程的 fsync() 会阻塞我们同步的write(2)调用。
#
# 为了缓解这个问题，可以用下面这个选项。它可以在 BGSAVE 或 BGREWRITEAOF
# 处理时阻止fsync()。
#
# 这意味着当另一个子进程在保存时, 那么Redis就处于"不可同步"的状态。
# 这实际上是说，在最差的情况下可能会丢掉30秒钟的日志数据。（默认Linux设定）
#
# 如果把这个设置成"yes"带来了延迟问题，就保持"no"，这是保存持久数据的最安全的方式。

no-appendfsync-on-rewrite no

# 自动重写AOF文件
# 当AOF日志大小增加指定的百分比时，Redis能够调用BGREWRITEAOF自动重写AOF的日志文件。
#
# 工作原理:Redis记住上次重写AOF文件的大小(如果重启后还没有写操作，就直接用启动时的AOF大小)
#
# 这个基准大小和当前大小做比较。如果当前大小超过指定比例，就会触发重写操作。
# 你还需要指定被重写日志的最小尺寸，这样避免了达到指定百分比但尺寸仍然很小的情况还要重写。
#
# 指定百分比为0会禁用AOF自动重写特性

auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb

# 如果设置为yes，如果一个因异常被截断的AOF文件被redis启动时加载进内存，
# redis将会发送日志通知用户。如果设置为no，erdis将会拒绝启动。
# 此时需要用"redis-check-aof"工具修复文件。
aof-load-truncated yes

# 在重写AOF文件时，Redis可以使用AOF文件中的RDB前导码来更快地重写和恢复。
# 启用此选项时，重写的AOF文件由两个不同的参数组成。
#
#   [RDB file][AOF tail]
#
# 当加载redis时，识别出AOF文件以“redis”字符串开头并加载前缀RDB文件，然后继续加载AOF尾部。
aof-use-rdb-preamble yes

################################ LUA 脚本  ###############################

# Lua脚本的最大执行时间（毫秒）
lua-time-limit 5000

################################ REDIS 集群  ###############################
#
# 如果想让Redis实例作为集群的一部分，需要去掉下方配置的注释
# cluster-enabled yes

# 配置redis自动生成的集群配置文件名。确保同一系统中运行的各redis实例该配置文件不要重名。
# cluster-config-file nodes-6379.conf

# 集群节点超时毫秒数。超时的节点将被视为不可用状态。
# cluster-node-timeout 15000


# 如果发生故障的主机从服务的数据太旧了，这个从服务会避免升级为主服务，
# 如果主从失联时间超过：
#   (node-timeout * replica-validity-factor) + repl-ping-replica-period
# 则不会被提升为master
#
# 如node-timeout为30秒，slave-validity-factor为10,slave-validity-factor为10,
# 默认default repl-ping-slave-period为10秒,失联时间超过310秒slave就不会成为master。
#
# 较大的slave-validity-factor值可能允许包含过旧数据的从服务器成为主服务器，
# 同时较小的值可能会阻止集群选举出新主服务。
# 为了达到最大限度的高可用性，可以设置为0，即从服务不管和主服务失联多久都可以提升为主服务
#
# cluster-replica-validity-factor 10

# 只有在之前master有其它指定数量的工作状态下的slave节点时，slave节点才能提升为master。
# 默认为1（即该集群至少有3个节点，1 master＋2 slaves，master宕机，
# 仍有另外1个slave的情况下其中1个slave可以提升）
# 测试环境可设置为0，生成环境中至少设置为1
# cluster-migration-barrier 1

# 默认情况下如果redis集群如果检测到至少有1个hash slot不可用，集群将停止查询数据。
# 如果所有slot恢复则集群自动恢复。
# 如果需要集群部分可用情况下仍可提供查询服务，设置为no。
# cluster-require-full-coverage yes


# 选项设置为yes时，会阻止replicas尝试对其主服务在主故障期间进行故障转移
# 然而，主服务仍然可以执行手动故障转移,如果强制这样做的话。
# cluster-replica-no-failover no

# In order to setup your cluster make sure to read the documentation
# available at http://redis.io web site.

########################## CLUSTER DOCKER/NAT support  ########################

# In certain deployments, Redis Cluster nodes address discovery fails, because
# addresses are NAT-ted or because ports are forwarded (the typical case is
# Docker and other containers).
#
# In order to make Redis Cluster working in such environments, a static
# configuration where each node knows its public address is needed. The
# following two options are used for this scope, and are:
#
# * cluster-announce-ip
# * cluster-announce-port
# * cluster-announce-bus-port
#
# Each instruct the node about its address, client port, and cluster message
# bus port. The information is then published in the header of the bus packets
# so that other nodes will be able to correctly map the address of the node
# publishing the information.
#
# If the above options are not used, the normal Redis Cluster auto-detection
# will be used instead.
#
# Note that when remapped, the bus port may not be at the fixed offset of
# clients port + 10000, so you can specify any port and bus-port depending
# on how they get remapped. If the bus-port is not set, a fixed offset of
# 10000 will be used as usually.
#
# Example:
#
# cluster-announce-ip 10.1.1.5
# cluster-announce-port 6379
# cluster-announce-bus-port 6380

################################## 慢日志 ###################################

# 慢查询日志，记录超过多少微秒的查询命令。
# 查询的执行时间不包括客户端的IO执行和网络通信时间，只是查询命令执行时间。
# 1000000等于1秒，设置为0则记录所有命令
slowlog-log-slower-than 10000

# 记录大小，可通过SLOWLOG RESET命令重置
slowlog-max-len 128

################################ 延时监控 ##############################

# redis延时监控系统在运行时会采样一些操作，以便收集可能导致延时的数据根源。
# 通过 LATENCY命令 可以打印一些图样和获取一些报告，方便监控
# 这个系统仅仅记录那个执行时间大于或等于预定时间（毫秒）的操作,
# 这个预定时间是通过latency-monitor-threshold配置来指定的，
# 当设置为0时，这个监控系统处于停止状态
latency-monitor-threshold 0

############################# 事件通知 ##############################

# K 	键空间通知，所有通知以 __keyspace@<db>__ 为前缀
# E 	键事件通知，所有通知以 __keyevent@<db>__ 为前缀
# g 	DEL 、 EXPIRE 、 RENAME 等类型无关的通用命令的通知
# $ 	字符串命令的通知
# l 	列表命令的通知
# s 	集合命令的通知
# h 	哈希命令的通知
# z 	有序集合命令的通知
# x 	过期事件：每当有过期键被删除时发送
# e 	驱逐(evict)事件：每当有键因为 maxmemory 政策而被删除时发送
# A 	参数 g$lshzxe 的别名
#  Example: to enable list and generic events, from the point of view of the
#           event name, use:
#
#  notify-keyspace-events Elg
#
#  Example 2: to get the stream of the expired keys subscribing to channel
#             name __keyevent@0__:expired use:
#
#  notify-keyspace-events Ex
# Redis能通知 Pub/Sub 客户端关于键空间发生的事件，默认关闭
notify-keyspace-events ""

############################### 高级配置 ###############################

# 当hash只有少量的entry时，并且最大的entry所占空间没有超过指定的限制时，会用一种节省内存的
# 数据结构来编码。可以通过下面的指令来设定限制
hash-max-ziplist-entries 512
hash-max-ziplist-value 64

# 当取正值的时候，表示按照数据项个数来限定每个quicklist节点上的ziplist长度。
# 比如，当这个参数配置成5的时候，表示每个quicklist节点的ziplist最多包含5个数据项。
# 当取负值的时候，表示按照占用字节数来限定每个quicklist节点上的ziplist长度。
# 这时，它只能取-1到-5
# 这五个值，每个值含义如下：
#    -5: 每个quicklist节点上的ziplist大小不能超过64 Kb。（注：1kb => 1024 bytes）
#    -4: 每个quicklist节点上的ziplist大小不能超过32 Kb。
#    -3: 每个quicklist节点上的ziplist大小不能超过16 Kb。
#    -2: 每个quicklist节点上的ziplist大小不能超过8 Kb。（-2是Redis给出的默认值）
#    -1: 每个quicklist节点上的ziplist大小不能超过4 Kb。
list-max-ziplist-size -2

# 这个参数表示一个quicklist两端不被压缩的节点个数。
# 注：这里的节点个数是指quicklist双向链表的节点个数，而不是指ziplist里面的数据项个数。
# 实际上，一个quicklist节点上的ziplist，如果被压缩，就是整体被压缩的。
# 参数list-compress-depth的取值含义如下：
#    0: 是个特殊值，表示都不压缩。这是Redis的默认值。
#    1: 表示quicklist两端各有1个节点不压缩，中间的节点压缩。
#    2: 表示quicklist两端各有2个节点不压缩，中间的节点压缩。
#    3: 表示quicklist两端各有3个节点不压缩，中间的节点压缩。
#    依此类推…
# 由于0是个特殊值，很容易看出quicklist的头节点和尾节点总是不被压缩的，
# 以便于在表的两端进行快速存取。
list-compress-depth 0

# set有一种特殊编码的情况：当set数据全是十进制64位有符号整型数字构成的字符串时。
# 下面这个配置项就是用来设置set使用这种编码来节省内存的最大长度。
set-max-intset-entries 512

# 与hash和list相似，有序集合也可以用一种特别的编码方式来节省大量空间。
# 这种编码只适合长度和元素都小于下面限制的有序集合
zset-max-ziplist-entries 128
zset-max-ziplist-value 64

# HyperLogLog稀疏结构表示字节的限制。该限制包括
# 16个字节的头。当HyperLogLog使用稀疏结构表示
# 这些限制，它会被转换成密度表示。
# 值大于16000是完全没用的，因为在该点
# 密集的表示是更多的内存效率。
# 建议值是3000左右，以便具有的内存好处, 减少内存的消耗
hll-sparse-max-bytes 3000

# Streams宏节点最大大小/项目。 流数据结构是基数编码内部多个项目的大节点树。
# 使用此配置可以配置单个节点的字节数，以及切换到新节点之前可能包含的最大项目数
# 追加新的流条目。 如果以下任何设置设置为0，忽略限制，因此例如可以设置一个
# 大入口限制将max-bytes设置为0，将max-entries设置为所需的值
stream-node-max-bytes 4096
stream-node-max-entries 100

# 启用哈希刷新，每100个CPU毫秒会拿出1个毫秒来刷新Redis的主哈希表（顶级键值映射表）
activerehashing yes

# 客户端的输出缓冲区的限制，可用于强制断开那些因为某种原因从服务器
# 读取数据的速度不够快的客户端
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit replica 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60

# 客户端查询缓冲区累积新命令。 它们仅限于固定的默认情况下，
# 多数情况下为了避免协议不同步导致客户端查询缓冲区中未绑定的内存使用量的错误
# 但是，如果你有使用的话，你可以在这里配置它，比如我们有很多执行请求或类似的。
# client-query-buffer-limit 1gb

# 在Redis协议中，批量请求，即表示单个的元素strings，通常限制为512 MB。
# 但是，您可以z更改此限制
# proto-max-bulk-len 512mb

# 默认情况下，“hz”的被设定为10。提高该值将在Redis空闲时使用更多的CPU时，但同时当有多个key
# 同时到期会使Redis的反应更灵敏，以及超时可以更精确地处理
hz 10

# Normally it is useful to have an HZ value which is proportional to the
# number of clients connected. This is useful in order, for instance, to
# avoid too many clients are processed for each background task invocation
# in order to avoid latency spikes.
#
# Since the default HZ value by default is conservatively set to 10, Redis
# offers, and enables by default, the ability to use an adaptive HZ value
# which will temporary raise when there are many connected clients.
#
# When dynamic HZ is enabled, the actual configured HZ will be used as
# as a baseline, but multiples of the configured HZ value will be actually
# used as needed once more clients are connected. In this way an idle
# instance will use very little CPU time while a busy instance will be
# more responsive.
dynamic-hz yes

# 当一个子进程重写AOF文件时，如果启用下面的选项，则文件每生成32M数据会被同步
aof-rewrite-incremental-fsync yes

# 当redis保存RDB文件时，如果启用了以下选项，每生成32 MB数据，文件将被fsync-ed。
# 这很有用，以便以递增方式将文件提交到磁盘并避免大延迟峰值。
rdb-save-incremental-fsync yes

# Redis LFU eviction (see maxmemory setting) can be tuned. However it is a good
# idea to start with the default settings and only change them after investigating
# how to improve the performances and how the keys LFU change over time, which
# is possible to inspect via the OBJECT FREQ command.
#
# There are two tunable parameters in the Redis LFU implementation: the
# counter logarithm factor and the counter decay time. It is important to
# understand what the two parameters mean before changing them.
#
# The LFU counter is just 8 bits per key, it's maximum value is 255, so Redis
# uses a probabilistic increment with logarithmic behavior. Given the value
# of the old counter, when a key is accessed, the counter is incremented in
# this way:
#
# 1. A random number R between 0 and 1 is extracted.
# 2. A probability P is calculated as 1/(old_value*lfu_log_factor+1).
# 3. The counter is incremented only if R < P.
#
# The default lfu-log-factor is 10. This is a table of how the frequency
# counter changes with a different number of accesses with different
# logarithmic factors:
#
# +--------+------------+------------+------------+------------+------------+
# | factor | 100 hits   | 1000 hits  | 100K hits  | 1M hits    | 10M hits   |
# +--------+------------+------------+------------+------------+------------+
# | 0      | 104        | 255        | 255        | 255        | 255        |
# +--------+------------+------------+------------+------------+------------+
# | 1      | 18         | 49         | 255        | 255        | 255        |
# +--------+------------+------------+------------+------------+------------+
# | 10     | 10         | 18         | 142        | 255        | 255        |
# +--------+------------+------------+------------+------------+------------+
# | 100    | 8          | 11         | 49         | 143        | 255        |
# +--------+------------+------------+------------+------------+------------+
#
# NOTE: The above table was obtained by running the following commands:
#
#   redis-benchmark -n 1000000 incr foo
#   redis-cli object freq foo
#
# NOTE 2: The counter initial value is 5 in order to give new objects a chance
# to accumulate hits.
#
# The counter decay time is the time, in minutes, that must elapse in order
# for the key counter to be divided by two (or decremented if it has a value
# less <= 10).
#
# The default value for the lfu-decay-time is 1. A Special value of 0 means to
# decay the counter every time it happens to be scanned.
#
# lfu-log-factor 10
# lfu-decay-time 1

########################### ACTIVE DEFRAGMENTATION #######################
# 启用主动碎片整理
# activedefrag yes
 
# 启动活动碎片整理的最小碎片浪费量
# active-defrag-ignore-bytes 100mb
 
# 启动碎片整理的最小碎片百分比
# active-defrag-threshold-lower 10
 
# 使用最大消耗时的最大碎片百分比
# active-defrag-threshold-upper 100
 
# 在CPU百分比中进行碎片整理的最小消耗
# active-defrag-cycle-min 5
 
# 磁盘碎片整理的最大消耗
# active-defrag-cycle-max 75
 
# 将从主字典扫描处理的最大set / hash / zset / list字段数
# active-defrag-max-scan-fields 1000

```







## FAQ & troubleshooting

```shell
1. 
hash 类型的 field 类型必定是 string
hash string key string value

2. 
Pipeline 在某些场景下非常有用, 比如有多个 command 需要被 “及时的” 提交, 而且他们对相应结果没有互相依赖, 而且对结果响应也无需立即获得, 那么 pipeline 就可以充当这种 “批处理” 的工具; 而且在一定程度上, 可以较大的提升性能, 性能提升的原因主要是 TCP 链接中较少了 “交互往返” 的时间.
. 一堆指令, 同时可以不分先后提交
. 这堆指令之间没有相互依赖
. 对返回顺序无要求

```



## See Also

