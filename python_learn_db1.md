# Python 学习: 数据库操作(PyMySQL)

## Python MySQL Drive Overview

`MySQL` 是一款非常流行开源跨平台的关系型数据库, `Python` 中有若干个优秀的连接驱动.

* `MySQLdb`: 曾经最流行, 但是由于是 `C` 语言编写, 不易维护, 并且不支持 `Python 3`
* `mysql-connector-python`: `Oracle/MySQL` 官方提供, 用的不是很多.
* `PyMySQL`: 目前最流行的 `mysql` 连接驱动, 纯 `Python` 编写, 支持 `Python 3`


这篇笔记主要介绍, `PyMySQL` 的基本使用.


## 环境准备, 基础操作

```sql
mysql> create database demo;

mysql> use demo;

mysql> CREATE TABLE author (
`id` INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
`name` VARCHAR(45) NOT NULL,
`country` VARCHAR(20)
);

mysql> INSERT INTO author (name, country) VALUES ('AnyISalIn', 'CN');

mysql> SELECT * FROM author;
+----+-----------+---------+
| id | name      | country |
+----+-----------+---------+
|  1 | AnyISalIn | CN      |
+----+-----------+---------+
```

### 通过 PyMySQL 增删改查

```python
import pymysql
from pymysql.cursors import  DictCursor

conn = pymysql.connect(host='192.168.20.179', user='root', password='passwd', database='demo', cursorclass=DictCursor)  #实例化一个连接对象.
cur = conn.cursor() #返回一个游标对象
cur.execute('select * from author') #通过 execute 方法执行 SQL 语句.
print(cur.fetchall()) #通过 fetchall 所有执行结果.

[{'id': 1, 'name': 'AnyISalIn', 'country': 'CN'}] #返回值
```

```python
cur.execute('delete from author where id=1')
print(cur.fetchall())

() #返回值

'''
按理说, 应该这条记录已经被我们删掉了, 那么我们在查询试试.
'''

cur.execute('select * from author')
print(cur.fetchall())

[{'country': 'CN', 'name': 'AnyISalIn', 'id': 1}  #仍然还在

'''
这是因为我们没有提交事务(transaction)
'''

cur.execute('delete from author where id=1')
conn.commit()
print(cur.fetchall())

() #返回值

cur.execute('select * from author')
conn.commit()
print(cur.fetchall())

() #返回值

'''
这次就删除成功了, 事务是数据库中的重要概念, 可以 Google 查找相关资料.
'''

'''
同理, 增删改都需要 commit 才能存储到数据库中.
'''
```

### fetchall 和 fetchone

`fetchall` 和 `fetchone` 分别表示, 一次返回所有查询的记录和一次返回一条记录, 但是它们并不会修改 `SQL` 语句, 使用类似 `limit`、`offset` 等语句, 而是返回 **sql** 语句返回的 **全部记录** 和 **一条记录**

也就意味着, 如果我们执行了一条 `select * from users` 语句, 实际上这条 `sql` 语句可能有数千条记录返回, 这数千条记录存储由 ` PyMySQL` 临时存储,  `fetchall` 将它们全部返回, `fetchone` 只返回一条, 但是 `select *` 是大忌, 一般不会这样使用, 在数据较多情况下, 会导致数据库崩溃, 所以如果我们要限定返回记录的行数, 建议使用 `LIMIT, OFFSET` 等语句.


### 关闭连接

```python
'''
conn 对象就和类文件和 socks 操作一样, 在用完之后要 close
'''

conn.close()

'''
如果不关闭, 连接会一直存在, 占用资源
'''

tcp6       0      0 172.24.1.173:3306       192.168.20.151:61503    ESTABLISHED 935/mysqld


'''
这时候我们就需要 context manager 了,
如果 PyMySQL 的 cursor 实现了 __enter__, __exit__ 这几个魔术方法,
我们就可以通过 with 语句自动实现 commit, close 等操作了, 下面是 PyMySQL cursors.py 的代码
'''

def __enter__(self):
    """Context manager that returns a Cursor"""
    return self.cursor()

def __exit__(self, exc, value, traceback):
    """On successful exit, commit. On exception, rollback"""
    if exc:
        self.rollback()
    else:
        self.commit()

'''
有了 context manager, 我们就能够使用 with 语句
如果有异常, 会自动 rollback, 否则 commit
'''

with conn as cur:
    cur.execute('select * from author')
    print(cur.fetchall())

'''
但是可以看出, cursor 实现了 context manager, 但是 __exit__ 只 commit, 并没有 close, 这是为什么?

这就和连接复用有关了, 如果我们的代码中要执行多次 sql 语句, 难道还要建立多次连接么, 这显然是不科学的.

其实这种情况, 完全可以使用连接池(connections pool) 来解决, 下一篇我们就来解析一个简单的连接池的实现.
'''
```
