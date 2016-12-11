# Python 学习: 高阶函数

一个函数能够接受另一个函数作为参数, 或者能返回一个函数, 这样的函数就是高阶函数.

**Example**

```python
In [1]: def y(n):
   ...:     return n * n
   ...:

In [2]: def fun(x, z, y):
   ...:     return y(x) +  y(z)
   ...:

In [3]: fun(1, 2, y)
Out[3]: 5

In [4]: fun(2, 4, y)
Out[4]: 20
```

## `Python` 中常用的高阶函数

### `map`

`map` 接受两个参数, 一个函数, 一个可迭代对象, `map` 将传入的函数依次作用到可迭代对象中的每一个元素, 并返回一个迭代器.

```python
In [16]: def y(x):
    ...:     return x * x
    ...:

In [17]: list(map(y, [1, 2, 4 ,8]))
Out[17]: [1, 4, 16, 64]
```

### `reduce`

`reduce` 接受两个参数, 一个函数, 一个序列对象, 传入的函数必须接受两个参数, `reduce` 把结果和序列下一个元素做累计计算.

```python
In [5]: from functools import reduce

In [6]: def add(x, y):
   ...:     return x + y
   ...:

In [7]: reduce(add, [1, 2, 3, 4])
Out[7]: 10


In [8]: from operator import concat

In [10]: reduce(concat, ['AB', 'AC', 'DY'])
Out[10]: 'ABACDY'
```

### `filter`

`filter` 用于过滤序列中的元素, `filter` 也接受两个参数, 一个函数, 一个序列对象, `filter` 也将传入的函数作用于序列中的每个元素, 和 `map/reduce` 不同的是, `filter` 根据函数返回的值是 `True` 或者 `False` 来判定是否保留元素.

```python
In [14]: def fun(x):
    ...:     if x > 10:
    ...:         return False
    ...:     else:
    ...:         return True
    ...:

In [15]: list(filter(fun, [10, 8, 11, 2, 21, 7]))
Out[15]: [10, 8, 2, 7]
```

### `sorted`

`sorted` 函数默认情况下可以只传入一个序列对象, 对其进行排序, 但是它也是一个高阶函数, 可以接受一个 `key` 函数来进行排序

```python
# 不接受函数的 sorted
In [17]: sorted(['111', '222', '121', '001'])
Out[17]: ['001', '111', '121', '222']
```

```python
In [18]: sorted([-1, -3, 100, -23, 8, 9])
Out[18]: [-23, -3, -1, 8, 9, 100]

In [19]: sorted([-1, -3, 100, -23, 8, 9], key=abs)  # 接受 abs 函数, 按照绝对值排序
Out[19]: [-1, -3, 8, 9, -23, 100]
```
