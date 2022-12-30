# FAQ MongoDB

----



## FAQ

1. 创建索引

   查看表索引
   db.predict.getIndexes()

   

   db.predict.ensureIndex({'fid':1}, {unique:true, background:true})
   db.predict.ensureIndex({'mad':1}, {background:true})
   db.predict.ensureIndex({'ncd':1}, {background:true})
   db.predict.ensureIndex({'sna.Lottery.MatchTime':1}, {background:true})
   db.predict.ensureIndex({'isend':1}, {background:true})
   db.predict.ensureIndex({'ispause':1}, {background:true})


   对 ‘fid’ 和 ‘cowry.ouo’ 字段创建唯一索引





db.peadesire.getIndexes()

db.peadesire.ensureIndex({'cowry.eid':1, 'cowry.ouo':1}, {unique:true, background:true})

对 ‘cowry.eid’ 和 ‘cowry.ouo’ 字段创建唯一索引

##  KK







## See Also

