# Import Data into MongoDB

---



[TOC]



## Overview

用 MongoDB 连带发行的 mongoimport 工具大块的导入数据到 MongoDB 实例里.

### Check Your Environment

```shell
# 确保 MongoDB 实例正在运行并且是可访问的
ps -e| grep 'mongod'
89780 ttys026    0:53.48 ./mongod
```

### Procedure 步骤

```json
1. 准备好将要导入的数据文件, .json 文件, 文件内容如下
# inventory.crud.json 文件
{ "item": "journal", "qty": 25, "size": { "h": 14, "w": 21, "uom": "cm" }, "status": "A" }
{ "item": "notebook", "qty": 50, "size": { "h": 8.5, "w": 11, "uom": "in" }, "status": "A" }
{ "item": "paper", "qty": 100, "size": { "h": 8.5, "w": 11, "uom": "in" }, "status": "D" }
{ "item": "planner", "qty": 75, "size": { "h": 22.85, "w": 30, "uom": "cm" }, "status": "D" }
{ "item": "postcard", "qty": 45, "size": { "h": 10, "w": 15.25, "uom": "cm" }, "status": "A" }
```

```shell
2. 通过 mongoimport 工具导入数据到 xxx collection(表)中
# mongoimport 工具位于 MongoDB 仓库的 /bin 目录下
# 参数说明:
--host
--port
--drop, 导入前先删除已经存在的 collection, 这样可以保证导入 collection 中的数据足够干净.


mongoimport --db test --collection inventory \
          --authenticationDatabase admin --username <user> --password <password> \
          --drop --file ~/Downloads/inventory.crud.json
```



## Summary



## See Also

[Import Data into MongoDB](https://docs.mongodb.com/guides/server/import/)

[Install MongoDB](https://docs.mongodb.com/guides/server/install/)

