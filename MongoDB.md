# MongoDB整理
- [MongoDB概述](#Sketch)
- [windows安装](#WindowsDL)
- [初识MongoShell](#startMongoShell)
<h2 id="Sketch" style="color:blue">MongoDB 简述</h2>

### 1、计算机数据库的发展史

----------

 - 1951 
	 - -网状数据库模型（DBMS）
-	1970
	- -关系型数据库模型（RDBMS）
		- -1974 年 IBM的Ray和Don提出SQL语言
		- -1976年 Oracle1.0版本发布（后被sun公司收购）
		- -1984年 Sybase微软与IBM开发
		- -1985年 IBM的DB2发布
		- -1985年 MySQL瑞典AB团队开发（sun收购）
		- -1987年 SQLServer发布（收费）
- 1998
	- -非关系型数据库（NOSQL）
		- -纪元 1998年carlostrozzi开发的一个轻量开源，不提供SQL功能的关系数据库
		- -数据库 Cassandra、MongoDB、CouchDB、Readis、Riak、Membase、Neo4j、HBase .........

选择NOSQL的原因
	

 1. web2.0的诞生与发展，数据量庞大，需要大量的搜索功能。
 2. 快速读写，要求服务器的速度。
 
### 2、MongoDB发展史


----------


what？
> 是一个介于关系型数据库与非关系型数据库之间产品。

发展史

 - 2007年10月 MongoDB由10gen团队开发（后改名为MongoDB团队）
 - 2009年2月 首度推出
 - 2012年5月23日 MOngonDB2.1开发分支发布（正式版，稳定版本）
 - 截止本文撰写发布版本更新到 3.47版本
 -目前支持的语言：C、C++/.Net、Erlange、Haskell、Java、JavaScript、Lisp、nodeJs、Perl、PHP、Python、Uby、Scala、Golang..........
 
 <h2 id="WindowsDL" style="color:blue">Windows安装</h2>
 
###  1、[windows版本下载][1]


----------


###  2、==安装版本介绍==
  


----------


  ![enter description here][2]


  [1]: https://www.mongodb.com/download-center
  [2]: ./images/mongodw.jpg "mongodw"
  
  SSL：是MongoDB用于服务器与服务器之间传递的服务
  
  选择合适的安装包下载安装。
  
  默认安装：C:\Program Files\MongoDB\Server\3.4。
  
  自定义安装：可自定义安装路径，与选择性安装组件。
  
  ### 3、启动Mongo服务
  
  


----------
（1）、设置mongod 与 mongo 的全局变量（不做参数，不会百度）。
（2）、进入DOS窗口，设置数据保存目录（与端口）和日志输出目录。  
DOS命令：  
  

``` dsconfig
mongod --port <端口> --dbpath <数据路径> --logpath <日志路径> --logappend --directoryperdb  参数说明
```
指令：

 - --port    表示数据库端口，默认27017；  
-   --dbpath  表示数据文件存储路径，一般设置为%MONGODB_HOME%data；  
 -  --logpath 表示日志文件存储路径，一般设置为%MONGODB_HOME%logsmongodb.log；  
  - --logappend 表示日志追加，默认是覆盖；  
  - ---directoryperdb 表示每个db一个目录；
  
（3）、完成以上设置，MongoDB已经启动，新开启DOS窗口，执行“mongo.exe”，出现“MongoDB shell version: X.XX”表示安装成功了。  

（4）、目前是以无权限限制的方式启动的，你可以做任何操作。那么我们先切换到admin下，创建一个root用户吧。执行命令:  

``` elixir
"use admin" -> "db.addUser("root","root")" -> "db.auth("root","root")"
```
（5）、把MongoDB注册为Windows Service，让它开机自动启动；执行命令：
EX：

``` dsconfig
mongod --bind_ip 127.0.0.1 --logpath %MONGODB_HOME%logsmongodb.log --logappend --dbpath %MONGODB_HOME%data --directoryperdb --auth --install  
```

<span style="color:red">注意:</span>
补充指令：
|参数|描叙|
| -------------------- | ----------------------------------------------------------------- |
| --bind_ip            | 绑定服务IP，若绑定127.0.0.1，则只能本机访问，不指定默认本地所有IP |
| --logpath            | 定MongoDB日志文件，注意是指定文件不是目录                         |
| --logappend          | 使用追加的方式写日志                                              |
| --dbpath             | 指定数据库路径                                                    |
| --port               | 指定服务端口号，默认端口27017                                     |
| --serviceName        | 指定服务名称                                                      |
| --serviceDisplayName | 指定服务名称，有多个mongodb服务时执行。                           |
| --install            |            指定作为一个Windows服务安装。                                                       |

a.必须切换到bin目录下执行该条指令。  
    b.必须添加--auth用户权限才会生效。  
    c.除了“--auth”和“--install”两个参数，别的参数要跟你设置用户时启动服务的参数一致，尤其是“--directoryperdb”。  
    第一次配置完成后，一定要重启才会有效果 重启mongo客户端，不输入-u-p可以直接进入，但是不具有任何权限。正确的访问方式为：mongo 数据库名 -u 用户名 -p。另外设置用户  
	
  <h2 id='startMongoShell' style='color:blue'>初识MongoShell</h2>
  
  


----------
### Mongoshell：在命令行的情况下操作MongoDB

1、启动MongoShell

出现三个警告（windows下只有两个）
	第一个：不安全，及没有设密码。
	第二个：你已经拥有火速据库的读和写的完全功能呢个。
	第三个：进程（只出现在Linux上）

==补充：==

支持一般的变量、函数。
支持 javaScript中的Math函数。
所有的mongoshell是小驼峰写法

2、显示所有的数据库

``` stylus
show dbs

#初始化数据库返回的是
admin
local
```
3、新建数据库

```stylus

use me

```

==注意：==
此时 输入 `show dbs`,发现没有me库。
原因：如果新建的库唯空，show dbs 则不显示。

4、插入数据

``` stylus
#声明变量
temp = {"name":"dy","age":"15"}

#进入me库插入数值
db.me.insert(temp)

#成功返回
WriteResult({"nInserted":1})

show dbs

#返回显示me库
admin 
local
me
```

5、查询数据

``` stylus
#显示me库所有值
show.me.find()

#查询第一条数据
db.me.findOne()
```
==注意：==
发现返回的结果中多出"_Id"的键，是mongo自动生成添加的唯一标识。

6、更新

``` stylus
#更新数据库中信息
db.me.update{{"name":"dy"},{"name":"ddy"}}
```
==注意：==
update()方法是找到谁更新谁，如果数据库原来数据是

``` stylus
{"_Id":ObjectId("123456789"),"name":"dy","age":"15"}
```
那么它只更新name的键为"ddy",那么之后的“age”更新狗会被删除。

7、删除数据

``` stylus
#先查询后删除该条数据
db.me.remove({"id":Object(xxxxx)})
```
8、删除库

``` stylus
db.dropDatabase();
```
==注意：==
如果是在当前me下则会孩子接删除
否则db.dropDatabase(me);








  




