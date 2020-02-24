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

### 习题

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

  