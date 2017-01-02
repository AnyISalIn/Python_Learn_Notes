# Python 学习: 数据库操作(实现连接池)

## 代码解析

```python
import queue
from contextlib import contextmanager
import pymysql


class ConnectionPool:


    def __init__(self, **kwargs):
        '''
        __init__ 构造方法传入 kwargs, mysql 连接信息
        size 和 idle 等实例属性, 通过 dict 的 pop 方法, 获取对应的值/默认值, 并将 key 从 kwargs 中删除
        属性 size 是连接池允许使用的最大连接数
        属性 idle 是连接池内没有使用的最大连接数
        属性 kwargs 是经过 pop 后的 kwargs
        属性 length 用来记录当前连接池内的连接个数
        属性 connections 是一个FIFO队列, 最大值为 idle 的值
        '''
        self.size = kwargs.pop('size', 10)
        self.idle = kwargs.pop('idle', 3)
        self.kwargs = kwargs
        self.length = 0
        self.connections = queue.Queue(maxsize=self.idle)

    def _connect(self):
        '''
        _connect 方法
        如果 connections 队列没有被塞满
            通过 pymysql 来建立连接
            传入的连接参数是 实例属性 kwargs
            建立连接后, 将连接放到 connections 队列中
            然后 length 属性加 1
        否则
            抛出 RuntimeError 异常
        '''
        if not self.connections.full():
            conn = pymysql.connect(**self.kwargs)
            self.connections.put(conn)
            self.length += 1
        else:
            raise RuntimeError('Sorry, Lot of Connections')

    def _close(self, conn):
        '''
        _close 方法很简单, 传入一个 conn 对象, 关闭后, length 属性减1
        ''''
        conn.close()
        self.length -= 1

    def get(self, timeout=None):
        '''
        get 方法用来获取连接对象

        如果 connections 队列为空, 并且 length 小于 size
             调用 _connect 方法建立一个新连接
        获取 connections 队列中的连接对象
        重新检测连接状态, 如果连接超时了, 重新建立连接.
        '''
        if self.connections.empty() and self.length < self.size:
            self._connect()
        conn = self.connections.get(timeout=timeout)
        conn.ping(reconnect=True)
        return conn

    def return_resource(self, conn):
        '''
        return_resource 方法用于关闭或者重新将连接放到 connections 队列中

        传入 conn 对象
        如果 connections 队列满了
             调用 _close 方法关闭连接
             return 提前结束函数
        否则将 conn 对象重新放到队列中, 实现连接复用.
        '''
        if self.connections.full():
            self._close(conn)
            return
        self.connections.put(conn)

    @contextmanager
    def __call__(self, timeout=None):
        '''
        通过 contextmanager 装饰器, 实现上下文管理.

        定义 __call__ 方法, 让对象变为一个 callable 对象
        传入 timeout
            首先获取一个连接
        异常处理:
            返回一个 cursor 游标对象
            结束时 commit
        如果出现异常:
            打印异常
            rollback, 回滚事务
        无论是否出现异常:
            调用 return_resource 方法, 处理 conn 对象.
        '''
        conn = self.get(timeout)
        try:
            yield conn.cursor()
            conn.commit()
        except Exception as e:
            print('Some Error {}'.format(e))
            conn.rollback()
        finally:
            self.return_resource(conn)

```


## 测试

```python
'''
刚才的连接池代码, 文件名为 db_pool2.py
'''
from db_pool2 import ConnectionPool

'''
实例化一个 pool 对象
'''
pool = ConnectionPool(host='192.168.20.179',
                      user='root',
                      password='passwd',
                      database='demo')

'''
由于 ConnectionPool 实现了 __call__ 方法, 所以 pool 是一个 callable 对象
可以直接调用, 并且使用了 contextmanager 装饰器, 实现了 上下文管理
可以使用 with 语句.
'''
with pool() as cur:
    cur.execute('''select * from author''')
    print(cur.fetchone())

'''
返回值
'''
(2, 'AnyIsalIn', None)
```

## 总结

我们通过短短几十行代码就实现了基于 `PyMySQL` 的连接池管理, 有了连接池, 对连接的管理更加的灵活简单.
