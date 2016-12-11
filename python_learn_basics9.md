# Python 学习: 函数和递归

我们如果需要对代码进行重用, 就需要用到函数, 函数可以让代码更加抽象和简洁.

## 定义一个函数

```python
In [1]: def hello():  #定义一个名字为 hello 的函数
   ...:     print('Hello World')
   ...:

In [2]: hello() #调用函数
Hello World
```

## 传入参数

## 关键词参数

```python
In [3]: def hello_param(name):  #传入一个参数
   ...:     print('Hello, %s' % name)
   ...:

In [4]: hello_param('AnyISalIn')
Hello, AnyISalIn
```

```python
In [5]: def hello_multi_param(name, age): #传入多个参数
   ...:     print('Hello, %s, %s' % (name, age))
   ...:

In [6]: hello_multi_param('AnyISalIn', 18)
Hello, AnyISalIn, 18

In [8]: hello_multi_paramm(age=19, name='sjmjj')  #通过关键字形式传入
Hello, sjmjj, 19
```
## 默认值

**默认值**

```python
In [10]: def hello(name='AnyISalIn'): #定义默认值为 AnyISalIn
    ...:     print('Hello, %s' % name)
    ...:

In [11]: hello()
Hello, AnyISalIn
```
## 收集参数

**收集参数**

```python
In [20]: def hello(*params):  #可以通过在参数前加 *, 将传入一个元组.
    ...:     print(params)
    ...:

In [21]: hello('aaa', 'bbb', 'ccc')
('aaa', 'bbb', 'ccc')
```

```python
In [28]: def hello(**params): #也可以通过关键字形式传入, 将传入一个字典
    ...:     print(params)
    ...:

In [29]: hello(x=1, y=2)
{'x': 1, 'y': 2}
```

## return

**return 单个值**

其实我们之前定义的不能算是一个完整的函数, 一个完整的函数一般都有 `return` 语句来结束函数.

```python
In [57]: def hello(name):
    ...:     if name == 'AnyISalIn':
    ...:         return '你好, 帅哥'  #如果 name 等于 AnyISalIn 提前中断函数并返回值.
    ...:     else:
    ...:         return 'Hello, %s' % name
    ...:

In [58]: hello(name='AnyISalIn')
Out[58]: '你好, 帅哥'

In [59]: hello(name='sj')
Out[59]: 'Hello, sj'
```

**return 多个值**

```python
In [60]: def hello(name, age):
    ...:     if name == 'AnyISalIn' and age == 18:
    ...:         return name, age
    ...:     else:
    ...:         return name
    ...:

In [61]: hello('AnyISalIn', 17)
Out[61]: 'AnyISalIn'

In [62]: hello('AnyISalIn', 18) #返回一个元组.
Out[62]: ('AnyISalIn', 18)
```

## 递归

简单的来说, 递归就是函数调用自身

**乘阶的计算**

`for` 循环计算乘阶

```python
In [80]: def fac(x):
    ...:     result = x
    ...:     for x in range(1, x):
    ...:         result *= x
    ...:     return result
    ...:

In [81]: fac(10)
Out[81]: 3628800
```

`递归` 计算乘阶

可以看出递归的代码更加简洁明了, 但是 `python` 中递归性能太差, 不推荐使用.

```python
In [71]: def fac(x):
    ...:     if x == 1:
    ...:         return x
    ...:     else:
    ...:         return x * fac(x-1)
    ...:

In [72]: fac(5)
Out[72]: 120
```
