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
db = pymysql.connect("localhost","testuser","test123","TESTDB" )
 
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
