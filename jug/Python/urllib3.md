







```py
# docCookie2file.py
-----------------------------
#! /usr/bin/env python
# -*- coding:UTF-8 -*-


"""
    通过用户名密码登录后台, 获取 cookie, 然后去访问接口文档集合接口
"""

"""
    https://www.nami.com/docs?id=1002
    https://www.nami.com/api/v2/doc/get?id=1002
"""


import ssl
import certifi
import urllib.parse
import urllib3
import json
import sys 



def main():
    print(certifi.where())
    cdt()
    


def cdt():
    http = urllib3.PoolManager(
        cert_reqs   = "CERT_REQUIRED",
        ca_certs    = certifi.where(),
    )
        
    loginUrl = 'https://www.nami.com/api/v1/account/login'
    loginFields = {
        'login_name':   'iosdev@staff.caitong.sina.com',
        'password':     'Lottery2021',
    }   
    loginResp = http.request('POST', loginUrl, fields = loginFields, headers = {}, timeout = urllib3.Timeout(connect = 1.0, read = 2.0))
    
    # print(type(loginResp.headers["Set-Cookie"]), loginResp.headers["Set-Cookie"])
    
    pat = '/data1/www/htdocs/chenchen/python31011/workspace/nano/assets/cookie.txt'
    with open(pat, 'w', encoding = 'utf-8') as f:
        f.write(loginResp.headers['Set-Cookie'])
    
    
if __name__ == '__main__': 
    main()
```





```py
# docCfg2file.py
------------------------------
#! /usr/bin/env python
# -*- coding:UTF-8 -*-


"""
    通过用户名密码登录后台, 获取 cookie, 然后去访问接口文档集合接口
"""

"""
    https://www.nami.com/docs?id=1002
    https://www.nami.com/api/v2/doc/get?id=1002
"""



import ssl
import certifi
import urllib.parse
import urllib3
import json
import sys



def main():
    print(certifi.where())
    cdt()



def cdt():
    
    pat = '/data1/www/htdocs/chenchen/python31011/workspace/nano/assets/cookie.txt'
    with open(pat, 'r') as f:
        hea = f.read()

    print(type(hea), hea)
    
    http1 = urllib3.PoolManager(
        cert_reqs   = "CERT_REQUIRED",
        ca_certs    = certifi.where(),
    )
    # http1 = urllib3.PoolManager(cert_reqs = "CERT_NONE")
    
    url = 'https://www.nami.com/api/v2/doc/get?id=1002'
    resp = http1.request('GET', url, headers = {'Cookie': hea}, timeout = urllib3.Timeout(connect = 1.0, read = 2.0))
    print(resp.data.decode('raw_unicode_escape'))
    
    pkf = '/data1/www/htdocs/chenchen/python31011/workspace/nano/assets/doc_1002.json'
    with open(pkf, 'w') as f:
        # f.write(resp.data.decode('raw_unicode_escape'))
        f.write(resp.data.decode('utf-8'))
    
if __name__ == '__main__':
    main()
```

