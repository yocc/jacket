# command

----

[toc]



## Overview

### DNS

```sh
[chenchen@pluto10 ~]$ cat /etc/resolv.conf
options timeout:1
nameserver 10.10.10.10
nameserver 10.10.10.11
---- or ----
[chenchen@pluto10 ~]$ dig |grep SERVER
;; SERVER: 10.10.10.10#53(10.10.10.10)
```

### Date

```sh
date -d @timestamp
date -d '2017-06-06T14:16:15+0000' +%s
```

### PHP

```sh
[chenchen@dev3_10.211.21.18 ex4]$ php -m
[PHP Modules]
bcmath
...



[chenchen@dev3_10.211.21.18 ex4]$ php --ri mongodb

mongodb

MongoDB support => enabled
MongoDB extension version => 1.3.3
MongoDB extension stability => stable
libbson bundled version => 1.8.2
libmongoc bundled version => 1.8.2
libmongoc SSL => enabled
libmongoc SSL library => OpenSSL
libmongoc crypto => enabled
libmongoc crypto library => libcrypto
libmongoc crypto system profile => disabled
libmongoc SASL => enabled

Directive => Local Value => Master Value
mongodb.debug => no value => no value
[chenchen@dev3_10.211.21.18 ex4]$ 
```

### PhpSpreadsheet

```php
PhpSpreadsheet: https://phpspreadsheet.readthedocs.io/en/latest/
PhpSpreadsheet API: https://phpoffice.github.io/PhpSpreadsheet/namespaces/phpoffice-phpspreadsheet.html

科学计数法, Scientific notation
https://phpoffice.github.io/PhpSpreadsheet/classes/PhpOffice-PhpSpreadsheet-Worksheet-Worksheet.html#method_toArray

导入时会将 excel 中的科学计数法转成错误的数字, 比如: 2.59E+07 => 25900000, 这是错误的. 
避免转换时自动应用格式化.

// $spreadsheet = \PhpOffice\PhpSpreadsheet\IOFactory::load("ingx03.xlsx");
// $spreadsheet = \PhpOffice\PhpSpreadsheet\IOFactory::load("ingx0302.xlsx");
$spreadsheet = \PhpOffice\PhpSpreadsheet\IOFactory::load($o['f']);
$arr = $spreadsheet->getActiveSheet()->toArray(null, true, false, false);
$spreadsheet->__destruct();

toArray(null, true, false, false);		// 第三个参数从 true(默认) 改为 false

Parameters
$nullValue : mixed = null
Value returned in the array entry if a cell doesn't exist

$calculateFormulas : bool = true
Should formulas be calculated?

$formatData : bool = true
Should formatting be applied to cell values?

$returnCellRef : bool = false
False - Return a simple array of rows and columns indexed by number counting from zero True - Return rows and columns indexed by their actual row and column IDs

```

### JSON

```sh
# json_reformat
# json_verify
# jq, 彩色, https://blog.csdn.net/fm18771120556/article/details/125036807

[chenchen@dev3_10.211.21.18 ex4]$ echo '{"f":"\/usr\/home\/wenqiang1\/htdocs\/dashboard\/data\/board_data.xlsx","a":{"b":"2022-01","e":"2022-02","bY":"2022","bm":"01","eY":"2022","em":"02","md":"2","mdYs":["2022-01","2022-02"],"mds":["01","02"],"ddYs":["31","28"],"dd":"59"},"b":{"b":"2023-01","e":"2023-02","bY":"2023","bm":"01","eY":"2023","em":"02","md":"2","mdYs":["2023-01","2023-02"],"mds":["01","02"],"ddYs":["31","28"],"dd":"59"}}' | json_reformat

[chenchen@dev3_10.211.21.18 ex4]$ echo '{"f":"\/usr\/home\/wenqiang1\/htdocs\/dashboard\/data\/board_data.xlsx","a":{"b":"2022-01","e":"2022-02","bY":"2022","bm":"01","eY":"2022","em":"02","md":"2","mdYs":["2022-01","2022-02"],"mds":["01","02"],"ddYs":["31","28"],"dd":"59"},"b":{"b":"2023-01","e":"2023-02","bY":"2023","bm":"01","eY":"2023","em":"02","md":"2","mdYs":["2023-01","2023-02"],"mds":["01","02"],"ddYs":["31","28"],"dd":"59"}}' | json_verify
JSON is valid

[chenchen@dev3_10.211.21.18 ex4]$ echo '{"f":"\/usr\/home\/wenqiang1\/htdocs\/dashboard\/data\/board_data.xlsx","a":{"b":"2022-01","e":"2022-02","bY":"2022","bm":"01","eY":"2022","em":"02","md":"2","mdYs":["2022-01","2022-02"],"mds":["01","02"],"ddYs":["31","28"],"dd":"59"},"b":{"b":"2023-01","e":"2023-02","bY":"2023","bm":"01","eY":"2023","em":"02","md":"2","mdYs":["2023-01","2023-02"],"mds":["01","02"],"ddYs":["31","28"],"dd":"59"}}' | jq .

```

### tty

```sh
(grpc02) [chenchen@grpc01 nlp]$ tty
/dev/pts/1

(grpc02) [chenchen@grpc01 nlp]$ sudo chvt 0
chvt: VT_ACTIVATE: No such device or address
```

### chmod

```sh
(ai) [chenchen@dev3_10.211.21.18 ai]$ chmod u+x test.py 
u,g,o,a
u 代表用户（user）
g 代表用户组（group）
o 代表其他（other）
a 代表所有（all）
```

### ln -s

```sh
[chenchen@dev3_10.211.21.18 ~]$ ln -s /data1/www/htdocs/chenchen/python3916 ~/python3916
ln -s 实际目录或者文件 符号
```

### clash

```
[chenchen@dev3_10.211.21.18 clash]$ sudo ./clash -d ./
```

### find

```sh
[chenchen@dev3_10.211.21.18 library]$ find ./ -type d -name .git
./vendor/sinasports/security/.git
./vendor/sinasports/phplib/.git
[chenchen@dev3_10.211.21.18 library]$ rm ./vendor/sinasports/security/.git -rf
[chenchen@dev3_10.211.21.18 library]$ rm ./vendor/sinasports/phplib/.git -rf
[chenchen@dev3_10.211.21.18 library]$ find ./ -type d -name .git
```

### du

```sh
.[!.]* 是正则式, 意思是第一位是点, 第二位是除了点以外的字符, 第三位是任意字符或者不存在
后面可以再加一个" *"来包括非隐藏文件
后面还可以接 sort -h来对占用空间进行排序(sort 的 -k2 可以指明根据第二列来排序)
下面这条命令就是显示所有隐藏文件和非隐藏文件的大小并根据占用空间排序的语句

du -sh .[!.]* * | sort -hr
```

### df

```sh
df -lh
```

nohup

```sh
# nohup
# 只输出错误信息到日志文件
$ nohup ./program >/dev/null 2>log &
# 什么信息也不要
$ nohup ./program >/dev/null 2>&1 &
```







## FAQ



## Troubleshooting





## See Also





