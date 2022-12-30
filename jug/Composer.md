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
https://getcomposer.org/download/
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



## 要点

```shell
1. 项目 / VCS 存储库都要用到 composer.json, 通常在最顶层目录中
2. 如果要将包发布到 Packagist.org, composer.json 必须能够在您的 VCS 存储库顶部找到该文件.
3. 包名 = 供应商名/项目名
4. 添加 autoload 字段后，您必须重新运行此命令: php composer.phar dump-autoload. 此命令将重新生成vendor/autoload.php文件
5. 只要目录中有一个composer.json，该目录就是一个包
6. 项目和库之间的唯一区别是您的项目是一个没有名称的包. 包名必须小写，约定用破折号分隔单词
7. 存储库仅对根包可用，并且不会加载依赖项中定义的存储库。如果您想了解原因，请阅读 常见问题条目。
8. 在解决依赖关系时，包从上到下从存储库中查找，默认情况下，一旦在一个存储库中找到包，Composer 就会停止在其他存储库中查找。阅读 存储库优先级文章以获取更多详细信息并了解如何更改此行为。
9. 默认情况下，在 Composer 2.x 中，所有存储库都是规范的。Composer 1.x 将所有存储库视为非规范的。另一个默认设置是 packagist.org 存储库总是隐式添加为最后一个存储库，除非您禁用它。
10. 唯一的要求是为 git 客户端安装 SSH 密钥。也就是说用自己的仓库, 也只能用 ssh 方式, 不能 https
11. 要让 Composer 选择使用哪个驱动程序，需要将存储库类型定义为“vcs”



https://getcomposer.org/doc/articles/repository-priorities.md 明天尝试使存储库非规范
```





## composer

```shell
[chenchen@dev3_10.211.21.18 test.composer.sports.weibo.com]$ composer
   ______
  / ____/___  ____ ___  ____  ____  ________  _____
 / /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
/ /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
\____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                    /_/
Composer version 2.2.18 2022-08-20 11:33:38

Usage:
  command [options] [arguments]

Options:
  -h, --help                     Display this help message
  -q, --quiet                    Do not output any message
  -V, --version                  Display this application version
      --ansi                     Force ANSI output
      --no-ansi                  Disable ANSI output
  -n, --no-interaction           Do not ask any interactive question
      --profile                  Display timing and memory usage information
      --no-plugins               Whether to disable plugins.
      --no-scripts               Skips the execution of all scripts defined in composer.json file.
  -d, --working-dir=WORKING-DIR  If specified, use the given directory as working directory.
      --no-cache                 Prevent use of the cache
  -v|vv|vvv, --verbose           Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Available commands:
  about                Shows a short information about Composer.
  archive              Creates an archive of this composer package.
  browse               Opens the package's repository URL or homepage in your browser.
  cc                   Clears composer's internal package cache.
  check-platform-reqs  Check that platform requirements are satisfied.
  clear-cache          Clears composer's internal package cache.
  clearcache           Clears composer's internal package cache.
  config               Sets config options.
  create-project       Creates new project from a package into given directory.
  depends              Shows which packages cause the given package to be installed.
  diagnose             Diagnoses the system to identify common errors.
  dump-autoload        Dumps the autoloader.
  dumpautoload         Dumps the autoloader.
  exec                 Executes a vendored binary/script.
  fund                 Discover how to help fund the maintenance of your dependencies.
  global               Allows running commands in the global composer dir ($COMPOSER_HOME).
  help                 Displays help for a command
  home                 Opens the package's repository URL or homepage in your browser.
  i                    Installs the project dependencies from the composer.lock file if present, or falls back on the composer.json.
  info                 Shows information about packages.
  init                 Creates a basic composer.json file in current directory.
  install              Installs the project dependencies from the composer.lock file if present, or falls back on the composer.json.
  licenses             Shows information about licenses of dependencies.
  list                 Lists commands
  outdated             Shows a list of installed packages that have updates available, including their latest version.
  prohibits            Shows which packages prevent the given package from being installed.
  reinstall            Uninstalls and reinstalls the given package names
  remove               Removes a package from the require or require-dev.
  require              Adds required packages to your composer.json and installs them.
  run                  Runs the scripts defined in composer.json.
  run-script           Runs the scripts defined in composer.json.
  search               Searches for packages.
  self-update          Updates composer.phar to the latest version.
  selfupdate           Updates composer.phar to the latest version.
  show                 Shows information about packages.
  status               Shows a list of locally modified packages.
  suggests             Shows package suggestions.
  u                    Upgrades your dependencies to the latest version according to composer.json, and updates the composer.lock file.
  update               Upgrades your dependencies to the latest version according to composer.json, and updates the composer.lock file.
  upgrade              Upgrades your dependencies to the latest version according to composer.json, and updates the composer.lock file.
  validate             Validates a composer.json and composer.lock.
  why                  Shows which packages cause the given package to be installed.
  why-not              Shows which packages prevent the given package from being installed.
```



## composer.json 和 composer.lock

```shell
1) 调用 composer install 时会根据 composer.json 文件来创建 composer.lock 文件
2) composer 会把安装时的确切的版本号列表写入 composer.lock 文件中
3) composer.lock 文件的作用时让所有开发者的开发环境保持统一, 因此, 我们应该提交应用程序的 composer.lock 到版本库中
4) 如果对 composer.json 进行了修改, 那么就要用 composer update 来更新以来和 composer.lock 文件

总结:
1) 当 composer.lock 不存在时, composer install 将根据 composer.json 文件安装依赖, 并创建 composer.lock 文件
2) 当 composer.lock 文件存在时, composer install 将直接根据 composer.lock 文件拉取依赖
3) 使用 composer update 更新依赖与 composer.lock 文件
4) 升级 = 文档研读 + 代码修改 + 全面测试(非常重要)
```



## See also

https://getcomposer.org/download/

https://www.phpcomposer.com/