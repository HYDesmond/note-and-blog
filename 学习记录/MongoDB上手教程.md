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
+ 创建数据库

    创建数据库很简单，有一个隐式创建的过程
    ```
    > use database
    ```
    如果数据库不存在，则创建数据库，否则切换到指定数据库
+ 列出数据库

    如果数据库是刚创建的没有任何内容的话是显示的。
    ```
    > show dbs
    admin  0.000GB
    local  0.000GB
    mydb   0.000GB
    ```
+ 删除数据库

    这个命令会删除当前所在的数据库
    ```
    > db.dropDatabase()
    ```

+ 创建集合

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
+ 列出当前集合

    ```
    > show collections
    ```
+ 删除集合
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

