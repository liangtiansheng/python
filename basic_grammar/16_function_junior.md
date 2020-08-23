# 函数的高级用法

## Python 递归函数

函数的执行流程

```bash
def foo1(b,b1=3):
    print("foo1 called",b,b1)

def foo2(c):
    foo3(c)
    print("foo2 called",c)

def foo3(d):
    print("foo3 called",d)

def main():
    print("main called")
    foo1(100,101)
    foo2(200)
    print("main ending")

main()
```

+ 全局帧中生成 foo1、foo2、foo3、main 函数对象
+ main 函数调用
+ main 中查找内建函数压栈，将常量字符串压栈，调用函数，弹出栈顶
+ main 中全局查找函数 foo1 压栈，将常量 100、101 压栈，调用函数 foo1，创建栈帧；print函数压栈，字符串和变量b、b1压栈，调用函数，弹出栈顶，返回值
+ main 中全局查找 foo2 函数压栈，将常量200压栈，调用foo2，创建栈帧，foo3函数压栈，变量c引用压栈，调用 foo3，创建栈帧，foo3 完成 print 函数调用后返回；foo2 恢复调用，执行 print 后，返回值；main 中 foo2 调用结束弹出栈顶，main 继续执行 print 函数调用，弹出栈顶。main 函数返回

递归函数的基本概念

+ 函数直接或者间接调用自身就是**递归**
+ 递归需要有边界条件、递归前进段、递归返回段
+ 递归一定要有**边界条件**
+ 当边界条件不满足时，递归前进
+ 当边界条件满足的时候，递归返回

```bash
1、斐波那契数列Fibonacci number: 1,1,2,3,5,8,13,21,34,55,89,144......
2、如果设F(n)为该数列的第n项(n∈N*)，那么这句话可以写成如下形式：F(n) = F(n-1) + F(n-2)
3、F(0)=0，F(1)=1，F(n)=F(n-1)+F(n-2)
# for 循环实现
pre = 0
cur = 1
print(pre,cur,end=" ")
n=8
for i in range(n):
    pre, cur = cur, pre+cur
    print(cur,end=" ")

# 递归函数实现
def fib(n):
    return 1 if n < 2 else fib(n-1) + fib(n-2)

for i in range(5):
    print(fib(i),end=" ")

解析：
fib(3) + fib(2)
fib(3)调用fib(3)、fib(2)、fib(1)
fib(2)调用fib(2)、fib(1)
fib(1)是边界

```

递归函数的要求

+ 递归一定要有退出条件，递归调用一定要执行到这个退出条件，没有退出条件的递归调用，就是无限调用
+ 递归调用的深度不宜过深
  + Python 对递归调用的深度做了限制，以保护解释器
  + 超过递归深度限制，抛出 RecursionError：maxinum recursion depth exceeded 超出最大深度
  + sys.getrecursionlimit()

递归的性能

```bash
import datetime
start = datetime.datetime.now()
pre = 0
cur = 1
print(pre,cur,end=" ")
n = 35
for i in range(n-1):
    pre,cur = cur,pre+cur
    print(cur,end=" ")
delta = (datetime.datetime.now() - start).total_seconds()
print()
print("This program used {}s".format(delta))
-------------------------------------------------------------
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181 6765 10946 17711 28657 46368 75025 121393 196418 317811 514229 832040 1346269 2178309 3524578 5702887 9227465
This program used 0.002004s

=============================================================
import datetime
n = 35
start = datetime.datetime.now()
def fib(n):
    return 1 if n < 2 else fib(n-1) + fib(n-2)
for i in range(n):
    print(fib(i),end=" ")
delta = (datetime.datetime.now() - start).total_seconds()
print()
print("This program used {}s".format(delta))
-------------------------------------------------------------
1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181 6765 10946 17711 28657 46368 75025 121393 196418 317811 514229 832040 1346269 2178309 3524578 5702887 9227465
This program used 6.641008s
```

+ 循环稍微复杂一些，但是只要不是死循环，可以多次迭代直至算出结果
+ fib函数代码极简易懂，但是只能获取到最外层的函数调用，内部递归结果都是中间结果，而且给定一个 n 都要进行近 2n 次递归，深度越深，效率越低，为了获取斐波那契数列需要外面再套一个 n 次循环，效率就更低了
+ 递归还有深度限制，如果递归复杂，函数反复压栈，栈内存很快就溢出了

上面这个递归可以改进

```bash
import datetime
start = datetime.datetime.now()
pre = 0
cur = 1
print(pre,cur,end=" ")
def fib(n,pre=0,cur=1):
    pre,cur = cur,pre+cur
    print(cur,end=" ")
    if n == 2:
        return
    fib(n-1,pre,cur)
fib(35)
delta = (datetime.datetime.now()-start).total_seconds()
print()
print("This program used {}s".format(delta))
----------------------------------------------------------
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181 6765 10946 17711 28657 46368 75025 121393 196418 317811 514229 832040 1346269 2178309 3524578 5702887 9227465
This program used 0.001973s
```

+ 此 fib 函数和循环的思想类似
+ 参数 n 是边界条件，用 n 来计数
+ 上一次的计算结果直接作为函数的实参
+ 效率很高
+ 和循环比较，性能相近，所以并不是说递归一定效率低下，但是递归有深度限制

间接递归

```bash
def foo1():
    foo2()
def foo2():
    foo1()
foo1()
```

+ 间接递归，是通过别的函数调用了函数自身
+ 但是，如果构成了循环递归调用是非常危险的，但是往往在代码非常复杂的情况下有可能发生，要用代码规范来避免

递归总结

+ 递归是一种很自然的表达，符合逻辑思维
+ 递归相对运行效率低，每一次调用函数都要开辟栈帧
+ 递归有深度限制，如果递归层次太深，函数反复压栈，栈内存很快就溢出了
+ 如果是有限次数的递归，可以使用递归调用，或者使用循环代替，循环代码稍微复杂一些，但是只要不是死循环，可以多次迭代直至算出结果
+ 绝大多数递归，都可以使用循环实现
+ 即使递归代码很简洁，但是**能不用则不用递归**

习题练习

```bash
# 求 n 的阶乘
def fac(n):
    if n == 1:
        return 1
    return n * fac(n-1)
print(fac(5))
print(type(5))
-------------------------------
120
<class 'int'>

===============================

def fac1(n,factorial=1):
    if n == 1:
        return factorial
    factorial *= n
    return fac1(n-1,factorial)
print(fac1(5))
print(type(fac1(5)))
--------------------------------
120
<class 'int'>

================================

def fac2(n,factorial=1):
    if n == 1:
        return factorial
    factorial *= n
    fac2(n-1,factorial)
print(fac2(5))
print(type(fac2(5)))
--------------------------------
None
<class 'NoneType'>
```

+ 对比 fac1 和 fac2，两个函数唯一的区别是 fac1 有两个 return，fac2 只有一个 return
  + fac1 在递归循环中，执行到 n = 1 时，if 中的 return 把 factorial 返回给了递归中最后一层的 fac1，而并不是 def 后面的 fac1，而最后一个 return 正好把最后一层的 fac1 的返回值再一次返回给 def 后面的 fac1
  + 通过上面分析，很明显，fac2 没有最后一个 return，所以无法把最后一层的 fac2 返回的值再一次返回给 def 后的 fac2

```bash
# 将一个数逆序放入列表中，例如 1234 => [4,3,2,1]
data = str(123456798)
def revert(x):
    if x == -1:
        return ""
    return data[x] + revert(x-1)
print(revert(len(data)-1))
---------------------------------
897654321

=================================

def revert(n,lst=None):
    if lst is None:
        lst = []
    x,y = divmod(n,10)
    lst.append(y)
    if x == 0:
        return lst
    return revert(x,lst)
revert(123456)
----------------------------------
[6, 5, 4, 3, 2, 1]

==================================

def revert(num=str(12345),target=[]):
    if num:
        target.append(num[len(num)-1])
        revert(num[:len(num)-1])
    return target
print(revert())
-----------------------------------
['5', '4', '3', '2', '1']
```

```bash
# 解决猴子吃桃问题
# 猴子第一天摘n个桃子，当即吃了一半零一个，往后每天都是吃一半零一个，到第10天还没吃，发现只剩一个，求n
day1 d1 = n//2-1
day2 d2 = d1//2-1
day3 d3 = d2//2-1
...
day9 1 = d8//2-1
所以反推到 n
day8 d8 = (1+1)*2
day7 d7 = (d8+1)*2
day6 d6 = (d7+1)*2
...
day1 d1 = (d2+1)*2
n  n = (d1+1)*2
def peach(days=9,num=1):
    num = (num+1)*2
    if days == 1:
        return num  
    return peach(days-1,num)
print(peach())
-----------------------------
1534

=============================

def peach(days=10):
    if days == 1:
        return 1
    return (peach(days-1)+1)*2
print(peach())
-----------------------------
1534

==============================

def peach(days=1):
    if days == 10:
        return 1
    return (peach(days+1)+1)*2
print(peach())
------------------------------
1534
```

## 高阶函数

函数进阶概述

+ 函数在 Python 中是一等公民
+ 函数也是对象，可调用的对象
+ 函数可以作为普通变量、参数、返回值等等

高阶函数

+ 数学概念 y=g(f(x))
+ 在数学和计算机科学中，高阶函数应当是至少满足下面一个条件的函数
  + 接受一个或多个函数作为参数
  + 输出一个函数

举个列子分析

```bash
def counter(base):
    def inc(step=1):
        nonlocal base
        base += step
        return base
    return inc
print(1,"-->",counter(1)())
print(2,"-->",counter(1)())
c = counter(1)
print(3,"-->",c())
print(4,"-->",c())
--------------------------
1 --> 2
2 --> 2
3 --> 2
4 --> 3
```

+ 函数 counter 是不是一个高阶函数
  + 根据高阶函数的定义，要么函数作为函数的参数，要么输出一个函数，很明显 return inc 即是输出一个函数，所以是高阶函数
+ 分析1 2 和 3 4 的差别
  + 1 2 都是直接使用 counter(1)()，相当于生成两个新的对象； 3 4 相当于对同一个对象进行两次调用，counter 函数没有销毁，base 值可以累加
+ 分析 f1 = counter(5) 和 f2 = counter(5)，f1 和 f2 相等吗？
  + 不相等，因为 f1 和 f2 都是 inc 对象，而 inc 对象只是一个标识符，通过 id(f1) id(f2) 可以发现在内存中它们并不是同一个对象
  + 但是 f1() is f2() 返回的是 True，可以理解为 f1() 和 f2() 指向的是同一个值，相当于这个值的引用计数增加了

**自定义 sort 函数**，深入理解高阶函数的用法

思路

+ 内建函数 sorted 函数是返回一个**新的列表**，可以设置**升序或降序**，可以设置一个**排序的函数**。自定义的 sort 函数也要实现这个功能
+ 新建一个列表，遍历原列表，和新列表的值依次比较决定如何插入到新列表中

思考

+ sorted 函数的实现原理，扩展到 map、filter 函数的实现原理

```bash
def sort(iterable):
    ret = []
    for x in iterable:
        for i,y in enumerate(ret):
            if x > y:
                ret.insert(i,x) # 降序
                break
        else:
            ret.append(x)
    return ret
print(sort([1,2,5,4,2,3,5,6]))
------------------------------------------
[1, 2, 2, 3, 4, 5, 5, 6]

==========================================

def sort(iterable,reverse=False):
    ret = []
    for x in iterable:
        for i,y in enumerate(ret):
            flag = x > y if reverse else x < y # 表达技巧，将表达式转换成逻辑值
            if flag:
                ret.insert(i,x)
                break
        else:
            ret.append(x)
    return ret
print(sort([1,2,5,4,2,3,5,6],reverse=True))
--------------------------------------------
[6, 5, 5, 4, 3, 2, 2, 1]

============================================

def sort(iterable,key=lambda a,b:a>b):
    ret = []
    for x in iterable:
        for i,y in enumerate(ret):
            if key(x,y): # 函数的返回值是 bool
                ret.insert(i,x)
                break
        else:
            ret.append(x)
    return ret
print(sort([1,2,5,4,2,3,5,6]))
--------------------------------------------
[6, 5, 5, 4, 3, 2, 2, 1]

============================================

def sort(iterable,reverse=False,key=lambda x,y:x<y):
    ret = []
    for x in iterable:
        for i,y in enumerate(ret):
            flag = key(x,y) if not reverse else not key(x,y)
            if flag:
                ret.insert(i,x)
                break
        else:
            ret.append(x)
    return ret
print(sort([1,2,5,4,3,6]))
---------------------------------------------
[1, 2, 3, 4, 5, 6]
```

通过自定义 sort 函数的不断演化，可以举一反三，理解下面几个函数的逻辑

sorted(iterable \[,key\] \[,reverse\])

+ 返回一个新的列表，对一个可迭代对象的所有元素排序，排序规则为 key 定义的函数，reverse 表示是否排序翻转
+ sorted(lst,key=lambda x:6-x) 返回新列表
+ list.sort(key=lambda x:6-x) 就地修改

filter(function,iterable)

+ 过滤可迭代对象的元素，返回一个迭代器
+ function 是一个具有一个参数的函数，返回 bool
+ 比如过滤列表中能被 3 整除的数字 list(filter(lambda x:x%3==0,[1,9,55,150,-3,78,28,123]))

map(function,*iterables) --> map object

+ 对多个可迭代对象的元素按照指定的函数进行映射，返回一个迭代器
+ list(map(lambda x:2*x+1,range(5)))
+ dict(map(lambda x:(x%5,x),range(500)))

## 柯里化 Currying

柯里化

+ 指的是将原来接受两个参数的函数变成新的接受一个参数的函数的过程，新的函数返回一个以原有第二个参数为参数的函数
+ z = f(x,y) 转换成 z = f(x)(y) 的形式

举个例子说明

```bash
# 将加法函数柯里化
def add(x,y):
    return x + y

# 转换成
def add1(x):
    def _add1(y):
        return x+y
    return _add1
add(5,6) == add1(5)(6)
------------------------
True
```

+ 通过嵌套函数就可以把函数转换成柯里化函数

## 装饰器

需求

一个加法函数，想增强它的功能，能够输出被调用过以及调用的参数信息

```bash
def add(x,y):
    return x+y

# 增加信息输出功能
def add(x,y):
    print("call add, x+y") # 日志输出到控制台
    return x+y
```

上面加法函数是完成了需求，但是有以下的缺点

+ 打印语句的耦合太高
+ 加法函数属于业务功能，而输出信息的功能，属于非业务功能代码，不该放在业务函数加法中

改进

```bash
def add(x,y):
    return x + y
def logger(fn):
    print("begin") # 增强的输出
    x = fn(4,5)
    print("end") # 增强的功能
    return x
print(logger(add))
-----------------------------------------
begin
end
9
```

+ 做到了业务功能分离，但是 fn 函数调用传参是个问题

再改进

```bash
def add(x,y):
    return x + y
def logger(fn,*args,**kwargs):
    print("begin")
    x = fn(*args,**kwargs)
    print("end")
    return x
print(logger(add,5,y=60))
------------------------------------------
begin
end
65
```

+ 解决了传参的问题

再改进，这一次改进是用柯里化进行的改进，目的是把函数和函数所需要的参数进行分开传参，从进化的历程来看，思想维度在不断进化

```bash
# 柯里化
def add(x,y):
    return x + y
def logger(fn):
    def wrapper(*args,**kwargs):
        print("begin")
        x = fn(*args,**kwargs)
        print("end")
        return x
    return wrapper
print(logger(add)(5,y=50)) # 换一种写法 add=logger(add) print(add(x=5,y=10))
---------------------------------------------------------------------------
begin
end
55
```

+ 进化到这里，其实就是装饰器的演化过程，需要注意的是，最后一句的等价写法，两个 add 虽然都指向各自的对象，但它不过是一个标识符而已，跟变量是一个道理，可以随时按需求覆盖

**真正的装饰器**来了，装饰器其实是一个语法糖

```bash
def logger(fn):
    def wrapper(*args,**kwargs):
        print("begin")
        x = fn(*args,**kwargs)
        print("end")
        return x
    return wrapper
@logger # 等价于 add = logger(add)
def add(x,y):
    return x + y
print(add(45,40))
--------------------------------------------
begin
end
85
```

+ @logger 就是装饰器语法
+ 这是一个无参装饰器

下面对无参装饰器进行简单总结

+ 它是一个函数
+ 函数作为它的参数
+ 返回值也是一个函数
+ 可以使用 @functionname 方式，简化调用

**装饰器**是**高阶函数**，但装饰器是对传入函数的功能的装饰(**功能增强**)

```bash
import datetime
import time
def logger(fn):
    def wrap(*args,**kwargs):
        # before 功能增强
        print("args={},kwargs={}".format(args,kwargs))
        start = datetime.datetime.now()
        ret = fn(*args,**kwargs)
        # after 功能增强
        duration = datetime.datetime.now() - start
        print("function {} took {}s.".format(fn.__name__,duration.total_seconds()))
        return ret
    return wrap
@logger # 相当于 add = logger(add)
def add(x,y):
    print("========call add========")
    time.sleep(2)
    return x+y
print(add(4,y=7))
------------------------------------------------------------------------------------
args=(4,),kwargs={'y': 7}
========call add========
function add took 2.000493s.
11
```

+ 怎么理解装饰器？
  + 装饰器函数-->前置功能增强-->被增强函数<--后置功能增强

装饰器的副作用

```bash
def logger(fn):
    def wrapper(*args,**kwargs):
        """I am wrapper"""
        print("begin")
        x = fn(*args,**kwargs)
        print("end")
        return x
    return wrapper
@logger # add=logger(add)
def add(x,y):
    """This is a function for add"""
    return x+y
print("name={},doc={}".format(add.__name__,add.__doc__))
--------------------------------------------------------
name=wrapper,doc=I am wrapper
```

+ 很明显，原函数的属性都被替换了，我们希望使用装饰器后，看到的仍然是被封装函数的属性，如何解决？

改进方案，提供一个函数，被封装函数属性 == copy ==> 包装函数属性

```bash
def copy_properties(src,dst): # 可以改造成装饰器
    dst.__name__ = src.__name__
    dst.__doc__ = src.__doc__
def logger(fn):
    def wrapper(*args,**kwargs):
        "I am wrapper"
        print("begin")
        x = fn(*args,**kwargs)
        print("end")
        return x
    copy_properties(fn,wrapper)
    return wrapper
@logger # add = logger(add)
def add(x,y):
    """This is a function for add"""
    return x + y
print("name={},doc={}".format(add.__name__,add.__doc__))
----------------------------------------------------------
name=add,doc=This is a function for add
```

+ 通过 copy_properties 函数将被包装函数的属性覆盖掉包装函数
+ 凡是被包装的函数都需要复制这些属性，这个函数很通用
+ 可以将复制属性的函数构建成装饰器函数，带参装饰器

再改进，提供一个函数，被封装函数属性 == copy ==> 包装函数属性，改造成带参装饰器

```bash
def copy_properties(src):
    def _copy(dst):
        dst.__name__ = src.__name__
        dst.__doc__ = src.__doc__
        return dst
    return _copy
def logger(fn):
    @copy_properties(fn) # wrapper = copy_properties(fn)(wrapper)
    def wrapper(*args,**kwargs):
        """I am wrapper"""
        print("begin")
        x = fn(*args,**kwargs)
        print("end")
        return x
    return wrapper
@logger # add = logger(add)
def add(x,y):
    """This is a function for add"""
    return x+y
print("name={},doc={}".format(add.__name__,add.__doc__))
---------------------------------------------------------------------
name=add,doc=This is a function for add
```

再进化一步，使用带参装饰器

获取函数的执行时长，对时长超过阈值的函数记录一下

```bash
import datetime,time
def copy_properties(src):
    def _copy(dst):
        dst.__name__ = src.__name__
        dst.__doc__ = src.__doc__
        return dst
    return _copy
def logger(duration):
    def _logger(fn):
        @copy_properties(fn) # wrapper = copy_properties(fn)(wrapper)
        def wrapper(*args,**kwargs):
            """I am wrapper"""
            start = datetime.datetime.now()
            print("begin")
            x = fn(*args,**kwargs)
            delta = (datetime.datetime.now() - start).total_seconds()
            print("end")
            print("so slow") if delta > duration else print("so fast")
            return x
        return wrapper
    return _logger
@logger(2) # add = logger(5)(add)
def add(x,y):
    """This is a function for add"""
    time.sleep(3)
    return x+y
print("name={},doc={}".format(add.__name__,add.__doc__))
print(add(5,6))
-------------------------------------------------------------------------
name=add,doc=This is a function for add
begin
end
so slow
11
```

+ 这个环境已经很难用一句话概括了，需要从头至尾不断优化代码，思路才能清析

进一步美化，将记录的功能提取出来，这样就可以通过外部提供的函数来灵活地控制输出

```bash
import datetime,time
def copy_properties(src):
    def _copy(dst):
        dst.__name__ = src.__name__
        dst.__doc__ = src.__doc__
        return dst
    return _copy
def logger(duration,func=lambda name,duration:print("{} took {}s".format(name,duration))):
    def _logger(fn):
        @copy_properties(fn) # wrapper = copy_properties(fn)(wrapper)
        def wrapper(*args,**kwargs):
            """I am wrapper"""
            start = datetime.datetime.now()
            print("begin")
            x = fn(*args,**kwargs)
            delta = (datetime.datetime.now() - start).total_seconds()
            print("end")
            if delta > duration:
                func(fn.__name__,delta)
            return x
        return wrapper
    return _logger
@logger(2) # add = logger(5)(add)
def add(x,y):
    """This is a function for add"""
    time.sleep(3)
    return x+y
print("name={},doc={}".format(add.__name__,add.__doc__))
print(add(5,6))
------------------------------------------------------------------------
name=add,doc=This is a function for add
begin
end
add took 3.000332s
11
```

### functools 模块

functools.update_wrapper(wrapper,wrapped,assigned=WRAPPER_ASSIGNMENTS,updated=WRAPPER_UPDATES)

+ 类似 copy_properties 功能
+ wrapper 包装函数、被更新者，wrapped 被包装函数、数据源
+ 元组 WRAPPER_ASSIGNMENTS 中是要被覆盖的属性 "\_\_module\_\_"，"\_\_name\_\_"，"\_\_qualname\_\_"，"\_\_doc\_\_"，"\_\_annotations\_\_" 模块名、名称、限定名、文档、参数注解
+ 元组 WRAPPER_UPDATES 中是要被更新的属性，\_\_dict\_\_属性字典
+ 增加一个\_\_wrapped\_\_属性，保留着 wrapped 函数

```bash
import datetime,time,functools
def logger(duration,func=lambda name,duration:print("{} took {}s".format(name,duration))):
    def _logger(fn):
        def wrapper(*args,**kwargs):
            start = datetime.datetime.now()
            ret = fn(*args,**kwargs)
            delta = (datetime.datetime.now() - start).total_seconds()
            if delta > duration:
                func(fn.__name__,duration)
            return ret
        return functools.update_wrapper(wrapper,fn)
    return _logger
@logger(5) # add = logger(5)(add)
def add(x,y):
    time.sleep(1)
    return x + y
print(add(5,6),add.__name__,add.__wrapped__,add.__dict__,sep="\n")
-----------------------------------------------------------------------
11
add
<function add at 0x000002547F1D4A60>
{'__wrapped__': <function add at 0x000002547F1D4A60>}
```

@functools.wraps(wrapped,assigned=WRAPPER_ASSIGNMENTS,updated=WRAPPER_UPDATES)

+ 类似 copy_properties 功能
+ wrapped 被包装函数
+ 元组 WRAPPER_ASSIGNMENTS 中是要被覆盖的属性 "\_\_module\_\_"，"\_\_name\_\_"，"\_\_qualname\_\_"，"\_\_doc\_\_"，"\_\_annotations\_\_" 模块名、名称、限定名、文档、参数注解
+ 元组 WRAPPER_UPDATES 中是要被更新的属性，\_\_dict\_\_ 属性字典
+ 增加一个 \_\_wrapped\_\_ 属性，保留着 wrapped 函数

```bash
import datetime,time,functools
def logger(duration,func=lambda name,duration:print("{} tooks {}s".format(name,duration))):
    def _logger(fn):
        @functools.wraps(fn)
        def wrapper(*args,**kwargs):
            start = datetime.datetime.now()
            ret = fn(*args,**kwargs)
            delta = (datetime.datetime.now() - start).total_seconds()
            if delta > duration:
                func(fn.__name__,duration)
            return ret
        return wrapper
    return _logger
@logger(5) # add = logger(5)(add)
def add(x,y):
    "This is add func"
    time.sleep(1)
    return x+y
print(add(5,6),add.__name__,add.__doc__,add.__wrapped__,add.__dict__,sep="\n")
------------------------------------------------------------------------------
11
add
This is add func
<function add at 0x000002547FD8AF28>
{'__wrapped__': <function add at 0x000002547FD8AF28>}
```

## Python 类型注解

函数定义的弊端

+ Python 是动态语言，变量随时可以被赋值，且能赋值为不同的类型
+ Python 不是静态编译型语言，变量类型是在运行器决定的
+ 动态语言很灵活，但是这种特性也是弊端
  + 难发现，由于不做任何类型检查，直到运行期问题才显现出来，或者线上运行时才能暴露出问题
  + 难使用，函数的使用者看到函数的时候，并不知道你函数的设计，并不知道应该传入什么类型的数据

解决方法

1. 增加文档 Documentation String

   1. 这只是一种惯例，不是强制标准，不能要求程序员一定为函数提供说明文档
   2. 函数定义更新了，文档未必同步更新

   ```bash
   def add(x,y):
       """
       :param x:int
       :param y:int
       :return: int
       """
       return x+y
   print(help(add))
   ```

2. 函数注解

   1. Python 3.5 引入
   2. 对函数的参数进行类型注解
   3. 对函数的返回值进行类型注解
   4. 只对函数参数做一个**辅助说明**，并不对函数参数进行类型检查
   5. 提供给第三方工具，做代码分析，发现隐藏的 bug
   6. 函数注解的信息，保存在 \_\_annotations\_\_ 属性中

   ```bash
   def add(x:int,y:int) -> int:
       """
       :param x:int
       :param y:int
       :return: int
       """
       return x + y
   print(help(add))
   print(add(4,5))
   print(add("mag","edu")) # 不会报错，类型注解并非强制要求
   print(add.__annotations__)
   --------------------------------------
   Help on function add in module __main__:

   add(x:int, y:int) -> int
       :param x:int
       :param y:int
       :return: int

   None
   9
   magedu
   {'y': <class 'int'>, 'return': <class 'int'>, 'x': <class 'int'>}
   ```

3. 变量注解

   1. Python 3.6 引入，i : int = 3

函数参数类型检查思路

+ 函数参数的检查，一定是在函数外
+ 函数应该作为参数，传入到检查函数中
+ 检查函数拿到函数传入的实际参数，与形参声明对比
+ \_\_annotations\_\_ 属性是一个字典，其中包括返回值类型的声明。假设要做位置参数的判断，无法和字典中的声明对应。使用 inspect 模块

**inspect 模块**，提供获取对象信息的函数，可以检查函数和类、类型检查

**inspect.signature(callable)**，获取签名(函数签名包含了一个函数的信息，包括函数名、它的参数类型、它所在的类和名称空间及其他信息)

```bash
import inspect
def add(x:int,y:int,*args,**kwargs) -> int:
    return x + y
sig = inspect.signature(add)
print(1,"-->",sig,type(sig))
print(2,"-->","params:",sig.parameters) # OrderedDict
print(3,"-->","return:",sig.return_annotation)
print(4,"-->",sig.parameters['y'],type(sig.parameters['y']))
print(5,"-->",sig.parameters['x'].annotation)
print(6,"-->",sig.parameters["args"])
print(7,"-->",sig.parameters["args"].annotation)
print(8,"-->",sig.parameters["kwargs"])
print(9,"-->",sig.parameters["kwargs"].annotation)
-----------------------------------------------------------------
1 --> (x:int, y:int, *args, **kwargs) -> int <class 'inspect.Signature'>
2 --> params: OrderedDict([('x', <Parameter "x:int">), ('y', <Parameter "y:int">), ('args', <Parameter "*args">), ('kwargs', <Parameter "**kwargs">)])
3 --> return: <class 'int'>
4 --> y:int <class 'inspect.Parameter'>
5 --> <class 'int'>
6 --> *args
7 --> <class 'inspect._empty'>
8 --> **kwargs
9 --> <class 'inspect._empty'>
```

```bash
inspect.isfunction(add) 是否是函数
inspect.ismethod(add) 是否是类的方法
inspect.isgenerator(add) 是否是生成器对象
inspect.isgeneratorfunction(add) 是否是生成器函数
还有很多 is 函数，需要的时候查阅 inspect 模块帮助
```

Parameter 对象

+ 保存在元组中，是只读的
+ name，参数的名字
+ annotation，参数的注解，可能没有定义
+ default，参数的缺省值，可能没有定义
+ empty，特殊的类，用来标记 default 属性或者注释 annotation 属性的空值
+ kind，实参如何绑定到形参，就是形参的类型
  + POSITIONAL_ONLY，值必须是位置参数提供，python 中并没有实现
  + POSITIONAL_OR_KEYWORD，值可以作为关键字或者位置参数提供
  + VAR_POSITIONAL，可变位置参数，对应 *args
  + KEYWORD_ONLY，keyword-only 参数，对应 \* 或者 \*args 之后出现的非可变关键字参数
  + VAR_KEYWORD，可变关键字参数，对应 \*\*kwargs

```bash
import inspect

def add(x,y:int=7,*args,z,t=10,**kwargs) -> int:
    return x + y
sig = inspect.signature(add)
print(1,"-->",sig)
print(2,"-->","params:",sig.parameters) # 有序字典
print(3,"-->","return:",sig.return_annotation)
print()
for i,item in enumerate(sig.parameters.items()):
    name,param = item
    print(i+1,name,param.annotation,param.kind,param.default)
    print(param.default is param.empty,end="\n\n")
--------------------------------------------------------------------
1 --> (x, y:int=7, *args, z, t=10, **kwargs) -> int
2 --> params: OrderedDict([('x', <Parameter "x">), ('y', <Parameter "y:int=7">), ('args', <Parameter "*args">), ('z', <Parameter "z">), ('t', <Parameter "t=10">), ('kwargs', <Parameter "**kwargs">)])
3 --> return: <class 'int'>

1 x <class 'inspect._empty'> POSITIONAL_OR_KEYWORD <class 'inspect._empty'>
True

2 y <class 'int'> POSITIONAL_OR_KEYWORD 7
False

3 args <class 'inspect._empty'> VAR_POSITIONAL <class 'inspect._empty'>
True

4 z <class 'inspect._empty'> KEYWORD_ONLY <class 'inspect._empty'>
True

5 t <class 'inspect._empty'> KEYWORD_ONLY 10
False

6 kwargs <class 'inspect._empty'> VAR_KEYWORD <class 'inspect._empty'>
True
```

业务应用

请检查用户输入是否符合参数注解的要求

思路

+ 调用时，判断用户输入的实参是否符合要求
+ 调用时，用户感觉上还是在调用 add 函数
+ 对用户输入的数据和声明的类型进行对比，如果不符合，提示用户

```bash
import inspect
def add(x,y:int=7) -> int:
    return x + y
def check(fn):
    def wrapper(*args,**kwargs):
        sig = inspect.signature(fn)
        params = sig.parameters
        values = list(params.values())
        for i,p in enumerate(args):
            if isinstance(p,values[i].annotation):
                print(p,"==",values[i].annotation)
            else:
                print(p,"!=",values[i].annotation)
        for k,v in kwargs.items():
            if isinstance(v,params[k].annotation):
                print(v,"is",params[k].annotation)
            else:
                print(v,"is not",params[k].annotation)
        return fn(*args,**kwargs)
    return wrapper
print(check(add)(10,y=20))
------------------------------------------------------------
10 != <class 'inspect._empty'>
20 is <class 'int'>
30
```

进一步改进，业务需求是参数有注解就要求实参类型和声明应该一致，没有注解的参数不比较，如何修改代码？

```bash
import inspect
def check(fn):
    def wrapper(*args,**kwargs):
        sig = inspect.signature(fn)
        params = sig.parameters
        values = list(params.values())
        for i,p in enumerate(args):
            param = values[i]
            if param.annotation is not param.empty and not isinstance(p,param.annotation):
                print(p,"!==",values[i].annotation)
        for k,v in kwargs.items():
            if params[k].annotation is not inspect._empty and not isinstance(v,params[k],annotation):
                print(k,v,"!===",params[k].annotation)
        return fn(*args,**kwargs)
    return wrapper
@check # add = check(add)
def add(x,y:int=7) -> int:
    return x + y
print(add("a","b"))
--------------------------------------------------------------------
b !== <class 'int'>
ab
```

### functools 模块之 partial

partial 方法

+ 偏函数，把函数的部分参数固定下来，相当于为部分参数添加了一个固定的默认值，形成一个新的函数并返回
+ 从 partial 生成的新函数，是对原函数的封装

```bash
import functools

def add(x,y) -> int:
    return x + y
newadd = functools.partial(add,y=5)

print(1,"-->",newadd(8))
print(2,"-->",newadd(8,y=6))
print(3,"-->",newadd(y=10,x=6))

import inspect
print(11,"-->",inspect.signature(newadd))
----------------------------------------------
1 --> 13
2 --> 14
3 --> 16
11 --> (x, *, y=5) -> int
```

```bash
import functools
def add(x,y,*args) -> int:
    print(args)
    return x + y
newadd = functools.partial(add,1,3,6,5)
print(1,"-->",newadd(7))
print(2,"-->",newadd(7,10))
# print(3,"-->",newadd(9,10,y=20,x=26)) 报错
print(4,"-->",newadd())

import inspect
print(inspect.signature(newadd))
----------------------------------------------------
(6, 5, 7)
1 --> 4
(6, 5, 7, 10)
2 --> 4
(6, 5)
4 --> 4
(*args) -> int
```

partial 函数本质

```bash
def partial(func,*args,**keywords):
    def newfunc(*fargs,**fkeywords): # 包装函数
        newkeywords = keywords.copy()
        newkeywords.update(fkeywords)
        return func(*(args+fargs),**newkeywords)
    newfunc.func = func # 保留原函数
    newfunc.args = args # 保留原函数的位置参数
    newfunc.keywords = keywords # 保留原函数的关键字参数
    return newfunc
def add(x,y):
    return x + y
foo = partial(add,4)
foo(5)
```

+ 这个代码片段不太好理解的部分是 "保留原函数及参数"，一开始是从装饰器的角度来理解，但它并不是装饰器，当 partial 打造了一个函数之后，就会进入 newfunc 片段，这个片段是要接受新的参数，问题是 partial 固定的那一部分参数怎么办呢，按理说必须跟 newfunc 接受的新参数合并才行，所以必须要把 partial 固定的函数及参数保留下来，便于合并

 @functools.lru_cache(maxsize=128, typed=False)

+ Least-recently-used 装饰器，lru 最近最少使用，cache 缓存
+ 如果 maxsize 设置为 None，则禁用 LRU 功能，并且缓存可以无限制增长，当maxsize是二的幂时，LRU功能执行得最好
+ 如果typed设置为True，则不同类型的函数参数将单独缓存。例如，f(3)和f(3.0)将被视为具有不同结果的不同调用

```bash
import functools
import time
@functools.lru_cache()
def add(x, y, z=3):
    time.sleep(z)
    return x + y
print(1,"-->",add(4, 5)) # 等 5 秒
print(2,"-->",add(4.0, 5)) # 立即返回
print(3,"-->",add(4, 6)) # 等 5 秒
print(4,"-->",add(4, 6, 3)) # 等 5 秒
print(5,"-->",add(6, 4)) # 等 5 秒
print(6,"-->",add(4, y=6)) # 等 5 秒
print(7,"-->",add(x=4, y=6)) # 等 6 秒
print(8,"-->",add(y=6, x=4)) # 立即返回
```

+ 缓存的机制，根据一个引索，查看其值是否有缓存，很明显使用 kv 对实现是最方便的，所以用字典数据结构来实现缓存最便捷，lru 源码的实现也正是利用字典。根据这个例子的效果分析，如果 key 是一样的，那取缓存值就很快了，问题是 key 是如何确定的，这就引出了下面的 \_make\_key 函数

\_make\_key 函数

```bash
print(1,"-->",functools._make_key((4,6),{'z':3},False))
print(2,"-->",functools._make_key((4,6,3),{},False))
print(3,"-->",functools._make_key(tuple(),{'z':3,'x':4,'y':6},False))
print(4,"-->",functools._make_key(tuple(),{'z':3,'x':4,'y':6}, True))
---------------------------------------------------------------------
1 --> [4, 6, <object object at 0x000002AD0E8E0090>, 'z', 3]
2 --> [4, 6, 3]
3 --> [<object object at 0x000002AD0E8E0090>, 'x', 4, 'y', 6, 'z', 3]
4 --> [<object object at 0x000002AD0E8E0090>, 'x', 4, 'y', 6, 'z', 3, <class 'int'>, <class 'int'>, <class 'int'>]
```

\_make\_key 源码

```bash
def _make_key(args, kwds, typed,
             kwd_mark = (object(),), # 默认值是一个空对象元组
             fasttypes = {int, str, frozenset, type(None)},
             sorted=sorted, tuple=tuple, type=type, len=len): # 会用到的函数作为默认值传参进来
    key = args # 首先将位置参数元组赋值给 key
    if kwds:
        sorted_items = sorted(kwds.items()) # 关键字参数排序后立即返回一个排好序的二元组列表
        key += kwd_mark # 这一步结果类似 (4,6,<object object at 0x000002AD0E8E0090>)
        for item in sorted_items: # 二元组也是元组，元素直接加到 key 里面
            key += item # 这一步结果类似 (4,6,<object object at 0x000002AD0E8E0090>,'z', 3)
    if typed:
        key += tuple(type(v) for v in args) # 如果 type 为 True，3.0 和 3 这两个数是不一样的，key 自然要加以区分
        if kwds:
            key += tuple(type(v) for k, v in sorted_items)
    elif len(key) == 1 and type(key[0]) in fasttypes:
        return key[0]
    return _HashedSeq(key) # key 做出来后，一定要 hash 一下，不然如果传的参是 y=[2]，这就没法作为字典的 key 了
```

lru_cache 装饰器加速一下 fibonacci 数列

```bash
import functools
@functools.lru_cache() # maxsize=None
def fib(n):
    if n < 3:
        return n
    return fib(n-1) + fib(n-2)
print([fib(x) for x in range(35)])
```

+ 这个学习的过程中，有一个同学提出如果 maxsize=5，fib(500)，速度还会快吗，针对上面的 fibonacci 数列，依然很快，fib(500) fib(499) ... fib(3) 都是一路问下来，缓存中 fib(1) fib(2) ... fib(499) 一路缓存上去，发生的计算很少，所以照样很快

lru_cache装饰器应用
使用前提

1. 同样的函数参数一定得到同样的结果
2. 函数执行时间很长，且要多次执行
3. 本质是函数调用的参数=>返回值
4. 适用场景，单机上需要空间换时间的地方，可以用缓存来将计算变成快速的查询

缺点

1. 不支持缓存过期，key无法过期、失效
2. 不支持清除操作
3. 不支持分布式，是一个单机的缓存

#### 举例

##### 实现一个 cache 装饰器

简化设计，函数的形参定义不包含可变位置参数，可变关键词参数和 keyword-only 参数

可以不考虑缓存满了之后的换出问题

> 数据类型的选择

缓存的应用场景，是有数据需要频繁查询，且每次查询都需要大量计算或者等待时间之后才能返回结果的情况。使用缓存来提高查询速度，用空间换取时间

> cache 应该选用什么数据结构

通过输入某些参数，获得另外一个值，这就是查询的原型，查询用到的数据结构应该就是字典，输入 key，获得 value 值。难点在 key 的设计上面，

> key 的存储

key 必须是可 hashable

key 能接受位置参数和关键字参数

位置参数被收集在 tuple 中，本身就有序；关键字参数被收集在字典中，本身无序

让关键字参数有序：1. OrderDict 记录顺序，2. 用一个 tuple 保存排过序的字典的 item 的 kv 对

> key 的设计分析

假设 add(x,y) 是一个加法函数，接受的参数方式可能有以下几种

1. add(4,5)
2. add(4,y=5)
3. add(y=4,x=5)
4. add(x=5,y=4)

可以有两种理解：第一种是 3 和 4 相同，而1、2 和 3 都不同；第二种是全部相同。

lru_cache 实现了第一种，可以看出单独的处理了位置参数和关键字参数

如果函数定义为 def add(4,y=5)，使用了默认值，如何理解 add(4,5) 和 add(4) 是否一样呢？

如果认为一样，那么 lru_cache 无能为力，需要使用 inspect 来自己实现算法

> key 的算法设计

inspect 模块获取函数签名后，取 parameters，这是一个有序字典，会保存所有参数的信息。

构建一个字典 params_dict，按照位置顺序从 args 中依次对应参数名和传入的实参，组成 kv 对，存入 params_dict 中。

kwargs 所有值 update 到 params_dict 中。

如果使用了缺省值的参数，不会出现在实参 params_dict 中，会出现在签名的取 parameters 中，缺省值也在定义中。

> 调用的方式

普通的函数调用可以，但是过于明显，最好类似 lru_cache 的方式，让调用者无察觉的使用缓存，构建装饰器函数。

目标：下面几种都等价，也就是 key 一样，这样都可以缓存。

```bash
def add(x,z,y=6):
    return x + y + z
add(4,5)
add(4,z=5)
add(4,y=6,z=5)
add(y=6,z=5,x=4)
add(4,5,6)
```

代码框架如下

```bash
from functools import wraps
import inspect

def ly_cache(fn):
    local_cache = {} # 设计不同函数名对应不同的 cache
    @wraps(fn)
    def wrapper(*args,**kwargs): # 接收各种参数
        ret = fn(*args,**kwargs)
        return ret
    return wrapper
@ly_cache
def add(x,y,z=6):
    return x + y + z
```

完成 key 的生成，这里使用普通的字典 params_dict，先把位置参数的对应好，再填充关键字参数，最后补充缺省值，然后再**排序**生成 key

```bash
from functools import wraps
import inspect
import time

def ly_cache(fn):
    local_cache = {} # 设计不同函数名对应不同的 cache
    @wraps(fn)
    def wrapper(*args,**kwargs): # 接收各种参数
        sig = inspect.signature(fn) # 签名可以辅助构建 key，重点还可以记录默认值
        params = sig.parameters # 只读有序字典

        param_names = [key for key in params.keys()] # list(params.keys())
        params_dict = {}

        for i,v in enumerate(args):
            k = param_names[i]
            params_dict[k] = v

        params_dict.update(kwargs) # 这是优化的结果，最开始 params_dict 用的是元组，改成字典的好处

        for k,v in params.items(): # 缺省值处理
            if k not in params_dict.keys():
                params_dict[k] = v.default

        key = tuple(sorted(params_dict.items()))

        if key not in local_cache.keys(): # 判断是否需要缓存
            local_cache[key] = fn(*args,**kwargs)

        return key, local_cache[key]

    return wrapper

@ly_cache
def add(x,y,z=6):
    time.sleep(3)
    return x + y + z

print(add(4,5))
print(add(y=5,x=4))
print(add(4,5,6))
----------------------------------------------------------------------------------------
((('x', 4), ('y', 5), ('z', 6)), 15)
((('x', 4), ('y', 5), ('z', 6)), 15)
((('x', 4), ('y', 5), ('z', 6)), 15)
```

增加 logger 装饰器查看执行时间

```bash
from functools import wraps
import inspect,time,datetime

def logger(fn):
    @wraps(fn)
    def wrapper(*args,**kwargs):
        start = datetime.datetime.now()
        ret = fn(*args,**kwargs)
        delta = (datetime.datetime.now() - start).total_seconds()
        print(fn.__name__,delta)
        return ret
    return wrapper

def ly_cache(fn):
    local_cache = {} # 设计不同函数名对应不同的 cache
    @wraps(fn)
    def wrapper(*args,**kwargs): # 接收各种参数
        sig = inspect.signature(fn) # 签名可以辅助构建 key，重点还可以记录默认值
        params = sig.parameters # 只读有序字典

        param_names = [key for key in params.keys()] # list(params.keys())
        params_dict = {}

        for i,v in enumerate(args):
            k = param_names[i]
            params_dict[k] = v

        params_dict.update(kwargs) # 这是优化的结果，最开始 params_dict 用的是元组，改成字典的好处

        for k,v in params.items(): # 缺省值处理
            if k not in params_dict.keys():
                params_dict[k] = v.default

        key = tuple(sorted(params_dict.items()))

        if key not in local_cache.keys(): # 判断是否需要缓存
            local_cache[key] = fn(*args,**kwargs)

        return key, local_cache[key]

    return wrapper
@logger # 等价 add = logger(add)
@ly_cache # 等价 add = ly_cache(add)
def add(x,y,z=6):
    time.sleep(3)
    return x + y + z

print(add(4,5))
print(add(y=5,x=4))
print(add(4,5,6))
-----------------------------------------------------------------------------------------
add 3.00635
((('x', 4), ('y', 5), ('z', 6)), 15)
add 0.0
((('x', 4), ('y', 5), ('z', 6)), 15)
add 0.0
((('x', 4), ('y', 5), ('z', 6)), 15)
```

##### cache 过期功能

一般缓存系统都有过期功能

过期指的是某一个 key 过期，可以对每一个 key 单独设置过期时间，也可以对这些 key 统一设定过期时间

本次设计简单点，统一设定 key 的过期时间，当 key 生存周期超过了这个时间，就自动被清除

这里并没有考虑多线程问题，而且这种过期机制，每一次都有遍历所有数据，大量数据的时候，有效率问题

> 清除时机，何时清除过期 key ?

1. 用到某个 key 之前，先判断是否过期，如果过期重新调用函数生成新的 key 对应 value 值
2. 一个线程负责清除过期的 key，这个以后实现，本次在创建 key 之前，清除所有过期的 key

> value 的设计

1. key => (v,createtimestamp)，适合 key 过期时间都是统一的设定
2. key => (v,createtimestamp,duration)，duration 是过期时间，这样每一个 key 就可以单独控制过期时间，在这种设计中，-1 可以表示永不过期，0 可以表示立即过期，正整数表示持续一段时间过期

本次采用第一种实现

```bash
from functools import wraps
import inspect,time,datetime

def logger(fn):
    @wraps(fn)
    def wrapper(*args,**kwargs):
        start = datetime.datetime.now()
        ret = fn(*args,**kwargs)
        delta = (datetime.datetime.now() - start).total_seconds()
        print(fn.__name__,delta)
        return ret
    return wrapper

def ck_cache(duration):
    def _cache(fn):
        local_cache = {} # 设计不同函数名对应不同的 cache

        @wraps(fn)
        def wrapper(*args,**kwargs): # 接收各种参数
            expire_keys = []  # 清除过期的key
            for k,(_,stamp) in local_cache.items():
                now = datetime.datetime.now().timestamp()
                if now - stamp > duration:
                    expire_keys.append(k)
            for k in expire_keys:
                local_cache.pop(k)


            sig = inspect.signature(fn) # 签名可以辅助构建 key，重点还可以记录默认值
            params = sig.parameters # 只读有序字典

            param_names = [key for key in params.keys()] # list(params.keys())
            params_dict = {}

            for i,v in enumerate(args):
                k = param_names[i]
                params_dict[k] = v

            params_dict.update(kwargs) # 这是优化的结果，最开始 params_dict 用的是元组，改成字典的好处

            for k,v in params.items(): # 缺省值处理
                if k not in params_dict.keys():
                    params_dict[k] = v.default

            key = tuple(sorted(params_dict.items()))

            if key not in local_cache.keys(): # 判断是否需要缓存
                local_cache[key] = (fn(*args,**kwargs),datetime.datetime.now().timestamp())

            return key, local_cache[key]

        return wrapper
    return _cache

@logger # 等价 add = logger(add)
@ck_cache(6) # 等价 add = ly_cache(add)
def add(x,y,z=6):
    time.sleep(3)
    return x + y + z

print(add(4,5))
print(add(y=5,x=4))
print(add(4,5,6))
time.sleep(7)
print(">>>>>>>>>>>>>>>>>>>>>>>")
print(add(4,5,6))
----------------------------------------------------------------------------------------
add 3.004313
((('x', 4), ('y', 5), ('z', 6)), (15, 1586068635.485182))
add 0.0
((('x', 4), ('y', 5), ('z', 6)), (15, 1586068635.485182))
add 0.0
((('x', 4), ('y', 5), ('z', 6)), (15, 1586068635.485182))
>>>>>>>>>>>>>>>>>>>>>>>
add 3.014408
((('x', 4), ('y', 5), ('z', 6)), (15, 1586068645.502859))
```

+ 可以进一步美化一下代码，把过期删除代码块抽象成函数 clear_expire()，把制作 key 的代码块抽象成函数 make_key()

抽象函数美化代码

```bash
from functools import wraps
import inspect,time,datetime

def logger(fn):
    @wraps(fn)
    def wrapper(*args,**kwargs):
        start = datetime.datetime.now()
        ret = fn(*args,**kwargs)
        delta = (datetime.datetime.now() - start).total_seconds()
        print(fn.__name__,delta)
        return ret
    return wrapper

def ck_cache(duration):
    def _cache(fn):
        local_cache = {} # 设计不同函数名对应不同的 cache

        @wraps(fn)
        def wrapper(*args,**kwargs): # 接收各种参数
            def clear_expire(cache):
                expire_keys = []  # 清除过期的key
                for k,(_,stamp) in cache.items():
                    now = datetime.datetime.now().timestamp()
                    if now - stamp > duration:
                        expire_keys.append(k)
                for k in expire_keys:
                    cache.pop(k)

            clear_expire(local_cache)

            def make_key():
                sig = inspect.signature(fn) # 签名可以辅助构建 key，重点还可以记录默认值
                params = sig.parameters # 只读有序字典

                param_names = [key for key in params.keys()] # list(params.keys())
                params_dict = {}

                for i,v in enumerate(args):
                    k = param_names[i]
                    params_dict[k] = v

                params_dict.update(kwargs) # 这是优化的结果，最开始 params_dict 用的是元组，改成字典的好处

                for k,v in params.items(): # 缺省值处理
                    if k not in params_dict.keys():
                        params_dict[k] = v.default

                return tuple(sorted(params_dict.items()))

            key = make_key()

            if key not in local_cache.keys(): # 判断是否需要缓存
                local_cache[key] = (fn(*args,**kwargs),datetime.datetime.now().timestamp())

            return key, local_cache[key]

        return wrapper
    return _cache

@logger # 等价 add = logger(add)
@ck_cache(6) # 等价 add = ly_cache(add)
def add(x,y,z=6):
    time.sleep(3)
    return x + y + z

print(add(4,5))
print(add(y=5,x=4))
print(add(4,5,6))
time.sleep(7)
print(">>>>>>>>>>>>>>>>>>>>>>>")
print(add(4,5,6))
-----------------------------------------------------------------------------------------
add 3.000478
((('x', 4), ('y', 5), ('z', 6)), (15, 1586069416.768573))
add 0.001008
((('x', 4), ('y', 5), ('z', 6)), (15, 1586069416.768573))
add 0.0
((('x', 4), ('y', 5), ('z', 6)), (15, 1586069416.768573))
>>>>>>>>>>>>>>>>>>>>>>>
add 3.00155
((('x', 4), ('y', 5), ('z', 6)), (15, 1586069426.772568))
```
