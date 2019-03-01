# python-数据存储
* 把数据存储在 TXT
```
title = 'this is a test contents'
with open(r'c:\user\desktop\title.txt','a+') as f:
    f.write(title)
```
打开文件的方式

| 读写方式 | 可否读写 | 若文件不存在 | 写入方式 |
| --- | --- | --- | --- |
| W | 写入 | 创建 | 覆盖写入 |
