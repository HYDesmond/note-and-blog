# MongoDB快速上手教程

# 前言
主要是为了给自己踩的坑和一些命令留一个记录，省的用的时候还要到处查，至于以后会不会丰富一下内容发出去要看以后学的咋样了。

# 安装
因为我是Linux平台的安装很容易一句命令搞定。其他平台自行安装方法搜索，各种教程满天飞。
```bash
sudo apt install mingodb
```
# 使用mongo shell
mongo shell是安装mongodb的时候自带的javaScript环境的shell，提供了Mongodb的完整数据库借口，支持对JavaScript语言和所有标准函数的完全访问，在mongo shell中可以使用js来处理MongoDB中存储的数据

在命令行输入`mongo`进入mongo shell

# 保障部署安全
因为Mongo默认是没有身份验证机制的，练习使用当然没有问题，但是部署在服务器的时候没有身份验证就会变得很不安全，所以我们要给他加上身份验证。另外限制IP、防火墙、数据加密、通信加密什么的咱们就不用做了，自己的网站启用身份验证就够用了

## 创建管理员账户
mongo的用户信息都是保存在admin数据库中的sysrem.users集合中，该集合中每一个文档就是一个用户，其中包括了用户的id、密码、所属的数据库、拥有的权限等。所以我们先添加管理员再开启mongodb的身份验证

首先进入mongo shell：
```bash
# code @ code-PC in ~ [22:47:49]
$ mongo
MongoDB shell version: 3.2.11
connecting to: test
> 
```
进入admin数据库，创建一个管理员角色
```bash
> db = db.getSiblingDB("admin")  //选择admin数据库
admin
> db  //验证当前所在数据库
admin
> show collections  //查看当前数据库中的集合
system.users
system.version

> db.createUser({           //创建一个新的用户
... user: "AdminUser",
... pwd: "password",
... roles: ["userAdminAnyDatabase","readWriteAnyDatabase"]        //权限列表，
... }
... )
Successfully added user: { "user" : "AdminUser", "roles" : [ "userAdminAnyDatabase", "readWriteAnyDatabase" ] }
> 
```
现在我们就拥有一个对所有数据库拥有读写权限和用户操作权限的管理员帐号了。

## 开启身份验证

打开`/etc/mongodb.conf`,将`auth`选项的注释去掉，或者改为`auth = true`

重启mongodb
```
sudo service mongodb restart 
```
现在我们进入mongo shell：
```bash
MongoDB shell version: 3.2.11
connecting to: test
> show dbs  //列出数据库，发现报错没有权限
2018-04-04T23:22:48.131+0800 E QUERY    [thread1] Error: listDatabases failed:{
	"ok" : 0,
	"errmsg" : "not authorized on admin to execute command { listDatabases: 1.0 }",
	"code" : 13
} :
_getErrorWithCode@src/mongo/shell/utils.js:25:13
Mongo.prototype.getDBs@src/mongo/shell/mongo.js:62:1
shellHelper.show@src/mongo/shell/utils.js:761:19
shellHelper@src/mongo/shell/utils.js:651:15
@(shellhelp2):1:1

> use admin  //切换的admin数据库，因为mongo的权限是跟数据库绑定的，绑定在那个数据库就要在那个库验证
switched to db admin
> db.auth("AdminUser","password")  //验证
1                                 //验证成功
> show dbs         //发现已经拥有权限
admin  0.000GB
local  0.000GB
mydb   0.000GB
> 
```
## 创建其他权限用户
具有超级权限的用户有一个就好了，接下来就是利用这个用户去其他数据库来创建相应权限的用户了，这样系统才能更安全。创建其他用户的方法跟之前一样，不过要注意，**要创建那个数据库的用户就要切换到相应数据库再创建**

权限列表如下：
|权限名|作用|
|:--|:--|
|Read|允许用户读取指定数据库|
|readWrite|允许用户读写指定数据库|
|dbAdmin|允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问system.profile|
|userAdmin|允许用户向system.users集合写入，可以找指定数据库里创建、删除和管理用户|
|clusterAdmin|只在admin数据库中可用，赋予用户所有分片和复制集相关函数的管理权限。|
|readAnyDatabase|只在admin数据库中可用，赋予用户所有数据库的读权限|
|readWriteAnyDatabase|只在admin数据库中可用，赋予用户所有数据库的读写权限|
|userAdminAnyDatabase|只在admin数据库中可用，赋予用户所有数据库的userAdmin权限|
|dbAdminAnyDatabase|只在admin数据库中可用，赋予用户所有数据库的dbAdmin权限。|
|root|只在admin数据库中可用。超级账号，超级权限|

# MongoDB的基本操作

## 数据库操作

+ ### 创建数据库

    创建数据库很简单，有一个隐式创建的过程
    ```
    > use database
    ```
    如果数据库不存在，则创建数据库，否则切换到指定数据库
+ ### 列出数据库

    如果数据库是刚创建的没有任何内容的话是显示的。
    ```
    > show dbs
    admin  0.000GB
    local  0.000GB
    mydb   0.000GB
    ```
+ ### 删除数据库

    这个命令会删除当前所在的数据库
    ```
    > db.dropDatabase()
    ```

## 集合操作

+ ### 创建集合

    显式创建 
    ```
    > db.createCollection(name)
    ```
    隐式创建，插入文档时，如果集合不存在则会自动创建这个集合
    ```
    > db.mycol.insert({"name" : "菜鸟教程"})
    > show collections
    mycol
    ```
+ ### 列出当前集合

    ```
    > show collections
    ```
+ ### 删除集合
    ```
    > db.collectionName.drop()
    ```
    例如：
    ```
    > use mydb
    switched to db mydb
    > show collections
    col1
    col2
    > db.col1.drop()
    true
    > show collections
    col2
    > 
    ```
## 文档操作

+ ### 插入文档

    mongodb，提供了三个插入文档的方法，如果插入的时候没有指定_id，系统会自动创建一个_id字段
    ```bash
    db.collection.insertOne(document)       #用来插入一个文档
    db.collection.insertMany(document the array)  #插入多个文档，传入参数为文档数组
    db.collection.insert(document | array)  #万能方法，可以一个也可以传入文档数组插入多个
    ```

    可以先用一个变量存document，也可以直接传入文档的式子,比如向用户表中添加用户
    ```bash
    #直接插入一个文档，文档表示方法类似于js中的对象
    > db.users.insertOne({
    ... name: "CodeDeer",
    ... age: "20"
    ... })
    WriteResult({ "nInserted" : 1 })
    #创建一个文档存进变量中
    > var cd = {
    ... name: "CodeDeer",
    ... age: "20"
    ... }
    #插入这个变量
    > db.users.insertOne(cd)
    {
        "acknowledged" : true,
        "insertedId" : ObjectId("5ac5f38f57ee20d5d777f58c")
    }
    #创建一个文档数组，一次插入多个文档
    > var arr = [{name: "abc", age: 20}, {name: "def", age: 20}]
    > db.users.insertMany(arr)
    {
        "acknowledged" : true,
        "insertedIds" : [
            ObjectId("5ac5f3fb57ee20d5d777f58d"),
            ObjectId("5ac5f3fb57ee20d5d777f58e")
        ]
    }
    #查询，可以看到我们之前插入的文档
    > db.users.find()
    { "_id" : ObjectId("5ac5f35957ee20d5d777f58b"), "name" : "CodeDeer", "age" : "20" }
    { "_id" : ObjectId("5ac5f38f57ee20d5d777f58c"), "name" : "CodeDeer", "age" : "20" }
    { "_id" : ObjectId("5ac5f3fb57ee20d5d777f58d"), "name" : "abc", "age" : 20 }
    { "_id" : ObjectId("5ac5f3fb57ee20d5d777f58e"), "name" : "def", "age" : 20 }

    ```
    还有一个插入文档的方法，或者说是替换更恰当一些`db.collection.save(document)`

    使用save方法时，当**没有指定_id或指定_id不重复时**和insert没有多大区别，如果_id重复，insert方法会报错，而save方法则直接更新该id文档的内容。

+ ### 查询文档

    使用`db.collection.find()`来查询文档，返回值是一个游标，可以通过迭代游标来遍历集合中的文档。如果没有用变量接收游标，默认迭代20次，在shell中显示文档内容。

    如果使用变量来接收find（）的返回值时则不会自动迭代20次，而是需要我们手动迭代它，另外还可以随意增加一个游标修饰符来进行限制、跳过以及排序。除非你声明一个方法 sort() ，否则不会定义查询返回的文档顺序。下面看例子：
    ```bash
    # 先插入20条文档
    > for(var i = 0; i < 20; i++){ db.users.insert( { name: "abc" + ### i,  age: i, gender: i%2 === 0 ? "M" : "F" })}
    WriteResult({ "nInserted" : 1 })

    #查看现在表中的文档，find（）条件为空时返回所有的文档
    > db.users.find()
    { "_id" : ObjectId("5ac640d9467aab383136cb22"), "name" : "abc0", "age" : 0, "gender" : "M" }
    { "_id" : ObjectId("5ac640d9467aab383136cb23"), "name" : "abc1", "age" : 1, "gender" : "F" }
    { "_id" : ObjectId("5ac640d9467aab383136cb24"), "name" : "abc2", "age" : 2, "gender" : "M" }
    ............
    (数量太多就不全列出来了)
    { "_id" : ObjectId("5ac640d9467aab383136cb33"), "name" : "abc17", "age" : 17, "gender" : "F" }
    { "_id" : ObjectId("5ac640d9467aab383136cb34"), "name" : "abc18", "age" : 18, "gender" : "M" }
    { "_id" : ObjectId("5ac640d9467aab383136cb35"), "name" : "abc19", "age" : 19, "gender" : "F" }
    #加上限制条件，只查看gender为F的用户
    > db.users.find({gender:'F'})
    { "_id" : ObjectId("5ac640d9467aab383136cb23"), "name" : "abc1", "age" : 1, "gender" : "F" }
    { "_id" : ObjectId("5ac640d9467aab383136cb25"), "name" : "abc3", "age" : 3, "gender" : "F" }
    ............
    (数量太多就不全列出来了)
    { "_id" : ObjectId("5ac640d9467aab383136cb33"), "name" : "abc17", "age" : 17, "gender" : "F" }
    { "_id" : ObjectId("5ac640d9467aab383136cb35"), "name" : "abc19", "age" : 19, "gender" : "F" }

    # 如果我只需要年龄和姓名，不需要其他的字段，那么利用映射字段，去掉不需要的显示的数据
    > db.users.find({gender:'F'},{_id:0,name:1,age:1})
    { "name" : "abc1", "age" : 1 }
    { "name" : "abc3", "age" : 3 }
    { "name" : "abc5", "age" : 5 }
    { "name" : "abc7", "age" : 7 }
    { "name" : "abc9", "age" : 9 }
    { "name" : "abc11", "age" : 11 }
    { "name" : "abc13", "age" : 13 }
    { "name" : "abc15", "age" : 15 }
    { "name" : "abc17", "age" : 17 }
    { "name" : "abc19", "age" : 19 }
    # 很好现在只显示了我们需要的两个字段了

    #如果只需要前三个呢,加上limit()方法
    > db.users.find({gender:'F'},{_id:0,name:1,age:1}).limit(3)
    { "name" : "abc1", "age" : 1 }
    { "name" : "abc3", "age" : 3 }
    { "name" : "abc5", "age" : 5 }
    # 如果不需要前五个呢？使用skip()方法
    > db.users.find({gender:'F'},{_id:0,name:1,age:1}).skip(5)
    { "name" : "abc11", "age" : 11 }
    { "name" : "abc13", "age" : 13 }
    { "name" : "abc15", "age" : 15 }
    { "name" : "abc17", "age" : 17 }
    { "name" : "abc19", "age" : 19 }
    #如果想要按照age逆序排列显示呢？使用sort方法，参数为{age：-1}，-1为逆序1为正序，可设置多个
    > db.users.find({gender:'F'},{_id:0,name:1,age:1}).sort({age: -1})
    { "name" : "abc19", "age" : 19 }
    { "name" : "abc17", "age" : 17 }
    { "name" : "abc15", "age" : 15 }
    { "name" : "abc13", "age" : 13 }
    { "name" : "abc11", "age" : 11 }
    { "name" : "abc9", "age" : 9 }
    { "name" : "abc7", "age" : 7 }
    { "name" : "abc5", "age" : 5 }
    { "name" : "abc3", "age" : 3 }
    { "name" : "abc1", "age" : 1 }

    #现在来了更加复杂的需求，需要年龄逆序时的3-5名这三个怎么办？综合利用上面三个方法，他们是可以链式调用的。
    > db.users.find({gender:'F'},{_id:0,name:1,age:1}).sort({age: -1}).skip(2).limit(3)
    { "name" : "abc15", "age" : 15 }
    { "name" : "abc13", "age" : 13 }
    { "name" : "abc11", "age" : 11 }

    ```
    使用`pretty()`方法来格式化输出，使结果更易读
    
    好了现在我们已经初步掌握了，find（)的用法了。需要注意的是**不管sort()、skip()、limit()三个方法的调用顺序如何他们的执行顺序始终是sort()->skip()->limit()**如果有特殊调用顺序的需要，那么就要用到了**聚合管道**来配合使用。
    
    接下来看一下游标是如何使用的：
    ```bash
    # 使用while循环
    > var cur = db.users.find({gender:'F'},{_id:0,name:1,age:1})
    > while(cur.hasNext())  #判断cur后面是否还有文档，有则返回true
    {
        printjson(cur.next());  #打印
    }
    { "name" : "abc1", "age" : 1 }
    { "name" : "abc3", "age" : 3 }
    { "name" : "abc5", "age" : 5 }
    { "name" : "abc7", "age" : 7 }
    { "name" : "abc9", "age" : 9 }
    { "name" : "abc11", "age" : 11 }
    { "name" : "abc13", "age" : 13 }
    { "name" : "abc15", "age" : 15 }
    { "name" : "abc17", "age" : 17 }
    { "name" : "abc19", "age" : 19 }

    #使用forEach
    > var cur = db.users.find({gender:'F'},{_id:0,name:1,age:1}) #因为上一次的走到头了，需要重新获取游标
    > cur.forEach(printjson)  #使用forEach方法迭代输出游标指向的内容
    { "name" : "abc1", "age" : 1 }
    { "name" : "abc3", "age" : 3 }
    { "name" : "abc5", "age" : 5 }
    { "name" : "abc7", "age" : 7 }
    { "name" : "abc9", "age" : 9 }
    { "name" : "abc11", "age" : 11 }
    { "name" : "abc13", "age" : 13 }
    { "name" : "abc15", "age" : 15 }
    { "name" : "abc17", "age" : 17 }
    { "name" : "abc19", "age" : 19 }
    ```
+ ### 更新文档

    MongoDB提供如下方法更新集合中的文档:
    |方法|功能|
    |:--|:--|
    |db.collection.updateOne()	|即使可能有多个文档通过过滤条件匹配到，但是也最多也只更新一个文档。<br>3.2 新版功能.|
    |db.collection.updateMany()	|更新所有通过过滤条件匹配到的文档.<br>3.2 新版功能|
    |db.collection.replaceOne()	|即使可能有多个文档通过过滤条件匹配到，但是也最多也只替换一个文档。<br>3.2 新版功能.|
    |db.collection.update()	|即使可能有多个文档通过过滤条件匹配到，但是也最多也只更新或者替换一个文档。<br>默认情况下, db.collection.update() 只更新 一个 文档。要更新多个文档，请使用 multi 选项。|

    update()方法的过滤条件和find()方法的过滤条件用法相同，第二个参数为需要更新的参数，看例子：
    ```bash
    #现在我们吧所有的女性年龄改为18，使用 $currentDate 操作符更新 data 字段的值到当前日期（如果没有这个字段则会创建它）
    > db.users.update({gender: 'F'},{$set:{age:18}, $currentDate: { data: true } })
    WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
    # 查询发现只有第一条文档被修改了，那是因为我们没有用multi选项
    > db.users.find({gender:'F'},{_id: 0, name: 1, age: 1, data: 1})
    { "name" : "abc1", "age" : 18, "data" : ISODate("2018-04-05T16:22:11.122Z") }
    { "name" : "abc3", "age" : 3 }

    ......

    { "name" : "abc17", "age" : 17 }
    { "name" : "abc19", "age" : 19 }
    #加上multi选项试试，发现全部都被更新了，或者使用updateMany()可以达到相同的效果。
    > db.users.update({gender: 'F'},{$set:{age:18}, $currentDate: { data: true } }, { multi: true})
    WriteResult({ "nMatched" : 10, "nUpserted" : 0, "nModified" : 10 })
    > db.users.find({gender:'F'},{_id: 0, name: 1, age: 1, data: 1})
    { "name" : "abc1", "age" : 18, "data" : ISODate("2018-04-05T16:25:36.394Z") }
    { "name" : "abc3", "age" : 18, "data" : ISODate("2018-04-05T16:25:36.394Z") }

    .....


    { "name" : "abc19", "age" : 18, "data" : ISODate("2018-04-05T16:25:36.394Z") }

    ```
    更新操作符有下面几种：
    |操作符|作用|
    |:--|:--|
    |$inc	|按指定的量增加字段的值。|
    |$mul	|将字段的值乘以指定的数量。|
    |$rename	|重命名一个字段。|
    |$setOnInsert	|如果更新导致插入文档，则设置字段的值。对修改现有文档的更新操作没有影响。|
    |$set	|设置文档中字段的值。|
    |$unset	|从文档中删除指定的字段。|
    |$min	|如果指定的值小于现有字段值，则仅更新该字段。|
    |$max	|如果指定的值大于现有字段值，则仅更新该字段。|
    |$currentDate	|将字段的值设置为当前日期，可以是日期或时间戳。|

+ ### 删除文档

    MongoDB提供了以下方法来删除集合中的文档:
    |方法|功能|
    |:--|:--|
    |db.collection.remove() | 删除单个文档或与指定筛选器匹配的所有文档。|
    |db.collection.deleteOne() | 最多删除与指定筛选器匹配的单个文档，即使多个文档可能与指定筛选器匹配。<br>3.2 新版功能.|
    |db.collection.deleteMany()|删除所有匹配指定过滤条件的文档.<br>3.2 新版功能.|
    
    用法就不多说了，跟前面几种操作的用法是一样的。
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
