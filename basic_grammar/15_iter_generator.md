# Python3 迭代器与生成器

## 迭代器

+ 迭代是Python最强大的功能之一，是访问集合元素的一种方式。
+ 迭代器是一个可以记住遍历的位置的对象。
+ 迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退。
+ 迭代器有两个基本的方法：iter() 和 next()。

字符串，列表或元组对象都可用于创建迭代器：

```bash
>>> list=[1,2,3,4]
>>> it = iter(list)    # 创建迭代器对象
>>> print (next(it))   # 输出迭代器的下一个元素
1
>>> print (next(it))
2
>>>
```

迭代器对象可以使用常规for语句进行遍历：

```bash
#!/usr/bin/python3

list=[1,2,3,4]
it = iter(list)    # 创建迭代器对象
for x in it:
    print (x, end=" ")
```

执行以上程序，输出结果如下：

```bash
1 2 3 4
```

也可以使用 next() 函数：

```bash
#!/usr/bin/python3

import sys         # 引入 sys 模块

list=[1,2,3,4]
it = iter(list)    # 创建迭代器对象

while True:
    try:
        print (next(it))
    except StopIteration:
        sys.exit()
```

执行以上程序，输出结果如下：

```bash
1
2
3
4
```

## 创建一个迭代器

+ 把一个类作为一个迭代器使用需要在类中实现两个方法 __iter__() 与 __next__() 。
+ 如果你已经了解的面向对象编程，就知道类都有一个构造函数，Python 的构造函数为 __init__(), 它会在对象初始化的时候执行。
+ __iter__() 方法返回一个特殊的迭代器对象， 这个迭代器对象实现了 __next__() 方法并通过 StopIteration 异常标识迭代的完成。
+ __next__() 方法（Python 2 里是 next()）会返回下一个迭代器对象。

创建一个返回数字的迭代器，初始值为 1，逐步递增 1：

```bash
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self

  def __next__(self):
    x = self.a
    self.a += 1
    return x

myclass = MyNumbers()
myiter = iter(myclass)

print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
```

执行输出结果为：

```bash
1
2
3
4
5
```

### StopIteration

StopIteration 异常用于标识迭代的完成，防止出现无限循环的情况，在 __next__() 方法中我们可以设置在完成指定循环次数后触发 StopIteration 异常来结束迭代。

在 20 次迭代后停止执行：

```bash
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self

  def __next__(self):
    if self.a <= 20:
      x = self.a
      self.a += 1
      return x
    else:
      raise StopIteration

myclass = MyNumbers()
myiter = iter(myclass)

for x in myiter:
  print(x)
```

执行输出结果为：

```bash
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
```

## 生成器

> 生成器表达式语法

+ (返回值 for 元素 in 可迭代对象 if 条件)
+ 列表解析式的中括号换成小括号就行了
+ 返回一个生成器

> 和列表解析式的区别

+ 生成器表达式是按需计算( 或称惰性求值、延迟计算 )，需要的时候才计算值
+ 列表解析式是立即返回值

> 生成器

+ 可迭代对象
+ 迭代器

```bash
g = ("{:04}".format(i) for i in range(1,11))
next(g)

for x in g:
    print(x)
print("----------------------")

for x in g:
    print(x)
===============================================
0002
0003
0004
0005
0006
0007
0008
0009
0010
----------------------

```

+ 延迟计算
+ 返回迭代器，可以迭代
+ 从前到后走完一遍后，不能回头

```bash
g = ["{:04}".format(i) for i in range(1,11)]
for x in g:
    print(x)
print("-----------------------")

for x in g:
    print(x)
================================================
0001
0002
0003
0004
0005
0006
0007
0008
0009
0010
-----------------------
0001
0002
0003
0004
0005
0006
0007
0008
0009
0010
```

+ 立即计算
+ 返回的不是迭代器，返回可迭代对象列表
+ 从头到尾走完一遍后，可以再回头迭代

> yield 函数

+ 在 Python 中，使用了 yield 的函数被称为生成器（generator）。
+ 跟普通函数不同的是，生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器。
+ 在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 next() 方法时从当前位置继续运行。
+ 调用一个生成器函数，返回的是一个迭代器对象。

以下实例使用 yield 实现斐波那契数列：

```bash
#!/usr/bin/python3

import sys

def fibonacci(n): # 生成器函数 - 斐波那契
    a, b, counter = 0, 1, 0
    while True:
        if (counter > n):
            return
        yield a
        a, b = b, a + b
        counter += 1
f = fibonacci(10) # f 是一个迭代器，由生成器返回生成

while True:
    try:
        print (next(f), end=" ")
    except StopIteration:
        sys.exit()
```

执行以上程序，输出结果如下：

```bash
0 1 1 2 3 5 8 13 21 34 55
```

### 生成器和列表解析式的对比

计算方式

+ 生成器表达式延迟计算，列表解析式立即计算

内存占用

+ 单从返回值本身来说，生成器表达式省内存，列表解析式返回新的列表
+ 生成器没有数据，内存占用极少，但是使用的时候，虽然一个个返回数据，但是合起来占用的内存也差不多
+ 列表解析式构造新的列表需要占用内存

计算速度

+ 单看计算时间看，生成器表达式耗时非常短，列表解析式耗时长
+ 但是生成器本身并没有返回任何值，只返回了一个生成器对象
+  列表解析式构造并返回了一个新的列表

### 习题分析

```bash
it = (print("{}".format(i+1)) for i in range(2))
first = next(it)
second = next(it)
val = first + second
```

+ val的值是什么？
  + print() 函数的返回值是 None，所以 first 和 second 都是 None，val 抛出异常
+ val = first + second 语句之后能否再次 next(it)？
  + 生成器已经迭代完了，所以 next(it) 会抛出 StopIteration 异常

```bash
it = (x for x in range(10) if x % 2)
first = next(it)
second = next(it)
val = first + second
```

+ val 的值是什么？

  + val 的值是 1 + 3 = 4
+ val = first + second 语句之后能否再次 next(it)？

  + 当然可以，因为这个生成器跟上面那个不同，first 和 second 没用 print() 函数，所以有返回值

```bash
def inc():
    for i in range(5):
        yield i
print(1,"-->",type(inc))
print(2,"-->",type(inc()))
x = inc()
print(3,"-->",type(x))
print(4,"-->",next(x))
for m in x:
    print(5,"-->",m,"*")
for m in x:
    print(6,"-->",m,"**")
------------------------------
1 --> <class 'function'>
2 --> <class 'generator'>
3 --> <class 'generator'>
4 --> 0
5 --> 1 *
5 --> 2 *
5 --> 3 *
5 --> 4 *

===============================

y = (i for i in range(5))
print(type(y))
print(next(y))
print(next(y))
-------------------------------
<class 'generator'>
0
1
```

+ 普通函数调用后立即执行完毕，但是生成器函数可以使用 next 函数多次执行
+ 生成器函数等价于生成器表达式，只不过生成器函数可以更加复杂

```bash
def gen():
    print("line 1")
    yield 1
    print("line 2")
    yield 2
    print("line 3")
    return 3
print(1,"-->",next(gen()))
print(2,"-->",next(gen()))
print(3,"-->",next(gen()))
print()
g = gen()
print(4,"-->",next(g))
print(5,"-->",next(g))
print(6,"-->",next(g))
print(7,"-->",next(g,"End"))
-----------------------------------------
line 1
1 --> 1
line 1
2 --> 1
line 1
3 --> 1

line 1
4 --> 1
line 2
5 --> 2
line 3
---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-5-7a4c4921825b> in <module>
     13 print(4,"-->",next(g))
     14 print(5,"-->",next(g))
---> 15 print(6,"-->",next(g))
     16 print(7,"-->",next(g,"End"))

StopIteration: 3
```

+ 其中 1-3 都是使用 next(gen())，实际上 3 次调用函数生成了 3 个生成器，而并不是 3 次拨动同一个生成器
+ 其中 4-7 是 g=gen() 后，g 就是一个生成器，多次 next(g) 实际上是拨动同一个生成器，直到耗尽
+ 6 抛出异常，是因为被 return 语句打断，无法继续获取下一个值，抛出 StopIteration 异常
+ 如果注释 6 让 7 执行，则 7 不会抛出异常，因为 7 的 next 有默认值 "End"，生成器耗尽后就会输出默认值
+ 在生成器函数中，使用多个 yield 语句，执行一次后会暂停执行，把 yield 表达式的值返回
+ 再次执行会执行到下一个 yield 语句
+ return 语句依然可以终止函数运行，但 return 语句的返回值不能被获取到
+ return 会导致无法继续获取下一个值，抛出 StopIteration 异常
+ 如果函数没有显示的 return 语句，如果生成器函数执行到结尾，一样会抛出 StopIteration 异常

生成器函数应用也是极为广泛

+ 包含 yield 语句的生成器函数生成**生成器对象**的时候，**生成器函数的函数体不会立即执行**
+ next(generator) 会从函数的当前位置向后执行到之后碰到的第一个 yield 语句，会弹出值，并暂停函数执行
+ 再次调用 next 函数，和上一条一样的处理过程
+ 没有多余的 yield 语句能被执行，继续调用 next 函数，会抛出 StopIteration 异常

生成器的一些简单应用

下面实现一个计数器

```bash
def counter():
    i = 0
    while True:
        i += 1
        yield i
def inc(c):
    return next(c)

c = counter()

print(inc(c),end=" ")
print(inc(c),end=" ")
print(inc(c),end=" ")
print(inc(c),end=" ")
--------------------------------
1 2 3 4 

================================
def counter():
    i = 0
    while True:
        i += 1
        yield i
def inc():
    c = counter()
    return next(c)
print(inc(),end=" ")
print(inc(),end=" ")
print(inc(),end=" ")
print(inc(),end=" ")
--------------------------------
1 1 1 1
```

+ 两个例子对比，前一个例子相当于多次拨动同一个生成器，后一个例子相当于每一次都是重新生成一个生成器

```bash
def inc():
    def couter():
        i = 0
        while True:
            i += 1
            yield i
    c = couter()
    return lambda:next(c)
foo = inc()
print(foo(),end=" ")
print(foo(),end=" ")
print(foo(),end=" ")
print(foo(),end=" ")
--------------------------------
1 2 3 4 

================================
def inc():
    def counter():
        i = 0
        while True:
            i += 1
            yield i
    c = counter()
    
    def _inc():
        return next(c)
    return _inc
foo = inc()
print(foo(),end=" ")
print(foo(),end=" ")
print(foo(),end=" ")
print(foo(),end=" ")
--------------------------------
1 2 3 4
```

+ 两个例子是等效的，借用 lambda 表达式更高级

下面实现处理递归问题

```bash
def fib():
    x = 0
    y = 1
    while True:
        yield y
        x,y=y,x+y
foo = fib()
for _ in range(5):
    print(next(foo),end=" ")
---------------------------------
1 1 2 3 5 

=================================
pre = 0
cur = 1
print(pre,cur,end=" ")
def fib(n,pre=0,cur=1):
    pre,cur = cur,pre+cur
    print(cur,end=" ")
    if n == 2:
        return
    fib(n-1,pre,cur)
fib(5)
----------------------------------
0 1 1 2 3 5
```

+ 两个例子等价

生成器与协程 coroutine 

+ 生成器的高级用法
+ 比进程、线程轻量级
+ 是在用户空间调度函数的一种实现
+ Python3 asyncio 就是协程实现，已经加入到标准库
+ Python3.5 使用 async、await 关键字直接原生支持协程
+ 协程调度器实现思路
  + 有 2 个生成器 A、B
  + next(A) 后，A 执行到 yield 语句暂停，然后去执行 next(B)，B执行到 yield 语句也暂停，然后再次调用 next(A)，再调用 next(B)，周而复始，就实现了调度的效果
  + 可以引入调度的策略来实现切换的方式
+ 协程是一种非抢占式调度

yield from 可以简化代码

```bash
def inc():
    for x in range(1000):
        yield x
foo = inc()
print(next(foo))
print(next(foo))
===============================
def inc():
    yield from range(1000)
foo = inc()
print(next(foo))
print(next(foo))
```

+ 上面表达方式等效
+ yield from 是 Python3.3 出现的新的语法
+ yield from iterable 是 for item in iterable：yield item 形式的语法糖







