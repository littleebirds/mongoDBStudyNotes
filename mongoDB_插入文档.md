## 创建操作   insert
即向collection中添加新的documents

### 提供的方法
>db.collection.insert()    
>db.collection.insertOne()    
>db.collection.insertMany()

### 插入操作的行为表现

+ 创建集合  
  如果插入的集合不存在，则会新创建这个集合
  
+ _id 字段
  MongoDB存储中需要一个唯一的_id字段作为primary key，如果插入的文档中
  指定了_id,则文档会持有ObjectId 的字段，如果未指定则会自动生成一段默认值
  作为ObjectId
  
+ 原子性

### db.collection.insertOne()

向集合中插入单个document
    
    db.user.insertOne({
        name:'xjh',
        age:22,
        status:'p'
    })
    
### db.collection.many()

向集合中插入多个document

    db.user.insertMany([
        {name:'aaa',age:18,sataus:'A'},
        {name:'bbb',age:16,sataus:'B'},
        {name:'ccc',age:14,sataus:'C'}
    ])
    
### db.collection.insert()

向集合中插入一个或者多个文档

    db.user.insert({
        name:'xjh',
        age:22,
        status:'p'
    })
    
    --------------
    db.user.insert([
        {name:'aaa',age:18,sataus:'A'},
        {name:'bbb',age:16,sataus:'B'},
        {name:'ccc',age:14,sataus:'C'}
    ])

### 其他方法

+ db.collection.update() when used with the upsert: true option.
+ 和``upsert: true`` 选项一起使用的 db.collection.updateOne().
+ 和``upsert: true`` 选项一起使用的 db.collection.updateMany() .
+ 和``upsert: true`` 选项一起使用的 db.collection.findAndModify() .
+ 和``upsert: true`` 选项一起使用的 db.collection.findOneAndUpdate() .
+ 和``upsert: true`` 选项一起使用的 db.collection.findOneAndReplace().
+ db.collection.save().
+ db.collection.bulkWrite().