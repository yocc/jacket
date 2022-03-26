# Composer

---

[toc]



## Overview

```shell
[chenchen@dev3_10.211.21.18 model]$ composer --help
Usage:
  help [options] [--] [<command_name>]

Arguments:
  command                        The command to execute
  command_name                   The command name [default: "help"]

Options:
      --xml                      To output help as XML
      --format=FORMAT            The output format (txt, xml, json, or md) [default: "txt"]
      --raw                      To output raw command help
  -h, --help                     Display this help message
  -q, --quiet                    Do not output any message
  -V, --version                  Display this application version
      --ansi                     Force ANSI output
      --no-ansi                  Disable ANSI output
  -n, --no-interaction           Do not ask any interactive question
      --profile                  Display timing and memory usage information
      --no-plugins               Whether to disable plugins.
  -d, --working-dir=WORKING-DIR  If specified, use the given directory as working directory.
  -v|vv|vvv, --verbose           Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Help:
  The help command displays help for a given command:
  
    php /usr/local/bin/composer help list
  
  You can also output the help in other formats by using the --format option:
  
    php /usr/local/bin/composer help --format=xml list
  
  To display the list of available commands, please use the list command.
[chenchen@dev3_10.211.21.18 model]$ vim composer.json 
[chenchen@dev3_10.211.21.18 model]$ composer require sina_sports/phplib -vv
Reading composer.json of sina_sports/phplib (1.0.0)
Importing tag 1.0.0 (1.0.0.0)
Reading composer.json of sina_sports/phplib (1.1.0)
Importing tag 1.1.0 (1.1.0.0)
Reading composer.json of sina_sports/phplib (1.1.1)
Importing tag 1.1.1 (1.1.1.0)
Reading composer.json of sina_sports/phplib (1.2.0)
Importing tag 1.2.0 (1.2.0.0)
Reading composer.json of sina_sports/phplib (1.3.0)
Importing tag 1.3.0 (1.3.0.0)
Reading composer.json of sina_sports/phplib (2.0.0)
Importing tag 2.0.0 (2.0.0.0)
Reading composer.json of sina_sports/phplib (2.0.1)
Importing tag 2.0.1 (2.0.1.0)
Reading composer.json of sina_sports/phplib (2.0.2)
Importing tag 2.0.2 (2.0.2.0)
Reading composer.json of sina_sports/phplib (2.1.0)
Importing tag 2.1.0 (2.1.0.0)
Reading composer.json of sina_sports/phplib (2.1.1)
Importing tag 2.1.1 (2.1.1.0)
Reading composer.json of sina_sports/phplib (master)
Importing branch master (dev-master)
Warning from https://mirrors.aliyun.com/composer: Support for Composer 1 is deprecated and some packages will not be available. You should upgrade to Composer 2. See https://blog.packagist.com/deprecating-composer-1-support/
Using version ^2.1 for sina_sports/phplib
./composer.json has been updated
Loading composer repositories with package information
Reading composer.json of sina_sports/phplib (1.0.0)
Importing tag 1.0.0 (1.0.0.0)
Reading composer.json of sina_sports/phplib (1.1.0)
Importing tag 1.1.0 (1.1.0.0)
Reading composer.json of sina_sports/phplib (1.1.1)
Importing tag 1.1.1 (1.1.1.0)
Reading composer.json of sina_sports/phplib (1.2.0)
Importing tag 1.2.0 (1.2.0.0)
Reading composer.json of sina_sports/phplib (1.3.0)
Importing tag 1.3.0 (1.3.0.0)
Reading composer.json of sina_sports/phplib (2.0.0)
Importing tag 2.0.0 (2.0.0.0)
Reading composer.json of sina_sports/phplib (2.0.1)
Importing tag 2.0.1 (2.0.1.0)
Reading composer.json of sina_sports/phplib (2.0.2)
Importing tag 2.0.2 (2.0.2.0)
Reading composer.json of sina_sports/phplib (2.1.0)
Importing tag 2.1.0 (2.1.0.0)
Reading composer.json of sina_sports/phplib (2.1.1)
Importing tag 2.1.1 (2.1.1.0)
Reading composer.json of sina_sports/phplib (master)
Importing branch master (dev-master)
Warning from https://mirrors.aliyun.com/composer: Support for Composer 1 is deprecated and some packages will not be available. You should upgrade to Composer 2. See https://blog.packagist.com/deprecating-composer-1-support/
Updating dependencies (including require-dev)
Dependency resolution completed in 0.001 seconds
Analyzed 153 packages to resolve dependencies
Analyzed 115 rules to resolve dependencies
Package operations: 1 install, 0 updates, 0 removals
Installs: sina_sports/phplib:2.1.1
  - Installing sina_sports/phplib (2.1.1): Cloning 22f0bc7917004e0aae4e64fe692c5e51673919a6
    REASON: Required by the root package: Install command rule (install sina_sports/phplib 2.1.0|install sina_sports/phplib 2.1.1)

Writing lock file
Generating autoload files
[chenchen@dev3_10.211.21.18 model]$ 
```



## Download Composer

```shell
# 1. 在线下载安装, 需要事先安装了 PHP, 因为 Composer 安装器本身就是一个 PHP 脚本
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
# 此安装程序脚本将简单地检查一些 php.ini 设置, 如果设置不正确, 则会警告您, 然后在当前目录中下载最新的 composer.phar

# 对以上 4 行代码的解释:
. Download the installer to the current directory
. Verify the installer SHA-384, which you can also cross-check here
. Run the installer
. Remove the installer

sudo mv composer.phar /usr/local/bin/composer				// 改名字, 便于使用. 二进制本体

# 总结:
. composer-setup.php: 安装器, PHP 脚本(批处理脚本)
. 运行 “安装器” 之后, 得到 二进制 composer.phar === composer 本体
```



## See also

https://getcomposer.org/download/