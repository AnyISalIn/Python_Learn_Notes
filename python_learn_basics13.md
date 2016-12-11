# Python 学习: 异常处理

程序在运行的时候, 可能会遇到很多异常, 我们需要对这些异常进行处理, 本文介绍 `python` 中对异常的处理.

#### `Python` 中的异常

`python` 中使用异常对象 (`exception object`) 来表示异常状态, 遇到错误后会引发异常, 如果异常对象未被捕捉或处理, 程序就会 `Traceback` 并终止运行.

```python
In [93]: 1 / 0
---------------------------------------------------------------------------
ZeroDivisionError                         Traceback (most recent call last)
<ipython-input-93-b710d87c980c> in <module>()
----> 1 1 / 0

ZeroDivisionError: division by zero
```

#### 触发异常

可以通过 `raise` 函数引发一个异常.

```python
In [114]: raise Exception(KeyError) #引发 KeyError 异常
---------------------------------------------------------------------------
Exception                                 Traceback (most recent call last)
<ipython-input-114-8c6e432915e6> in <module>()
----> 1 raise Exception(KeyError)

Exception: <class 'KeyError'>

In [115]: raise Exception(ZeroDivisionError)  #引发 ZeroDivisionError 异常
---------------------------------------------------------------------------
Exception                                 Traceback (most recent call last)
<ipython-input-115-5b05cf9d5726> in <module>()
----> 1 raise Exception(ZeroDivisionError)

Exception: <class 'ZeroDivisionError'>

In [116]: raise Exception('Sorry, Value Error') #自定义错误信息
---------------------------------------------------------------------------
Exception                                 Traceback (most recent call last)
<ipython-input-116-867bf308ce01> in <module>()
----> 1 raise Exception('Sorry, Value Error')

Exception: Sorry, Value Error
```

##### 异常类列表

| 类名              | 异常                                                                   |
|-------------------|------------------------------------------------------------------------|
| Exception         | 所有异常的基类                                                         |
| AttributeError    | 特性引用或赋值失败引发                                                 |
| IOError           | 试图打开不存在的文件引发                                               |
| IndexError        | 使用序列中不存在的元素时引发                                           |
| KeyError          | 使用映射中不存在的键时引发                                             |
| NameError         | 在找不到变量时引发                                                     |
| SyntaxError       | 在代码为错误形式时引发                                                 |
| TypeError         | 在内建操作或者函数应用于错误类型的对象时引发                           |
| ValueError        | 在内建操作或者函数应用于正确类型的对象, 但是该对象使用不合适的值时引发 |
| ZeroDivisionError | 在除法或取模操作第二个参数使用 0 时引发                                |

#### 异常捕捉

我们可以对异常进行捕获并处理.

```python
In [152]: x = 1

In [153]: y = 0

In [154]: try:
     ...:     x/y
     ...: except ZeroDivisionError: #捕获 ZeroDivisionError
     ...:     print('The Second Number Can\'t be Zero!')  #执行这段代码.
     ...:
The Second Number Can't be Zero!
```

如果不是 `ZeroDivisionError` 错误呢?

```python
In [155]: x = 'fdfhuefheu'

In [156]: y = 'de'

In [157]: try:
     ...:     x/y
     ...: except ZeroDivisionError:
     ...:     print('The Second Number Can\'t be Zero!')
     ...:
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last) #出现 TypeError
<ipython-input-157-fb7dce3841ad> in <module>()
      1 try:
----> 2     x/y
      3 except ZeroDivisionError:
      4     print('The Second Number Can\'t be Zero!')
      5

TypeError: unsupported operand type(s) for /: 'str' and 'str'
```

我们并没有对 `TypeError` 也做捕捉, 可以使用多个 `except` 语句对多种类型的异常进行捕捉.

```python
In [160]: try:
     ...:     x/y
     ...: except ZeroDivisionError:
     ...:     print('The Second Number Can\'t be Zero!')
     ...: except TypeError:
     ...:     print('Value Type Error')
     ...:
Value Type Error
```

也可以用一个代码块对多个异常进行捕获, 不过要将异常用元组列出.

```python
In [161]: try:
     ...:     x/y
     ...: except (ZeroDivisionError, TypeError):
     ...:     print('Value has some Error')
     ...:
Value has some Error
```

如果想打印出异常并且让程序继续运行, 可以用下面这种写法

```python
In [162]: try:
     ...:     x/y
     ...: except (ZeroDivisionError, TypeError) as e: # python2 中使用 ,e, EXAMPLE: except (ZeroDivisionError, TypeError), e: print e
     ...:     print(e)
     ...:
unsupported operand type(s) for /: 'str' and 'str'
```

如果想捕捉所有的异常, 在 `except` 后面不加参数即可捕捉所有异常, 但是通常不建议使用这种写法, 因为程序执行中可能有一些不可预料的错误, 建议使用 'as e' 来打印出所有异常.

```python
In [164]: try:
     ...:     x/y
     ...: except:
     ...:     print('SOME THING ERROR')
     ...:
SOME THING ERROR
```

`else` 子句, 类似 `while/for` 循环的 `else` 子句, 如果没有捕获到异常, 则运行此代码块.

```python
In [166]: x = 2

In [167]: y = 1

In [168]: try:
     ...:     x/y
     ...: except:
     ...:     print('SOME THING ERROR')
     ...: else:
     ...:     print('This OK!!!!')
     ...:
This OK!!!!
```

`finally` 子句可以在最后对可能的异常进行清理, 可以结合 `try`、`except`、`else` 使用

```python
In [169]: try:
     ...:     x/y
     ...: except:
     ...:     print('SOME THING ERROR')
     ...: else:
     ...:     print('This OK!!!!')
     ...: finally:
     ...:     print('Clean ALL')
     ...:     del x, y
     ...:
This OK!!!!
Clean ALL
```

#### 异常和函数

如果异常在函数内引发而没有被处理, 它就会被传播到函数调用的地方, 如果那里也没有处理异常, 异常就回被传播到主程序, 程序会带着堆栈跟踪中止..

**Example**

```python
In [180]: def error():
     ...:     raise ValueError
     ...:

In [181]: def handle_exce(exception):
     ...:     try:
     ...:         exception()
     ...:     except:
     ...:         print('Has some Error')
     ...:

In [182]: error()
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-184-eee6c6f25dda> in <module>()
----> 1 error()

<ipython-input-180-b2ecde0ffcf6> in error()
      1 def error():
----> 2     raise ValueError

ValueError:

In [183]: handle_exce(error)  #异常被 handle_exec 函数处理
Has some Error
```
