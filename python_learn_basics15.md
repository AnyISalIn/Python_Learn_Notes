# Python 学习: property, 静态方法, 类成员方法, 装饰器, 拦截方法


#### `property`

如果类中有一些方法是用来得到或者重新绑定特性的, 那么它就是这个特性的 `访问器方法`

**Example**

未使用 `property` 函数

```python
class Person:
  def __init__(self):
    self.name = 'Bob'
    self.age = 18
  def setInfo(self, info):
    self.name, self.age = info
  def getInfo(self):
    return self.name, self.age   

In [443]: p = Person()

In [444]: p.getInfo()
Out[444]: ('Bob', 18)

In [445]: p.setInfo(('AnyIsalIn', 27))

In [446]: p.getInfo()
Out[446]: ('AnyIsalIn', 27)

In [447]: p.info  #info 并不是一个真正的特性.
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-447-d7da7e894d76> in <module>()
----> 1 p.info

AttributeError: 'Person' object has no attribute 'info'
```

使用 `property` 函数

```python
class Person:
  def __init__(self):
    self.name = 'Bob'
    self.age = 18
  def setInfo(self, info):
    self.name, self.age = info
  def getInfo(self):
    return self.name, self.age
  info = property(getInfo, setInfo)

In [449]: p = Person()

In [450]: p.name
Out[450]: 'Bob'

In [451]: p.age
Out[451]: 18

In [452]: p.info = ('AnyISalIn', 17)

In [453]: p.info  #info 是一个属性.
Out[453]: ('AnyISalIn', 17)

In [454]: p.getInfo()
Out[454]: ('AnyISalIn', 17)
```

#### 静态方法、类成员方法、装饰器

静态方法和类成员方法可以被类直接调用, 不需要实例化对象, 也不需要 `self` 参数.

```python
class Class:
  def smethod():
    print('This is staticmethod')
  smethod = staticmethod(smethod)

  def cmethod(cls): #类成员方法需要传入 class 自身.
    print('This is classmethod', cls)
  cmethod = classmethod(cmethod)

In [456]: Class.smethod()
This is staticmethod

In [457]: Class.cmethod()
This is classmethod <class '__main__.Class'>
```

我们也可以使用装饰器对其进行包装

```python
class Class:
  @staticmethod
  def smethod():
    print('This is staticmethod')

  @classmethod
  def cmethod(cls):
    print('This is classmethod', cls)

In [461]: Class.smethod()
This is staticmethod

In [462]: Class.cmethod()
This is classmethod <class '__main__.Class'>
```

#### 拦截方法

之前介绍过, `__setitem__`, `__delitem__` 等序列, 映射的特殊方法, 对于特性我们可以使用如下方法.

* `__getattribute__(self, name)`: 当特性 `name` 被访问时自动调用
* `__getattr__(self, name)`: 当特性 `name` 被访问且对象没有相应特性时自动调用.
* `__setattr__(self, name, value)`: 当试图给特性 `name` 赋值时被自动调用
* `__delattr__(self, name)`: 当试图删除特性 `name` 时被自动调用


定义一个只能对特性进行赋值的类, 不能删除, 不能访问

```python
from six import string_types
class Class:
  def __init__(self):
    self.name = 'AnyIsalIn'
  def __getattribute__(self, name):
    raise Exception('Sorry, Can\'t Access Attribute')
  def __setattr__(self, name, value):
    if not isinstance(value, string_types):
      raise Exception('Sorry, Name Only Can Use String')
  def __delattr__(self, name):
    raise Exception('Sorry, Can\'t Delete Attribute')

In [578]: p.name = 'wwwww'  #可以进行赋值

In [579]: p.name  #当访问 name 特性时会提示 'can't access attribute'
---------------------------------------------------------------------------
Exception                                 Traceback (most recent call last)
<ipython-input-579-1c57ed665d7c> in <module>()
----> 1 p.name

<ipython-input-568-480b64a444f1> in __getattribute__(self, name)
      3     self.name = 'AnyIsalIn'
      4   def __getattribute__(self, name):
----> 5     raise Exception('Sorry, Can\'t Access Attribute')
      6   def __setattr__(self, name, value):
      7     if not isinstance(value, string_types):

Exception: Sorry, Can't Access Attribute

In [580]: del p.name  #当删除 name 特性时会提示, 'can't delete attribute'
---------------------------------------------------------------------------
Exception                                 Traceback (most recent call last)
<ipython-input-580-82a48009a62c> in <module>()
----> 1 del p.name

<ipython-input-568-480b64a444f1> in __delattr__(self, name)
      8       raise Exception('Sorry, Name Only Can Use String')
      9   def __delattr__(self, name):
---> 10     raise Exception('Sorry, Can\'t Delete Attribute')

Exception: Sorry, Can't Delete Attribute
```
