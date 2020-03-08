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