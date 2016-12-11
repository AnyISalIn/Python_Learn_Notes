# Python 学习: 模块和包

`Python` 标准安装包含了一系列强大的模块, 称之为 `Python` 标准库 (`Python Standard Library`).

## 导入模块

任何 `Python` 程序都可以作为模块被导入.

编写一个简单的程序, 名为 `hello.py`

```shell
$ cat hello.py  #
# hello.py
print("Hello World!")

$ pwd #当前路径为 ~/pymodule
/Users/anyisalin/pymodule
```
作为模块导入

```python
In [2]: import sys

In [3]: sys.path  #通过 sys.path 方法可以查看默认的导入路径.
Out[3]:
['',
 '/Users/anyisalin/.pyenv/versions/3.5.2/bin',
 '/Users/anyisalin/.pyenv/versions/3.5.2/lib/python35.zip',
 '/Users/anyisalin/.pyenv/versions/3.5.2/lib/python3.5',
 '/Users/anyisalin/.pyenv/versions/3.5.2/lib/python3.5/plat-darwin',
 '/Users/anyisalin/.pyenv/versions/3.5.2/lib/python3.5/lib-dynload',
 '/Users/anyisalin/.pyenv/versions/3.5.2/lib/python3.5/site-packages',
 '/Users/anyisalin/.pyenv/versions/3.5.2/lib/python3.5/site-packages/IPython/extensions',
 '/Users/anyisalin/.ipython']

#如果我们在模块的目录启动 ipython, 可以直接通过 import 导入.
In [1]: import hello
Hello World!

# 在其他目录就没法导入, 我们需要向存储了查找 path 的列表中 append 我们要查找的路径
In [1]: import hello
---------------------------------------------------------------------------
ImportError                               Traceback (most recent call last)
<ipython-input-1-f2ff2800b3ef> in <module>()
----> 1 import hello

ImportError: No module named 'hello'

In [3]: sys.path.append('/Users/anyisalin/pymodule/') #append hello.py 文件所在的位置

In [4]: import hello
Hello World!
```

**第二次导入**

第二次导入 hello 模块就不会执行 hello.py 的代码了, 这是为了避免两个模块相互导入可能会发生的无限递归.

```python
In [5]: import hello  
# 没有返回值

# 可以通过 reload 函数强制重新导入
In [8]: from importlib import reload  #python 2 无需导入 importlib 模块.

In [9]: reload(hello)
Hello World!
Out[9]: <module 'hello' from '/Users/anyisalin/pymodule/hello.py'>
```

## 模块用于定义

一般情况下, 模块在导入时是不会执行的, 一般情况下, 模块中只包含类和方法.

## 模块中定义方法

```python
$ cat hello2.py
# hello2.py

def hello(name="Bob"):
    print('HelloWorld, {}'.format(name))

In [3]: import hello2

In [4]: hello2.hello()
HelloWorld, Bob

In [5]: hello2.hello('AnyISalIn')
HelloWorld, AnyISalIn
```

**带有测试代码的模块**

```python
# hello2.py

def hello(name="Bob"):
    print('HelloWorld, {}'.format(name))

def module():
    print('Thanks Import This Module')

if __name__ == '__main__':  #以文件的方式执行, 默认调用 hello() 模块
    hello()

if __name__ == 'hello2': #以模块的方式执行, 默认调用 module() 模块
    module()
```

```shell
$ python hello2.py
HelloWorld, Bob
```

```python
In [3]: import hello2
Thanks Import This Module

In [6]: hello2.__name__ #可以看到当 hello2.py 作为模块导入后, __name__ 变量为模块名.
Out[6]: 'hello2'
```

一般情况下, 我们将扩展的模块放到 `site-packages` 目录下


## 包

为了组织好模块, 我们可以将模块分组为包, 包就是另一类模块, 包也可以可以包含其他模块, 当模块存储在文件中时(`.py`), 包就是模块所在的目录, 为了让 `python` 将其作为包来对待, 目录下必须包含一个 `__init__.py` 的模块, 可以将它作为一个普通模块导入, `__init__` 模块的内容就是包的内容.


下面是一个简单的包布局.

| 文件/目录                  | 描述                 |
|----------------------------|----------------------|
| ~/python/                  | PYTRHONPATH 的包目录 |
| ~/python/darwing/          | 包目录(darwing 包)   |
| ~/python/\__init\__.py       | 包代码(darwing 模块) |
| ~/python/colors.py         | colors 模块          |
| ~/python/darwing/shapes.py | shapes 模块          |

## 探索模块

我们想看一个模块中有什么, 可以通过 `dir` 函数来看, `dir` 函数可以查看对象、模块的所有方法、特性等.

```python
In [872]: import copy

In [873]: [ x for x in dir(copy) if not x.startswith('_') ] #通过列表推导式过滤特殊方法.
Out[873]:
['Error',
 'PyStringMap',
 'builtins',
 'copy',
 'deepcopy',
 'dispatch_table',
 'error',
 'name',
 't',
 'weakref']
```

**`__all__`** 变量

模块的 `__all__` 变量定义了模块的公有接口

如果通过下面这种方式导入, 则会把 `__all__` 中的方法/特性给导入.

```python
In [875]: from copy import *
```

如果模块中没有定义 `__all__` 变量, `import *` 会导入所有没有以 `__` 开头的方法/特性.


## 获取帮助

可以通过 `help` 函数来查看模块/方法的帮助.

```python
In [880]: help(copy.deepcopy)

Help on function deepcopy in module copy:

deepcopy(x, memo=None, _nil=[])
    Deep copy operation on arbitrary Python objects.

    See the module's __doc__ string for more info.
```

这是通过模块/方法中的 `docstring` 定义的.

## 查看模块源文件

可以通过 `__file__` 方法查看模块文件路径

```python
In [881]: copy.__file__
Out[881]: '/Users/anyisalin/.pyenv/versions/3.5.2/lib/python3.5/copy.py'
```
