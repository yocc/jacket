# urllib3, 字符串前缀, JSON

----

[toc]





## Overview

```shell
1. venv
$ . ~/tmp/venv/python39/grpc02/bin/activate

2. install
$ python -m pip install urllib3

3. import
import urllib3

4. certificate

```

```python
#! /usr/bin/env python
# -*- coding:UTF-8 -*-

# https://urllib3.readthedocs.io/en/latest/user-guide.html

import ssl
import certifi
import urllib3
import json
import time
import random


def main():
    print(certifi.where())
    cdt()

def req():
    url = 'https://saga.sports.sina.com.cn/api/data/player_litse?player=6841&dpc=1'
    resp = urllib3.request("GET", "https://saga.sports.sina.com.cn/api/data/player_litse?player=6841&dpc=1")
    resp.status
    200
    resp.data
    b"User-agent: *\nDisallow: /deny\n"

def cdt():
    # Creating a PoolManager instance for sending requests.
    # 两种解决证书方式, 1. CERT_NONE; 2. CERT_REQUIRED; 方式2更好(采用), 方式1是忽略证书
    # http = urllib3.PoolManager(cert_reqs = "CERT_NONE")				# 方式1, 会报 warnings
    
    http = urllib3.PoolManager(																	# 方式2
        cert_reqs="CERT_REQUIRED",
        ca_certs=certifi.where()
    )

    # Sending a GET request and getting back response as HTTPResponse object.
    url = 'https://saga.sports.sina.com.cn/api/data/player_litse?player=6841&dpc=1'
    resp = http.request('GET', url, timeout=urllib3.Timeout(connect = 1.0, read = 2.0))
    # resp = http.request('GET', url, context=ssl.create_default_context(cafile=certifi.where()))
    # resp = http.request('GET', url, verify=False)
    # resp = http.request('GET', url, verify='/home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages/certifi/cacert.pem')

    # Print the returned data.
    print(resp.data)
    # b"User-agent: *\nDisallow: /deny\n"

if __name__ == '__main__':
    main()
```

## Certificate

### 缘起

通过脚本请求 URL 时, 就像是浏览器一样, 当遇到 https 链接时也需要客户端浏览器地址栏中出现绿锁标志. 那么接下来我们需要在代码里手动构造证书.

### 查看浏览器证书信息

- 点浏览器“绿锁”, 选“连接是安全的”, 选“证书有效”

- 选择“详细信息”

- “证书层次结构”在树状结构中选中在根节点“DigiCert Global Root CA”, 点“导出”按钮

- 在弹出的保存对话框中, 格式: 选择“Base64 编码 ASCII, 证书链”, 保存本地文件 “DigiCert Global Root CA.cer”

- 用文本编辑器打开 “DigiCert Global Root CA.cer” 文件, 拷贝(含)-----BEGIN CERTIFICATE----- (...) -----END CERTIFICATE-----

- 粘贴到 certifi.where() 返回的 本地路径字符串(含单引号)文件顶部, 如:  `'/home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages/certifi/cacert.pem'`

  ```certificate
  -----BEGIN CERTIFICATE-----
  MIIDrzCCApegAwIBAgIQCDvgVpBCRrGhdWrJWZHHSjANBgkqhkiG9w0BAQUFADBh
  MQswCQYDVQQGEwJVUzEVMBMGA1UEChMMRGlnaUNlcnQgSW5jMRkwFwYDVQQLExB3
  d3cuZGlnaWNlcnQuY29tMSAwHgYDVQQDExdEaWdpQ2VydCBHbG9iYWwgUm9vdCBD
  QTAeFw0wNjExMTAwMDAwMDBaFw0zMTExMTAwMDAwMDBaMGExCzAJBgNVBAYTAlVT
  MRUwEwYDVQQKEwxEaWdpQ2VydCBJbmMxGTAXBgNVBAsTEHd3dy5kaWdpY2VydC5j
  b20xIDAeBgNVBAMTF0RpZ2lDZXJ0IEdsb2JhbCBSb290IENBMIIBIjANBgkqhkiG
  9w0BAQEFAAOCAQ8AMIIBCgKCAQEA4jvhEXLeqKTTo1eqUKKPC3eQyaKl7hLOllsB
  CSDMAZOnTjC3U/dDxGkAV53ijSLdhwZAAIEJzs4bg7/fzTtxRuLWZscFs3YnFo97
  nh6Vfe63SKMI2tavegw5BmV/Sl0fvBf4q77uKNd0f3p4mVmFaG5cIzJLv07A6Fpt
  43C/dxC//AH2hdmoRBBYMql1GNXRor5H4idq9Joz+EkIYIvUX7Q6hL+hqkpMfT7P
  T19sdl6gSzeRntwi5m3OFBqOasv+zbMUZBfHWymeMr/y7vrTC0LUq7dBMtoM1O/4
  gdW7jVg/tRvoSSiicNoxBN33shbyTApOB6jtSj1etX+jkMOvJwIDAQABo2MwYTAO
  BgNVHQ8BAf8EBAMCAYYwDwYDVR0TAQH/BAUwAwEB/zAdBgNVHQ4EFgQUA95QNVbR
  TLtm8KPiGxvDl7I90VUwHwYDVR0jBBgwFoAUA95QNVbRTLtm8KPiGxvDl7I90VUw
  DQYJKoZIhvcNAQEFBQADggEBAMucN6pIExIK+t1EnE9SsPTfrgT1eXkIoyQY/Esr
  hMAtudXH/vTBH1jLuG2cenTnmCmrEbXjcKChzUyImZOMkXDiqw8cvpOp/2PV5Adg
  06O/nVsJ8dWO41P0jmP6P6fbtGbfYmbW0W5BjfIttep3Sp+dWOIrWcBAI+0tKIJF
  PnlUkiaY4IBIqDfv8NZ5YBberOgOzW6sRBc4L0na4UU+Krk2U886UAb3LujEV0ls
  YSEY1QSteDwsOoBrp+uvFRTp2InBuThs4pFsiv9kuXclVzDAGySj4dzp30d8tbQk
  CAUw7C29C79Fv1C5qfPrmAESrciIxpg0X40KPMbp1ZWVbd4=
  -----END CERTIFICATE-----
  ```

### 查看证书物理存放位置 (两种方式)

```python
# py 脚本
import certifi
print(certifi.where())
```
``` shell
# shell 交互
(grpc02) [chenchen@grpc01 pypgm]$ python
Python 3.9.10 (main, Mar 12 2022, 17:09:05) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-44)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import certifi
>>> certifi.where()
'/home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages/certifi/cacert.pem'
```

### 手动构造证书, 两种方式

方式一: (未采用)

从 https://www.digicert.com/kb/digicert-root-certificates.htm 这里找到合适的证书(DigiCert Global Root CA)下载, 然后用文本编辑器打开, 拷贝其中内容, 放到 venv py 证书位置, 如: certifi.where(), '/home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages/certifi/cacert.pem'

所谓合适证书就是通过上面“查看证书信息”里面的根节点名字, 用此名字在证书页面搜索并下载.

拷贝(含)-----BEGIN CERTIFICATE----- (...) -----END CERTIFICATE-----

方式二:(采用)

```shell
# 根据 “查看浏览器证书信息” 导出证书, 然后手动放到 venv python 证书存放位置的证书文件里, 比如: cacert.pem
certifi.where() 返回 '/home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages/certifi/cacert.pem'
拷贝(含)-----BEGIN CERTIFICATE----- (...) -----END CERTIFICATE-----
```

## 字符串和字符串前缀

- `# -*- coding: UTF-8 -*-` 说明了文件级的字符串编码

- """..."""

  使用三引号, 同perl的 <<EOL; EOL 或者php的<<<HTML

  print """

  Usage: thingy [OPTIONS]

     -h            Display this usage message

     -H hostname        Hostname to connect to

  """

- f'...'

  print(f'{name} done in {time.time() - t0:.2f} s')

  \# 以 f开头表示在字符串内支持大括号内的python 表达式

- r"..."

  的作用是去除转义字符.

  即, 如果是 "\n" 那么表示一个反斜杠字符 和 一个字母 n, 而不是表示换行了.

  以 r 开头的字符, 常用于正则表达式, 对应着re模块.

- b"..."

  前缀表示: 后面字符串是 bytes 类型.

  用处: 网络编程中, 服务器和浏览器只认 bytes 类型数据

- u"..."

  u"我是含有中文字符组成的字符串。"

  作用: 后面字符串以 Unicode 格式进行编码, 一般用在中文字符串前面, 防止因为源码储存格式问题, 导致再次使用时出现乱码.

## 中文

```shell
#! /usr/bin/env python
# -*- coding:UTF-8 -*-


import ssl
import certifi
import urllib3
import json
import time
import random


def main():
    cdt()

def cdt():
    http = urllib3.PoolManager(
        cert_reqs="CERT_REQUIRED",
        ca_certs=certifi.where()
    )
    url = 'https://saga.sports.sina.com.cn/api/data/player_litse?player=6841&dpc=1'
    resp = http.request('GET', url, timeout=urllib3.Timeout(connect = 1.0, read = 2.0))
    
    # 以下是各种返回结果的说明:
    # 通过 python -m json.tool --no-ensure-ascii 访问
    # (grpc02) [chenchen@grpc01 pypgm]$ ./u3.py | python -m json.tool --no-ensure-ascii
    # print(resp.data)   														# 带 b'...', bytes 类型
    #print(resp.data.decode('utf-8'))   						# 中文 
    #print(resp.data.decode('unicode_escape'))  		# 中文 
    #print(resp.data.decode('raw_unicode_escape'))  # 中文
    
    # json 解码
    #st = json.loads(resp.data.decode('utf-8'))
    #print(st['result']['status']['code'])                  # 可访问
    #print(st['result']['status']['msg'])                   # 可访问
    #print(st['result']['data']['Info']['PlayerID'])        # 可访问
    #print(st['result']['data']['Info']['CNName'])          # 可访问中文
    
    #print(type(st['result']['status']['code']))						# <class 'int'>
    #print(type(st['result']['status']['msg']))							# <class 'str'>
    #print(type(st['result']['data']['Info']['PlayerID']))	# <class 'str'>
    #print(type(st['result']['data']['Info']['CNName']))		# <class 'str'>
    
    # json 编码
    #print(json.dumps(st))                                  # 没缩进没中文
    		#(grpc02) [chenchen@grpc01 pypgm]$ ./u3.py | python -m json.tool											# 缩进没中文
    		#(grpc02) [chenchen@grpc01 pypgm]$ ./u3.py | python -m json.tool --no-ensure-ascii		# 缩进中文
    #print(json.dumps(st, ensure_ascii=True, indent=4))     # 缩进没中文
    #print(json.dumps(st, ensure_ascii=False, indent=4))    # 缩进中文
		#print(json.dumps(st, ensure_ascii=False, indent=2))    # 缩进中文
    #print(json.dumps(st, ensure_ascii=False))      				# 没缩进中文
    		#(grpc02) [chenchen@grpc01 pypgm]$ ./u3.py | python -m json.tool --no-ensure-ascii		# 缩进中文
    #print(json.dumps(st, indent=2))      									# 缩进没中文
				#(grpc02) [chenchen@grpc01 pypgm]$ ./u3.py | python -m json.tool --no-ensure-ascii		# 缩进中文
    
if __name__ == '__main__':
    main()
```

## unicode_escape, raw_unicode_escape

```shell
unicode_escape
unicode_escape 编码适合作为 ASCII 编码的 Python 源代码中的 Unicode 文本内容, 但引号不转义.
从 Latin-1 源代码解码.
请注意, 默认情况下, Python 源代码实际上使用 UTF-8.
Encoding suitable as the contents of a Unicode literal in ASCII-encoded Python source code, except that quotes are not escaped. Decode from Latin-1 source code. Beware that Python source code actually uses UTF-8 by default.

raw_unicode_escape
raw_unicode_escape 拉丁语-1 编码, 使用 \uXXXX 和 \UXXXXXXXX 表示其他代码点. 不会以任何方式对现有反斜杠进行转义.
它用于 Python pickle 协议
Latin-1 encoding with \uXXXX and \UXXXXXXXX for other code points. Existing backslashes are not escaped in any way. It is used in the Python pickle protocol.
```

## pickle

```shell
pickle — Python 对象序列化

陈晨理解: “pickling腌制”和“unpickling脱腌” 是指将 python 内部对象的内存形态和二进制协议之间的转换.

pickle 模块实现用于序列化和反序列化 Python 对象结构的二进制协议.
“Pickling”是将Python对象层次结构转换为字节流的过程, “unpickling”是反向操作, 其中字节流(来自二进制文件或类似字节的对象)被转换回对象层次结构.
酸洗（和脱酸）也称为“serialization序列化”、“marshalling编组” 或“flattening扁平化” ;
但是, 为避免混淆, 此处使用的术语是“pickling腌制”和“unpickling脱腌”.

警告: 
pickle泡菜 模块不安全. 仅能 unpickle脱腌 您信任的数据. (也就是仅对自己玩法(pickle, unpickle)可以用, 不能和其他技术交互)
可以构建恶意的 pickle 数据, 这些数据将在 unpickle 解酸期间执行任意代码. (可以主动伪造 pickle 数据)
切勿 unpickle脱腌 可能来自不受信任来源或可能已被篡改的数据. 
如果需要确保数据未被篡改, 请考虑使用 hmac 对数据进行签名. 
如果要处理不受信任的数据, 则更安全的序列化格式(如 json)可能更合适. 请参阅与 json 的比较. https://docs.python.org/3/library/pickle.html#comparison-with-json
```

## hmac 算法

```shell
hmac — 用于消息身份验证的密钥哈希
Source code: Lib/hmac.py
此模块实现 RFC 2104 中所述的 HMAC 算法.

HMAC 基于hash的消息认证码, 哈希消息认证码, https://baike.baidu.com/item/hmac/7307543
HMAC是密钥相关的哈希运算消息认证码（Hash-based Message Authentication Code）的缩写，由H.Krawezyk，M.Bellare，R.Canetti于1996年提出的一种基于Hash函数和密钥进行消息认证的方法，并于1997年作为RFC2104被公布，并在IPSec和其他网络协议（如SSL）中得以广泛应用，现在已经成为事实上的Internet安全标准。它可以与任何迭代散列函数捆绑使用。
```



## json.tool

```shell
(grpc02) [chenchen@grpc01 pypgm]$ python -m json.tool --help
usage: python -m json.tool [-h] [--sort-keys] [--no-ensure-ascii] [--json-lines] [--indent INDENT | --tab | --no-indent | --compact] [infile] [outfile]

A simple command line interface for json module to validate and pretty-print JSON objects.

positional arguments:
  infile             a JSON file to be validated or pretty-printed
  outfile            write the output of infile to outfile

optional arguments:
  -h, --help         show this help message and exit
  --sort-keys        sort the output of dictionaries alphabetically by key
  --no-ensure-ascii  disable escaping of non-ASCII characters
  --json-lines       parse input using the JSON Lines format. Use with --no-indent or --compact to produce valid JSON Lines output.
  --indent INDENT    separate items with newlines and use this number of spaces for indentation
  --tab              separate items with newlines and use tabs for indentation
  --no-indent        separate items with spaces rather than newlines
  --compact          suppress all whitespace separation (most compact)
```



## 2.x 与 3.x

```shell
https://docs.python.org/3/library/functions.html 3.x 内建函数
https://docs.python.org/3/library/stdtypes.html	 3.x 内建类型
```



## Text Sequence Type — [`str`](https://docs.python.org/3/library/stdtypes.html#str)

```shell
see also: https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str

文本序列类型 str, text sequence type
Python 中的文本数据由 str 对象(str objects) 或 字符串(string) 处理. 
字符串(string) 是不可变的 Unicode 代码点(code points) 序列. 字符串(string) 字面值(literals) 以多种方式编写:

- 单引号 Single quotes: 'allows embedded "double" quotes'
- 双引号 Double quotes: "allows embedded 'single' quotes"
- 三引号 Triple quoted: '''Three single quotes''', """Three double quotes"""
	三引号可以涵盖多行 - 所有相关的空白字符就会包含在 string literals 里.
	
String literals 作为单个表达式的一部分, 且它们之间只有空白字符将被隐式转换为单个 string literals, 
比如: ("spam " "eggs") == "spam eggs"

有关各种形式的 string literals(字符串文本)的详细信息, 请参阅 String and Bytes literals(字符串和字节文本), 包括支持的 escape sequences(转义序列), 以及禁用大多数 escape sequence(转义序列)处理的 r("raw")前缀

也可以使用 class str(...), https://docs.python.org/3/library/stdtypes.html#str 构造函数 从其他对象创建 string(字符串)

由于没有 separate(单独)的"character(字符)"类型, 因此索引字符串会产生长度为 1 的字符串. 
也就是说, 对于 non-empty(非空字) string(符串) s, s[0] == s[0：1]. 

也没有 mutable(可变的)字符串类型, 只有 str.join() 或 io.StringIO 可用于从 multiple fragments(多个片段)有效地构造字符串.
https://docs.python.org/3/library/stdtypes.html#str.join
https://docs.python.org/3/library/io.html#io.StringIO

在 3.3 版更改: 为了向后兼容 Python 2 系列, string literals(字符串文字)再次允许使用 u 前缀. 它对 string literals(字符串文本)的含义没有影响, 不能与 r 前缀组合.
```



## class str(...)

```shell
class str(object='')
class str(object=b'', encoding='utf-8', errors='strict')

返回一个对象的 string(字符串)版本. [string](https://docs.python.org/3/library/stdtypes.html#textseq)
如果对象没有提供, 则返回空字符串. 否则, str() 的行为取决于是否给出了 encoding(编码) 或 errors(错误), 如下所示.

(情况一, 无参数)
如果既没有给出 encoding(编码) 也没有给出 errors(错误), 则 str(object) 返回 type(object).__str__(object), 这是对象的“非正式”或可打印的字符串表示形式. [type(object).__str__(object)](https://docs.python.org/3/reference/datamodel.html#object.__str__)
对于 string objexts(字符串对象), 这是 string(字符串)本身. 如果 object(对象) 没有 __str__() 方法, 则 str() 返回 repr(object). https://docs.python.org/3/library/functions.html#repr

(情况二, 至少一个参数)
如果至少给出了 encoding(编码) 或 errors(错误) 之一, 则 objects(对象) 应该是类似字节的对象(例如 bytes(字节)或bytearray(字节数组)).
https://docs.python.org/3/glossary.html#term-bytes-like-object
https://docs.python.org/3/library/stdtypes.html#bytes
https://docs.python.org/3/library/stdtypes.html#bytearray
在这种情况下, 如果 objects(对象)是 bytes(字节)(或bytearray(字节数组))对象, 则 str(bytes, encoding, errors) 等同于 bytes.decode(encoding, errors). https://docs.python.org/3/library/stdtypes.html#bytes.decode
否则, 在调用 bytes.decode() 之前获取 buffer object(缓冲区对象)下的 bytes object(字节对象). 
有关缓冲区对象的信息, 请参阅 Binary Sequence Types -- bytes, bytearray, memoryview(二进制序列类型——字节、字节数组、内存视图)和 Buffer Protocol(缓冲区协议).
https://docs.python.org/3/library/stdtypes.html#binaryseq
https://docs.python.org/3/c-api/buffer.html#bufferobjects

在没有 encoding(编码) 或 errors(错误) 参数的情况下将 bytes(字节)对象传递给 str() 属于返回非正式字符串表示的第一种情况(另请参阅 Python 的 -b 命令行选项). 例如:
>>> str(b'Zoot!')
"b'Zoot!'"

有关 str 类及其方法的更多信息, 请参阅下面的 Text Sequence Type -- str(文本序列类型 — str)和 String Methods(字符串方法)部分. 
https://docs.python.org/3/library/stdtypes.html#textseq, https://docs.python.org/3/library/stdtypes.html#string-methods

若要输出格式化字符串, 请参阅 Formatted string literals(格式化字符串文本)和 Format String Syntax(格式化字符串语法)部分. 此外, 请参阅 Text Processing Services(文本处理服务)部分.
https://docs.python.org/3/reference/lexical_analysis.html#f-strings
https://docs.python.org/3/library/string.html#formatstrings
https://docs.python.org/3/library/text.html#stringservices
```

## class bytes(...)

```shell
字节对象
字节对象是单个字节的不可变序列. 
由于许多主要的二进制协议都基于 ASCII 文本编码, 因此字节对象提供了几种方法, 这些方法仅在处理 ASCII 兼容数据时才有效, 并且以各种其他方式与字符串对象密切相关.
class bytes([source[, encoding[, errors]]])
首先，字节字面量的语法与字符串字面量的语法基本相同，只是添加了 b 前缀.
字节字面量中只允许使用 ASCII 字符(无论声明的源代码编码如何). 必须使用适当的转义序列将任何超过 127 的二进制值输入到字节字面量中.
与字符串字面量一样, 字节字面量也可以使用 r 前缀来禁用转义序列的处理.
字节对象实际上的行为类似于不可变的整数序列.

classmethod fromhex(string)
hex([sep[, bytes_per_sep]])
```

## Bytes and Bytearray Operations

```shell
字节和字节数组操作 
字节和字节数组对象都支持常见的序列操作.
它们不仅与相同类型的操作数互操作, 还与任何类似字节的对象互操作.
由于这种灵活性, 它们可以在操作中自由混合而不会引起错误. 但是, 结果的返回类型可能取决于操作数的顺序.
注意: 字节和字节数组对象上的方法不接受字符串作为其参数，就像字符串上的方法不接受字节作为其参数一样. 
例如, 您必须编写:
a = "abc"
b = a.replace("a", "f")
and:
a = b"abc"
b = a.replace(b"a", b"f")
某些字节和字节数组操作假定使用 ASCII 兼容的二进制格式, 因此在处理任意二进制数据时应避免使用. 
下面介绍了这些限制。 
注意: 使用这些基于 ASCII 的操作来操作不存储在基于 ASCII 的格式中的二进制数据可能会导致数据损坏.

以下关于字节和字节数组对象的方法可用于任意二进制数据.
https://docs.python.org/3/library/stdtypes.html#bytes-and-bytearray-operations
bytes.decode(encoding='utf-8', errors='strict'|'ignore'|'replace'), bytes 类型(对象)解码转成 str 类型(对象)
bytearray.decode(encoding='utf-8', errors='strict'|'ignore'|'replace')

反过来:
str.encode(encoding='utf-8', errors='strict'), string 编码成 bytes 类型(对象)
Other possible values are 'ignore', 'replace', 'xmlcharrefreplace', 'backslashreplace' and any other name registered via codecs.register_error(). See Error Handlers for details.

```

## bytes-like object

```shell
https://docs.python.org/3/glossary.html#term-bytes-like-object
类字节对象
支持缓冲区协议并且可以导出 C 连续缓冲区的对象。这包括所有字节、字节数组和 array.array 对象，以及许多常见的内存视图对象。类似字节的对象可用于处理二进制数据的各种操作;其中包括压缩、保存到二进制文件和通过套接字发送。 某些操作需要二进制数据是可变的。文档通常将这些称为“类似读写字节的对象”。示例可变缓冲区对象包括字节数组和字节数组的内存视图。其他操作要求二进制数据存储在不可变对象（“只读字节类对象”）中;这些示例包括字节和字节对象的内存视图。
```



## String Methods

```shell
strings(字符串)实现所有常见的序列操作, 以及下面介绍的其他方法.
https://docs.python.org/3/library/stdtypes.html#typesseq-common

strings(字符串)还支持两种样式风格的字符串格式, 
一种提供很大程度的灵活性和自定义(请参阅 str.format()、Format String Syntax和Custom String Formatting自), 
另一种基于 C printf 样式格式, 处理 范围较窄的类型 并且稍微难以正确使用, 但对于它可以处理的情况通常更快(printf-style String Formatting). 
https://docs.python.org/3/library/stdtypes.html#str.format
https://docs.python.org/3/library/string.html#formatstrings
https://docs.python.org/3/library/string.html#string-formatting
https://docs.python.org/3/library/stdtypes.html#old-string-formatting

标准库的 Text Processing Services 部分涵盖了许多其他模块, 这些模块提供各种与文本相关的实用程序(包括 re 模块中的正则表达式支持)
https://docs.python.org/3/library/text.html#textservices
https://docs.python.org/3/library/re.html#module-re
```



## String and Bytes literals

```shell
see also: https://docs.python.org/3/reference/lexical_analysis.html#string-and-bytes-literals

String literals
前缀: r, u, f, fr, 不区分大小写, 不区分顺序. (python3.3以上, 没有了 'ur', 'u'还保留仅仅是为了兼容2.x, 其实已经没用了)
string(字符串)对于 python 解释器的词法含义: 任何不含\反斜线 source character , \n(换行), 引号(四种:单引号, 双引号, 三个单, 三个双), \反斜线后接任何 source character 

Bytes literals
前缀: b, br, 不区分大小写, 不区分顺序
bytes(字节流)对于 python 解释器的词法含义: 任何不含\反斜线 ASCII character, \n(换行), 引号(四种:单引号, 双引号, 三个单, 三个双), \反斜线后接任何 ASCII character

⚠️ source character 我理解是相对于 source file 而说的, 意思是 源文件中的敲入的程序字符.

source character 集由编码声明定义; 如果 source file(程序源文件) 中没有给出编码声明, 则为 UTF-8; 请参阅 Encoding declarations(编码声明)部分.
https://docs.python.org/3/reference/lexical_analysis.html#encodings
Encoding declarations(编码声明):
python 源程序文件的前两行如果满足 coding[=:]\s*([-\w.]+) 这个正则, 那就被认为是编码声明了; 推荐如下写法:
# -*- coding: <encoding-name> -*-
如果不显式声明, 那么默认是作为 UTF-8 处理. 并且源码文件的 UTF-8 的字节序为 (b'\xef\xbb\xbf')
<encoding-name>: python 支持的所有编码名称: https://docs.python.org/3/library/codecs.html#standard-encodings

在文本编辑器 vim 中查看的当前源文件的编码:
方法一: (vim 环境下)
# vim:fileencoding=<encoding-name>
:set fileencoding				# 在 vim 环境下查看
fileencoding=utf-8
方法二: (shell 环境下)
(grpc02) [chenchen@grpc01 pypgm]$ file u3.py 
u3.py: Python script, UTF-8 Unicode text executable

String literals 和 Bytes literals 的相同点:
- 在 单, 双, 三 引号情况下都一样的表现.
- 'r' 前缀, 均将反斜杠视为literal characters(字符字面), 不会转义处理.

String literals 和 Bytes literals 的不同点:
- Bytes literals 一定总有 'b' 前缀. 这表示的是前缀 'b' 前缀后面的字符串是 bytes 类型的实例, 而不是 str 类型的实例.
- 'b' 前缀字符串, 只能包含 ASCII 字符; 数值为 128 或更大的字节必须用转义符表示.

Escape Sequence: (分两类)
1. 在没有 'r'前缀情况下, 在 string literals 和 bytes literals 中都会自动转移的 反斜线开头的特定 escape sequence, 如表格: https://docs.python.org/3/reference/lexical_analysis.html#string-and-bytes-literals
2. 在没有 'r'前缀情况下, 仅在 string literals 中被辨识处理的特定 escape sequence, 一共3个:
	 - \N{name}, Character named name in the Unicode database, (我觉得是 emoji)
	 - \uxxxx, Character with 16-bit hex value xxxx, (常见 unicode 反斜线小写u码点, json 中的中文)
	 - \Uxxxxxxxx, Character with 32-bit hex value xxxxxxxx, (不常见)

字符串有两种类型, str 类型 和 bytes 类型
str 类型
- 以字符为处理单位, 不是以字节为单位处理
- 长用于字符串的常规处理, 截取, 连接, 替换, 等等

bytes 类型
- 字节流, 仅用于传输, 传输时是串行字节流通过网卡.
```

## JSON

```shell
import json
JSON 暴露出了标准库 marshal 和 pickle 模块的用户熟悉的 API.
https://docs.python.org/3/library/marshal.html#module-marshal
https://docs.python.org/3/library/pickle.html#module-pickle

1. 从内存数据结构编码输出到文件或者字符串
内存 --> 编码(encode) --> 文件|字符串
dump(文件, sort_keys=True, indent=4), 文件包括二进制文件和文本文件
dumps(字符串, sort_keys=True, indent=4)

str(object), object 的 string(字符串) version(版本), 即 object.__str__()方法, 如果对象没有__str__()方法, 就返回 repr(object). 
string version 等同于 object.__str__()方法
最后, str(object) === object 的 string 化表示 === string(字符串)

2. 从文件或者字符串输入解码进内存
文件|字符串 --> 解码(decode) --> 内存
load(文件)
loads(字符串)



----
json.tool, 命令行接口用于验证和格式化打印 json 对象
https://docs.python.org/3/library/json.html?highlight=json#module-json.tool
- 从命令行字符串
(grpc02) [chenchen@grpc01 pypgm]$ echo '{"json": "obj"}' | python -m json.tool
{
    "json": "obj"
}
- 从文件
python -m json.tool mp_films.json
[
    {
        "title": "And Now for Something Completely Different",
        "year": 1971
    },
    {
        "title": "Monty Python and the Holy Grail",
        "year": 1975
    }
]
```

## file-like object, file object

```shell
# https://docs.python.org/3/glossary.html#term-file-object
file object(文件对象)也称为 file-like object(类似文件的对象) 或 streams(流)
一个对象给基础资源, 暴露出面向文件的API.
根据创建方式, 文件对象可以调解对实际磁盘文件或其他类型的存储或通信设备(例如标准输入/输出、内存中缓冲区、套接字、管道等)的访问.

实际上有三类文件对象: 
- 原始二进制文件 raw binary files, https://docs.python.org/3/glossary.html#term-binary-file
- 缓冲二进制文件 buffered binary files, https://docs.python.org/3/glossary.html#term-binary-file
- 文本文件 text files, https://docs.python.org/3/glossary.html#term-text-file
它们的接口在 io 模块中定义. 创建文件对象的规范方法是使用 open() 函数.
```

## binary file

```shell
# https://docs.python.org/3/glossary.html#term-binary-file
陈晨理解: 二进制文件 = 仅有字节流, 没有编码. (与 text file(文本文件对比))
能够读取和写入 bytes objects(类似字节的对象)的 file object(文件对象).这个文件对象叫 binary file(二进制文件).
二进制文件的示例包括以二进制模式('rb'、'wb' 或 'rb+')打开的文件、sys.stdin.buffer、sys.stdout.buffer 和 io.BytesIO实例 和 gzip.GzipFile实例. 
另请参阅文本文件, 了解能够读取和写入 str 对象的文件对象.

陈晨理解: 
buffer 代指内存里
sys.stdin.buffer、sys.stdout.buffer, 这俩都是内存, 标准输入内存和标准输出内存, 也就是标准输入缓冲区和标准输出缓冲区.
io.BytesIO实例, 就是二进制实体文件了.
gzip.GzipFile实例, 二进制实体文件的特例, gzip压缩编码后的二进制文件.
其他二进制数据都不算二进制文件了, 都算变量, 数据结构了, 他们不能使用文件操作方法.
```

## text file

```shell
# https://docs.python.org/3/glossary.html#term-text-file
陈晨理解: 文本文件 = 可访问面向字节的数据流(字节数组) + 编码(字节数组的编码方式)
一个能读写 str objects 的 file object(文件对象). 这个文件对象叫 text file(文本文件).
通常，文本文件实际上访问面向字节的数据流并自动处理文本编码.
文本文件的例子, 在文本模式('r' 或者 'w')下打开文件, sys.stdin, sys.stdout, 和 io.StringIO实例.
```



## file object

```shell
# https://docs.python.org/3/glossary.html#term-file-object

```



## 网文

```shell
https://www.cnblogs.com/-qing-/p/10934261.html
Python3的编码整理总结

python3在内存中是用unicode编码方式存储的，所以不能直接储存和传输，要转化为其他编码进行储存和传输.
字符串通过编码转换成字节码，字节码通过解码成为字符串
encode：str --> bytes
decode：bytes --> str
下面是一些编码的关系

python2和python3的一些不同
1) python2 中默认使用 ascii, python3 中默认使用 utf-8
2) Python2 中, str 就是编码后的结果 bytes, str=bytes, 所以 s 只能decode
3) python3 中的字符串与 python2 中的 u'字符串', 都是 unicode, 只能encode, 所以无论如何打印都不会乱码, 因为可以理解为从内存打印到内存, 即内存->内存, unicode->unicode
4) python3中, str 是 unicode, 当程序执行时, 无需加u, str 也会被以 unicode 形式保存新的内存空间中, str 可以直接 encode 成任意编码格式, s.encode('utf-8'), s.encode('gbk')
#unicode(str)-----encode---->utf-8(bytes)
#utf-8(bytes)-----decode---->unicode
5)在 windows 终端编码为 gbk, linux 是 UTF-8.
```

## unicode, str, bytes

```shell
陈晨总结:

# Creating a PoolManager instance for sending requests. 
# https://urllib3.readthedocs.io/en/stable/advanced-usage.html#custom-tls-certificates
http = urllib3.PoolManager(
    cert_reqs="CERT_REQUIRED",
    ca_certs=certifi.where()
)
# Sending a GET request and getting back response as HTTPResponse object.
url = 'https://saga.sports.sina.com.cn/api/data/player_litse?player=6841&dpc=1'
resp = http.request('GET', url, timeout=urllib3.Timeout(connect = 1.0, read = 2.0))



1. python3 系统内部(内存)使用 unicode code point(字节序列) + utf-8(编码), = 内存字符串, 简称 unicode
2. str 是系统要处理的各种字符串的统称, 由于也是 unicode code point(字节序列) + utf-8(默认编码, 但可指定), = 要处理的字符串, 简称 string, 抽象为类型就是 str. 
	 由于 1. 和 2. 都是在 python3 运行时处理的, 并且通常都是 utf-8 编码, 所以大多数时候将 str 和 unicode 视为一样的处理方式. 但事实上意义完全不同, 是两个截然不同的东西.
	 更多时候提到 str 是将 '字节序列' 视为一个不可分割整体统一考虑和处理.
3. bytes 是要传输时的字节流, unicode code point(字节序列) + utf-8(默认编码, 但可指定为传输目标需要的编码), = 字节流, 简称 bytes, 抽象为类型就是 bytes. 由于 3. 受目标需要编码的影响比较大, 经常是 latin-1, gbk等, 所以习惯上并不将 bytes.
	 更多时候提到 bytes 是将 '字节序列' 的每一个字节, 视为处理对象, 比如, 传输流经网卡时是一个字节一个字节的通过.


概念含义分别: 
unicode: 
	 - python3 系统内部编码, 广义理解为 unicode 码点(code point). 
	 - 由于是 python3 系统内部使用, 具体采用何种 unicode 编码, 对外界来说并不重要也不用关心, 实际观察形如: \xFF\xFF, 每一个 \xFF 都是一个字节.
	 - 对外界来说, python3 系统默认是 utf-8 编码
str:
	 - str 是一种类型(str), 一种对象(string object). 
	 - python 世界皆为对象, 字符串对象, 即为常规字符串外带了一些方法. 
	 - str.encode(self, /, encoding='utf-8', errors='strict')
   -  class str(object)
       |  str(object='') -> str
       |  str(bytes_or_buffer[, encoding[, errors]]) -> str
       |  
       |  Create a new string object from the given object. If encoding or
       |  errors is specified, then the object must expose a data buffer
       |  that will be decoded using the given encoding and error handler.
       |  Otherwise, returns the result of object.__str__() (if defined)
       |  or repr(object).
       |  encoding defaults to sys.getdefaultencoding().
       |  errors defaults to 'strict'.
       两种情况: 
           1. (类型转换)将给定类型对象转成字符串类型对象
           2. (特定编码)内存对象可编码, 否则返回 object.__str__() 或者 repr(object)
bytes:
	 - 字节流, 用于传输, unicode 码点的具体编码后的结果, 比如 utf-8
	 - 'b'前缀
	 - bytes.decode(self, /, encoding='utf-8', errors='strict')
	 - class bytes(object)
       |  bytes(iterable_of_ints) -> bytes
       |  bytes(string, encoding[, errors]) -> bytes
       |  bytes(bytes_or_buffer) -> immutable copy of bytes_or_buffer
       |  bytes(int) -> bytes object of size given by the parameter initialized with null bytes
       |  bytes() -> empty bytes object
       |  
       |  Construct an immutable array of bytes from:
       |    - an iterable yielding integers in range(256)
       |    - a text string encoded using the specified encoding
       |    - any object implementing the buffer API.
       |    - an integer
       五种情况:
       		 1. 创建一个由一堆整数组成的字节数组, 每个元素是一个字节, 8位, 也是一个整数. 所以不会大于1个字节表示的整数
       		 2. 将字符串转换成字节数组
       		 3. 拷贝一个已有的字节数组
       		 4. 创建一个指定大小的字节数组, 每个元素是一个null
       		 5. 创建一个空字节数组, 没有元素
```



## FAQ

```python
问题:
运行 urllib3.py 脚本时, 报错, 提示证书验证失败
ssl.SSLCertVerificationError: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1129)

原因:
因为访问了 'https://saga.sports.sina.com.cn/api/data/player_litse?player=6841&dpc=1' https 链接, 需要客户端提供CA根证书链

解决:
通过 “手动构造证书” 方法二, 来解决, 代码同 ‘Overview’


#! /usr/bin/env python
# -*- coding:UTF-8 -*-

import ssl
import certifi
import urllib3
import json
import time
import random


def main():
    print(certifi.where())
    cdt()

def cdt():
    # Creating a PoolManager instance for sending requests.
    # 两种解决证书方式, 1. CERT_NONE; 2. CERT_REQUIRED; 方式2更好(采用), 方式1是忽略证书
    # http = urllib3.PoolManager(cert_reqs = "CERT_NONE")				# 方式1
    
    http = urllib3.PoolManager(																	# 方式2
        cert_reqs="CERT_REQUIRED",
        ca_certs=certifi.where()
    )

    # Sending a GET request and getting back response as HTTPResponse object.
    url = 'https://saga.sports.sina.com.cn/api/data/player_litse?player=6841&dpc=1'
    resp = http.request('GET', url, timeout=urllib3.Timeout(connect = 1.0, read = 2.0))
    # resp = http.request('GET', url, context=ssl.create_default_context(cafile=certifi.where()))
    # resp = http.request('GET', url, verify=False)
    # resp = http.request('GET', url, verify='/home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages/certifi/cacert.pem')

    # Print the returned data.
    print(resp.data)
    # b"User-agent: *\nDisallow: /deny\n"

if __name__ == '__main__':
    main()
```

```shell
问题: 
http = urllib3.PoolManager(cert_reqs = "CERT_NONE")				# 方式1, 会报 warnings
warnings 如下: 
(grpc02) [chenchen@grpc01 pypgm]$ ./u3.py 
/home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages/certifi/cacert.pem
/home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages/urllib3/connectionpool.py:1045: InsecureRequestWarning: Unverified HTTPS request is being made to host 'saga.sports.sina.com.cn'. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/1.26.x/advanced-usage.html#ssl-warnings
  warnings.warn(
/home/chenchen/tmp/venv/python39/grpc02/lib/python3.9/site-packages/urllib3/connectionpool.py:1045: InsecureRequestWarning: Unverified HTTPS request is being made to host 'saga.sports.sina.com.cn'. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/1.26.x/advanced-usage.html#ssl-warnings
  warnings.warn(
200

原因:
与请求 https 需要证书有关, 并且在代码请求中将证书关闭所致, 所以会报warnings, 同时也会返回正确结果json字符串.

解决: 
不用使用: 
cert_reqs = "CERT_NONE", 
而是使用: 
cert_reqs="CERT_REQUIRED",
ca_certs=certifi.where()
```

```shell
问题:
NameError: name 'unicode' is not defined

原因:
unicode() 方法是 python2.x 带有
str() 是 python3.x

解决:
if type(resFile) == str or type(resFile) == bytes:

```



## See Also

https://www.digicert.com/kb/digicert-root-certificates.htm  Download DigiCert root and intermediate certificates

https://urllib3.readthedocs.io/en/latest/user-guide.html  urllib3

https://stackoverflow.com/questions/51925384/unable-to-get-local-issuer-certificate-when-using-requests-in-python

https://medium.com/@menakajain/export-download-ssl-certificate-from-server-site-url-bcfc41ea46a2#id_token=eyJhbGciOiJSUzI1NiIsImtpZCI6ImFmYzRmYmE2NTk5ZmY1ZjYzYjcyZGM1MjI0MjgyNzg2ODJmM2E3ZjEiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJuYmYiOjE2NzQ2NzgzOTAsImF1ZCI6IjIxNjI5NjAzNTgzNC1rMWs2cWUwNjBzMnRwMmEyamFtNGxqZGNtczAwc3R0Zy5hcHBzLmdvb2dsZXVzZXJjb250ZW50LmNvbSIsInN1YiI6IjExMjU4NDc4MTE0NDEzNjA3OTU4MyIsImVtYWlsIjoieW9jY3pyQGdtYWlsLmNvbSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJhenAiOiIyMTYyOTYwMzU4MzQtazFrNnFlMDYwczJ0cDJhMmphbTRsamRjbXMwMHN0dGcuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJuYW1lIjoieW9jYyB5b2NjIiwicGljdHVyZSI6Imh0dHBzOi8vbGgzLmdvb2dsZXVzZXJjb250ZW50LmNvbS9hL0FFZEZUcDVLRXU5bWF2VFBYeVFJZExkaEpLMFktSDMtM2FiRHFmX1NjRTFpcEE9czk2LWMiLCJnaXZlbl9uYW1lIjoieW9jYyIsImZhbWlseV9uYW1lIjoieW9jYyIsImlhdCI6MTY3NDY3ODY5MCwiZXhwIjoxNjc0NjgyMjkwLCJqdGkiOiI1MmMwZWVhNzVkNDJmNzMyNTZhODZmNTM4NTgxNjA5OTk1Nzc2NjM3In0.dxWbg1EqQpxmoUm4ZQUvt-8xhX9_yCdF5btGsLhebzMw-71quaFZ6vrjohdTU7cUdJ17jp94zpH8FCB49lwJluyZtGqZeViEiAJX2b5IYOoDhYCy2krR0b-fQjH0-TNjEvgJecmjSbWyX0LH_Yq6JlzwQGMEbm-Gy6v7Y7uIPkhrEXL3J61Xun5N-51qJjoLwQ5myPK0AZKEoTYEdcV69GEo5ct70wrvOjym3I1OIh1T9TwIvXBFkIqotErtMfrNoGDxOLoePdjcMv3pzLkuCodSioHxEViPdHqphJxrwhuwa0n7k29X88VU3JjcUgVf3obbqqi1l4Q0XiBiq6QwnQ

https://medium.com/@superseb/get-your-certificate-chain-right-4b117a9c0fce

https://www.esri.com/arcgis-blog/products/bus-analyst/field-mobility/learn-how-to-download-a-ssl-certificate-for-a-secured-portal/