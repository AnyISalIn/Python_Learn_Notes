# Python 学习: 内置容器(列表)

序列是`Python`中最基本的数据结构。序列中的每个元素都分配一个数字 - 它的位置，或索引，第一个索引是0，第二个索引是1，依此类推

## 初始化一个空列表

```python
lst = []
或
lst = list()

In [12]: lst
Out[12]: []

In [13]: lst = [1, 2, 3]

In [14]: lst
Out[14]: [1, 2, 3]
```

## 下标/索引操作

```python
In [14]: lst
Out[14]: [1, 2, 3]

In [15]: lst[1]
Out[15]: 2

In [16]: lst[0]
Out[16]: 1

In [17]: lst[-1]
Out[17]: 3
```

## 添加元素

### `append`
`append` 方法原地修改`list`， 给`list`增加一个元素，`append`的方法的返回值是`None`

```python
In [27]: lst
Out[27]: [1, 2, 3]

In [28]: lst.append('444')

In [29]: lst
Out[29]: [1, 2, 3, '444']
```

### `insert`

`insert`操作的索引超出范围， 如果是正索引， 等效于`append`， 如果是负索引， 等效于`insert(0, object)`

```python
In [18]: lst
Out[18]: [1, 2, 3]

In [19]: lst.insert(0, '7777')

In [20]: lst
Out[20]: ['7777', 1, 2, 3]

In [21]: lst.insert(2, '9999')

In [22]: lst
Out[22]: ['7777', 1, '9999', 2, 3]

In [23]: lst.insert(-1, '0000')

In [24]: lst
Out[24]: ['7777', 1, '9999', 2, '0000', 3]
```

### `extend`

`extend` 可以一次指定多个元素.

```python
In [33]: lst
Out[33]: [1, 2, 3]

In [35]: lst.extend([4, 5, 6])

In [36]: lst
Out[36]: [1, 2, 3, 4, 5, 6]

```

## 删除元素


### `pop`

删除指定的索引, 默认为 `-1`

```python
In [62]: lst
Out[62]: [1, 2, 3]

In [63]: lst.pop()
Out[63]: 3

In [64]: lst
Out[64]: [1, 2]

In [65]: lst.append(3)

In [66]: lst
Out[66]: [1, 2, 3]

In [67]: lst.pop(1)
Out[67]: 2

In [68]: lst
Out[68]: [1, 3]

In [69]: lst.pop(-1)
Out[69]: 3

In [70]: lst
Out[70]: [1]

In [91]: lst.pop(8) #如果`index`超出索引返回，会抛出`IndexError`,
---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
<ipython-input-91-04ae2f3ecd71> in <module>()
----> 1 lst.pop(8)

IndexError: pop index out of range
```

### `remove`

`pop` 是弹出索引对应的值 `remove`是删除最左边的一个值 `pop`针对的是索引 `remove`针对的是值

```python
In [82]: lst
Out[82]: [1, 2, 3]

In [83]: lst.remove(3)

In [84]: lst
Out[84]: [1, 2]

In [85]: lst.insert(0, 7)

In [86]: lst
Out[86]: [7, 1, 2]

In [87]: lst.remove(7)

In [88]: lst
Out[88]: [1, 2]

In [89]: lst.remove(8) #remove 的时候，如果值不存在，会抛出ValueError
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-89-6e447793d6e1> in <module>()
----> 1 lst.remove(8)

ValueError: list.remove(x): x not in list

```

### `clear`

`clear` 方法删除列表中所有元素

```python
In [105]: lst
Out[105]: [1, 2, 3, 4, 5]

In [106]: lst.clear()

In [107]: lst
Out[107]: []
```

## 查找/统计元素

### `index`

`index` 方法通过值来查找索引

```python
In [111]: lst
Out[111]: [1, 2, 3, 4, 5]

In [112]: lst.index(1)
Out[112]: 0

In [113]: lst.index(2)
Out[113]: 1

In [114]: lst.append(1)

In [115]: lst.index(1) #默认情况下, 返回查找到的第一个索引
Out[115]: 0

In [116]: lst.index(1, 2) #也可以指定值, 返回第二个索引
Out[116]: 5
```

### `count`

`count` 方法通过给的值在列表中查找值的个数

```python
In [126]: lst
Out[126]: [1, 2, 3, 4, 5, 1]

In [127]: lst.count(1)
Out[127]: 2

In [128]: lst.count(2)
Out[128]: 1
```

### `len`

`len` 返回列表索引个数

```python
In [129]: lst
Out[129]: [1, 2, 3, 4, 5, 1]

In [130]: len(lst)
Out[130]: 6

In [131]: lst.append('a')

In [132]: len(lst)
Out[132]: 7
```

## 修改列表

### `sort`

`sort` 方法对列表中的值重新排序

```python
In [10]: lst
Out[10]: [1, 4, 5, 3, 2]

In [11]: lst.sort()

In [12]: lst
Out[12]: [1, 2, 3, 4, 5]

In [20]: lst.sort(reverse=True) #倒序

In [21]: lst
Out[21]: [5, 4, 3, 2, 1]
```

### `reverse`

`reverse` 和 `sort` 相反

```python
In [33]: lst
Out[33]: [1, 3, 2, 4, 5]

In [34]: lst.reverse()

In [35]: lst
Out[35]: [5, 4, 2, 3, 1]
```

## 其他方法

### `copy`

`copy` 方法返回列表中的所有值

```python
In [37]: lst
Out[37]: [5, 4, 2, 3, 1]

In [39]: lst.copy()
Out[39]: [5, 4, 2, 3, 1]

In [42]: lst2 = lst.copy()

In [43]: lst2
Out[43]: [5, 4, 2, 3, 1]

In [44]: id(lst)
Out[44]: 4382739912
                    #这里可以看出来, copy方法生成新新列表虽然列表完全相同, 但是指向的内存空间不同.
In [45]: id(lst2)
Out[45]: 4382776712

In [46]: lst3 = lst #而直接通过赋值的方式生成的新列表, 指向的内存空间和原来的相同, 也就意味着, 修改 lst3, lst 也会随之修改.

In [47]: id(lst)
Out[47]: 4382739912
```
