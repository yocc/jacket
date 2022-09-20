# wrk2

---

## 目录

[TOC]



## 概述

wrk2 被 wrk 修改以产生恒定的吞吐量负载, 以及高 9s 的准确延迟细节(即, 当运行足够长的时间时可以产生准确的 99.9999%&#39;ile). 除了 wrk 的参数之外, wrk2 还通过 `--rate 或 -R` 参数(默认为 1000)接受吞吐量参数(每秒请求总数, QPS).

- wrk 与 wrk2 的区别: wrk2 比 wrk 多了一个 -R, --rate 参数, 表示并发率.



## 安装和问题

```shell
1) 下载, 通过 github 获取 https://github.com/giltene/wrk2
2) 解压, unzip wrk-master.zip
3) make clean
4) make
5) 报错, 在 FAQ 里
6) 修改 Makefile, 然后构建成功. 在源码目录生成一个 wrk 可执行文件. 
```



## 使用和说明

```shell
# 参数说明:
[chenchen@grpc01 wrk2]$ ./wrk --help
Usage: wrk <options> <url>                            
  Options:                                            
    -c, --connections <N>  Connections to keep open   # HTTP 连接数, 1k, 2M, 1G
    -d, --duration    <T>  Duration of test           # 测试持续的时间, 50s, 5m, 2h
    -t, --threads     <N>  Number of threads to use   # 开启的线程数
                                                      
    -s, --script      <S>  Load Lua script file       
    -H, --header      <H>  Add header to request      # 添加请求头
    -L  --latency          Print latency statistics   # 打印详细延迟统计
    -U  --u_latency        Print uncorrected latency statistics		# 打印未更正的延迟统计信息
        --timeout     <T>  Socket/request timeout     # Socket或者请求超时, 大于该时间的请求将被记录
    -B, --batch_latency    Measure latency of whole   # 测量整批流水线运维的延迟(与每个运维相对)
                           batches of pipelined ops   
                           (as opposed to each op)    
    -v, --version          Print version details      # 打印版本详情
    -R, --rate        <T>  work rate (throughput)     # 工作速率, QPS, 吞吐量, 每个线程每秒完成的请求数
                           in requests/sec (total)    
                           [Required Parameter]       
                                                      
                                                      
  Numeric arguments may include a SI unit (1k, 1M, 1G)
  Time arguments may include a time unit (2s, 2m, 2h)
[chenchen@grpc01 wrk2]$ 

# 经验上参数设定
-t, 线程数: 依照 CPU 核心数来定, 最大值不超过 2 倍 CPU 核心数.
-c, 连接数: 可以理解为并发数, 连接数需要在测试过程中多次调试, 找到 QPS 达到最大临界点时的最大并发量; 
					 由于服务都有自身的负载极限, 也会出现连接数越大 QPS 越低的情况, 
					 这种情况是因为连接数设置的过高, 导致待测系统超出自身能承受的负载.
-R, 吞吐量: 是每个线程每秒完成的请求数, 这个数值是 wrk2 必带的一个参数, 在测试过程中也是需要测试人员多次调试, 通过不断上调其数值, 测试出 QPS 的临界值及保证服务可用的时延.

# 4线程, 4链接, 持续5分钟, 并发QPS=50, 打印延迟统计
[chenchen@grpc01 wrk2]$ ./wrk -t4 -c4 -d5m -R50 --latency http://baidu.com
Running 5s test @ http://baidu.com
  4 threads and 4 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    57.24ms   62.46ms 248.58ms   82.73%
    Req/Sec       -nan      -nan   0.00      0.00%
  Latency Distribution (HdrHistogram - Recorded Latency)
 50.000%   23.89ms
 75.000%   81.54ms
 90.000%  156.93ms
 99.000%  248.19ms
 99.900%  248.70ms
 99.990%  248.70ms
 99.999%  248.70ms
100.000%  248.70ms

  Detailed Percentile spectrum:
       Value   Percentile   TotalCount 1/(1-Percentile)

       8.215     0.000000            1         1.00
      12.383     0.100000           25         1.11
      13.615     0.200000           51         1.25
      14.615     0.300000           75         1.43
      17.599     0.400000          100         1.67
      23.887     0.500000          125         2.00
      28.639     0.550000          137         2.22
      37.087     0.600000          150         2.50
      49.759     0.650000          162         2.86
      73.023     0.700000          175         3.33
      81.535     0.750000          187         4.00
      95.679     0.775000          193         4.44
      97.023     0.800000          201         5.00
     116.607     0.825000          206         5.71
     128.831     0.850000          212         6.67
     140.543     0.875000          218         8.00
     141.567     0.887500          221         8.89
     158.847     0.900000          225        10.00
     166.527     0.912500          228        11.43
     189.567     0.925000          231        13.33
     202.367     0.937500          234        16.00
     202.623     0.943750          235        17.78
     203.263     0.950000          237        20.00
     214.015     0.956250          239        22.86
     214.271     0.962500          240        26.67
     226.431     0.968750          242        32.00
     226.431     0.971875          242        35.56
     226.943     0.975000          244        40.00
     226.943     0.978125          244        45.71
     227.327     0.981250          245        53.33
     247.807     0.984375          246        64.00
     247.807     0.985938          246        71.11
     247.807     0.987500          246        80.00
     248.191     0.989062          247        91.43
     248.191     0.990625          247       106.67
     248.319     0.992188          248       128.00
     248.319     0.992969          248       142.22
     248.319     0.993750          248       160.00
     248.319     0.994531          248       182.86
     248.319     0.995313          248       213.33
     248.703     0.996094          249       256.00
     248.703     1.000000          249          inf
#[Mean    =       57.240, StdDeviation   =       62.463]
#[Max     =      248.576, Total count    =          249]
#[Buckets =           27, SubBuckets     =         2048]
----------------------------------------------------------
  253 requests in 5.06s, 95.37KB read
Requests/sec:     50.00
Transfer/sec:     18.85KB
[chenchen@grpc01 wrk2]$ 
```



## Test

```shell
一般线程数不宜过多. 核数的2到4倍足够了。 多了反而因为线程切换过多造成效率降低。因为 wrk 不是使用每个连接一个线程的模型, 而是通过异步网络 io 提升并发量，所以网络通信不会阻塞线程执行。这也是 wrk 可以用很少的线程模拟大量网路连接的原因。而现在很多性能工具并没有采用这种方式, 而是采用提高线程数来实现高并发，所以并发量一旦设的很高, 测试机自身压力就很大，测试效果反而下降。



```





## FAQ

```shell
1. make 时报错
问题和现象: 
LINK wrk
obj/ssl.o: In function `ssl_init':
ssl.c:(.text+0x9): undefined reference to `OPENSSL_init_ssl'
ssl.c:(.text+0x12): undefined reference to `OPENSSL_init_ssl'
ssl.c:(.text+0x1e): undefined reference to `OPENSSL_init_crypto'
ssl.c:(.text+0x48): undefined reference to `TLS_client_method'
collect2: error: ld returned 1 exit status
make: *** [wrk] Error 1
[chenchen@grpc01 wrk2]$ 
分析和解决:
分析, 看报错是 OpenSSL 的问题, 重新在新位置安装了 OpenSSL3.x, 具体细节在 OpenSSL.md 中.
安装过程顺利, ld, ldconfig 均符合逻辑未见异常.
构建是依然无法通过, 于是先去安装了一遍 wrk, 在安装 wrk 时发现, INSTALL 中有对 OpenSSL 自定义位置的配置. 并在 Makefile 文件中得到证实. 
回过头来再看 wrk2 的 Makefile 文件, 明显是作者去掉了自定义 OpenSSL 的配置能力, 并且可以看出作者是用 Mac 系统开发. 
于是备份 wrk2 的 Makefile 文件, 并修改, 在合适的位置增加以下两行配置, 作用是指明自定义安装的 OpenSSL 的位置.
#
CFLAGS  += -I/usr/local/openssl-3.0.4/include
LDFLAGS += -L/usr/local/openssl-3.0.4/lib
#
重新 make clean, make, 构建成功. 

```





## See Also

Git: https://github.com/wg/wrk

Git: https://github.com/giltene/wrk2

https://blog.csdn.net/ccccsy99/article/details/105958366











