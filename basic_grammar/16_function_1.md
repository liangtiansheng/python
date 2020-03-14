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
day1	d1 = n//2-1
day2	d2 = d1//2-1
day3	d3 = d2//2-1
...
day9	1 = d8//2-1
所以反推到 n
day8	d8 = (1+1)*2
day7	d7 = (d8+1)*2
day6	d6 = (d7+1)*2
...
day1	d1 = (d2+1)*2
n		n = (d1+1)*2
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
            flag = x > y if reverse else x < y	# 表达技巧，将表达式转换成逻辑值
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


