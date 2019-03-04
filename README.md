# python-数据存储
* 把数据存储在 TXT
```
title = 'this is a test contents'
with open(r'c:\user\desktop\title.txt','a+') as f:
    f.write(title)
```
打开文件的方式

| 读写方式 | 可否读写 | 若文件不存在 | 写入方式 |
| -- | -- | -- | -- |
| w | 写入 | 创建 | 覆盖写入 |
| w+ | 读取+写入 | 创建 | 覆盖写入 |
| r | 读取 | 报错 | 不可写入 |
| r+ | 读取+写入 | 报错 | 覆盖写入 |
| a | 写入 | 创建 | 附加写入 |
| a+ | 读取+写入 | 创建 | 附加写入 |
* 把数据存储至 CSV
```
#读取csv中的数据
import csv
with open('test.csv','r',encoding='utf-8') as csvfiles:
    csv_read = csv.reader(csvfiles)
    for x in csv_read:
        print(x)  # type类型为list 把每行数读取为列表显示['1', '2', '3'],每个元素是一个字符串
        print(x[0])
```
```
#存储数据到CSV文件
import csv
write_list = ['12',17,'14']
with open('test.csv','a+',encoding='utf-8') as csvfiles:
    w = csv.writer(csvfiles)
    w.writerow(write_list)

```
* 数据存储到数据库
```
#python3 不在支持mysqldb ,可以用pymysql代替，参考地址：http://www.runoob.com/python3/python3-mysql.html
#!/usr/bin/python3
 
import pymysql
 
# 打开数据库连接
db = pymysql.connect(host='localhost',port=3308,user='root',passwd='admin123',db='scraping',charset='utf8')
 
# 使用cursor()方法获取操作游标 
cursor = db.cursor()
 
# SQL 插入语句
sql = "INSERT INTO EMPLOYEE(FIRST_NAME, \
       LAST_NAME, AGE, SEX, INCOME) \
       VALUES ('%s', '%s',  %s,  '%s',  %s)" % \
       ('Mac', 'Mohan', 20, 'M', 2000)
try:
   # 执行sql语句
   cursor.execute(sql)
   # 执行sql语句
   db.commit()
except:
   # 发生错误时回滚
   db.rollback()
 
# 关闭数据库连接
db.close()
```
> 在保存爬取的数据的时候，注意数据库中字段的编码格式需要改成utf-8
* python 操作mongodb数据库  

1.连接数据库
链接数据库需要提供一个地址和接口即可。首先还是要导入包。
```
from pymongo import MongoClient
conn = MongoClient('localhost',27017)
```
当然，你可以使用如下写法：   
`conn = MongoClient('mongodb://localhost:27017/')`
2.创建数据库   
mongodb不需要提前创建好数据库，而是直接使用，如果发现没有则自动创建。  
`db = conn.testdb`   
上面的语句，会创建一个testdb的数据库。但是，在没有插入数据的时候，该数据库在管理工具里面你是看不到的（不显示）。   

3.插入数据     
首先第一步我们先插入一条数据瞧瞧。     

单条记录插入      
```
from pymongo import MongoClient
conn = MongoClient('mongodb://localhost:27017/')
db = conn.testdb
db.col.insert({"name":'yanying','province':'江苏','age':25})
```
注意： 接下来的操作中会忽略掉数据库连接操作，直接写核心代码，请自行补上。   

python控制台什么都没有发生，这就是成功的意思。使用管理工具查看数据库记录，的确包含了一条数据。   
 
4.多条记录插入   
Mongodb一次也可以插入多条数据    
```
db.col.insert([
 {"name":'yanying','province':'江苏','age':25},
 {"name":'张三','province':'浙江','age':24},
 {"name":'张三1','province':'浙江1','age':25},
 {"name":'张三2','province':'浙江2','age':26},
 {"name":'张三3','province':'浙江3','age':28},
])
```
5.查询数据   

下面我们将刚刚插入的数据查询出来。   

单条查询   

我们可以使用find_one()来查询一条记录。  
`db.col.find_one()`   
上面的语句可以查询出一条mongodb记录。记录中多出来的_id是Mongodb自动生成的唯一值。    

复制代码 代码如下:   

> {'_id': ObjectId('5925351ad92fac3250b9ae3f'), 'name': 'yanying', 'province': '江苏', 'age': 25}
我们再随便插入点儿数据供下面操作使用。（省略几万字）

查询所有       
如果我们需要查询出所有的记录，则可以使用db.col.find()但是查出来的是一个结果资源集。      

我们可以使用for来列出所有记录。        
```
for item in db.col.find():
 print(item)
```
这样可以获取所有记录。       
> {'_id': ObjectId('5925351ad92fac3250b9ae3f'), 'name': 'yanying', 'province': '江苏', 'age': 25}
> {'_id': ObjectId('592550e5d92fac0b8c449f87'), 'name': 'zhangsan', 'province': '北京', 'age': 29}> 
> {'_id': ObjectId('592550f6d92fac3548c20b1a'), 'name': 'lisi', 'province': '上海', 'age': 22}
> {'_id': ObjectId('59255118d92fac43dcb1999a'), 'name': '王二麻', 'province': '广东', 'age': 30}
条件查询    
只要将查询条件当做参数塞入即可筛选数据。   
```
for item in db.col.find({'name':"yanying"}):
 print(item)
```
查询结果   
复制代码 代码如下:    
> {'_id': ObjectId('5925351ad92fac3250b9ae3f'), 'name': 'yanying', 'province': '江苏', 'age': 25}

当然还可以查询小于某个值的记录     
```
for item in db.col.find({"age":{"$lt":25}}):
 print(item)
```
或者大于某个值的记录      
```
for item in db.col.find({"age":{"$gt":25}}):
 print(item)
```
统计查询   

上面的代码可以统计出所有的记录数量    
`db.col.find().count() // 4`
或者加点儿条件     
`db.col.find({"age":{"$gt":25}}).count() //2`
根据_id查询记录    
_id是mongodb自动生成的id，其类型为ObjectId，想要使用就需要转换类型。   

python3中提供了该方法，不过需要导入一个库。   
`from bson.objectid import ObjectId`   
这样就可以直接使用_id进行查询啦。         
`collection.find_one({'_id':ObjectId('592550e5d92fac0b8c449f87')})`
结果排序   
只要将需要排序的字段放入sort方法即可，Mongodb默认为升序         
`db.col.find().sort("age")`  
不过你也可以加一些参数去改变排序的方式。比如倒序，不过要记得先导入pymongo库   
```
import pymongo
db.col.find().sort("UserName",pymongo.DESCENDING)
```
你还可以让他升序，尽管默认如此    
```
for item in db.col.find().sort('age',pymongo.ASCENDING):
 print(item)
```

更新数据   
更新数据很简单，只需要一个条件和需要更新的数据即可   
复制代码 代码如下:  
`db.col.update({'_id':ObjectId('59255118d92fac43dcb1999a')},{'$set':{'name':'王二麻33333'}})`
结果如下：王二麻变成了王二麻33333  
复制代码 代码如下:   
> {'_id': ObjectId('59255118d92fac43dcb1999a'), 'name': '王二麻33333', 'province': '广东', 'age': 30}

删除数据   
删除数据使用remove()方法，如果方法带条件，则删除指定条件数据，否则删除全部   

删除name为王二麻33333的用户。   
`db.col.remove({'name':'王二麻33333'})`   
删除全部数据（慎用）    
`db.col.remove()`    
