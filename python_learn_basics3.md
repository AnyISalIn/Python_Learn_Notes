# Python 学习: 基础语法(程序控制语句)

## 程序控制语句

### 顺序

顾名思义，所谓顺序结构，就是指按照语句在程序中的先后次序一条一条的顺次执行。顺序控制语句是一类简单的语句，操作运算语句即是顺序控制语句，包括表达式语句，输入/输出等。

### 分支

可以通过条件判断来确定是否执行某些代码块, `if` `elif` `else` 就是一个条件判断语句

#### 单分支

```python
In [47]: a = 1

In [49]: if a > 0:
    ...:     print('Sure, a > 0')
    ...:
Sure, a > 0
```

#### 多分支

```python
In [50]: a = 0

In [53]: if a > 0:
    ...:     print('Sure, a > 0')
    ...: else:
    ...:     print('Sorry, a <= 0')
    ...:
Sorry, a <= 0
```

#### 双分支

```python
In [50]: a = 0

In [55]: if a > 0:
    ...:     print('Sure, a > 0')
    ...: elif a < 0:
    ...:     print('Sorry, a < 0')
    ...: else:
    ...:     print('Oh, a = 0')
    ...:
Oh, a = 0
```

### 循环
循环语句允许我们执行一个语句或语句组多次, `python` 提供了 `for` `while` 循环
#### `for` 语句

```python
In [57]: for i in range(5):
    ...:     print(i)
    ...:
0
1
2
3
4
```



#### `while` 语句

```python
In [58]: c = 0

In [61]: while c < 10:
    ...:     print(c)
    ...:     c += 1
    ...:
0
1
2
3
4
5
6
7
8
9
```

#### break/continue

```python
In [63]: c = 0

In [64]: while c < 10:
    ...:     if c == 5:
    ...:       break
    ...:
    ...:     print(c)
    ...:     c += 1
    ...:
0
1
2
3
4
```

```python
In [90]: while c < 10:
    ...:     c+=1
    ...:     if c == 5:
    ...:         continue
    ...:     print(c)
    ...:
1
2
3
4
6
7
8
9
10
```

#### `else` 子句

如果没有提前跳出循环, 则执行 `else` 字句中的代码块.

```python

Example:

#求10以内的素数
In [6]: for i in range(2, 11):
   ...:    for x in range(2, i-1):
   ...:      if i % x == 0:
   ...:        break
   ...:    else: #如果没有提前跳出, print
   ...:        print('%s yes' % i)
   ...:
   ...:
2 yes
3 yes
5 yes
7 yes

```
