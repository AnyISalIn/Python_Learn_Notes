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
