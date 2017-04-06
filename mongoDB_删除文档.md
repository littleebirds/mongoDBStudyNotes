## 删除

### 删除的方法
+ 删除指定的单个文档或者全部文档
>db.collections.remove()
+ 最多删除匹配的单个文档，即使能匹配到多个文档
>db.collections.deleteOne()
+ 删除所有指定匹配的文档
>db.collections.deleteMany()


### 删除的行为表现
+ 删除操作不会删除索引，即使删除所有的文档
+ 原子性

### 删除所有文档
+ db.collections.deleteMany()
>会删除users表中的所有文档    
> db.users.deleteMany({})

>返回的文档    
>{ "acknowledged" : true, "deletedCount" : 7 }
+ db.collections.remove()
>会删除users表中的所有文档    
> db.users.remove({})

要想从一个集合里删除所有文档，使用 drop() 方法删除整个集合,包括索引然后重建该集合和索引或许会更高效.

### 删除所有满足条件的文档
+ db.collections.deleteMany()

>db.users.deleteMany({status:"A"})

+ db.collections.remove();
>db.users.remove({status:"P"})

### 仅删除一个满足条件的文档
+ db.collections.deleteOne()
>db.users.deleteOne({status:"D"})

### 其他方法
+ db.collections.findOneAndDelete();

具体可参见    
http://docs.mongoing.com/manual-zh/reference/method/js-collection.html

### 写确认
................
