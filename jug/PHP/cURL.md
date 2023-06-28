# cURL

[toc]



## Overview

说在前面, 有关 PHP curl_multi 的一些内容和常量的详细说明和定义需要查阅系统调用和 cURL(libcurl) 工具. 

```php
$active = null;
do { 
    $mrc = curl_multi_exec($mh, $active); 
} while ($mrc == CURLM_CALL_MULTI_PERFORM);

while ($active and $mrc == CURLM_OK) {
    if (curl_multi_select($mh) != -1) { 
        do { 
            $mrc = curl_multi_exec($mh, $active); 
        } while ($mrc == CURLM_CALL_MULTI_PERFORM); 
    } 
}	
```

```php
1. PHP 的 curl_multi_exec($mh, $active), 相当于 cURL 的 curl_multi_perform, 参见2
PHP:
curl_multi_exec(CurlMultiHandle $multi_handle, int &$still_running): int
执行(读写)批量句柄中的每一个句柄. 该函数是非阻塞.
也就是异步批量执行, 并且返回-1就规定要再次执行.
This is not really an error. It means you should call curl_multi_perform again without doing select() or similar in between. 
不是0, 都是异常, 但-1文档里说这不是一个真正的错误, 而是告诉你应该继续重复调用此函数.
$mrc == -1, 表示执行异常, 但不是真正的异常. 等于0, 表示执行正常. 等于1,2,3,4,5, 表示异常
也就是, 当-1时重复执行, 当其他时, 向下执行.
  
参数: 
mh, cURL 多个句柄, 批量句柄
sr, 还在执行的 cURL 句柄的数量的值的引用
返回:
cURL(libcurl) 的 Curlmcode (libcurl error codes) 
参见: https://curl.se/libcurl/c/libcurl-errors.html#CURLM_CALL_MULTI_PERFORM
CURLM_CALL_MULTI_PERFORM （-1） 这不是一个真正的错误。这意味着您应该再次调用 curl_multi_perform 而不执行 select（） 或介于两者之间的类似操作。在版本 7.20.0（2010 年 2 月 9 日发布）之前，curl_multi_perform可以返回此代码，但在更高版本中，从不使用此返回代码。
CURLM_OK (0) 一切正常

cURL:
curl_multi_perform - reads/writes available data from each easy handle. 从每个简易句柄读取/写入可用数据
#include <curl/curl.h>
CURLMcode curl_multi_perform(CURLM *multi_handle, int *running_handles);
. 此函数以非阻塞方式对所有需要注意的添加句柄执行传输. 简易手柄之前已添加到带有 curl_multi_add_handle 的多手柄中. 
. 当应用程序发现有可用于 multi_handle 的数据或超时已过时, 应用程序应调用此函数来读取/写入现在要读取或写入的任何内容等. curl_multi_perform 在读取/写入完成后立即返回. 此函数不需要实际有任何数据可供读取或可以写入数据, 可以调用它以防万一. 它将在第二个参数的整数指针中存储仍传输数据的句柄数. 
. 如果 running_handles 量与上一次调用相比发生了变化（或小于添加到多句柄的简易句柄的数量）, 则您知道有一个或多个传输的“运行”较少. 然后, 您可以调用 curl_multi_info_read 以获取有关每个已完成传输的信息, 返回的信息包括 CURLcode 等. 如果添加的句柄快速失败, 则可能永远不会计为 running_handle. 在这种情况下, 您可以使用 curl_multi_info_read 来跟踪添加的句柄的实际状态.
. 当 running_handles 在此函数返回时设置为零(0)时, 不再有任何正在进行的传输.
. 当此函数返回错误时, 所有传输的状态都是不确定的, 并且无法继续. 返回错误后, 不应在同一多句柄上再次调用curl_multi_perform, 除非先删除所有句柄并添加新句柄.

2. curl_multi_select(resource $mh, float $timeout = 1.0): int
习惯写诸如 connect、accept、recv 或 recvfrom 这样的阻塞程序（所谓阻塞方式block，顾名思义，就是进程或是线程执行到这些函数时必须等待某个事件的发生，如果事件没有发生，进程或线程就被阻塞，函数不能立即返回）。
使用Select就可以完成非阻塞（所谓非阻塞方式non-block，就是进程或线程执行此函数时不必非要等待事件的发生，一旦执行肯定返回，以返回值的不同来反映函数的执行情况，如果事件发生则与阻塞方式相同，若事件没有发生则返回一个代码来告知事件未发生，而进程或线程继续执行，所以效率较高）方式工作的程序，它能够监视我们需要监视的文件描述符的变化情况——读写或是异常。

PHP:
参数: 
mh, 多句柄
timeout, 秒
返回:
成功时返回描述符集合中描述符的数量。如果没有任何活动的描述符，则为 0。失败时，此函数将在 select 失败时返回 -1（来自底层 select 系统调用）。

系统:
select函数是IO多路复用的函数，它主要的功能是用来等文件描述符中的事件是否就绪，select可以使我们在同时等待多个文件缓冲区 ，减少IO等待的时间，能够提高进程的IO效率。
select()函数允许程序监视多个文件描述符，等待所监视的一个或者多个文件描述符变为“准备好”的状态。所谓的”准备好“状态是指：文件描述符不再是阻塞状态，可以用于某类IO操作了，包括可读，可写，发生异常三种

返回值:
如果没有文件描述符就绪就返回0；
如果调用失败返回-1；
如果timeout中中readfds中有事件发生，则返回timeout剩下的时间。
```

```php
cURL, Curlmcode, libcurl error codes
https://curl.se/libcurl/c/libcurl-errors.html#CURLM_CALL_MULTI_PERFORM

PHP constants:
https://www.php.net/manual/zh/curl.constants.php

以下是 PHP 预定义常量 与 cURL 交集部分的说明:
. CURLM_CALL_MULTI_PERFORM（-1）表示应该再次调用 curl_multi_perform 即, PHP 的 curl_multi_exec()
. CURLM_OK (0) 一切正常
. CURLM_BAD_HANDLE (1) 传入的句柄不是有效的 CURLM 句柄。
. CURLM_BAD_EASY_HANDLE (2) 简单句柄不好/无效。这可能意味着它根本不是一个简单的句柄，或者可能这个或另一个多句柄已经使用了该句柄。
. CURLM_OUT_OF_MEMORY (3) 内存溢出
. CURLM_INTERNAL_ERROR (4) libcrul内部错误, 反馈给官方
```

PHP是单进程同步模型，一个请求对应一个进程，I/O是同步阻塞的。

```php
// 简单demo，默认支持为GET请求
public function multiRequest($urls) {
    $mh = curl_multi_init();
    $urlHandlers = [];
    $urlData = [];
    // 初始化多个请求句柄为一个
    foreach($urls as $value) {
        $ch = curl_init();
        $url = $value['url'];
        $url .= strpos($url, '?') ? '&' : '?';
        $params = $value['params'];
        $url .= is_array($params) ? http_build_query($params) : $params;
        curl_setopt($ch, CURLOPT_URL, $url);
        // 设置数据通过字符串返回，而不是直接输出
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
        $urlHandlers[] = $ch;
        curl_multi_add_handle($mh, $ch);
    }
    $active = null;
    // 检测操作的初始状态是否OK，CURLM_CALL_MULTI_PERFORM为常量值-1
    do {
        // 返回的$active是活跃连接的数量，$mrc是返回值，正常为0，异常为-1
        $mrc = curl_multi_exec($mh, $active);
    } while ($mrc == CURLM_CALL_MULTI_PERFORM);
    // 如果还有活动的请求，同时操作状态OK，CURLM_OK为常量值0
    while ($active && $mrc == CURLM_OK) {
        // 持续查询状态并不利于处理任务，每50ms检查一次，此时释放CPU，降低机器负载
        usleep(50000);
        // 如果批处理句柄OK，重复检查操作状态直至OK。select返回值异常时为-1，正常为1（因为只有1个批处理句柄）
        if (curl_multi_select($mh) != -1) {
            do {
                $mrc = curl_multi_exec($mh, $active);
            } while ($mrc == CURLM_CALL_MULTI_PERFORM);
        }
    }
    // 获取返回结果
    foreach($urlHandlers as $index => $ch) {
        $urlData[$index] = curl_multi_getcontent($ch);
        // 移除单个curl句柄
        curl_multi_remove_handle($mh, $ch);
    }
    curl_multi_close($mh);
    return $urlData;
}
```

```php
1.
$mrc = curl_multi_exec($mh, $active);
有三重功能: 
. 第一个是发起批量执行请求 $mh
. 第二个是每次请求都返回当时批量请求的活跃连接的数量 $active
. 第三个是每次请求都返回本次调用本函数执行后的返回状态 $mrc
  状态用预定义常量(PHP)表示: 
		执行正常, 但未必结束, CURLM_OK, 0
		执行异常, 但不是真异常, 仅仅是让重复执行, CURLM_CALL_MULTI_PERFORM, -1
    常见以上两种, 另外还有更多, 参考, https://curl.se/libcurl/c/libcurl-errors.html#CURLMOK
	也就是调用 exec 之后, 系统就开始异步非阻塞去执行了, 如果能正确执行就向下执行. 异常的话应该抛出.

2. 
while ($active && $mrc == CURLM_OK) {
. $active 倒计数, 不是0即为true; 同时 CURLM_OK 表示 exec 执行正确. 和一起就是还有子句柄正在正确执行中.

3. 
if (curl_multi_select($mh) != -1) {
. select() 是阻塞等待
. 由于前面 exec 是非阻塞的, 需要不停的询问是否执行完毕, 这样空转会无谓的消耗cpu, 为此, 主动调用阻塞来等待响应.
. 返回的是活动的描述符的数量(有几个字句柄执行完成返回了), 0是没有活动描述符(即没有执行完的子句柄返回), -1 是 select()失败
. 也就是阻塞等待, 一旦有子句柄执行完成(放入了活动描述符队列里)就会继续向下执行.

4. 
$mrc = curl_multi_exec($mh, $active);
. 此时是问一下异步处理, 是否还有子句柄没执行完. 如果没有了, 即 $active = 0, 那么结束执行(循环)
. 此时所有子句柄的返回结果都保存在各个子句柄中, $ch
. 下面是从子句柄中读取数据内容了

5. 
$urlData[$index] = curl_multi_getcontent($ch);
. 注意顺序同子句柄的顺序
. 这种是 $mh 都执行完, 并且都返回完. 然后一次性处理结果. 还有一种 read() 方法, 是随返回随时就处理了, 而不是必须等所有子句柄都有结果再处理. curl_multi_info_read()
do {
    do {
        $mrc = curl_multi_exec($mh, $active);
    } while ($mrc == CURLM_CALL_MULTI_PERFORM);

    while ($done = curl_multi_info_read($mh)) {
        $fileContent = curl_multi_getcontent($done['handle']);
        curl_multi_remove_handle($mh, $done['handle']);
        curl_close($done['handle']);
    }
} while ($active); 

6. 
curl_multi_remove_handle($mh, $ch);
. 从多句柄中移除子句柄

  
```

```php
$images = [
    'http://videoactivity.bookan.com.cn/1586506139026y8m9vjUIYzykwZzTUY940C05AA84-EA86-48DC-B6F8-B0E8B54B54B5.jpeg',
    'http://videoactivity.bookan.com.cn/15865061392582bXOjQQ9cuJp6VPAsLp49AD00515-9ED4-4B3C-9105-D6A9FE174DDD.jpeg',
    'http://videoactivity.bookan.com.cn/15865061393178TMXKkneQ3K7ECeBpGyd75803A83-F5A4-43BB-A1E8-38EE549441EA.jpeg',
    'http://videoactivity.bookan.com.cn/1586506139377tcqthlvk8NypRXEb6C1Y3758B584-C7A9-442F-A903-AF4800D12E8E.jpeg',
    'http://videoactivity.bookan.com.cn/1586506139456zYIUk8qqcOLHSpCrzlWD0577C380-3EC7-4BA3-9F49-01795B0DA989.jpeg',
    'http://videoactivity.bookan.com.cn/1586506139681VF6h3I8eS9NnC2B65dl4302E2840-4BB4-4CA7-AAAF-AF9EA22B43CA.jpeg',
    'http://videoactivity.bookan.com.cn/1586506140023a6yV2w8ViYnQX7Aj5rznEEB948FB-A3A9-43F3-B701-36F6EEAAB45E.jpeg',
    'http://videoactivity.bookan.com.cn/1586506140324CHSJrMAjzMyO2sfTizSwEEBBE191-B9A5-4203-8DB2-1825B18DCB3B.jpeg',
    'http://videoactivity.bookan.com.cn/1586506140650ll6gZo4dgKEUQb5N1Q1s2FCA6C40-5781-43D4-961F-C86D31361F68.jpeg',
];

function testCurl($images){
    $chArr   = [];
    $nameArr = [];
    $mh      = curl_multi_init();
    foreach ($images as $i => $v) {
        $chArr[$i] = curl_init();
        curl_setopt($chArr[$i], CURLOPT_URL, $v);
        curl_setopt($chArr[$i], CURLOPT_RETURNTRANSFER, 1);
        curl_setopt($chArr[$i], CURLOPT_HEADER, 0);
        // curl_setopt($chArr[$i], CURLOPT_TIMEOUT, 5);
        curl_multi_add_handle($mh, $chArr[$i]);

        $nameArr[intval($chArr[$i])] = basename($v);
    }

    $active = null;

    do {
        do {
            $mrc = curl_multi_exec($mh, $active);
        } while ($mrc == CURLM_CALL_MULTI_PERFORM);

        while ($done = curl_multi_info_read($mh)) {
            $fileContent = curl_multi_getcontent($done['handle']);
            $name        = $nameArr[intval($done['handle'])];
            file_put_contents("img/2/{$name}", $fileContent);

            curl_multi_remove_handle($mh, $done['handle']);
            curl_close($done['handle']);
        }

    } while ($active);

    curl_multi_close($mh);
}

testCurl($images);

```



## select, poll, epoll

https://www.php.cn/linux-493721.html linux中poll和select有什么区别

https://zhuanlan.zhihu.com/p/451817921 Linux内核poll/select机制简析

https://blog.csdn.net/uunubt/article/details/127304947 poll函数_Linux select/poll机制原理分析

https://blog.csdn.net/littlezls/article/details/117744825 嵌入式 Linux——Select机制 和poll机制

## FAQ



1、GET      /url/list           查看

2、POST    /url/create        创建

3、PUT      /url/update        更新

4、DELETE  /url/delete         删除

5、HEAD    /url/is_exists      检查资源

6、PATCH    /url/update_by_id  更新某些字段

7、OPTIONS  /url/get_methods  检查请求方式









## See Also

1. https://curl.se/libcurl/c/libcurl-errors.html#CURLMOK CURLM_CALL_MULTI_PERFORM

2. https://linux.die.net/man/3/curl_multi_perform curl_multi_perform(3) - Linux man page

3. https://www.php.net/manual/zh/function.curl-multi-select.php curl_multi_select
4. https://zhuanlan.zhihu.com/p/59461633?utm_id=0 PHP实现并发请求
5. https://zhuanlan.zhihu.com/p/624620835 
6. https://www.cnblogs.com/Anker/archive/2013/08/14/3258674.html [IO多路复用之select总结](https://www.cnblogs.com/Anker/p/3258674.html)
7. https://www.cnblogs.com/Anker/p/3265058.html [select、poll、epoll之间的区别总结[整理\]](https://www.cnblogs.com/Anker/p/3265058.html)
8. https://blog.csdn.net/raoxiaoya/article/details/106483607 通过系统调用分析curl_multi运行原理 PHP



