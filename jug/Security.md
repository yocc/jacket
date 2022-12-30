# Security

---

## FAQ

```shell
# 自签名 私有证书
1. [.key] 生成私钥, -des3 私钥是否加密算法, 生成 rsa 私钥, openssl格式, 2048位强度
openssl genrsa -des3 -out ./yocc2022102403.key 2048			# 需要一个至少四位的密码
openssl genrsa -out ./yocc2022102403.key 1024						# 无密码私钥
openssl rsa -in ./private.key -out ./private.key 2048		# 无密码私钥

2. [.csr] 生成申请文件, 由 1. 私钥 生成 证书申请文件.
openssl req -new -key ./yocc2022102403.key -out ./yocc2022102403.csr

3. [.crt] 生成证书文件, 由 1. 和 2. 一起生成证书文件. -days 3650 当前之后的证书过期时间. -sha1 算法. 
openssl x509 -req -days 3650 -sha1 -extensions v3_ca -signkey ./yocc2022102403.key -in ./yocc2022102403.csr -out ./yocc2022102403.crt
```

```shell
# 自签名 CA 证书
CA 机构, 又称为证书认证中心 (Certificate Authority) 中心
. 自签名私有证书无法被吊销，自签名CA证书可以被吊销. 
. 为什么吊销, 吊销的必要性: 私钥泄露后挽救的办法, 否则就无能为力了.
. 如果你的规划需要创建多个客户端证书, 那么使用自建 CA 签名证书的方法比较合适, 只要给所有的客户端都安装了 CA 根证书, 那么以该 CA 根证书签名过的客户端证书都是信任的, 不需要重复的安装客户端证书.
. 请注意由于是自建 CA 证书, 在使用这个临时证书的时候, 会在客户端浏览器报一个错误, 签名证书授权未知或不可 (signing certificate authority is unknown and not trusted.), 但只要配置正确, 继续操作并不会影响正常通信. 自签名证书的 Issuer 和 Subject 是一样的. 
# 创建自签名 CA 证书, 主要分为两个部分:
1. 创建 CA 根证书
2. 签发客户端证书

# 
1. 创建 CA 根证书
1.1. 查看服务器 CentOS 6.x 上有关 ssl 证书的目录结构. 
[root@dev3_10.211.21.18 CA]# pwd
/etc/pki/CA
[root@dev3_10.211.21.18 CA]# tree
.
├── certs
├── crl							# 吊销的证书
├── newcerts				# 存放 CA 签署(颁发)过的数字证书(证书备份目录)
└── private					# 用于存放 CA 的私钥

4 directories, 0 files
[root@dev3_10.211.21.18 CA]# 
[root@dev3_10.211.21.18 tls]# pwd
/etc/pki/tls
[root@dev3_10.211.21.18 tls]# tree
.
├── cert.pem -> /etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem
├── certs
│   ├── ca-bundle.crt -> /etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem
│   ├── ca-bundle.trust.crt -> /etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt
│   ├── make-dummy-cert
│   ├── Makefile
│   └── renew-dummy-cert
├── misc
│   ├── CA
│   ├── c_hash
│   ├── c_info
│   ├── c_issuer
│   └── c_name
├── openssl.cnf
└── private

3 directories, 12 files
[root@dev3_10.211.21.18 tls]# 
```





SSL, OpenSSL, HTTPS
SSL: Security Socket Layer 加密套接字协议层. 是客户端和服务器之间建立一条 SSL 安全通道的安全协议
OpenSSL: OpenSSL 是 TLS/SSL 协议的开源实现, 提供开发库和命令行程序
HTTPS: 是HTTP的加密版, 底层使用的加密协议是SSL


PKI 就是 Public Key Infrastructure 的缩写, 翻译过来就是 公开密钥基础设施.
它是利用公开密钥技术所构建的, 解决网络安全问题的, 普遍适用的一种基础设施;
是一种遵循既定标准的密钥管理平台, 它能够为所有网络应用提供加密和数字签名等密码服务及所必需的密钥和证书管理体系.
PKI既不是一个协议, 也不是一个软件, 它是一个标准, 在这个标准之下发展出的为了实现安全基础服务目的的技术统称为PKI.
可以说 CA(认证中心) 是 PKI 的核心, 而数字证书是 PKI 的最基本元素, 还有如 apache等服务器、浏览器等客户端、银行等应用, 都是pki的组件.

为保证用户之间在网上传递信息的 安全性、真实性、可靠性、完整性和不可抵赖性
CA 机构, 又称为证书认证中心 (Certificate Authority) 中心, 是一个负责发放和管理数字证书的第三方权威机构, 
它负责管理PKI结构下的所有用户(包括各种应用程序)的证书, 把用户的 公钥 和用户的其他信息捆绑在一起, 在网上验证用户的身份. 
CA机构的数字签名使得攻击者不能伪造和篡改证书.
认证中心主要有以下5个功能:

1. 证书的颁发: 接收、验证用户(包括下级认证中心和最终用户)的数字证书的申请. 可以受理或拒绝
2. 证书的更新: 认证中心可以定期更新所有用户的证书, 或者根据用户的请求来更新用户的证书
3. 证书的查询: 查询当前用户证书申请处理过程; 查询用户证书的颁发信息, 这类查询由目录服务器ldap来完成
4. 证书的作废: 由于用户私钥泄密等原因, 需要向认证中心提出证书作废的请求; 证书已经过了有效期, 认证中心自动将该证书作废. 认证中心通过维护证书作废列表 (Certificate Revocation List, CRL) 来完成上述功能.
5. 证书的归档: 证书具有一定的有效期, 证书过了有效期之后就将作废, 但是我们不能将作废的证书简单地丢弃, 因为有时我们可能需要验证以前的某个交易过程中产生的数字签名, 这时我们就需要查询作废的证书.


X.509 标准
PKI 体系中有很多 SSL 证书格式标准.
PKI的标准规定了 PKI 的设计、实施和运营，规定了 PKI 各种角色的 "游戏规则", 提供数据语法和语义的共同约定.
x.509 是 PKI 中最重要的标准, 它定义了公钥证书的基本结构, 可以说 PKI 是在 X.509 标准基础上发展起来的:
. SSL公钥证书
. 证书废除列表CRL(Certificate revocation lists 证书黑名单)
参考: http://en.wikipedia.org/wiki/X.509


SSL 公钥证书格式:
1. 证书版本号(Version)
    版本号指明 X.509 证书的格式版本, 现在的值可以为:
    1) 0: v1
    2) 1: v2
    3) 2: v3
    也为将来的版本进行了预定义

2. 证书序列号(Serial Number)
    序列号指定由 CA 分配给证书的唯一的 "数字型标识符". 当证书被取消时, 实际上是将此证书的序列号放入由 CA签发的CRL中, 这也是序列号唯一的原因.

3. 签名算法标识符(Signature Algorithm)
    签名算法标识用来指定由CA签发证书时所使用的 "签名算法". 算法标识符用来指定CA签发证书时所使用的:
    1) 公开密钥算法
    2) hash算法
    example: sha256WithRSAEncryption
    须向国际知名标准组织(如ISO)注册

4. 签发机构名(Issuer) 发行人
    此域用来标识签发证书的CA的X.500 DN(DN-Distinguished Name)名字. 包括:
    1) 国家(C)
    2) 省市(ST)
    3) 地区(L)
    4) 组织机构(O)
    5) 单位部门(OU)
    6) 通用名(CN)
    7) 邮箱地址

5. 有效期(Validity)
    指定证书的有效期, 包括:
    1) 证书开始生效的日期时间
    2) 证书失效的日期和时间
    每次使用证书时, 需要检查证书是否在有效期内.

6. 证书用户名(Subject)
    指定证书持有者的 X.500 唯一名字. 包括:
    1) 国家(C)
    2) 省市(ST)
    3) 地区(L)
    4) 组织机构(O)
    5) 单位部门(OU)
    6) 通用名(CN)
    7) 邮箱地址

7. 证书持有者公开密钥信息(Subject Public Key Info)
    证书持有者公开密钥信息域包含两个重要信息:
    1) 证书持有者的公开密钥的值
    2) 公开密钥使用的算法标识符. 此标识符包含公开密钥算法和hash算法.

8. 扩展项(extension)
    X.509 V3 证书是在 v2 的基础上一标准形式或普通形式增加了扩展项, 以使证书能够附带额外信息.
    标准扩展是指由 X.509 V3 版本定义的对 V2 版本增加的具有广泛应用前景的扩展项, 任何人都可以向一些权威机构, 
    如ISO, 来注册一些其他扩展, 如果这些扩展项应用广泛, 也许以后会成为标准扩展项.

9. 签发者唯一标识符(Issuer Unique Identifier)
    签发者唯一标识符在第2版加入证书定义中. 此域用在当同一个X.500名字用于多个认证机构时, 用一比特字符串来唯一标识签发者的 X.500名字. 可选.

10. 证书持有者唯一标识符(Subject Unique Identifier)
    持有证书者唯一标识符在第2版的标准中加入 X.509 证书定义. 此域用在当同一个 X.500 名字用于多个证书持有者时, 用一比特字符串来唯一标识证书持有者的 X.500名字. 可选.

11. 签名算法(Signature Algorithm)
    证书签发机构对证书上述内容的签名算法
    example: sha256WithRSAEncryption

12. 签名值(Issuer's Signature)
    证书签发机构对证书上述内容的签名值


example:
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 9 (0x9)
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=CN, ST=GuangDong, L=ShenZhen, O=COMPANY Technologies Co., Ltd, OU=IT_SECTION, CN=registry.example.com.net/emailAddress=zhouxiao@example.com.net
        Validity
            Not Before: Feb 11 06:04:56 2015 GMT
            Not After : Feb  8 06:04:56 2025 GMT
        Subject: C=CN, ST=GuangDong, L=ShenZhen, O=TP-Link Co.,Ltd., OU=Network Management, CN=172.31.1.210
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:a4:b0:dd:eb:c1:cf:5d:47:61:a6:ea:ef:8b:aa:
                    4b:f0:b4:2c:d8:96:c7:7c:ac:fa:c7:35:88:53:d0:
                    ...
                    8a:76:dc:8f:8c:44:c8:0b:3c:36:88:5f:01:f0:44:
                    4e:81:e6:7a:2b:ff:ba:da:33:a5:27:11:c6:f0:08:
                    6e:f3
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Basic Constraints: 
                CA:FALSE
            Netscape Comment: 
                OpenSSL Generated Certificate
            X509v3 Subject Key Identifier: 
                07:C6:87:B7:C1:1E:28:E8:96:3F:EB:40:1E:82:41:45:CA:81:B6:3D
            X509v3 Authority Key Identifier: 
                keyid:A4:C2:14:6A:39:D1:95:1E:BD:DF:3B:92:4A:5C:12:42:1B:BC:53:B8

    Signature Algorithm: sha256WithRSAEncryption
         0c:c6:81:70:cd:0a:2d:94:4f:cb:a4:1d:ef:9e:8e:e4:73:ae:
         50:62:a8:9c:64:ef:56:0f:41:fe:6b:b4:d3:07:37:39:2c:ed:
         ...
         6f:62:61:b8:03:d7:97:31:ab:05:44:20:07:65:8b:ad:e2:cc:
         ad:65:73:f6:82:0f:9e:65:d0:ae:b7:1e:fd:9f:c1:d7:41:6c:
         0f:06:95:ee
-----BEGIN CERTIFICATE-----
MIIEMDCCAxigAwIBAgIBCTANBgkqhkiG9w0BAQsFADCBtTELMAkGA1UEBhMCQ04x
EjAQBgNVBAgMCUd1YW5nRG9uZzERMA8GA1UEBwwIU2hlblpoZW4xJjAkBgNVBAoM
...
ujwwRar6pPzusO95WuS93HsNmL2ZFZ63DS4LcW9iYbgD15cxqwVEIAdli63izK1l
c/aCD55l0K63Hv2fwddBbA8Gle4=
-----END CERTIFICATE-----


常用公钥算法:
* RSA: 
    适用于数字签名和密钥交换. 是目前应用最广泛的公钥加密算法, 特别适用于通过 Internet 传送的数据, RSA算法以它的三位发明者的名字命名. 

* DSA: 
    仅适用于数字签名. 数字签名算法 (Digital Signature Algorithm, DSA) 由美国国家安全署 (United States National Security Agency, NSA) 发明, 已作为数字签名的标准. 
    DSA 算法的安全性取决于自计算离散算法的困难. 
    这种算法, 不适用于数据加密.

* Diffie-Hellman: 
    仅适用于密钥交换. 
    Diffie-Hellman 是发明的第一个公钥算法, 以其发明者 Whitfield Diffie 和 Martin Hellman 的名字命名. Diffie-Hellman 算法的安全性取决于在一个有限字段中计算离散算法的困难.


单向散列算法
散列, 也称为散列值或消息摘要, 是一种与基于密钥(对称密钥或公钥)的加密不同的数据转换类型.
散列就是通过把一个叫做散列算法的单向数学函数应用于数据, 将任意长度的一块数据转换为一个定长的、不可逆转的数字, 其长度通常在128～256位之间.
所产生的散列值的长度应足够长, 因此使找到两块具有相同散列值的数据的机会很少.
如发件人生成邮件的散列值并加密它，然后将它与邮件本身一起发送.
而收件人同时解密邮件和散列值, 并由接收到的邮件产生另外一个散列值, 然后将两个散列值进行比较.
如果两者相同，邮件极有可能在传输期间没有发生任何改变.

下面是几个常用的散列函数:
* MD5: 
    是RSA数据安全公司开发的一种单向散列算法, MD5被广泛使用, 可以用来把不同长度的数据块进行暗码运算成一个128位的数值.

* SHA-1: 
    与 DSA 公钥算法相似, 安全散列算法1 (SHA-1) 也是由 NSA 设计的, 并由 NIST 将其收录到 FIPS 中, 作为散列数据的标准. 
    它可产生一个 160 位的散列值. SHA-1 是流行的用于创建数字签名的单向散列算法.

* MAC (Message Authentication Code): 
    消息认证代码, 是一种使用密钥的单向函数, 可以用它们在系统上或用户之间认证文件或消息, 常见的是HMAC (用于消息认证的密钥散列算法).

* CRC (Cyclic Redundancy Check): 
    循环冗余校验码, CRC校验由于实现简单, 检错能力强, 被广泛使用在各种数据校验应用中. 
    占用系统资源少, 用软硬件均能实现, 是进行数据传输差错检测地一种很好的手段 (CRC 并不是严格意义上的散列算法, 但它的作用与散列算法大致相同, 所以归于此类).



数字签名: 结合使用公钥与散列算法
以下常见签名的过程是, 先对长文本做摘要(digest), 对摘要做私钥加密得到签名(signature), 然后将加密结果(签名signature)和公钥一同发布出去. 
散列算法处理数据的速度比公钥算法快得多. 散列数据还缩短了要签名的数据的长度, 因而加快了签名过程.



密钥交换: 结合使用对称密钥与公钥
对称密钥算法非常适合于快速并安全地加密数据.
但其缺点是, 发件人和收件人必须在交换数据之前先交换机密密钥.
结合使用加密数据的对称密钥算法与交换机密密钥的公钥算法可产生一种既快速又灵活的解决方案.











X.509 证书一般会用到三类文件: key, csr, crt
key: 是私钥 openssl 格式, 通常是rsa算法.
csr: 是证书请求文件, 用于申请证书. 在制作csr文件的时, 必须使用自己的私钥来签署申, 还可以设定一个密钥.
crt: 是CA认证后的证书文, 签署人用自己的key给你签署的凭证.
这三类文件彼此之间没有必须的顺序, 只有在不同情境下的使用组合.

key 私钥生成: 
    openssl genrsa -des3 -out private.key 2048
    生成rsa私钥, des3算法, openssl格式, 2048位强度, private.key是密钥文件名, 为了生成这样的密钥, 需要一个至少四位的密码. 
    可以通过以下方法生成没有密码的key: openssl rsa -in private.key -out private.key 
    private.key 就是没有密码的版本了.

csr 申请文件: 
    openssl req -new -key private.key -out xxxx.csr
    需要依次输入国家, 地区, 组织, email. 
    最重要的是有一个common name, 可以写你的名字或者域名.
    如果为了https申请, 这个必须和域名吻合, 否则会引发浏览器警报. 
    生成的csr文件交给CA签名后形成服务端自己的证书.

crt 证书文件, cert:
    openssl x509 -req -days 3650 -sha1 -extensions v3_ca -signkey private.key -in private.csr -out private.crt
    openssl x509 -req -days 730 -sha1 -extensions v3_req -CA root.crt -CAkey root.key -CAcreateserial -in server.csr -out server.crt
    

    CSR文件必须有CA的签名才可形成证书, 可将此文件发送到verisign等地方由它验证, 要交一大笔钱, 何不自己做CA呢.
    openssl x509 -req -days 3650 -in server.csr -CA ca.crt -CAkey server.key -CA createserial -out server.crt
    输入key的密钥后, 完成证书生成. 
    -CA 选项指明用于被签名的csr证书,
    -CAkey 选项指明用于签名的密钥,
    -CAserial 指明序列号文件,
    -CAcreateserial 指明文件不存在时自动生成.
    最后生成了私用密钥: server.key 和自己认证的SSL证书: server.crt
    证书合并: 
    catserver.key server.crt > server.pem


私钥和证书合并:
    openssl pkcs12 -export -in client.crt -inkey client.key -out client.pfx
    catserver.key server.crt > server.pem   


Linux系统中的Openssl工具可以用来生成自签名证书，实现通信的加密，也可以基于自签名的CA证书模拟实际CA信任链的工作过程


自签名证书分为:
1. 自签名 私有证书

2. 自签名 CA 证书

  CA 机构, 又称为证书认证中心 (Certificate Authority) 中心
  自签名私有证书无法被吊销，自签名CA证书可以被吊销. 
  为什么吊销, 吊销的必要性: 私钥泄露后挽救的办法, 否则就无能为力了.
  如果你的规划需要创建多个客户端证书, 那么使用自建 CA 签名证书的方法比较合适, 只要给所有的客户端都安装了 CA 根证书, 那么以该 CA 根证书签名过的客户端证书都是信任的, 不需要重复的安装客户端证书.
  请注意由于是自建 CA 证书, 在使用这个临时证书的时候, 会在客户端浏览器报一个错误, 签名证书授权未知或不可（signing certificate authority is unknown and not trusted.）,
  但只要配置正确, 继续操作并不会影响正常通信.
  自签名证书的 Issuer 和 Subject 是一样的. 


1. 生成自签名私有证书
   1.1. 生成私钥
    openssl genrsa -out /path/to/keyfile 2048
   1.2. 私钥加密
    用于备份或者转移私钥
    openssl rsa -in /path/to/keyfile -des3 -out /path/to/encrypted.key
   1.3. 生成证书签名请求文件(Certificate Sign Request)
    生成 CSR 的过程中, 会提示输入一些信息, 包括: 序列号、公钥、用户名称、签发者、CA签名和其他一些附属信息等. 
    证书验证过程就是依赖于这信息和公钥对应的私钥进行.
    其中一个提示是 Common Name (e.g. YOUR name), 这个非常重要, 这一项应填入 FQDN(Fully Qualified Domain Name)完全合格域名/全称域名,
    如果您使用 SSL 加密保护网络服务器和客户端之间的数据流, 举例被保护的网站是 https://test.example.cn, 那么此处 Common Name 应输入 test.example.cn
    openssl req -new -key /path/to/keyfile -out /path/to/csrfile
   1.4. 使用上一步的 证书签名请求文件 签发证书(Certificate)
    openssl x509 -req -in /path/to/ssl.csr -signkey /path/to/ssl.key -out /path/to/ssl.crt
   1.5. 查看证书信息
    openssl x509 -in /path/to/certfile -noout -text
   
   ```shell
   [root@dev3_10.211.21.18 tmp]# openssl x509 -in ./server.crt -noout -text
   Certificate:
       Data:
           Version: 1 (0x0)
           Serial Number:
               bf:a9:07:0b:87:84:67:88
       Signature Algorithm: sha1WithRSAEncryption
           Issuer: C=CN, ST=BeiJing, L=BeiJing, O=SinaSports Corp.
           Validity
               Not Before: Dec 12 07:54:51 2018 GMT
               Not After : Dec 12 07:54:51 2019 GMT
           Subject: C=CN, ST=BeiJing, L=BeiJing, O=SinaSports Corp.
           Subject Public Key Info:
               Public Key Algorithm: rsaEncryption
                   Public-Key: (1024 bit)
                   Modulus:
                       00:b9:c3:9f:9b:62:03:54:64:ff:7a:c7:ff:d1:97:
                       c5:75:be:3a:fd:8a:ed:00:ec:f3:88:f9:66:7c:0d:
                       34:1d:20:c6:48:ff:f6:b6:59:00:a7:32:30:f3:62:
                       cb:ed:9a:08:6a:2d:e2:a1:3a:58:6f:cb:9e:fe:ee:
                       7d:14:eb:a5:d6:33:5c:c2:7d:5e:64:31:55:23:c0:
                       95:e7:16:d4:1c:9f:d7:45:88:f5:5e:52:15:28:4d:
                       86:12:02:76:d4:bb:4b:0e:15:46:2d:10:42:dc:14:
                       61:13:fc:61:78:e6:6f:0e:1d:ca:34:c6:0c:56:25:
                       27:ee:b3:d3:66:ee:3f:b5:47
                   Exponent: 65537 (0x10001)
       Signature Algorithm: sha1WithRSAEncryption
            09:d5:06:cb:b7:db:27:90:7b:41:58:64:a6:91:67:95:f4:15:
            9f:f3:02:1a:a2:8c:e2:d8:a4:c0:7e:ad:07:b1:d8:00:32:e4:
            5d:37:99:ff:6f:78:c9:d9:13:9b:0c:02:86:bf:81:ae:c2:ae:
            5d:66:e0:29:a6:21:64:ca:64:16:f1:60:c0:77:bb:86:92:69:
            b8:a4:6b:46:8e:f5:ef:0a:65:e8:f4:5e:0d:53:a0:91:f3:a3:
            05:e2:96:1a:6d:72:c7:17:f3:19:c7:3b:5d:e2:81:fc:c0:8b:
            bb:a3:7e:25:bc:78:1a:ba:f1:82:30:15:68:fc:9d:92:63:a2:
            70:f5
   [root@dev3_10.211.21.18 tmp]# 
   ```
   
   
* 上面的1-4步也可以通过命令行合并为一步实现(有问题, 不灵)
    openssl req -new -x509 -newkey rsa:2048 -keyout /path/to/server.key -out /path/to/server.crt
    
    ```shell
    [root@dev3_10.211.21.18 tmp]# mkdir ssl20221024
    [root@dev3_10.211.21.18 tmp]# openssl req -new -x509 -newkey rsa:2048 -keyout ~/tmp/ssl20221024/server.key -out ~/tmp/ssl20221024/server.crt
    Generating a 2048 bit RSA private key
    .................................+++
    ..........................................+++
    writing new private key to '/usr/home/chenchen/tmp/ssl20221024/server.key'
    Enter PEM pass phrase:
    139720476161952:error:28069065:lib(40):UI_set_result:result too small:ui_lib.c:831:You must type in 4 to 1024 characters
    139720476161952:error:0906406D:PEM routines:PEM_def_callback:problems getting password:pem_lib.c:116:
    139720476161952:error:0907E06F:PEM routines:DO_PK8PKEY:read key:pem_pk8.c:130:
    [root@dev3_10.211.21.18 tmp]# l
    ```
    
    


2. 生成自签名 CA 证书
  创建自签 CA 证书，主要分为两个部分:
3. 创建 CA 根证书
4. 签发客户端证书

2.1. 创建 CA 根证书
    使用 OpenSSL 可以创建自己的 CA, 给需要验证的用户或服务器颁发证书, 这就需要创建一个 CA 根证书. 
    默认情况 ubuntu 和 CentOS 上都已安装好 openssl. CentOS 6.x 上有关 ssl 证书的目录结构.

​    /etc/pki/CA/
​            newcerts    存放CA签署（颁发）过的数字证书（证书备份目录）
​            private     用于存放CA的私钥
​            crl         吊销的证书



    /etc/pki/tls/
            cert.pem    软链接到 certs/ca-bundle.crt
            certs/      该服务器上的证书存放目录, 可以房子自己的证书和内置证书
            ca-bundle.crt   内置信任的证书
            private         证书密钥存放目录
            openssl.cnf     openssl 的 CA 主配置文件


    在创建 CA 根证书之前, 需要做好如下准备工作: 修改好 CA 的配置文件、序列号、索引等等. 
    touch index.txt serial
    echo 01 >> serial
    openssl 的配置文件为 openssl.cnf, 一般存储在 /etc/pki/tls/ 目录下. 
    一定要注意配置文件中 [ policy_match ]标签下设定的匹配规则. 
    有可能因为证书使用的工具不一样, 导致即使设置了csr中看起来有相同的countryName, stateOrProvinceName等, 但在最终生成证书时依然报错. 
    一般情况下, 配置文件不需要改动.



2.1.1. 生成 CA 根证书密钥
    cd /etc/pki/CA/
    openssl genrsa -out ./private/ca.key 2048

```shell
[chenchen@localhost CA]$ sudo openssl genrsa -out ./private/ca01.key 2048
Generating RSA private key, 2048 bit long modulus
.................................................................................................................+++
.+++
e is 65537 (0x10001)
[chenchen@localhost CA]$ l
total 0
      86 drwxr-xr-x. 10 root root 116 Jan  6  2020 ..
13072775 drwxr-xr-x.  2 root root   6 Dec 17  2020 newcerts
 8734194 drwxr-xr-x.  2 root root   6 Dec 17  2020 crl
 4474238 drwxr-xr-x.  2 root root   6 Dec 17  2020 certs
  218828 drwxr-xr-x.  6 root root  61 Dec 17  2020 .
  218829 drwx------.  2 root root  22 Aug 30 19:33 private
[chenchen@localhost CA]$ sudo tree
.
├── certs
├── crl
├── newcerts
└── private
    └── ca01.key
```

2.1.2. 生成 CA 根证书请求
    openssl req -new -in /path/to/ca.key -out ca.csr

```shell
[chenchen@localhost CA]$ l
total 0
      86 drwxr-xr-x. 10 root root 116 Jan  6  2020 ..
13072775 drwxr-xr-x.  2 root root   6 Dec 17  2020 newcerts
 8734194 drwxr-xr-x.  2 root root   6 Dec 17  2020 crl
 4474238 drwxr-xr-x.  2 root root   6 Dec 17  2020 certs
  218829 drwx------.  2 root root  22 Aug 30 19:33 private
  218828 drwxr-xr-x.  6 root root  80 Aug 30 19:35 .
[chenchen@localhost CA]$ less privkey.pem 
[chenchen@localhost CA]$ sudo openssl req -new -in ./private/ca01.key -out ca01.csr
Generating a 2048 bit RSA private key
........................................................................................+++
...........................+++
writing new private key to 'privkey.pem'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:cn
State or Province Name (full name) []:beijing
Locality Name (eg, city) [Default City]:beijing
Organization Name (eg, company) [Default Company Ltd]:sina
Organizational Unit Name (eg, section) []:sports 
Common Name (eg, your name or your server's hostname) []:sports.sina.com.cn
Email Address []:chenchen@staff.sina.com   

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:sina
[chenchen@localhost CA]$ l
total 8.0K
      86 drwxr-xr-x. 10 root root  116 Jan  6  2020 ..
13072775 drwxr-xr-x.  2 root root    6 Dec 17  2020 newcerts
 8734194 drwxr-xr-x.  2 root root    6 Dec 17  2020 crl
 4474238 drwxr-xr-x.  2 root root    6 Dec 17  2020 certs
  218829 drwx------.  2 root root   22 Aug 30 19:33 private
  803094 -rw-r--r--.  1 root root 1.8K Aug 30 19:38 privkey.pem			# 加密后的私钥, 私钥在 ./private 目录, 加密密码是生成 .pem 时要求输入的密码
  803095 -rw-r--r--.  1 root root 1.1K Aug 30 19:38 ca01.csr				# 请求文件
  218828 drwxr-xr-x.  6 root root   96 Aug 30 19:38 .
[chenchen@localhost CA]$ less privkey.pem 
[chenchen@localhost CA]$ 
```

2.1.3. 生成 CA 根证书
    #用密钥自签名, 生成 CA 证书
    openssl x509 -req -in /path/to/ca.csr -signkey /path/to/ca.key -extensions v3_ca -out /path/to/ca.crt
    签发证书

```shell
[chenchen@localhost CA]$ sudo openssl x509 -req -in ./ca01.csr -signkey ./private/ca01.key -extensions v3_ca -out ./certs/ca01.crt
Signature ok
subject=/C=cn/ST=beijing/L=beijing/O=sina/OU=sports/CN=sports.sina.com.cn/emailAddress=chenchen@staff.sina.com
Getting Private key
[chenchen@localhost CA]$ sudo tree
.
├── ca01.csr
├── certs
│   └── ca01.crt
├── crl
├── newcerts
├── private
│   └── ca01.key
└── privkey.pem

4 directories, 4 files
[chenchen@localhost CA]$
[chenchen@localhost CA]$ l
total 8.0K
      86 drwxr-xr-x. 10 root root  116 Jan  6  2020 ..
13072775 drwxr-xr-x.  2 root root    6 Dec 17  2020 newcerts
 8734194 drwxr-xr-x.  2 root root    6 Dec 17  2020 crl
  218829 drwx------.  2 root root   22 Aug 30 19:33 private
  803094 -rw-r--r--.  1 root root 1.8K Aug 30 19:38 privkey.pem
  803095 -rw-r--r--.  1 root root 1.1K Aug 30 19:38 ca01.csr
  218828 drwxr-xr-x.  6 root root   96 Aug 30 19:38 .
 4474238 drwxr-xr-x.  2 root root   22 Aug 30 19:56 certs
[chenchen@localhost CA]$
```



```shell
# 自己创造一个 “CA” 证书流程:
1. 在 /etc/pki/CA/private 目录(700)下创建 私钥.key 文件
2. 在 /etc/pki/CA 目录下创建 申请.csr 文件 和 privkey.pem
3. 在 /etc/pki/CA/certs 目录下创建 证书.crt 文件

注: 在 /etc/pki/CA 目录下在第二步时会生成 privkey.pem 文件, 我理解这单纯是 /etc/pki/CA/private/私钥.key 的加密结果(用于移动和复制, 不应该传输未加密的私钥; 另外当继续生成不同证书时, 此 privkey.pem 文件会不断被覆盖, 文件名不变, 可见 privkey.pem 是个临时文件, 可以随便删除, 我估计有参数可以指定 privkey.pem 文件名, 只是默认叫这个名字罢了)
```



```shell
我们可以用CA根证书签发证书, 也可以创建中间CA, 使用中间CA签发证书.
创建中间CA的好处是即使中间CA的私钥泄露, 造成的影响也是可控的, 我们只需要使用root CA撤销对应中间CA的证书即可.
此外root CA的私钥可以脱机妥善保存, 只需要在撤销和更新中间CA证书时才会使用.
基于根证书创建中间CA与创建根证书过程类似. 我们新建一个目录用于保存中间证书信息.

#准备环境
mkdir /etc/pki/CA/intermediate
cd /etc/pki/CA/intermediate
mkdir certs crl newcerts private
chmod 700 private
touch index.txt
echo 1000 > serial

#生成密钥
cd /etc/pki/CA
openssl genrsa -aes256 -out intermediate/private/ca.key 2048
#新建请求
openssl req -config intermediate_CA.cnf -sha256 -new -key intermediate/private/ca.key -out intermediate/certs/ca.csr
#签发中间CA证书
openssl ca -config root_CA.cnf -extensions v3_ca -notext -md sha256 -in intermediate/certs/ca.csr -out intermediate/certs/ca.cert
```



利用 根证书 或者 中间证书 签发客户端证书的步骤如下:

即, 用 ca01.crt 根证书签发 ca02.crt 证书

1. 新建 sc02 证书密钥
    openssl genrsa -out /path/to/server.key 2048

  ```shell
  [chenchen@localhost CA]$ l
  total 8.0K
        86 drwxr-xr-x. 10 root root  116 Jan  6  2020 ..
  13072775 drwxr-xr-x.  2 root root    6 Dec 17  2020 newcerts
   8734194 drwxr-xr-x.  2 root root    6 Dec 17  2020 crl
    218829 drwx------.  2 root root   22 Aug 30 19:33 private
    803094 -rw-r--r--.  1 root root 1.8K Aug 30 19:38 privkey.pem
    803095 -rw-r--r--.  1 root root 1.1K Aug 30 19:38 ca01.csr
    218828 drwxr-xr-x.  6 root root   96 Aug 30 19:38 .
   4474238 drwxr-xr-x.  2 root root   22 Aug 30 19:56 certs
  [chenchen@localhost CA]$ sudo openssl genrsa -out ./private/sc02.key 2048
  Generating RSA private key, 2048 bit long modulus
  .......+++
  .............+++
  e is 65537 (0x10001)
  [chenchen@localhost CA]$ sudo tree
  .
  ├── ca01.csr
  ├── certs
  │   └── ca01.crt
  ├── crl
  ├── newcerts
  ├── private
  │   ├── ca01.key
  │   └── sc02.key
  └── privkey.pem
  
  4 directories, 5 files
  [chenchen@localhost CA]$ 
  ```

2. 新建 sc02 证书请求
   openssl req -new -in /path/to/server.key -out /path/to/server.csr

  ```shell
  [chenchen@localhost CA]$ sudo openssl req -new -in ./private/sc02.key -out ./sc02.csr
  Generating a 2048 bit RSA private key
  ...................................................+++
  ........................................................................................+++
  writing new private key to 'privkey.pem'
  Enter PEM pass phrase:
  Verifying - Enter PEM pass phrase:
  -----
  You are about to be asked to enter information that will be incorporated
  into your certificate request.
  What you are about to enter is what is called a Distinguished Name or a DN.
  There are quite a few fields but you can leave some blank
  For some fields there will be a default value,
  If you enter '.', the field will be left blank.
  -----
  Country Name (2 letter code) [XX]:cn
  State or Province Name (full name) []:beijing
  Locality Name (eg, city) [Default City]:beijing
  Organization Name (eg, company) [Default Company Ltd]:sina
  Organizational Unit Name (eg, section) []:sports
  Common Name (eg, your name or your server's hostname) []:sports.sina.com.cn
  Email Address []:chenchen@staff.sina.com
  
  Please enter the following 'extra' attributes
  to be sent with your certificate request
  A challenge password []:
  An optional company name []:sina
  [chenchen@localhost CA]$ l
  total 12K
        86 drwxr-xr-x. 10 root root  116 Jan  6  2020 ..
  13072775 drwxr-xr-x.  2 root root    6 Dec 17  2020 newcerts
   8734194 drwxr-xr-x.  2 root root    6 Dec 17  2020 crl
    803095 -rw-r--r--.  1 root root 1.1K Aug 30 19:38 ca01.csr
   4474238 drwxr-xr-x.  2 root root   22 Aug 30 19:56 certs
    218829 drwx------.  2 root root   38 Aug 30 20:33 private
    803094 -rw-r--r--.  1 root root 1.8K Aug 30 20:37 privkey.pem
    803097 -rw-r--r--.  1 root root 1.1K Aug 30 20:37 sc02.csr
    218828 drwxr-xr-x.  6 root root  112 Aug 30 20:37 .
  [chenchen@localhost CA]$ sudo tree
  .
  ├── ca01.csr
  ├── certs
  │   └── ca01.crt
  ├── crl
  ├── newcerts
  ├── private
  │   ├── ca01.key
  │   └── sc02.key
  ├── privkey.pem
  └── sc02.csr
  
  4 directories, 6 files
  [chenchen@localhost CA]$
  ```

  

3. 用 ca01 证书签发 sc02 证书
   openssl x509 -req -in /path/to/server.csr -CA /path/to/ca.crt -CAkey /path/to/ca.key -CAcreateserial -out /path/to/server.crt

  ```shell
  [chenchen@localhost CA]$ sudo openssl x509 -req -in ./sc02.csr -CA ./certs/ca01.crt -CAkey ./private/ca01.key -CAcreateserial -out ./certs/sc02.crt
  Signature ok
  subject=/C=cn/ST=beijing/L=beijing/O=sina/OU=sports/CN=sports.sina.com.cn/emailAddress=chenchen@staff.sina.com
  Getting CA Private Key
  [chenchen@localhost CA]$ sudo tree
  .
  ├── ca01.csr
  ├── certs
  │   ├── ca01.crt
  │   └── sc02.crt
  ├── crl
  ├── newcerts
  ├── private
  │   ├── ca01.key
  │   └── sc02.key
  ├── privkey.pem
  └── sc02.csr
  
  4 directories, 7 files
  [chenchen@localhost CA]$ l
  total 16K
        86 drwxr-xr-x. 10 root root  116 Jan  6  2020 ..
  13072775 drwxr-xr-x.  2 root root    6 Dec 17  2020 newcerts
   8734194 drwxr-xr-x.  2 root root    6 Dec 17  2020 crl
    803095 -rw-r--r--.  1 root root 1.1K Aug 30 19:38 ca01.csr
    218829 drwx------.  2 root root   38 Aug 30 20:33 private
    803094 -rw-r--r--.  1 root root 1.8K Aug 30 20:37 privkey.pem
    803097 -rw-r--r--.  1 root root 1.1K Aug 30 20:37 sc02.csr
    803098 -rw-r--r--.  1 root root   17 Aug 30 20:46 .srl
   4474238 drwxr-xr-x.  2 root root   38 Aug 30 20:46 certs
    218828 drwxr-xr-x.  6 root root  124 Aug 30 20:46 .
  [chenchen@localhost CA]$ 
  ```

  

4. 用 CA证书 验证 sc03.crt 证书是否有效
   openssl verify -CAfile /path/to/ca.crt /path/to/server.crt

```shell
## 不正常, error 18 at 0, 
## 原因: 必须两个证书的 Common Name 值不同. 相同就有报错提示和OK
[chenchen@localhost CA]$ sudo openssl verify -CAfile ./certs/ca01.crt ./certs/sc02.crt
./certs/sc02.crt: C = cn, ST = beijing, L = beijing, O = sina, OU = sports, CN = sports.sina.com.cn, emailAddress = chenchen@staff.sina.com
error 18 at 0 depth lookup:self signed certificate
OK
## 正常, 只显示 OK, 说明我们颁发的证书是有效的
[chenchen@localhost CA]$ sudo openssl verify -CAfile ./certs/ca01.crt ./certs/sc03.crt
./certs/sc03.crt: OK
```




不同客户端证书格式转换

```shell
# crt, pem 格式证书可用于 linux / nginx / node.js 格式客户端
# p12(pkcs12) 格式证书用于 tomcat / java / android 客户端

# 打包为 p12 证书
openssl pkcs12 -export -in /path/to/server.crt -inkey /path/to/server.key -out /path/to/server.p12

# 吊销证书
# 当证书已经不再使用或者密钥泄露时, 我们需要吊销证书来保证服务器的安全
# 获取要吊销的证书的 serial
openssl x509 -in /path/to/x.crt -noout -serial -subject
[chenchen@localhost CA]$ sudo openssl pkcs12 -export -in ./certs/ca01.crt -inkey ./private/ca01.key -out ./certs/ca01.p12
Enter Export Password:
Verifying - Enter Export Password:
[chenchen@localhost CA]$ sudo tree
.
├── ca01.csr
├── certs
│   ├── ca01.crt
│   ├── ca01.p12
│   ├── sc02.crt
│   ├── sc02.p12
│   ├── sc03.crt
│   └── sc03.p12
├── crl
├── newcerts
├── private
│   ├── ca01.key
│   ├── sc02.key
│   └── sc03.key
├── privkey.pem
├── sc02.csr
└── sc03.csr
[chenchen@localhost CA]$ sudo openssl pkcs12 -export -in ./certs/sc03.crt -inkey ./private/sc03.key -out ./certs/sc03.p12
No certificate matches private key
这是因为sc03证书是用ca01证书签发的. 证书签证书怎么办? 

# 对比 serial 与 subject 信息是否与 index.txt 中的信息一致
# 如果一致, 则可以吊销证书
openssl ca -revoke /etc/pki/CA/newcerts/SERIAL.pem

# 如果是第一次吊销证书, 需要指定吊销的证书编号
echo 01 > /etc/pki/CA/crlnumber

# 更新吊销证书列表
openssl ca -gencrl -out /etc/pki/CA/crl.pem

# 完成后, 可查看吊销的证书列表
openssl crl -in /etc/pki/CA/crl.pem -noout -text
```



