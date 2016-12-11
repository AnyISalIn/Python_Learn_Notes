# Python 学习: 字符串格式化

## printf style

绝大多数语言都支持 `printf style` 风格的语句对字符串进行格式化, `python` 也支持.

**Example**
```python
In [5]: 'Hello, %s!' % 'AnyIsalIn'
Out[5]: 'Hello, AnyIsalIn!'
```

**传递多个参数**

```python
In [8]: 'Hello, %s %s' % ('AnyIsalIn','....') #需要传递一个元组作为参数.
Out[8]: 'Hello, AnyIsalIn ....'

In [10]: 'Hello, %s %s' % ('AnyIsalIn','!')
Out[10]: 'Hello, AnyIsalIn !'
```
**Template**

可以传入一个字典使其支持关键词参数

```python
In [64]: 'Hi, %(name)s' % {'name': 'anyisalin'} #需要传入一个字典
Out[64]: 'Hi, anyisalin'

In [65]: 'Hi, %(name)s, %(age)s' % {'name': 'anyisalin', 'age': 18}
Out[65]: 'Hi, anyisalin, 18'
```

## format

`python3` 中支持更易用的 `format` 方法来对字符串进行格式化.

**Example**

```python
In [2]: 'Hi {}, Age {}'.format('AnyIsalIn', '18')  #传入一个元组
Out[2]: 'Hi AnyIsalIn, Age 18'

In [4]: 'Hi {name}, Age {age}'.format(name='AnyIsalIn', age=18) #同样支持关键字参数
Out[4]: 'Hi AnyIsalIn, Age 18'
```

还支持对传递的值进行操作后返回

```python
In [6]: '{lst[0]}'.format(lst=['a', 'b', 'c']) #返回列表的第一个元素.
Out[6]: 'a'

```
