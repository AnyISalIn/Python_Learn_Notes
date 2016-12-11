# Python 学习: 内置容器(字符串)

## 字符串的操作

### 初始化字符串
```python
In [5]: s = ''

In [6]: s = str()
```

### 字符串是不可变对象
```python
In [7]: s = "AnyISalIn"

In [8]: s[0]
Out[8]: 'A'

In [9]: s[0] = 'a'
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-9-55620f378bce> in <module>()
----> 1 s[0] = 'a'

TypeError: 'str' object does not support item assignment

```

### 字符串是可迭代对象
```python

In [10]: s
Out[10]: 'AnyISalIn'

In [12]: for i in s:
    ...:     print(i)
    ...:
A
n
y
I
S
a
l
I
n
```

### 字符串切分

#### `split` 方法

```python
In [13]: s = "My name is AnyISalIn"

In [14]: s.split()
Out[14]: ['My', 'name', 'is', 'AnyISalIn']
```

指定分隔符

```python
In [16]: s = "address:shanghai"

In [18]: s.split(':')
Out[18]: ['address', 'shanghai']
```

指定分割次数

```python
In [35]: s
Out[35]: 'My name is AnyISalIn'

In [36]: s.split(' ', 2)
Out[36]: ['My', 'name', 'is AnyISalIn']
```

#### `rsplit` 方法

与 `split` 方法相反, 从右往左切分.

```python
In [38]: s
Out[38]: 'My name is AnyISalIn'

In [40]: s.split(' ', 2)
Out[40]: ['My', 'name', 'is AnyISalIn']

In [41]: s.rsplit(' ', 2)
Out[41]: ['My name', 'is', 'AnyISalIn']

```

#### `splitlines` 方法

```python
In [42]: lines = '''
    ...: Hi!
    ...: This is second lines.
    ...: This is thrid lines.
    ...: '''

In [44]: lines.splitlines()
Out[44]: ['', 'Hi! ', 'This is second lines.', 'This is thrid lines.']

In [45]: lines.splitlines(True)  #留下换行符
Out[45]: ['\n', 'Hi! \n', 'This is second lines.\n', 'This is thrid lines.\n']
```

#### `partition` 方法

类似于 `split`, 但是只能切分成三片.

```python
In [53]: s.partition(' ')
Out[53]: ('My', ' ', 'name is AnyISalIn')
```

## 字符串修改

### 自然语言修改

```python
In [67]: s
Out[67]: 'My name is AnyISalIn'

In [68]: s.capitalize() #句首大写
Out[68]: 'My name is anyisalin'

In [69]: s.title() #每个单词首字母大写
Out[69]: 'My Name Is Anyisalin'

In [70]: s.lower() #全部小写
Out[70]: 'my name is anyisalin'

In [71]: s.upper() #全部大写
Out[71]: 'MY NAME IS ANYISALIN'

In [72]: s.swapcase()
Out[72]: 'mY NAME IS aNYisALiN'
```
### 程序世界的修改

```python
In [76]: s.center(40)
Out[76]: '          My name is AnyISalIn          '

In [77]: s.center(40, '$')
Out[77]: '$$$$$$$$$$My name is AnyISalIn$$$$$$$$$$'

In [78]: s.ljust(40)
Out[78]: 'My name is AnyISalIn                    '

In [79]: s.rjust(40)
Out[79]: '                    My name is AnyISalIn'


In [86]: s
Out[86]: '       aaaaa'

In [87]: s.strip()
Out[87]: 'aaaaa'

In [95]: s
Out[95]: 'aaaaa     '

In [96]: s.rstrip()
Out[96]: 'aaaaa'
```

### 连接

```python
In [118]: a
Out[118]: ','

In [119]: s
Out[119]: 'abcd'

In [120]: a.join(s)
Out[120]: 'a,b,c,d'
```

## 字符串判断

### `startwith` 方法

```python
In [121]: s
Out[121]: 'abcd'

In [122]: s.startswith('a')
Out[122]: True

In [123]: s.startswith('n')
Out[123]: False
```

### `endswith` 方法

```python
In [128]: s
Out[128]: 'abcd'

In [129]: s.endswith('b')
Out[129]: False

In [130]: s.endswith('d')
Out[130]: True
```

### 其他方法

```python
In [131]: s
Out[131]: 'abcd'

In [132]: s.isupper()
Out[132]: False

In [133]: s.islower()
Out[133]: True

In [134]: s.isdigit()
Out[134]: False

In [135]: '123'.isdigit()
Out[135]: True

In [136]: '123'.isalnum()
Out[136]: True
```


## 查找替换

### `count` 方法

```python
In [1]: str = 'aaaannnnn'

In [2]: str.count('a')
Out[2]: 4
```

### `find`,`rfind` 方法
检测 str 是否包含在 string 中, 可指定 `start`, `end` 范围.

```python
In [3]: str.find('a')
Out[3]: 0

In [4]: str.rfind('a')
Out[4]: 3

In [16]: str.find('n')
Out[16]: 4

In [17]: str.find('n', 5)
Out[17]: 5

In [18]: str.find('n', 6)
Out[18]: 6
```
### `index`,`rindex` 方法
跟`find`方法一样，只不过如果`str`不在 `string`中会报一个异常.
```python
In [19]: str
Out[19]: 'aaaannnnn'

In [20]: str.index('a')
Out[20]: 0

In [21]: str.index('b')
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-21-79c57da96497> in <module>()
----> 1 str.index('b')

ValueError: substring not found

In [22]: str.index('a', 2)
Out[22]: 2
```

### `replace` 方法
替换指定字符, 可指定替换次数.
```python
In [23]: str = 'abcxyzabc'

In [24]: str.replace('abc', 'replace')
Out[24]: 'replacexyzreplace'

In [25]: str = 'abcxyzabc'

In [26]: str.replace('abc', 'replace', 1)
Out[26]: 'replacexyzabc'
```
