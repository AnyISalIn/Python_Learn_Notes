# Python 学习: 内置容器(元组), 切片操作

`Python`的元组与列表类似，不同之处在于元组的元素不能修改。
元组使用小括号，列表使用方括号。


## 元组

```python
In [2]: t
Out[2]: ()

In [3]: t = ()

In [4]: t
Out[4]: ()

In [5]: t = (1, 2, 3)

In [6]: t
Out[6]: (1, 2, 3)

#初始化只有一个元素的元组
In [35]: type(t)
Out[35]: int

In [36]: t = (1)

In [37]: type(t)
Out[37]: int

In [38]: t = (1,)

In [39]: type(t)
Out[39]: tuple
```

```python
In [31]: t
Out[31]: (1, 2, 3)

In [32]: t[0] = 2  #元组不可变
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-32-29b3302c4f70> in <module>()
----> 1 t[0] = 2

TypeError: 'tuple' object does not support item assignment
```

## 切片

```python
In [61]: lst
Out[61]: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19]

In [62]: lst[0:5] #从左往右切片
Out[62]: [0, 1, 2, 3, 4]

In [72]: lst[-7:-1] #从右往左切片
Out[72]: [13, 14, 15, 16, 17, 18]

In [96]: lst[1:8]
Out[96]: [1, 2, 3, 4, 5, 6, 7]

In [97]: lst[1:8:2] #指定步长
Out[97]: [1, 3, 5, 7]

In [103]: lst[18:10:-2] #当步长为负数时, start 当大于 end
Out[103]: [18, 16, 14, 12]

In [109]: lst[:3]
Out[109]: [0, 4, 5]

In [110]: lst[:3] = [1, 2, 3] #对切片进行赋值, 会替换原有的元素.

In [111]: lst[:3]
Out[111]: [1, 2, 3]
```
