# Go Driver connect to a MongoDB instance

---



[toc]



## Connection URI

![Each part of the connection string](https://docs.mongodb.com/drivers/go/current/includes/figures/connection_uri_parts.png)

mongodb: protocol, 规定了 [Standard Connection String Format](https://docs.mongodb.com/manual/reference/connection-string/#std-label-connections-standard-connection-string-format)

credentials: 证书

connection options: 规定了连接和验证选项, [Connection Options](https://docs.mongodb.com/drivers/go/current/fundamentals/connection/#std-label-connection-options)



### Connect to a Replica Set

MongoDB 副本集部署是一组存储相同数据集的连接实例. 此配置提供数据 redundancy (冗余)和高数据可用性.

```shell
# 要确保在指定主机不可用时具有连接性, 应提供完整的主机列表
$ mongodb://host1:27017,host2:27017,host3:27017/?replicaSet=myRS
```

> 连接到副本集时, 驱动程序默认执行以下操作:
>
> 1. 在给定任何一个成员的地址时发现所有副本集成员. 
>
>    意思是给驱动一个成员, 驱动就能通过这个成员发现副本集中的其他所有成员
>
> 2. 将操作分派给相应的成员, 如针对主成员写入(w)指令
>
>    意思是给驱动一个列表, 当发送写入操作指令时, 会自动从主成员来写入w, 不用特比指定



## Direct Connection

直链:

. 不支持 SRV 字符串

. 当特定的主机不是 primary 主时, 会写w失败

. 当指定的主机不是 primary 主时, 需要你指定 **secondary** 选项 



## Connection Options

连接和验证选项

**replicaSet**, 副本集名字. string, 默认null

**w**, 指定写concern. string or integer, 默认null, [Write Concern options](https://docs.mongodb.com/manual/reference/write-concern/)

**connectTimeoutMS**, 毫秒, TCP连接超时时间. integer, 默认30000

**directConnection**, 是否强制发送所有选线给主机. boolean, 默认false



## See Also

Read Preference: https://docs.mongodb.com/manual/core/read-preference/#read-preference

https://docs.mongodb.com/manual/core/read-preference/#mongodb-readmode-secondary

[Write Concern options](https://docs.mongodb.com/manual/reference/write-concern/)



