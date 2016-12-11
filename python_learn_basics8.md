# Python 学习: 内置容器(字典)

### 什么是字典

在 `python` 中, 当索引不好用的时候, 就应该使用字典, 字典是一种通过名字引用值的数据结构, 这种结构类型称为映射'mapping', 字典是无序的数据结构.

**Example**

需要存储姓名和电话号码, 通过列表操作

```python
In [1]: names = ['AnyIsalIn', 'Bob', 'Bill Gates']
In [2]: phones = ['110', '119', '120']

#假设这两个列表的元素索引一一对应, 通过名字查找号码, 非常的麻烦

In [3]: phones[names.index('AnyIsalIn')]
Out[3]: '110'
```

通过字典操作.

```python
In [20]: phonebooks = {
    ...:
    ...:     'AnyISalIn': {
    ...:         'phone': 110
    ...:          },
    ...:     'Bob': {
    ...:         'phone': 119
    ...:         },
    ...:     'Bill Gates': {
    ...:         'phone': 120
    ...:         },
    ...: }

In [21]: phonebooks
Out[21]:
{'AnyISalIn': {'phone': 110},
 'Bill Gates': {'phone': 120},
 'Bob': {'phone': 119}}

In [22]: phonebooks['AnyISalIn']
Out[22]: {'phone': 110}

In [23]: phonebooks['AnyISalIn']['phone']
Out[23]: 110
```

### 字典的操作和常用方法

#### 初始化一个字典

```python

In [28]: d = {}

In [29]: type(d)
Out[29]: dict

In [30]: d = dict(d)

In [31]: type(d)
Out[31]: dict

#dict 还可以将其他映射转换为字典, 我们将之前的两个列表转换为字典.

In [33]: names
Out[33]: ['AnyIsalIn', 'Bob', 'Bill Gates']

In [34]: phones
Out[34]: ['110', '119', '120']

In [35]: mapping_list = list(zip(names, phones)) #通过 zip 函数合并两个列表

In [36]: mapping_list
Out[36]: [('AnyIsalIn', '110'), ('Bob', '119'), ('Bill Gates', '120')]

In [37]: dict(mapping_list)
Out[37]: {'AnyIsalIn': '110', 'Bill Gates': '120', 'Bob': '119'}
```

#### 添加Key, Value

```python
In [40]: d
Out[40]: {}

In [41]: d['key'] = 'value'

In [42]: d
Out[42]: {'key': 'value'}
```

#### 删除 Key

```python
In [42]: d
Out[42]: {'key': 'value'}

In [43]: del d['key']

In [44]: d
Out[44]: {}
```

#### 更新 Value

```python
#python 字典的 key 不可变. 和集合类似.
In [50]: d
Out[50]: {'key': 'value'}

In [51]: d['key'] = 'new_value'

In [52]: d
Out[52]: {'key': 'new_value'}
```

#### 清空字典

```python
In [63]: d
Out[63]: {'AnyIsalIn': '110', 'Bill Gates': '120', 'Bob': '119'}

In [64]: d.clear()

In [65]: d
Out[65]: {}
```

#### 复制字典(浅复制-->深度复制)

```python
#如果通过赋值的方式去 "复制" 一个字典(其实只是赋值而已),就会出现下面这样的情况, 因为他们指向的是同一段地址空间.
In [126]: d
Out[126]: {'AnyISalIn': '189', 'AnyIsalIn': '110', 'Bill Gates': '120', 'Bob': '119'}

In [127]: d2 = d

In [128]: d2
Out[128]: {'AnyISalIn': '189', 'AnyIsalIn': '110', 'Bill Gates': '120', 'Bob': '119'}

In [129]: d2.clear()

In [131]: d2
Out[131]: {}

In [132]: d
Out[132]: {}

In [137]: id(d)
Out[137]: 4557399880

In [138]: id(d2)
Out[138]: 4557399880
```

**通过 copy 方法进行复制**

```python
In [168]: d
Out[168]:
{'AnyIsalIn': '110',
 'Bill Gates': '120',
 'Bob': '119',
 'test': ['1', '2', '3']}

In [169]: d2 = d.copy()

In [170]: d2
Out[170]:
{'AnyIsalIn': '110',
 'Bill Gates': '120',
 'Bob': '119',
 'test': ['1', '2', '3']}

In [172]: id(d2) == id(d) #可以看出通过 copy 复制的列表
Out[172]: False

In [177]: d2['AnyISalIn'] = '187'

In [178]: d #可以看出对 d2的修改并没有影响原字典.
Out[178]:
{'AnyIsalIn': '110',
 'Bill Gates': '120',
 'Bob': '119',
 'test': ['1', '2', '3']}

In [179]: d2
Out[179]:
{'AnyISalIn': '187',
 'AnyIsalIn': '110',
 'Bill Gates': '120',
 'Bob': '119',
 'test': ['1', '2', '3']}
```

**copy 的问题**

```python
### 但是还是有问题!!!

# 当我们原地修改字典中的数据时, 会发生如下情况

In [181]: d2
Out[181]:
{'AnyISalIn': '187',
 'AnyIsalIn': '110',
 'Bill Gates': '120',
 'Bob': '119',
 'test': ['1', '2', '3']}

In [182]: d2['test'].remove('1') #删除字典 key test 对应的列表中的值.

In [183]: d2
Out[183]:
{'AnyISalIn': '187',
 'AnyIsalIn': '110',
 'Bill Gates': '120',
 'Bob': '119',
 'test': ['2', '3']}

In [185]: d #发现原字典还是被修改了.
Out[185]: {'AnyIsalIn': '110', 'Bill Gates': '120', 'Bob': '119', 'test': ['2', '3']}
```

**深度复制(deep copy)**

```python
#这时候我们就需要使用深度复制(deep copy) 来解决这个问题了!

In [188]: from copy import deepcopy #导入 deepcopy 模块.

In [189]: d = {}

In [191]: d['test'] = ['1', 2, 3]

In [192]: d
Out[192]: {'test': ['1', 2, 3]}

In [193]: c = deepcopy(d)

In [194]: c
Out[194]: {'test': ['1', 2, 3]}

In [195]: c['test'].append('aaa')

In [196]: c
Out[196]: {'test': ['1', 2, 3, 'aaa']} #修改新字典.

In [197]: d
Out[197]: {'test': ['1', 2, 3]} #原字典没有发生改变.
```

#### 其他方法

##### fromkeys

```python
In [206]: d = {}

In [207]: d.fromkeys(['key1', 'key2', 'key3']) #通过序列化来初始化字典的 keys,
Out[207]: {'key1': None, 'key2': None, 'key3': None}

In [214]: d.fromkeys(('key1', 'key2', 'key3'), 'Unkonw')  #也可以指定默认 value
Out[214]: {'key1': 'Unkonw', 'key2': 'Unkonw', 'key3': 'Unkonw'}
```

##### get

```python
In [221]: d['jobs'] #通过这种方式访问 key, 如果 key 不存在, 则会报 KeyError 的错误.
---------------------------------------------------------------------------
KeyError                                  Traceback (most recent call last)
<ipython-input-221-16a05acc8254> in <module>()
----> 1 d['jobs']

KeyError: 'jobs'

In [222]: d.get('jobs') #而通过 get 方法, 如果 key 不存在, 则不会出错, 默认返回空.

In [223]: d.get('jobs', 'Sorry, Key Not Found') #也可以指定默认返回值.
Out[223]: 'Sorry, Key Not Found'
```

##### items

```python
In [234]: d
Out[234]: {'AnyIsalIn': '110', 'Bill Gates': '120', 'Bob': '119'}

In [235]: list(d.items()) #返回序列数据,
Out[235]: [('Bill Gates', '120'), ('Bob', '119'), ('AnyIsalIn', '110')]

In [236]: tuple(d.items())
Out[236]: (('Bill Gates', '120'), ('Bob', '119'), ('AnyIsalIn', '110'))
```

##### keys

```python
In [238]: d.keys()  #返回字典中的所有 key
Out[238]: dict_keys(['Bill Gates', 'Bob', 'AnyIsalIn'])

In [239]: list(d.keys())
Out[239]: ['Bill Gates', 'Bob', 'AnyIsalIn']
```

##### values

```python
In [266]: d
Out[266]:
{'AnyIsalIn': '110',
 'Bill Gates': '120',
 'Bob': '119',
 'cbg': '197',
 'sjmjj': '789'}

In [268]: list(d.values()) #返回字典中的所有 value
Out[268]: ['120', '197', '119', '789', '110']
```

##### pop

```python
In [246]: d
Out[246]: {'AnyIsalIn': '110', 'Bill Gates': '120', 'Bob': '119'}

In [248]: d
Out[248]: {'AnyIsalIn': '110', 'Bill Gates': '120', 'Bob': '119'}

In [249]: d.pop('Bob')  #删除指定 key, 并返回 key 对应的 value
Out[249]: '119'

In [250]: d
Out[250]: {'AnyIsalIn': '110', 'Bill Gates': '120'}
```

##### popitem

```python
In [252]: d
Out[252]: {'AnyIsalIn': '110', 'Bill Gates': '120', 'Bob': '119'}

In [253]: d.popitem() #删除一组 key,value, 并返回，由于字典是无序的, 所以会随机删除.
Out[253]: ('Bill Gates', '120')

In [254]: d
Out[254]: {'AnyIsalIn': '110', 'Bob': '119'}

In [255]: d.popitem()
Out[255]: ('Bob', '119')

In [256]: d
Out[256]: {'AnyIsalIn': '110'}
```

##### update

```python
In [260]: d
Out[260]: {'AnyIsalIn': '110', 'Bill Gates': '120', 'Bob': '119'}

In [261]: d2
Out[261]: {'cbg': '197', 'sjmjj': '789'}

In [262]: d.update(d2)  #update方法通过另一个字典来更新当前字典.

In [263]: d
Out[263]:
{'AnyIsalIn': '110',
 'Bill Gates': '120',
 'Bob': '119',
 'cbg': '197',
 'sjmjj': '789'}
```
