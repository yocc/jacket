# Access-Control-Allow-Origin

----

[toc]









## Overview

### Reason

什么是跨域？

跨域: 指的是<u>浏览器</u>不能执行其他网站的脚本. 它是由<u>浏览器</u>的同源策略造成的, 是<u>浏览器</u>对 javascript 施加的安全限制.

例如: a页面想获取b页面资源, 如果a、b页面的协议、域名、端口、子域名不同, 所进行的访问行动都是跨域的, 而浏览器为了安全问题一般都限制了跨域访问, 也就是不允许跨域请求资源. 

> 注意: 跨域限制访问, 其实是浏览器的限制. 理解这一点很重要！！！
>
> 在浏览器调试信息中可以到, 接口还是被请求回来了, 数据也到本地了, 当后面浏览器又在本地阻止了.



特别说明:

非同源的请求，会额外产生一次 "预检"的请求 请求方法 ： OPTIONS

```sh
OPTIONS /cors HTTP/1.1
Origin: http://api.bob.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: X-Custom-Header
Host: api.alice.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```

"预检"请求用的请求方法是OPTIONS，表示这个请求是用来询问的。头信息里面，关键字段是Origin，表示请求来自哪个源。

除了Origin字段，"预检"请求的头信息包括两个特殊字段。

(1)Access-Control-Request-Method

该字段是必须的，用来列出浏览器的CORS请求会用到哪些HTTP方法，上例是PUT。

(2)Access-Control-Request-Headers

该字段是一个逗号分隔的字符串，指定浏览器CORS请求会额外发送的头信息字段，上例是X-Custom-Header。

服务器收到"预检"请求以后，检查了Origin、Access-Control-Request-Method和Access-Control-Request-Headers字段以后，确认允许跨源请求，就可以做出回应。

这个环节，由浏览器控制，用户无感知。

### Troubleshooting

PHP 和 Nginx 二选一.

#### PHP

```php
header('Access-Control-Allow-Origin:*'); // 设为星号，表示同意任意跨源请求。也可配置特定的域名可访问 如: https://www.xxxx.com
header('Access-Control-Allow-Methods:OPTIONS,POST,GET'); // 允许请求的类型
header('Access-Control-Allow-Headers:Content-Type,Content-Length,Accept-Encoding,X-Requested-with, Origin');
header('Access-Control-Expose-Headers:Content-Length,Content-Range');
```

#### Nginx

```nginx
################################################
# cassi.sports.weibo.cn 
################################################
server {
    listen 80;

    root /data1/www/htdocs/sportsdev/cassi.sports.weibo.cn/public;

    server_name cassi.sports.weibo.cn i.cassi.sports.weibo.cn admin.cassi.sports.weibo.cn dev-cassi.sports.weibo.cn test-cassi.sports.weibo.cn;

    access_log  off;
    error_log   /data1/www/logs/sportsdev/cassi.sports.weibo.cn-error_log;

    add_header 'Access-Control-Allow-Credentials' 'true';
    
    location ~ (system|include|config|conf|templates|template|\.svn|\.git)/ {
        deny all;
    }   
    
    location ~ \.(inc|tpl|sql|ini|bin|sh|log|bak|old)$ {
        deny all;
    }   
    
    location ~ \.(ico|gif|png|jpeg|css|js|xml|html|shtml|swf|mp3)$ {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Credentials' 'true';
        add_header 'Access-Control-Allow-Methods' '*';
        add_header 'Access-Control-Allow-Headers' '*';
        add_header 'Access-Control-Expose-Headers' '*';
        
        expires 1d;
        if ($uri ~ ^/favicon\.ico$) {
            expires 30d;
        }   
        if ($uri ~ index\.(html|shtml)$) {
            expires 600;
        }   
    }   
    
    location ~ \.php {
        fastcgi_pass unix:/usr/local/sinasrv5/var/sportsdev.common.sock;
    }   
    
    try_files $uri $uri/ /index.php$request_uri;
}
```

```nginx
add_header 'Access-Control-Allow-Origin' 'http://pub.admin.weibo.com';
add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
add_header 'Access-Control-Allow-Headers' 'Content-Type,Content-Length,Accept-Encoding,X-Requested-with, Origin,Range';
add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
```



## Tips

根据 chrome 浏览器 调试信息, 注意修改问题即可. 提示相对准确.

上行提交不存在跨域问题, 因为并无资源返回.

JSONP可以下行跨域.



## FAQ







## See Also

https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy 浏览器的同源策略

https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS 跨源资源共享（CORS）





