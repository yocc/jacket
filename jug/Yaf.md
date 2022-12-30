# Yaf

----

[toc]



## Overviews

Yaf

## Structure

### 执行顺序

1. 定义常量, 确定 DocumentRoot 目录
2. 实例化 Yaf\Application
3. 调用 Bootstrap
4. 加载路由
5. 调用 run

### 项目代码生成

 ```shell
 # 代码生成器
 https://github.com/laruence/php-yaf/tree/master/tools/cg
 ```

> 代码生成器生成的目录结构和下面的结构不同, 主要在 public 目录的有无上. 

### 项目目录结构

```shell
# 目录结构

+ public
  |- index.php										// 入口文件
  |- .htaccess										// 重写规则
  |+ css
  |+ img
  |+ js
+ conf
  |- application.ini							// 配置文件
+ application
	|- Bootstrap.php							  // 启动文件
  |+ controllers
     |- Index.php									// 默认控制器
  |+ views    
     |+ index   									// 控制器
        |- index.phtml 						// 默认视图
  |+ modules 											// 其他模块
  |+ library 											// 本地类库
  |+ models  											// model目录
  |+ plugins 											// 插件目录
```

> 💡以上代码生成器和目录结构可以自己重构, 以符合自己的认知和需要.

### index.php

```shell
define("APP_PATH",  realpath(dirname(__FILE__) . '/../')); /* 指向public的上一级 */
$app  = new Yaf_Application(APP_PATH . "/conf/application.ini");
$app->run();


<?php
/**
 * @Copyright (c) 2022, SINA.com.
 * All Rights Reserved.
 *
 * @name        index 
 * @desc        入口文件
 *
 * @author      chenchen <chenchen@staff.sina.com>
 * @version     $Id$
 */



// 线上要抑制所有错误, 应与环境相匹配
error_reporting(E_ALL & ~E_NOTICE &~E_WARNING);

/* 定义这个常量是为了在application.ini中引用 */
define("APP_PATH",  realpath(dirname(__FILE__) . '/../'));		/* 指向 public 的上一级 */

$app = new \Yaf\Application(APP_PATH . "/conf/application.ini");

$app->bootstrap()->run();
```

### conf/application.ini

```ini
[common]
application.directory = APP_PATH "/app"
application.dispatcher.catchException = TRUE
app.modules = Index

[develop : common]
; 继承

[product : common]

```





## Yaf 常量

https://www.laruence.com/manual/yaf.constant.html



## 配置

### php.ini

```php
# php.ini 位置, sinasrv5
/usr/local/sinasrv5/lib/php.ini

[yaf]
yaf.environ = "develop"
yaf.use_namespace = On 
yaf.use_spl_autoload = On

默认: "product", 此处的名字会在 application.ini 文件中成为小节名字
开启命名空间: yaf.use_namespace = 1
PHP层次的全局库: yaf.library
yaf.use_spl_autoload = 0, 建议关闭
```



### vhosts.conf

```nginx
server {
  listen 80;
  server_name  domain.com;
  root   document_root;
  index  index.php index.html index.htm;

  if (!-e $request_filename) {
    rewrite ^/(.*)  /index.php/$1 last;
  }
}
```

```nginx
# tengine 配置文件位置
/usr/local/sinasrv5/etc/vhost.d/chenchen.conf

################################################
# yocc.sports.weibo.cn:443
################################################
server {
    listen 443 ssl;

    # ssl_certificate      ssl/server.crt;
    # ssl_certificate_key  ssl/server.key;
    # ssl_certificate      ssl/yocc2022102402.crt;
    # ssl_certificate_key  ssl/yocc2022102402.key;
		
    ssl_certificate      ssl/yocc2022102403.crt;
    ssl_certificate_key  ssl/yocc2022102403.key;

    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;

    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers  on;

    root /data1/www/htdocs/chenchen/yocc.sports.weibo.cn/public;

    server_name yocc.sports.weibo.cn admin.yocc.sports.weibo.cn chenchen.yocc.sports.weibo.cn admin.chenchen.yocc.sports.weibo.cn chenchen.yocc.sports.weibo.cn test.yocc.sports.weibo.cn;

    index  index.php index.html index.htm;

    access_log  /data1/www/logs/chenchen/yocc.sports.weibo.cn-access_log;

    error_log   /data1/www/logs/chenchen/yocc.sports.weibo.cn-error_log;

    location ~ (system|include|config|conf|templates|template|\.svn|\.git)/ {
        deny all;
    }

    location ~ \.(inc|tpl|sql|ini|bin|sh|log|bak|old)$ {
        deny all;
    }

    location ~ \.(ico|gif|png|jpeg|css|js|xml|html|shtml|swf|mp3)$ {
        expires 1d;
        if ($uri ~ ^/favicon\.ico$) {
            expires 30d;
        }
        if ($uri ~ index\.(html|shtml)$) {
            expires 600;
        }
    }

    location ~ ^/data/(.*) {
        alias /data1/www/data/chenchen/yocc.sports.weibo.cn/$1;
    }

    location ~ ^/cache/(.*) {
        alias /data1/www/cache/chenchen/yocc.sports.weibo.cn/$1;
    }

    location ~ \.php {
        fastcgi_pass unix:/usr/local/sinasrv5/var/yocc.sports.weibo.cn.sock;
    }

    # try_files $uri $uri/ /index.php$request_uri;

    if (!-e $request_filename) {
        rewrite ^/(.*)  /index.php/$1 last;
    }
}
```



### php-fpm.conf

```ini
# php-fpm 配置文件位置
/usr/local/sinasrv5/etc/php-fpm.d/chenchen.conf

[yocc.sports.weibo.cn]
user = www
group = www
listen = /usr/local/sinasrv5/var/yocc.sports.weibo.cn.sock
listen.allowed_clients = 127.0.0.1
listen.owner = www
pm = dynamic
pm.max_children = 50
pm.start_servers = 3     
pm.min_spare_servers = 3 
pm.max_spare_servers = 10
pm.max_requests = 5000   
pm.status_path = /status 
request_slowlog_timeout = 5
request_terminate_timeout = 30
catch_workers_output = no
slowlog = /data1/www/logs/chenchen/$pool-slow_log
security.limit_extensions = .php .php3 .php4 .php5 .html .xml .ico
php_admin_value[include_path] = .:/usr/local/sinasrv5/lib/php
    
env[SINASRV_ZONE_ID] = "020901"
env[SINASRV_ZONE_IDC] = "BX1"
env[SINASRV_ZONE_ISP] = "CNC"
    
env[SINASRV_ROLE] = "dev5"
env[SINASRV_INTIP] = "i.dev.sports.sina.com.cn"
env[SINASRV_OUTIP] = "i.dev.sports.sina.com.cn"
    
env[SINASRV_MEMCACHED_HOST] = "10.211.21.18"
env[SINASRV_MEMCACHED_PORT] = "7600"

env[SINASRV_DATA_DIR] = "/data1/www/data/chenchen/yocc.sports.weibo.cn/"
env[SINASRV_CACHE_DIR] = "/data1/www/cache/chenchen/yocc.sports.weibo.cn/"
env[SINASRV_PRIVDATA_DIR] = "/data1/www/privdata/chenchen/yocc.sports.weibo.cn/"
env[SINASRV_APPLOGS_DIR] = "/data1/www/applogs/chenchen/yocc.sports.weibo.cn/"

env[SINASRV_CACHE_URL] = "http://yocc.sports.weibo.cn/cache"
env[SINASRV_DATA_URL] = "http://yocc.sports.weibo.cn/data"

env[SINASRV_DB_HOST] = "10.211.21.19"
env[SINASRV_DB_PORT] = "3307"
env[SINASRV_DB_NAME] = "utils"
env[SINASRV_DB_USER] = "root"
env[SINASRV_DB_PASS] = "root007"
env[SINASRV_DB_HOST_R] = "10.211.21.19"
env[SINASRV_DB_PORT_R] = "3307"
env[SINASRV_DB_NAME_R] = "utils"
env[SINASRV_DB_USER_R] = "root"
env[SINASRV_DB_PASS_R] = "root007"

env[SINASRV_REDIS_HOST] = "10.211.21.19"
env[SINASRV_REDIS_PORT] = "6380"
env[SINASRV_REDIS_HOST_R] = "10.211.21.18"
env[SINASRV_REDIS_PORT_R] = "6380"

env[SINASRV_MEMCACHED_SERVERS] = "10.211.21.18:7600"
env[SINASRV_MEMCACHED_KEY_PREFIX] = "yocc_sports_weibo_cn-"
```



### conf/application.ini

```ini
[common]
application.directory = APP_PATH "/app"
application.dispatcher.catchException = TRUE
app.modules = Index

[develop : common]
; 继承

[product : common]

```



### application/Bootstrap.php

```php
<?php
/**
 * @Copyright (c) 2022, SINA.com.
 * All Rights Reserved.
 *
 * @name        Bootstrap
 * @desc        所有在Bootstrap类中, 以_init开头的方法, 都会被Yaf调用,
 * @see         http://www.php.net/manual/en/class.yaf-bootstrap-abstract.php
 *              这些方法, 都接受一个参数:\Yaf\Dispatcher $dispatcher
 *              调用的次序, 和申明的次序相同
 *
 * @author      chenchen <chenchen@staff.sina.com>
 * @version     $Id$
 */






/**
 * Bootstrap 
 *
 */
class Bootstrap extends \Yaf\Bootstrap_Abstract
{   

    public function _initConfig()
    {
        // 把配置保存起来
    }

    public function _initPSR4()
    {
        // 把配置保存起来
        \Yaf\Loader::getInstance()->registerNamespace("\Pugr", APP_PATH . "/application/library");
        // \Yaf\Loader::getInstance()->registerNamespace("\Qatar\TrimSportsWeiboCn", APP_PATH . "/application/model");
    }

    public function _initPlugin(\Yaf\Dispatcher $dispatcher)
    {
        // 注册一个插件
    }

    public function _initRoute(\Yaf\Dispatcher $dispatcher)
    {
        // 在这里注册自己的路由协议,默认使用简单路由
        $dispatcher->getRouter()->addRoute("daggerLike", new DaggerLikeRouter());
    }

    public function _initView(\Yaf\Dispatcher $dispatcher)
    {
        // 在这里注册自己的view控制器, 例如 smarty, firekylin
    }
}
```





## Composer





## Dagger





## FAQ





## See Also

https://github.com/laruence/yaf

https://www.laruence.com/manual/tutorial.last.html



