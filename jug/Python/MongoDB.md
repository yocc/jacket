# MongoDB

[toc]





## Overview

```py
# tutorial
# https://pymongo.readthedocs.io/en/stable/tutorial.html

# pymongo
uri = 'mongodb://10.211.21.19:27018/sport_nm?replicaSet=Sport'

# 两种方法均可. inserted_ids 是获取插入后生成的新 oid.
# 第一种写法
# rs = MongoClient(uri).sport_nm.football_category_list.insert_many(com)
rid = MongoClient(uri).sport_nm.football_category_list.insert_many(com).inserted_ids
print(type(rid), "\n", rid)

# 第二种写法
# client = MongoClient(uri)
# db = client.sport_nm
# collection = db.football_category_list
# rs = collection.insert_many(com)
# rid = rs.inserted_ids
# print(type(rs), "\n", rs, type(rid), rid)

# 注意
rs = MongoClient(wb_db_mongo_dev_01["uri"]).nm_api_dev_01["category_list"]["mongodb"]["db"].nm_api_dev_01["category_list"]["mongodb"]["collection"].insert_many(arr).inserted_ids # 不工作, 生成的库是错的. 需要用字典风格访问.
rs = MongoClient(wb_db_mongo_dev_01["uri"])[nm_api_dev_01["category_list"]["mongodb"]["db"]][nm_api_dev_01["category_list"]["mongodb"]["collection"]].insert_many(arr).inserted_ids # 工作

rs = client.db.collection.insert_many(arr).inserted_ids # 此法不工作, 需要用字典风格访问, 如下.
rs = client[db][collection].insert_many(arr).inserted_ids


返回:
<class 'list'>
[ObjectId('64abeb6221f10a9e49a4eea6'), ObjectId('64abeb6221f10a9e49a4eea7'), ObjectId('64abeb6221f10a9e49a4eea8'), ObjectId('64abeb6221f10a9e49a4eea9'), ObjectId('64abeb6221f10a9e49a4eeaa'), ObjectId('64abeb6221f10a9e49a4eeab'), ObjectId('64abeb6221f10a9e49a4eeac')]

返回的 _id 正好放在 mongodb 命令行里用: 
db.getCollection('football_category_list').find({_id:ObjectId('64abeb6221f10a9e49a4eea6')})

# shell cli
[chenchen@dev3_10.211.21.18 glintaz2023.sports.weibo.cn]$ mongo mongodb://10.211.21.19:27018/sport_nm?replicaSet=Sport
Sport:PRIMARY> show dbs
Sport:PRIMARY> use sport_nm
switched to db sport_nm
Sport:PRIMARY> db.getCollection('football_category_list').find({_id:ObjectId('64abeb6221f10a9e49a4eea6')})
{ "_id" : ObjectId("64abeb6221f10a9e49a4eea6"), "id" : 1, "name_zh" : "国际", "name_zht" : "國際", "name_en" : "International", "updated_at" : 1539164101 }
Sport:PRIMARY> 
```



## Troubleshooting









## FAQ









## See Also

https://www.mongodb.com/docs/drivers/pymongo/

https://pymongo.readthedocs.io/en/stable/tutorial.html mongodb 写库