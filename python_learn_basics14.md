# Python 学习: 特殊方法

`python` 中有一些方法前面和后面会加上 `__`, 这种写法很特别, 就像 `docstring` 的 `__doc__` 方法用来查看一个函数/方法的说明. 这些特殊方法会在某种特殊情况下被 `python` 调用, 一般情况下不会直接调用特殊方法.


#### 构造方法

构造方法用来初始化一个对象, 当一个方法实例化一个对象时, 构造方法 `__init__` 方法中的代码就会自动被调用, 可以达到初始化一个对象特性的作用.

**Example**

```python
class Class:
  def __init__(self):
    self.value = 'This is initilize value'  #初始化时定义 value 的值.
  def getValue(self):
    return self.value

In [187]: i = Class()

In [188]: i.getValue()
Out[188]: 'This is initilize value'
```

定义一个可以传递参数的 `__init__` 方法

```python
class Class:
  def __init__(self, value='init value'): #就像普通函数一样定义.
    self.value = value
  def getValue(self):
    return self.value

In [195]: i = Class('external param') #实例化对象时可以传递参数.

In [197]: i.getValue()
Out[197]: 'external param'
```


#### 子类重写构造方法

之前我们介绍继承的时候, 子类可以继承超类的所有方法, 但是如果我们重写构造方法但又想继承超类的构造方法该怎么办, 可以使用 `super` 函数.

```python
class Bird:
  def __init__(self):
    self.hungry = False
  def eat(self):
    if self.hungry:
      print('No, Thanks!')
    else:
      print('Hahaahahah, Thanks')
      self.hungry = True

In [210]: a = Bird()

In [211]: a.eat()
Hahaahahah, Thanks

In [212]: a.eat()
No, Thanks!

class SongBird(Bird): #继承超类, 但是不使用 super 继承构造方法.
  def __init__(self):
    self.sound = 'HAHAHHAHA'
  def song(self):
    print(self.sound)

In [216]: s = SongBird()

In [217]: s.song()
HAHAHHAHA

In [218]: s.eat() #虽然有这个方法, 但是因为没有调用超类的构造方法初始化 hungry 特性.
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-218-a07222e65a25> in <module>()
----> 1 s.eat()

<ipython-input-209-cc280f0b82f4> in eat(self)
      3         self.hungry = False
      4     def eat(self):
----> 5         if self.hungry:
      6             print('No, Thanks!')
      7         else:

AttributeError: 'SongBird' object has no attribute 'hungry'
```

使用 `super` 方法

```python
class SongBird(Bird):
  def __init__(self):
    self.sound = 'HAHAHHAHA'
    super(SongBird, self).__init__()  #调用 super 方法.
  def song(self):
    print(self.sound)


In [220]: s = SongBird()

In [221]: s.eat()   #可以正常调用 eat, 说明特性 hungry 已经正常初始化.
Hahaahahah, Thanks

In [222]: s.eat()
No, Thanks!

In [223]: s.song()
HAHAHHAHA
```
#### 序列和映射方法, 自定义一个 `mapping` 类

* `__len__`: 该方法返回序列对象中元素的个数, 如果是映射对象, 返回 `key value` 的对数.
* `__getitem__`: 该方法返回所给键对应的值.
* `__setitem__`: 该方法按一定方法存储 `key`, `value`, 可通过`__getitem__` 方法获取值.
* `__delitem__`: 该方法在一部分对象使用 `del` 语句时被调用, 删除和元素或者键.

**Example**

通过以上四种方法, 可以自定义出一个 `mapping` 类.

```python
class CustomMapping:
  def __init__(self):
    self.store = {}
  def __setitem__(self, key, value):
    self.store[key] = value
  def __getitem__(self, key):
    try:
      return self.store[key]
    except KeyError:
      return 'Sorry, Please Set Key'
  def __delitem__(self, key):
    del self.store[key]
```

很多地方没有做异常处理, 但是基本功能都有.

```python
In [340]: d = CustomMapping()

In [341]: d['a'] = 'b'

In [342]: d['a']
Out[342]: 'b'

In [343]: del d['a']

In [344]: d['a']
Out[344]: 'Sorry, Please Set Key'

In [345]: len(d)
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-345-e46184627ba1> in <module>()
----> 1 len(d)

TypeError: object of type 'CustomMapping' has no len()
```
再定义一个 `__len__` 方法

```python
class CustomMapping:
  def __init__(self):
    self.store = {}
  def __setitem__(self, key, value):
    self.store[key] = value
  def __getitem__(self, key):
    try:
      return self.store[key]
    except KeyError:
      return 'Sorry, Please Set Key'
  def __delitem__(self, key):
    del self.store[key]
  def __len__(self):
    return len(self.store)


In [347]: d = CustomMapping()

In [348]: d['a'] = '2'

In [349]: d['b'] = '3'

In [350]: len(d)
Out[350]: 2

In [351]: d['e'] = '4'

In [352]: len(d)
Out[352]: 3
```

#### 子类化列表

如果想实现一个和内建列表功能相同的序列, 可以使用子类 `list`

**Example**

实现一个带有访问计数功能的列表

```python
class CounterList(list):
  def __init__(self, *args):
    super(CounterList, self).__init__(*args)
    self.number = 0
  def __getitem__(self, index):
    self.number += 1
    return super(CounterList, self).__getitem__(index)

# CounterList 功能几乎和列表一样, 但是在访问元素的时候会进行计数.
In [393]: c = CounterList((1, 2, 3))

In [394]: c.number
Out[394]: 0

In [395]: c[1]
Out[395]: 2

In [396]: c[2]
Out[396]: 3

In [397]: c.number
Out[397]: 2

In [398]: c[0]
Out[398]: 1

In [399]: c.number
Out[399]: 3
```
