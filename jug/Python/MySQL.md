# MySQL

[toc]



## Overview

### [PyMySQL](https://pymysql.readthedocs.io/en/latest/index.html)

```python
#! /usr/bin/env python
# -*- coding:UTF-8 -*-

# @see: https://pymysql.readthedocs.io/en/latest/
import pymysql

'''
match.sports.sina.com.cn matchsports matchsports_r N8k9XewOXGNDZ s3338i.mars.grid.sina.com.cn 3338
'''

con = pymysql.connect(host = '10.211.21.19',
                      port = 3307,
                      user = 'root',
                      password = 'root007',
                      database = 'matchsports',
                      charset = '',
                      cursorclass = pymysql.cursors.DictCursor)  # 使用游标类
''' 使用的游标类: 
Cursor - 返回结果为元组类型
DictCursor - 返回结果为字典类型
SSCursor - 返回元组类型, Python 不缓存结果, 合适处理大量数据, 但对内存敏感的应用不友好，也不支持fetchmany() 和 scroll() 方法
SSDictCursor - 返回字典类型, Python 不缓存结果, 合适处理大量数据, 但对内存敏感的应用不友好，也不支持fetchmany() 和 scroll() 方法
'''
                      
print(con.cursor())
with con:
    with con.cursor() as cursor:
        # sql = 'select distinct `id` from `autolive_team_players` where `league_id`=2022 and `type_id`=23'
        sql = 'select * from `autolive_team_players` where `league_id`=2022 and `type_id`=23'
        num = cursor.execute(sql)
        res = cursor.fetchall()
        print(num, res)
```





## FAQ





## See Also

https://pymysql.readthedocs.io/en/latest/

[PyMySQL](https://pymysql.readthedocs.io/en/latest/index.html)

