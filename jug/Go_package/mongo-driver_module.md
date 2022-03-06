# mongo-driver

---



[toc]



## Overview

1. import ()

   ```go
   import (
     	"go.mongodb.org/mongo-driver/bson"
       "go.mongodb.org/mongo-driver/mongo"
       "go.mongodb.org/mongo-driver/mongo/options"
       "go.mongodb.org/mongo-driver/mongo/readpref"
   )
   ```

2. mongo.Connect() , 使用 Connect() 函数创建 mongo.Client

   ```go
   client, err := mongo.Connect(ctx, options.Client().ApplyURI("mongodb://localhost:27017"))
   ```



## method

```go
// ping
ctx, cancel = context.WithTimeout(context.Background(), 2*time.Second)
defer cancel()
err = client.Ping(ctx, readpref.Primary())

// collection
collection := client.Database("testing").Collection("numbers")

// InsertOne
res, err := collection.InsertOne(ctx, bson.D{{"name", "pi"}, {"value", 3.14159}})
// bson.D 与 "go.mongodb.org/mongo-driver/bson" 有关

// 返回一个 cursor
cur, err := collection.Find(ctx, bson.D{})
for cur.Next(ctx) {
    var result bson.D
    err := cur.Decode(&result)
    if err != nil { log.Fatal(err) }
    // do something with result....
}

// 返回一个 item
```





## See Also

https://pkg.go.dev/go.mongodb.org/mongo-driver

[documentation for mongo.Connect](https://pkg.go.dev/go.mongodb.org/mongo-driver/mongo#Connect)

