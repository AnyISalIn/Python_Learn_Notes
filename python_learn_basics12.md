# Python 学习: 面向对象编程(类, 方法, 继承)

## 创建自己的类

我们先定义一个简单的类.

```python
class Person:
  def setInfo(self, name, age):
      self.name = name
      self.age = age
  def getInfo(self):
      return self.name, self.age
  def hello(self):
      return 'Hello , %s, %s !' % (self.name, self.age)
```

```python
In [29]: p = Person() #实例化一个对象

In [30]: p.setInfo('AnyISalIn', 18)

In [31]: p.getInfo()
Out[31]: ('AnyISalIn', 18)

In [32]: p.hello()
Out[32]: 'Hello , AnyISalIn, 18 !'

In [33]: a = Person()

In [34]: a.setInfo('ShuangJie', 58)

In [35]: a.getInfo()
Out[35]: ('ShuangJie', 58)

In [36]: a.hello()
Out[36]: 'Hello , ShuangJie, 58 !'
```

通过上面的例子我们可以明显看出面向对象封装的特性, 不同实例的变量不会相互影响. 但是我们会发现类里面定义的方法和普通的函数有一点不同, 有一个 `self` 的参数, 在 `python` 中, `self` 代表对于对象自身的引用.

刚才的例子, `p.getInfo`, 其实 `p` 就把自己当做第一个参数传入, 这个参数一般都是对象自身, 所以习惯上称为 `self`.

如果知道 `p` 是 `Person` 的实例的话, 可以把 `p.getInfo()` 看成 `Person.getInfo(p)` 的简写.

## 私有方法

很多时候, 我们可能希望类中的某一个方法不能被外界直接访问, 那么我们就需要定义私有方法.

```python
class Class:
  def accessible(self): #类内部还是能够直接调用
    print('This is accessible method')
    self.__inaccessible()
  def __inaccessible(self): #需要在方法名字前添加 __ 即可使其变为私有方法.
    print('Can\'t access')

In [39]: a = Class()

In [40]: a.accessible()
This is accessible method
Can't access

In [41]: a._Class__inaccessible() #也可以通过这种方式在外部调用.
Can't access
```

## 类的命名空间

有时候我们可能需要定义一个变量, 可供类的所有实例访问. 我们可以把变量放到方法的外部.

```python
class MemberCoutner:
  members = 0
  def init(self):
    MemberCoutner.members += 1

In [43]: m1 = MemberCoutner()

In [44]: m1.init()

In [45]: m1.members
Out[45]: 1

In [46]: m2 = MemberCoutner()

In [47]: m2.init()

In [48]: m2.members #类作用域的变量可以被所有实例访问.
Out[48]: 2
```

如果在实例中重新绑定 `members` 特性.

```python
In [50]: m1.members
Out[50]: 2

In [51]: m1.members = 'ACTIVE'

In [52]: m1.members #新值写到 m1的特性中, 屏蔽了类范围内的变量. 就相当于函数内的全局/局部变量.
Out[52]: 'ACTIVE'

In [53]: m2.members
Out[53]: 2
```

## 超类和继承

上一篇文章说过, 面向对象有继承的特性, 即子类继承和扩展超类的方法.

```python
class Bird: #定义 Bird 类
  def eat(self):
    print('Thanks')
  def fly(self):
    print('I\'m flying')

class penguin(Bird):  #定义 penguin, 超类为 Bird
  def fly(self):  #重写 fly 方法
    print('I can\'t fly')

In [61]: p = penguin()

In [62]: p.eat()  #我们并未对 penguin 类定义 eat 的方法, 可以看出已经从 Bird 类继承了 eat 方法.
Thanks

In [63]: p.fly()  #penguin 的 fly 方法明显使我们从写的.
I can't fly
```

查看一个类是否是另一个类的子类.

```python
In [64]: issubclass(penguin, Bird)  #可以看出 penguin 是 Bird 的子类
Out[64]: True

In [65]: issubclass(penguin, list)
Out[65]: False
```

检查一个对象是否是一个类的实例

```python
In [66]: isinstance(p, penguin) #实例 p 是类 penguin 的实例
Out[66]: True
```

查看一个对象属于哪一个类, 通过 `__class__` 特性来查看.

```python
In [67]: p.__class__
Out[67]: __main__.penguin
```

**多个超类**

我们可以定义多个超类, 子类从所有超类中继承所有方法.

```python
class Person:
  def eye(self):
    print('Beautiful world !')
  def leg(self):
    print('I can walk')
  def hand(self):
    print('I have a girlfriend')

class soundcard:
  def say(self):
    print('I was finally able to speak!')

class SoundPerson(Person, soundcard):  #类 SoundPerson 什么也做, 只继承所有超类的所有方法
  pass

In [73]: s = SoundPerson()

In [74]: s.eye()
Beautiful world !

In [75]: s.leg()
I can walk

In [76]: s.hand()
I have a girlfriend

In [77]: s.say()
I was finally able to speak!
```

检查对象的方法是否存在

```python
In [88]: hasattr(s, 'say')
Out[88]: True
```
