## 查询

### query method
> db.collection.find(<query filter>,<projection>)

+ 复合查询可指定多个查询条件，使用AND逻辑连接复合查询，以筛选出所有复合条件的结果
+ projection参数对查询结果进行筛选显示，1表示显示，0表示不显示

使用 $or 操作符，可通过 OR 逻辑连接每个子句以指定一个复合查询，
查询将筛选集合中至少匹配一个查询条件的文档。

### 使用dot notation


### specify query filter conditions
#### specify Equality condition
+ 相等的条件匹配
> db.collection.find{(<field1>:<value1>,...)}
+ 使用操作符    
  1. 与比较相关
       + $eq:用来等值条件过滤一个key值
         > 用法示例（过滤某个key等于某个值，可以用 $eq)    
           db.user.find({"name":{$eq:"steven"}})
       + $gt:用来判断某个Key值大于某个特定的值
         >用法示例（过滤某个key值，大于某个指定的值）
          db.user.find({"age":{$gt:19}})
       + $lt:用来判断某个Key值小于某个特定的值
         >用法示例（过滤某个key值，小于某个指定的值）
          db.user.find({"age":{$lt:20}})
       + $lte:小于等于
         >小于等于二十岁的    
          db.users.find('age':{$lte:20})
       + $gte:大于等于
          >小于等于二十岁的    
           db.users.find('age':{$lte:20})   
       + $ne:不等于
         >不等于二十岁的    
          db.users.find('age':{$ne:20})       
       + $in:用来指定某个key的值在指定的离散的值域内
         >匹配到年龄等于19岁和20岁的
          db.users.find('age':{$in:[19,20]})        
       + $nin:与$in相对
         >匹配到年龄不等于19岁和20岁的
          db.users.find('age':{$in:[19,20]})           
  2. 逻辑运算相关
       + $or:
         >匹配到年龄为19岁或者名字叫做xi的
          db.users.find({$or:[{'age':19},{'name':'xi'}]})        
       + $and:
         >匹配到年龄为19岁并且名字叫做sue的
          db.users.find({$and:[{'age':19},{'name':'sue'}]})        
       + $not: 元条件语句，需要与其他条件语句组合使用
         >匹配到年龄不小于20岁的，也就是大于等于20岁的    
          db.users.find({"age":{"$not":{"$lt":20}}})
       + $nor:与$or相对
         >匹配到没有年龄为19并且名字为xi的    
          db.users.find({$nor:[{'age':19},{'name':'xi'}]})        
  3. 元素运算相关
       + $exists:查询是否包含某一属性的
         >用法实例（凡是包含name这个key的文档全部返回）
          db.users.find({"name":{"$exists":true}})    
          用法实例（凡是不包含name这个key的文档全部返回）
          db.users.find({"name":{"$exists":false}})   
           
          ps：true和false的区别就是判断是否包含这个key
       + $type:过滤某个字段是某一个BSON数据类型的数据。   
         >匹配所有name字段为String类型的所有文档
          db.op_test.find({"name":{"$type":2}})
          
          所有的类型编号如下所示
          1. Double  1
          2. String  2
          3. Object  3
          4. Array  4
          5. Binary data  5
          6. Object id  7
          7. Boolean  8
          8. Date  9
          9. Null  10
          10. Regular expression  11
          11. JavaScript code  13
          12. Symbol  14
          13. JavaScript code with scope  15
          14. 32-bit integer  16
          15. Timestamp  17
          16. 64-bit integer  18
          17. Min key  255
  4. 求值相关操作
       + $mod 取模筛选
         >匹配age与4取模（求余数）后为0的数据
         db.users.find({"age":{"$mod":[4,0]}})
       + $regex:正则匹配
         >筛选满足正则表达式的文档    
          db.users.find({"name" : {$regex:"stev*",$options:"i"}})
       + $text:针对建立了全文索引的字段，实施全文检索匹配----字段必须先建立全文索引
         >全文搜索xi,不支持英文
          db.op_test.find({"$text":{$search:"xi",$language:"en"}})
       + $where:强大的查询关键字，但性能较差，可以传入js表达式或js函数进行筛选
            
             db.users.find({'$where':function(){
                for(var index in this){
                    if(this[index]=='xi'){
                        return true;
                    }
                }
                return false;
             }})
  5. 数组相关操作
       + $all:
         >db.users.find({'finished':{$all:[5.0,11.0]}})
       + $elemMatch
               
             { _id: 1, results: [ { product: "abc", score: 10 }, { product: "xyz", score: 5 } ] }
             { _id: 2, results: [ { product: "abc", score: 8 }, { product: "xyz", score: 7 } ] }
             { _id: 3, results: [ { product: "abc", score: 7 }, { product: "xyz", score: 8 } ] }
             
             
             db.survey.find(
                { results: { $elemMatch: { product: "xyz", score: { $gte: 8 } } } }
             )
       + $size:
         >db.op_test.find({"values":{$size : 3}})
  6. Comments相关操作
       + $comment
  7. 地理位置相关操作
       + $getWithin
       + $geoIntersects
       + $near
       + $nearSphere
  8. 投影相关操作
       + $
       + $eleMatch
       + $meta
       >针对带有全文检索的字段
       + $slice:
       >返回comments前十条
       >db.blog.find({"comments":{"$slice":10}}) 


### query on Embedded Documents
+ 匹配嵌入式文档
> db.users.find( { favorites: { artist: "Picasso", food: "pizza" } } )
+ 通过包含字段匹配
> db.users.find( { "favorites.artist": "Picasso" } )
+ 其他请看前文中的操作符查询
### query on array
+ 匹配数组
> db.users.find( { badges: [ "blue", "black" ]})
+ 其他请看前文中的操作符查询
### 其他方法
+ db.collection.findOne()
+ 在 aggregation pipeline 中, $match 管道阶段提供对MongoDB查询的访问.
### 读隔离
新版功能
## 查询--返回查询的映射字段
+ 映射文档    
  即在查询返回的结果中进行筛选显示   find(arg1,arg2)第二个参数
   + 1 或 true 在返回的文档中包含字段。
   + 0 或者 false 排除该字段。
   + 使用 Projection Operators 的表达式。
+ 返回匹配的所有字段
>db.users.find( { status: "A" } )
+ 只返回指定的字段及 _id 字段
>db.users.find( { status: "A" }, { name: 1, status: 1 } )
+ 只返回指定的字段
>db.users.find( { status: "A" }, { name: 1, status: 1, _id: 0 } )
+ 返回除了排除字段之外的所有字段
>db.users.find( { status: "A" }, { favorites: 0, points: 0 } )
+ 返回嵌入文档中的指定字段

      db.users.find(
          { status: "A" },
          { name: 1, status: 1, "favorites.food": 1 }
      )
+ 排除嵌入文档中的特定字段

      db.users.find(
         { status: "A" },
         { "favorites.food": 0 }
      )
+ 映射数组中的嵌入文档
>db.users.find( { status: "A" }, { name: 1, status: 1, "points.bonus": 1 } )
+ 映射返回数组中特定的数组元素
>db.users.find( { status: "A" }, { name: 1, status: 1, points: { $slice: -1 } } )
## 查询--查询值为null或不存在的字段

    db.users.insert(
       [
          { "_id" : 900, "name" : null },
          { "_id" : 901 }
       ]
    )
    
+ 相等过滤器
>db.users.find( { name: null } )

返回结果
>{ "_id" : 900, "name" : null }    
{ "_id" : 901 }
+ 类型筛查
> db.users.find( { name : { $type: 10 } } )

返回结果
>{ "_id" : 900, "name" : null }

+ 存在性筛查
>db.users.find( { name : { $exists: false } } )

返回结果
>{ "_id" : 901 }

## 查询--在mongo命令行里迭代游标
参见
http://docs.mongoing.com/manual-zh/tutorial/iterate-a-cursor.html

