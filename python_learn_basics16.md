# Python 学习: 迭代器, 生成器

#### 自定义可迭代对象

迭代的意思就是重复做一件事很多次, 就像之前经常使用的 `for` 循环一样, 使用 `for` 循环对 `list`、`tuple`、`dict` 等可迭代对象进行迭代. 我们不仅能对序列/映射的数据结构进行迭代, 也能对实现了 `__iter__` 方法的对象进行迭代.

`__iter__` 方法返回一个迭代器, 所谓的迭代器就是拥有 `next` 方法, 调用 `next` 方法时会返回迭代器的下一个值, 当迭代器没有值可以返回时, 抛出一个 `StopIteration` 的异常.

实现一个指定起始值和步长的类, 调用一次 `next`, 增加步长的值.

```python
class Increment:
  def __init__(self, value=1, step=1):
    self.value = value
    self.step = step
  def __next__(self):
    self.value += self.step
    return self.value
  def __iter__(self):
    return self

In [595]: i = Increment()

In [596]: next(i) #每调用一次 next 函数, 就调用一次 __next__ 方法.
Out[596]: 2

In [597]: next(i)
Out[597]: 3

In [598]: next(i)
Out[598]: 4

In [599]: iter(i) #返回一个迭代器.
Out[599]: <__main__.Increment at 0x10e4bb6d8>

In [600]: for i in iter(i):
     ...:     if i < 10:
     ...:       print(i)
     ...:     else:
     ...:         break
     ...:
5
6
7
8
9
```

#### 从自定义的迭代器中得到序列


```python
class Increment:
  def __init__(self, value=0, step=1, max_number=10):
    self.value = value
    self.step = step
    self.max_number = max_number
    self.counter = 1
  def __next__(self):
    if self.counter <= 10:
      self.value += self.step
      self.counter += 1
      return self.value
    else:
      raise StopIteration
  def __iter__(self):
    return self

In [706]: i = Increment()

In [707]: list(i) #隐式调用 next, 当抛出 StopIteration 异常时终止.
Out[707]: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```


#### 生成器

生成器是用普通函数语法定义的迭代器, 使用生成器可以写出更加优雅的代码.

**Example**

定义一个展开嵌套列表的函数

```python
nlist = [[1, 2], [2, 3], [4, 5, 6]]
```

```python
def flatten(nlist):
  for sublist in nlist:
    for element in sublist:
      yield element


In [715]: a = flatten(nlist)

In [716]: next(a) #生成器是简单的迭代器, 也可以使用 next 函数调用.
Out[716]: 1

In [717]: next(a)
Out[717]: 2

In [718]: next(a)
Out[718]: 2

In [719]: next(a)
Out[719]: 3

In [720]: next(a)
Out[720]: 4

In [721]: next(a)
Out[721]: 5

In [722]: next(a)
Out[722]: 6

In [723]: next(a) #没有值也会抛出 StopIteration
---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-723-3f6e2eea332d> in <module>()
----> 1 next(a)

StopIteration:
```

也可以通过 `for` 循环来对生成器返回的迭代器进行迭代.

```python
In [724]: for i in flatten(nlist):
     ...:     print(i)
     ...:
1
2
2
3
4
5
6
```

生成器不像普通函数, 不像 `return` 那样返回值, 而是通过 `yield` 语句返回一个值, 并把函数冻结(`freeze`), 函数就停在那个点等待激活, 激活后的函数就从停止的那个点重新执行.

##### 生成器推导式

`python`能够使用列表推导式(解析式) 来实现简单而高效的循环.

```python
In [740]: [ (x, y) for x in range(1, 3) for y in range(1, 3) ]
Out[740]: [(1, 1), (1, 2), (2, 1), (2, 2)]
```

`python` 中也有生成器推导式(解析式), 只需要把 `[]` 换成 `()` 就会返回一个生成器而不是列表.

```python
In [741]: ( (x, y) for x in range(1, 3) for y in range(1, 3) )
Out[741]: <generator object <genexpr> at 0x10e288e60>

In [742]: a = ( (x, y) for x in range(1, 3) for y in range(1, 3) )

In [743]: for i in a:
     ...:     print(i)
     ...:
(1, 1)
(1, 2)
(2, 1)
(2, 2)
```

##### 递归生成器

如果我们要处理一个下面这样的列表, 可能就需要用到递归生成器.

```python
nlist = [[[[1, 2 ,3]], [2, 4, 5]], [3, 5, 1]]
```

```python
def flatten(nlist):
  try:
    try: nlist + '' #做类型判断, 不递归类似字符串的对象.
    except TypeError: pass  #如果捕获TypeError 异常, 则 pass
    else: raise TypeError #否则再抛出 TypeError 异常
    for sublist in nlist: #如果是列表或者元组, 则正常递归.
      for element in flatten(sublist):
        yield element
  except TypeError: #如果捕获 TypeError 异常, 只生成 nlist 的值.
    yield nlist

In [817]: nlist = [[[[1, 2 ,3]], [2, 4, 5]], [3, 5, 1]]
     ...:

In [818]: d = flatten(nlist)

In [819]: for i in d:
     ...:     print(i)
     ...:
1
2
3
2
4
5
3
5
1

In [820]: list(flatten(nlist))
Out[820]: [1, 2, 3, 2, 4, 5, 3, 5, 1]
```

##### 通用生成器

生成器在被调用时, 函数体中的代码不会执行, 返回的是一个迭代器, 每次请求一个值, 就会执行生成器中的代码, 知道遇到 `return` 或者 `yield` 语句, `return` 语句意味着生成器要停止执行, `yield` 意味着函数要冻结.

生成器有两部分组成 ,生成器的函数和生成器的迭代器, 生成器的函数是用 `def` 语句定义的部分, 也包括 `yield` 语句, 而生成器的迭代器是函数的返回的部分. 生成器返回的迭代器可以像其他迭代器一样使用.


##### 生成器方法

生成器的属性是开始运行后为生成器提供值得能力, 是生成器和外部世界进行交流的渠道.

* `send`: 向生成器内部发送值
* `close`: 停止生成器
* `throw`: 引发异常

**Example**

```python
def rep(value):
  while True:
    new = (yield value)
    if new is not None: value = new

In [844]: d = rep(10)

In [845]: next(d)
Out[845]: 10

In [846]: next(d)
Out[846]: 10

In [847]: d.send('Hello World') #send 方法向生成器中发送值
Out[847]: 'Hello World'

In [848]: next(d)
Out[848]: 'Hello World'

In [857]: d.close() #close 方法可以停止生成器

In [858]: next(d)
---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-858-57d0022d53db> in <module>()
----> 1 next(d)

StopIteration:

In [862]: d.throw(ZeroDivisionError)  #throw 方法可以引发一个异常.
---------------------------------------------------------------------------
ZeroDivisionError                         Traceback (most recent call last)
<ipython-input-862-44c9b58044eb> in <module>()
----> 1 d.throw(ZeroDivisionError)

ZeroDivisionError:
```
