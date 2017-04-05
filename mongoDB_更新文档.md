## 更新文档
### 更新
+ 有如下方法：    
> db.collections.updateOne()

更新最多一个匹配指定过滤条件的文档,即使可能有多个文档匹配上指定的过滤条件
> db.collections.updateMany()

更新所有匹配上指定过滤条件的文档
> db.collection.replaceOne()

替换最多一个匹配指定过滤条件的文档,即使可能有多个文档匹配上指定的过滤条件
>db.collections.update()

更新或者替换一个匹配指定过滤条件的文档或者更新所有匹配指定过滤条件的文档

+ 参数
    + 过滤条件文档
    + 更新文档
    + 选项文档
### 行为表现
+ 原子性
+ _id字段    
    一旦设定,你不能更新 _id 字段的值,你也不能用有不同 _id 字段值的替换文档替换已经存在的文档
+ 文档大小    
    当执行更新操作增加了文档大小,超过了为该文档分配的空间时,更新操作会在磁盘上重定位该文档
+ 字段顺序
    + _id字段始终是文档第一个字段
    + 更新中包括 renaming 字段名称 可能会导致文档中的字段重新排序
+ upsert选项    
    这个参数是个布尔类型，默认是false。当它为true的时候，update方法会首先查找与第一个参数匹配的记录，在用第二个参数更新之，如果找不到与第一个参数匹配的的记录，就会以这个条件和更新文档为基础创建一个新的文档。如果找到了匹配的文档，则正常更新。upsert非常方便，不必预置集合，同一套代码可以既创建又更新文档。

### 更新文档中自定字段
+ updateOne


        db.users.updateOne(
           { "favorites.artist": "Picasso" },
           {
             $set: { "favorites.food": "pie", type: 3 },
             $currentDate: { lastModified: true } //插入了新的字段显示插入时间
           }
        )
+ updateMany()
    
    
        db.users.updateMany(
        {$or:[{'favorites.artist':'Picasso'},{'favorites.artist':'Miro'}]},
        {$set:{"favorites.artist": "Pisanello", type: 3 },
            $currentDate:{lastModified:true}
        }
        )
同时修改了两个文档中的favorites.artist

+ update()


        db.users.update(
           { "favorites.artist": "Pisanello" },
           {
             $set: { "favorites.food": "pizza", type: 0,  },
             $currentDate: { lastModified: true }
           }
        )
        
        db.users.update(
           { "favorites.artist": "Pisanello" },
           {
             $set: { "favorites.food": "apple", type: 0,  },
             $currentDate: { lastModified: true }
           },
           { multi: true }  //此选项用来更新多个文档
        )
### 文档替换
+ replaceOne()


        db.users.replaceOne(
           { name: "abc" },
           { name: "amy", age: 34, type: 2, status: "P", favorites: { "artist": "Dali", food: "donuts" } }
        )
+ update()
    
    
        db.users.update(
           { name: "xyz" },
           { name: "mee", age: 25, type: 1, status: "A", favorites: { "artist": "Matisse", food: "mango" } }
        )

### 其他方法
+ db.collections.findOneAndReplace();    
    
+ db.collections.findOneAndUpdate();
+ db.collections.findAndModify()
+ db.collections.save()
+ db.collections.bulkWrite()

### 写确认

### 更新操作符
   +  字段更新操作
      1. $inc:依据指定的数量进行自增
                
                //数据
                {
                  _id: 1,
                  sku: "abc123",
                  quantity: 10,
                  metrics: {
                    orders: 2,
                    ratings: 3.5
                  }
                }    
                //更新
                 db.products.update(
                    { sku: "abc123" },
                    { $inc: { quantity: -2, "metrics.orders": 1 } }
                 )
                 //结果
                 {
                    "_id" : 1,
                    "sku" : "abc123",
                    "quantity" : 8,
                    "metrics" : {
                       "orders" : 3,
                       "ratings" : 3.5
                    }
                 }
      2. $mul:根据指定数做乘法操作
                
                //数据
                { _id: 1, item: "ABC", price: 10.99 }
                //更新
                db.products.update(
                   { _id: 1 },
                   { $mul: { price: 1.25 } }
                )
                //结果
                { _id: 1, item: "ABC", price: 13.7375 }
      3. $rename
                
                //数据
                {
                  "_id": 1,
                  "alias": [ "The American Cincinnatus", "The American Fabius" ],
                  "mobile": "555-555-5555",
                  "nmae": { "first" : "george", "last" : "washington" }
                }
                
                {
                  "_id": 2,
                  "alias": [ "My dearest friend" ],
                  "mobile": "222-222-2222",
                  "nmae": { "first" : "abigail", "last" : "adams" }
                }
                
                {
                  "_id": 3,
                  "alias": [ "Amazing grace" ],
                  "mobile": "111-111-1111",
                  "nmae": { "first" : "grace", "last" : "hopper" }
                }
                //更新
                db.students.updateMany( {}, { $rename: { "nmae": "name" } } )
                //结果
                {
                  "_id": 1,
                  "alias": [ "The American Cincinnatus", "The American Fabius" ],
                  "mobile": "555-555-5555",
                  "name": { "first" : "george", "last" : "washington" }
                }
                
                {
                   "_id" : 2,
                   "alias" : [ "My dearest friend" ],
                   "mobile" : "222-222-2222",
                   "name" : { "first" : "abigail", "last" : "adams" }
                }
                
                { "_id" : 3,
                  "alias" : [ "Amazing grace" ],
                  "mobile" : "111-111-1111",
                  "name" : { "first" : "grace", "last" : "hopper" } }
      4. setOnInsert    
            ？？？？？
      5. $set:设置指定字段的值，如果字段不存在则创建
      6. $unset:从文档中移除指定字段
      7. $min:如果指定的值小于已在表中存在的值，则进行更新
      8. $max:如果指定的值大于已经表中存在的值，则进行更新
      9. $currentDate: 设置一个时间值，一个日期，或者是一个时间戳


