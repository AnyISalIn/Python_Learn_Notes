# Python 学习: 基础语法(`HelloWorld`, 常量/变量, 强类型/动态语言)


## 第一个程序: `Hello World`

```python
#先运行 python 解释器, 我这里使用 ipython

#在 python3 中, print 是一个内建的函数, 所以需要加()
In [1]: print('hello world') #python3
hello world

#在 python2 中, 直接附加上要打印的字符即可.

In [8]: print 'hello  world' #python2
hello  world
```


## 常量和变量

### 常量
在 `python` 的语法中并没有定义常量, 我们一般把 `python` 的数值、字符串等都称为`字面常量`, 之所以称作`字面常量`有两层意思, 一是常量: 例如数值 `1` 就代表 `1`, 它的值不能被修改. 二是字面: 例如数值 `1`, 他总是代表我们所看到的数值 `1`, 而不是其他对象.

### 变量
而变量, 我们认为它是一个标识符, 它可以让我们存储任何信息, 又可以对他们做处理. 变量通常由两部分组成, 标识符和值.

每个值, 都是内存上的一块区域, 标识符相当于这块区域的名称, 通过标识符, 我们可以使用这块区域, 这就是变量.

变量, 顾名思义, 是可变的, 指的是值是可变的, 被标识符命名的内存区域, 可以存储其他的信息.

```python
In [9]: a = "Hello World" #变量赋值

In [10]: print(a)
Hello World

In [11]: a = "AnyISalIn Blog" #修改变量的值

In [12]: print(a)
AnyISalIn Blog

```

### 变量的命名规范

然而，为了解释器能够正确识别，变量名需要遵守一定的命名规范，总结下来就是：

* 变量名首字符必须是字母或者下划线
* 变量名其他字符必须是字母、数字、或者下划线

以上两点是Python强制的命名规范，如果不遵守这个规范，解释器讲抛出错误，你的程序无法运行。

每个人的编程风格可能都略有差异, `Python` 有一个官方的编码风格指导规范 [PEP-8](http://legacy.python.org/dev/peps/pep-0008/)
, 不遵守这个编码风格虽然不会报错, 但如果我们遵循统一的编码风格规范, 这样会利于团队合作以及自动化工作的使用.

## 强类型的动态语言

`python` 是强类型的动态语言, 所谓强类型简单来说指的是不同的数据类型的值不能相加.

### 强类型
在 `python` 中
```python

In [1]: 1 + "a"
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-1-db0423a23ce2> in <module>()
----> 1 1 + "a"

TypeError: unsupported operand type(s) for +: 'int' and 'str'

```

在 `JavaScript` 中

```JavaScript
3 + "a"
"3a"
```

可以看出, `python` 是强类型、`JavaScript` 是弱类型.

### 动态语言

`python` 是一门动态语言, 简单的来说就是声明了一个变量, 能够随时改变它的类型.

```python
In [5]: var = "test"
In [6]: type(var)
Out[6]: str

In [7]: var = 3
In [8]: type(var)
Out[8]: int

```
