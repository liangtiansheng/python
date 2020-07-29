# Python3 函数

函数是组织好的，可重复使用的，用来实现单一，或相关联功能的代码段。

函数能提高应用的模块性，和代码的重复利用率。你已经知道Python提供了许多内建函数，比如print()。但你也可以自己创建函数，这被叫做用户自定义函数。

## 定义一个函数

你可以定义一个由自己想要功能的函数，以下是简单的规则：

+ 函数代码块以 def 关键词开头，后接函数标识符名称和圆括号 ()。
+ 任何传入参数和自变量必须放在圆括号中间，圆括号之间可以用于定义参数。
+ 函数的第一行语句可以选择性地使用文档字符串—用于存放函数说明。
+ 函数内容以冒号起始，并且缩进。
+ return [表达式] 结束函数，选择性地返回一个值给调用方。不带表达式的return相当于返回 None。

### 语法

Python 定义函数使用 def 关键字，一般格式如下：

```bash
def 函数名（参数列表）:
    函数体
```

默认情况下，参数值和参数名称是按函数声明中定义的顺序匹配起来的。

### 实例

让我们使用函数来输出"Hello World！"：

```bash
>>>def hello() :
   print("Hello World!")


>>> hello()
Hello World!
>>>
```

更复杂点的应用，函数中带上参数变量:

```bash
#!/usr/bin/python3

# 计算面积函数
def area(width, height):
    return width * height

def print_welcome(name):
    print("Welcome", name)

print_welcome("Runoob")
w = 4
h = 5
print("width =", w, " height =", h, " area =", area(w, h))
```

以上实例输出结果：

```bash
Welcome Runoob
width = 4  height = 5  area = 20
```

## 函数调用

定义一个函数：给了函数一个名称，指定了函数里包含的参数，和代码块结构。

这个函数的基本结构完成以后，你可以通过另一个函数调用执行，也可以直接从 Python 命令提示符执行。

如下实例调用了 printme() 函数：

```bash
#!/usr/bin/python3

# 定义函数
def printme( str ):
   # 打印任何传入的字符串
   print (str)
   return

# 调用函数
printme("我要调用用户自定义函数!")
printme("再次调用同一函数")
```

以上实例输出结果：

```bash
我要调用用户自定义函数!
再次调用同一函数
```

## 参数传递

在 python 中，类型属于对象，变量是没有类型的：

```bash
a=[1,2,3]

a="Runoob"
```

以上代码中，[1,2,3] 是 List 类型，"Runoob" 是 String 类型，而变量 a 是没有类型，她仅仅是一个对象的引用（一个指针），可以是指向 List 类型对象，也可以是指向 String 类型对象。

### 可更改(mutable)与不可更改(immutable)对象

在 python 中，strings, tuples, 和 numbers 是不可更改的对象，而 list,dict 等则是可以修改的对象。

+ _不可变类型_：变量赋值 a=5 后再赋值 a=10，这里实际是新生成一个 int 值对象 10，再让 a 指向它，而 5 被丢弃，不是改变a的值，相当于新生成了a。
+ **可变类型：**变量赋值 la=[1,2,3,4] 后再赋值 la[2]=5 则是将 list la 的第三个元素值更改，本身la没有动，只是其内部的一部分值被修改了。

python 函数的参数传递：

+ **不可变类型：**类似 c++ 的值传递，如 整数、字符串、元组。如fun（a），传递的只是a的值，没有影响a对象本身。比如在 fun（a）内部修改 a 的值，只是修改另一个复制的对象，不会影响 a 本身。
+ **可变类型：**类似 c++ 的引用传递，如 列表，字典。如 fun（la），则是将 la 真正的传过去，修改后fun外部的la也会受影响

python 中一切都是对象，严格意义我们不能说值传递还是引用传递，我们应该说传不可变对象和传可变对象。

### python 传不可变对象实例

```bash
#!/usr/bin/python3

def ChangeInt( a ):
    a = 10

b = 2
ChangeInt(b)
print( b ) # 结果是 2
```

实例中有 int 对象 2，指向它的变量是 b，在传递给 ChangeInt 函数时，按传值的方式复制了变量 b，a 和 b 都指向了同一个 Int 对象，在 a=10 时，则新生成一个 int 值对象 10，并让 a 指向它。

### 传可变对象实例

可变对象在函数里修改了参数，那么在调用这个函数的函数里，原始的参数也被改变了。例如：

```bash
#!/usr/bin/python3

# 可写函数说明
def changeme( mylist ):
   "修改传入的列表"
   mylist.append([1,2,3,4])
   print ("函数内取值: ", mylist)
   return

# 调用changeme函数
mylist = [10,20,30]
changeme( mylist )
print ("函数外取值: ", mylist)
```

传入函数的和在末尾添加新内容的对象用的是同一个引用。故输出结果如下：

```bash
函数内取值:  [10, 20, 30, [1, 2, 3, 4]]
函数外取值:  [10, 20, 30, [1, 2, 3, 4]]
```

## 参数

以下是调用函数时可使用的正式参数类型：

+ 必需(位置)参数
+ 关键字参数
+ 默认参数
+ 不定长参数

### 必需(位置)参数

必需参数须以正确的顺序传入函数。调用时的数量必须和声明时的一样。

调用 printme() 函数，你必须传入一个参数，不然会出现语法错误：

```bash
#!/usr/bin/python3

#可写函数说明
def printme( str ):
   "打印任何传入的字符串"
   print (str)
   return

# 调用 printme 函数，不加参数会报错
printme()
```

以上实例输出结果：

```bash
Traceback (most recent call last):
  File "test.py", line 10, in <module>
    printme()
TypeError: printme() missing 1 required positional argument: 'str'
```

### 关键字参数

关键字参数和函数调用关系紧密，使用形参的名字来传入实参。

使用关键字参数允许函数调用时参数的顺序与声明时不一致，因为 Python 解释器能够用参数名匹配参数值。

以下实例在函数 printme() 调用时使用参数名：

```bash
#!/usr/bin/python3

#可写函数说明
def printme( str ):  # 位置参数
   "打印任何传入的字符串"
   print (str)
   return

#调用printme函数
printme( str = "菜鸟教程") # 关键字的方式传入
```

以上实例输出结果：

```bash
菜鸟教程
```

以下实例中演示了函数参数的使用不需要使用指定顺序：

```bash
#!/usr/bin/python3

#可写函数说明
def printinfo( name, age ): # 位置参数
   "打印任何传入的字符串"
   print ("名字: ", name)
   print ("年龄: ", age)
   return

#调用printinfo函数
printinfo( age=50, name="runoob" ) # 关键字方式传入
```

以上实例输出结果：

```bash
名字:  runoob
年龄:  50
```

通过以上描述，很明显，**位置参数**并没有在 python 中**专门实现**，因为位置参数也可以用**关键字传入**

### 默认参数

调用函数时，如果没有传递参数，则会使用默认参数。以下实例中如果没有传入 age 参数，则使用默认值：

```bash
#!/usr/bin/python3

#可写函数说明
def printinfo( name, age = 35 ):
   "打印任何传入的字符串"
   print ("名字: ", name)
   print ("年龄: ", age)
   return

#调用printinfo函数
printinfo( age=50, name="runoob" )
print ("------------------------")
printinfo( name="runoob" )
```

以上实例输出结果：

```bash
名字:  runoob
年龄:  50
------------------------
名字:  runoob
年龄:  35
```

### 不定长参数

你可能需要一个函数能处理比当初声明时更多的参数。这些参数叫做不定长参数，和上述 2 种参数不同，声明时不会命名。基本语法如下：

```bash
def functionname([formal_args,] *var_args_tuple ):
   "函数_文档字符串"
   function_suite
   return [expression]
```

加了星号 *的参数会以**元组(tuple)**的形式导入，存放所有未命名的变量参数。

```bash
#!/usr/bin/python3
  
# 可写函数说明
def printinfo( arg1, *vartuple ):
   "打印任何传入的参数"
   print ("输出: ")
   print (arg1)
   print (vartuple)

# 调用printinfo 函数
printinfo( 70, 60, 50 )
```

以上实例输出结果：

```bash
输出:
70
(60, 50)
```

如果在函数调用时没有指定参数，它就是一个**空元组**。我们也可以不向函数传递未命名的变量。如下实例：

```bash
#!/usr/bin/python3

# 可写函数说明
def printinfo( arg1, *vartuple ):
   "打印任何传入的参数"
   print ("输出: ")
   print (arg1)
   for var in vartuple:
      print (var)
   return

# 调用printinfo 函数
printinfo( 10 )
printinfo( 70, 60, 50 )
```

以上实例输出结果：

```bash
输出:
10
输出:
70
60
50
```

还有一种就是参数带两个星号 **基本语法如下：

```bash
def functionname([formal_args,] **var_args_dict ):
   "函数_文档字符串"
   function_suite
   return [expression]
```

加了两个星号 \*\* 的参数会以**字典**的形式导入。

```bash
#!/usr/bin/python3
  
# 可写函数说明
def printinfo( arg1, **vardict ):
   "打印任何传入的参数"
   print ("输出: ")
   print (arg1)
   print (vardict)

# 调用printinfo 函数
printinfo(1, a=2,b=3)
```

以上实例输出结果：

```bash
输出:
1
{'a': 2, 'b': 3}
```

声明函数时，参数中星号 * 可以单独出现，例如:

```bash
def f(a,b,*,c):
    return a+b+c
```

如果单独出现星号 * 后的参数必须用关键字传入。

```bash
>>> def f(a,b,*,c):
...     return a+b+c
...
>>> f(1,2,3)   # 报错
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: f() takes 2 positional arguments but 3 were given
>>> f(1,2,c=3) # 正常
6
>>>
```

### 参数解构

参数解构和可变参数

+ 给函数提供实参的时候，可以在集合类型前使用\*或者\*\*，把集合类型的结构解开，提取出所有元素作为函数的实参
+ 非字典类型使用\*解构成位置参数
+ 字典类型使用\*\*解构成关键字参数
+ 提取出的元素数目要和参数的要求匹配，也要和参数的类型匹配

```bash
def add(x,*y):
    print(x,y,sep=",")

t = (4,5)
add(t[0],t[1])
add(*t)
add(*(4,5))
add(*{4,6,7,8,9,2})
add(*range(1,3))
--------------------------
4,(5,)
4,(5,)
4,(5,)
2,(4, 6, 7, 8, 9)
1,(2,)
```

+ 通过对比可以透析参数解构的用法
+ add(*{4,6,7,8,9,2}) 这个参数解构比较特殊，集合解构无序

```bash
def add(x,y):
    print(x,y,sep=",")

add(**{'x':5,'y':6}) # 解构成 x=5,y=6 作为关键字参数传递，正常运行
add(**{'a':5,'b':6}) # 解构成 a=5,b=6 作为关键字参数传递，不匹配形参，异常
add(*{'a':5,'b':6}) # 解构成 a,b 作为位置参数传递，正常运行
```

## 匿名函数

python 使用 lambda 来创建匿名函数。

所谓匿名，意即不再使用 def 语句这样标准的形式定义一个函数。

+ lambda 只是一个表达式，函数体比 def 简单很多。
+ lambda的主体是一个表达式，而不是一个代码块。仅仅能在lambda表达式中封装有限的逻辑进去。
+ lambda 函数拥有自己的命名空间，且不能访问自己参数列表之外或全局命名空间里的参数。
+ 虽然lambda函数看起来只能写一行，却不等同于C或C++的内联函数，后者的目的是调用小函数时不占用栈内存从而增加运行效率。
+ 不需要使用return，表达式的值，就是匿名函数返回值

**语法:**

lambda 函数的语法只包含一个语句

格式

```bash
lambda 参数列表：表达式
lambda [arg1 [,arg2,.....argn]]:expression
```

定义一个匿名函数

```bash
lambda x:x**2
```

调用

```bash
(lambda x:x**2)(4)
```

+ 不推荐 foo = lambda x,y:(x+y)**2，foo(4,5)；如果这样写还不如 def foo(x,y)，匿名函数就优秀在匿名二字

实例分析：

```bash
print(1,"-->",(lambda :0)())
print(2,"-->",(lambda x,y=3:x+y)(5))
print(3,"-->",(lambda x,y=3:x+y)(5,6))
print(4,"-->",(lambda x,*,y=30:x+y)(5))
print(5,"-->",(lambda x,*,y=30:x+y)(5,y=10))
print(6,"-->",(lambda *args:(x for x in args))(*range(5)))
print(7,"-->",(lambda *args:[x+1 for x in args])(*range(5)))
print(8,"-->",(lambda *args:{x+2 for x in args})(*range(5)))
------------------------------------------------------------
1 --> 0
2 --> 8
3 --> 11
4 --> 35
5 --> 15
6 --> <generator object <lambda>.<locals>.<genexpr> at 0x000001D06D19ABA0>
7 --> [1, 2, 3, 4, 5]
8 --> {2, 3, 4, 5, 6}
```

+ 注意第6行的返回值是一个生成器对象

```bash
[x for x in (lambda *args:map(lambda x:x+1,args))(*range(5))]
[x for x in (lambda *args:map(lambda x:(x+1,args),args))(*range(5))]
```

+ lambda 在高阶函数中应用得非常广泛，第一个 lambda 获取 (*range(5)) 参数，第二个 lambda 作为 map 函数的 func，来修改第一个 lambda 获取到的参数

## return语句

**return [表达式]** 语句用于退出函数，选择性地向调用方返回一个表达式。不带参数值的return语句返回None。之前的例子都没有示范如何返回数值，以下实例演示了 return 语句的用法：

```bash
#!/usr/bin/python3

# 可写函数说明
def sum( arg1, arg2 ):
   # 返回2个参数的和."
   total = arg1 + arg2
   print ("函数内 : ", total)
   return total

# 调用sum函数
total = sum( 10, 20 )
print ("函数外 : ", total)
```

以上实例输出结果：

```bash
函数内 :  30
函数外 :  30
```

```bash
def fn(x):
    for i in range(x):
        if i > 3:
            return i
    else:
        print("{} is not greater than 3".format(x))
print(fn(6))
print(fn(3))
---------------------------------------------------
4
3 is not greater than 3
None
```

+ 结合 for...else 的概念很容易理解，return 语句执行后，函数立即结束

> 函数返回值小结

+ Python 函数使用 return 语句返回“返回值”
+ 所有函数都有返回值，如果没有 return 语句，隐式调用 return None
+ return 语句并不一定是函数的语句块的最后一条语句
+ 一个函数可以存在多个 return 语句，但是只有一条可以被执行。如果没有一条 return 语句被执行到，隐式调用 return None
+ 如果有必要，可以显示调用 return None，可以简写为 return
+ 如果函数执行了 return 语句，函数就会返回，当前被执行的 return 语句之后的其它语句就不会被执行了
+ 作用：结束函数调用、返回值
+ 函数不能同时返回多个值
+ return [1, 3, 5] 是指明返回一个列表，是一个列表对象
+ return 1, 3, 5 看似返回多个值，隐式的被 python 封装成了一个元组，用 x, y, z = showlist() 解构提取更为方便

## 变量作用域

Python 中，程序的变量并不是在哪个位置都可以访问的，访问权限决定于这个变量是在哪里赋值的。

变量的作用域决定了在哪一部分程序可以访问哪个特定的变量名称。Python的作用域一共有4种，分别是：

+ L （Local） 局部作用域
+ E （Enclosing） 闭包函数外的函数中
+ G （Global） 全局作用域
+ B （Built-in） 内置作用域（内置函数所在模块的范围）

以 L –> E –> G –>B 的规则查找，即：在局部找不到，便会去局部外的局部找（例如闭包），再找不到就会去全局找，再者去内置中找。

```bash
g_count = 0  # 全局作用域
def outer():
    o_count = 1  # 闭包函数外的函数中
    def inner():
        i_count = 2  # 局部作用域
```

内置作用域是通过一个名为 builtin 的标准模块来实现的，但是这个变量名自身并没有放入内置作用域内，所以必须导入这个文件才能够使用它。在Python3.0中，可以使用以下的代码来查看到底预定义了哪些变量:

```bash
>>> import builtins
>>> dir(builtins)
```

Python 中只有模块（module），类（class）以及函数（def、lambda）才会引入新的作用域，其它的代码块（如 if/elif/else/、try/except、for/while等）是不会引入新的作用域的，也就是说这些语句内定义的变量，外部也可以访问，如下代码：

```bash
>>> if True:
...  msg = 'I am from Runoob'
...
>>> msg
'I am from Runoob'
>>>
```

实例中 msg 变量定义在 if 语句块中，但外部还是可以访问的。

如果将 msg 定义在函数中，则它就是局部变量，外部不能访问：

```bash
>>> def test():
...     msg_inner = 'I am from Runoob'
...
>>> msg_inner
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'msg_inner' is not defined
>>>
```

从报错的信息上看，说明了 msg_inner 未定义，无法使用，因为它是局部变量，只有在函数内可以使用。

### 全局变量和局部变量

定义在函数内部的变量拥有一个局部作用域，定义在函数外的拥有全局作用域。

局部变量只能在其被声明的函数内部访问，而全局变量可以在整个程序范围内访问。调用函数时，所有在函数内声明的变量名称都将被加入到作用域中。如下实例：

```bash
#!/usr/bin/python3

total = 0 # 这是一个全局变量
# 可写函数说明
def sum( arg1, arg2 ):
    #返回2个参数的和."
    total = arg1 + arg2 # total在这里是局部变量.
    print ("函数内是局部变量 : ", total)
    return total

#调用sum函数
sum( 10, 20 )
print ("函数外是全局变量 : ", total)
```

以上实例输出结果：

```bash
函数内是局部变量 :  30
函数外是全局变量 :  0
```

> 嵌套函数结构变量分析

```bash
def outer1():
    o = 65
    def inner():
        print("inner{}".format(o),chr(o),sep="   ")
    print("outer{}".format(o))
    inner()
outer1()
输出：
outer65
inner65   A
====================================================
def outer2():
    o = 65
    def inner():
        o = 97
        print("inner{}".format(o),chr(o),sep="   ")
    print("outer{}".format(o))
    inner()
outer2()
输出：
outer65
inner97   a
```

+ 外层变量作用域在内层作用域可见
+ 内层作用域inner中，如果定义了o=97，相当于当前作用域中重新定义了一个新的变量o，但是这个o并没有覆盖外层作用域outer中的o

```bash
x = 5
def foo():
    y = x + 1 # 报错吗
    x += 1 # 报错，报什么错？为什么？换成x=1还有错吗？
    print(x) # 为什么它不报错
foo()
```

+ x += 1 其实是 x = x + 1
+ 一旦出现 x=，相当于在foo内部定义一个局部变量x，那么foo内部所有x都是这个局部变量x了
+ 但是这个 x 还没有完成赋值，就被右边拿来做加 1 操作了
+ 如何解决这个问题？引入下面的 global 和 nonlocal 概念

### global 和 nonlocal关键字

当内部作用域想修改外部作用域的变量时，就要用到global和nonlocal关键字了。

```bash
x = 5
def foo():
    global x
    x += 1
print(x)
foo()
print(x)
输出：
5
6
```

+ 使用 global 关键字的变量，将 foo 内的 x 声明为使用外部的全局作用域中定义的 x
+ 全局作用域中必须有 x 的定义
+ 如果全局作用域中没有x定义会怎样？

```bash
# x = 5
def foo():
    global x
    x = 10
    x += 1 # 不会出错
    print(x)
# print(x) # 会报错
foo()
print(x) # 不会报错
```

+ 使用 global 关键字的变量，将 foo 内的 x 声明为使用外部的全局作用域中定义的 x
+ 但是，x = 10 赋值即定义，在内部作用域为一个外部作用域的变量 x 赋值，**不是在内部作用域定义一个新变量**，所以 x+=1 不会报错。注意，这里 x 的作用域还是全局的

> global 总结

+ x+=1这种是特殊形式产生的错误的原因？
  + 先引用后赋值，而 python 动态语言是赋值才算定义，才能被引用。解决办法，在这条语句前增加 x=0 之类的赋值语句，或者使用 global 告诉内部作用域，去全局作用域查找变量定义
  + 内部作用域使用x = 5之类的赋值语句会重新定义局部作用域使用的变量x，但是，一旦这个作用域中使用global声明x为全局的，那么x=5相当于在为全局作用域的变量x赋值
+ global使用原则
  + 外部作用域变量会内部作用域可见，但也不要在这个内部的局部作用域中直接使用，因为函数的目的就是为了封装，尽量与外界隔离
  + 如果函数需要使用外部全局变量，请使用**函数的形参传参**解决
  + 一句话：**不用global**。学习它就是为了深入理解变量作用域

**闭包**的两个概念

+ 自由变量：未在本地作用域中定义的变量。例如定义在内层函数外的外层函数的作用域中的变量
+ 闭包：就是一个概念，出现在嵌套函数中，指的是内层函数引用到了外层函数的自由变量，就形成了闭包。很多语言都有这个概念，最熟悉就是JavaScript

```bash
def counter():
    c = [0]
    def inc():
        c[0] += 1 # 报错吗？为什么？
        return c[0]
    return inc
foo = counter()
print(foo(),foo())
c = 100
print(foo())
输出：
1 2
3
```

+ 第 4 行会报错吗？为什么？
  + 不会报错，c 已经在 counter 函数中定义过了。而且 inc 中的使用方式是为 c 的元素修改值，而不是重新定义变量
+ 第 8 行打印什么结果？
  + 打印 1 2
+ 第 10 行打印什么结果？
  + 打印 3
  + 第 9 行的 c 和 counter 中的 c 不一样，而 inc 引用的是自由变量正是 counter 的变量 c
+ 这是 python2 中实现闭包的方式， python3 还可以使用 nonlocal 关键字

```bash
def counter():
    count = 0
    def inc():
        count += 1
        return count
    return inc
foo = counter()
foo()
foo()
```

+ 第 4 行会报错吗？怎么解决？
  + 使用global可以解决，但是这使用的是全局变量，而不是闭包
  + 如果要对普通变量闭包，Python3中可以使用nonlocal
  + 使用了nonlocal关键字，将变量标记为不在本地作用域定义，而在**上级的某一级**局部作用域中定义，但**不能是全局作用域**中定义

```bash
代码1：
def counter():
    count = 0
    def inc():
        nonlocal count
        count += 1
        return count
    return inc
foo = counter()
foo()
foo()
===========================
代码2：
a = 50
def counter():
    nonlocal a
    a += 1
    print(a)
    counter = 0
    def inc():
        nonlocal count
        count += 1
        return count
    return inc

foo = counter()
foo()
foo()
```

+ count 是外层函数的局部变量，被内部函数引用
+ 内部函数使用nonlocal关键字声明count变量在上级作用域而非本地作用域中定义
+ 代码 1 可以正常使用，且形成闭包
+ 代码 2 不能正常运行，变量 a 不能在全局作用域中

### 默认值的作用域

默认值举例

```bash
def foo(xyz=[]):
    xyz.append(1)
    print(xyz)
foo() # [1]
foo() # [1,1]
print(xyz) # NameError，当前作用域没有 xyz 变量
```

+ 为什么第二次调用foo函数打印的是[1,1]？
  + 因为**函数**也是**对象**，python把函数的默认值放在了属性中，这个属性就伴随着这个函数对象的整个生命周期
  + 查看 foo.\_\_defaults\_\_ 属性

引用类型举例

```bash
def foo(xyz=[],u="abc",z=123):
    xyz.append(1)
    return xyz
print(foo(),id(foo))
print(foo.__defaults__)
print(foo(),id(foo))
print(foo.__defaults__)
输出：
[1] 1830991902512
([1], 'abc', 123)
[1, 1] 1830991902512
([1, 1], 'abc', 123)
```

+ 函数地址并没有变，就是说函数这个对象的没有变，调用它，它的属性\_\_defaults\_\_中使用元组保存默认值
+ xyz默认值是引用类型，引用类型的元素变动，并不是元组的变化

非引用类型举例

```bash
def foo(w,u="abc",z=123):
    u = "xyz"
    z = 789
    print(w,u,z)
print(foo.__defaults__)
foo('greatwall')
print(foo.__defaults__)
输出：
('abc', 123)
greatwall xyz 789
('abc', 123)
```

+ 属性\_\_defaults\_\_中使用元组保存所有位置参数默认值，它不会因为在函数体内使用了它而发生改变

混合参数类型举例

```bash
def foo(w,u="abc",*,z=123,zz=[456]):
    u = "xyz"
    z = 789
    zz.append(1)
    print(w,u,z,zz)
print(foo.__defaults__)
foo("greatwall")
print(foo.__kwdefaults__)
输出：
('abc',)
greatwall xyz 789 [456, 1]
{'zz': [456, 1], 'z': 123}
```

+ 属性\_\_defaults\_\_中使用元组保存所有位置参数默认值
+ 属性\_\_kwdefaults\_\_中使用字典保存所有keyword-only参数的默认值

默认值作用域说明

+ 使用可变类型作为默认值，就可能修改这个默认值
+ 有时候这个特性是好的，有的时候这种特性是不好的，有副作用
+ 如何做到按需改变呢？看下面的2种方法

```bash
def foo(xyz=[],u="abc",z=123):
    xyz = xyz[:] # 影子拷贝
    xyz.append(1)
    print(xyz)
foo()
print(foo.__defaults__)
foo()
print(foo.__defaults__)
foo([10])
print(foo.__defaults__)
foo([10,5])
print(foo.__defaults__)
输出：
[1]
([], 'abc', 123)
[1]
([], 'abc', 123)
[10, 1]
([], 'abc', 123)
[10, 5, 1]
([], 'abc', 123)
```

+ 函数体内，不改变默认值
  + xyz都是传入参数或者默认参数的副本，如果就想修改原参数，无能为力

```bash
def foo(xyz=None,u="abc",z=132):
    if xyz is None:
        xyz = []
    xyz.append(1)
    print(xyz)
foo()
print(foo.__defaults__)
foo()
print(foo.__defaults__)
foo([10])
print(foo.__defaults__)
foo([10,5])
print(foo.__defaults__)
输出：
[1]
(None, 'abc', 132)
[1]
(None, 'abc', 132)
[10, 1]
(None, 'abc', 132)
[10, 5, 1]
(None, 'abc', 132)
```

+ 使用不可变类型默认值
  + 如果使用缺省值None就创建一个列表
  + 如果传入一个列表，就修改这个列表

> 小结

+ 第一种方法
  + 使用影子拷贝创建一个新的对象，永远不能改变传入的参数
+ 第二种方法
  + 通过值的判断就可以灵活的选择创建或者修改传入对象
  + 这种方式灵活，应用广泛
  + 很多函数的定义，都可以看到使用None这个不可变的值作为默认参数，可以说这是一种惯用法

## 函数的销毁

全局函数销毁

+ 重新定义同名函数
+ del 语句删除函数对象
+ 程序结束时

 局部函数销毁

+ 重新在上级作用域定义同名函数
+ del 语句删除函数名称，函数对象的引用计数减1
+ 上级作用域销毁时

## 思考

上面描述了很多有关函数的根本概念，接下来，思考一些不一样的场景

```bash
def f(x,y,z):
    pass

f(z=None,y=10,x=[1])
f((1,),z=6,y=4.1)
f(y=5,z=6,2)
```

+ f(y=5,z=6,2) 这种表达方式会报错，要求位置参数必须在关键字参数之前传入，位置参数是按位置对应的

```bash
def add(x=4,y=5):
    return x+y

print(add(6,10))
print(add(6,y=7))
print(add(x=5))
print(add())
print(add(y=7))
print(add(x=5,y=6))
print(add(y=5,x=6))
---------------------
16
13
10
9
11
11
11
```

+ 参数的默认值可以在未传入足够的实参的时候，对没有给定的参数赋值为默认值
+ 参数非常多的时候，并不需要每次都输入所有的参数，简化函数调用

```bash
def add(x=4,y=5):
    return x+y

print(add(x=5,6))
print(add(y=8,4))
---------------------
 File "<ipython-input-9-01c6d3f4eebe>", line 1
    print(add(x=5,6))
                 ^
SyntaxError: positional argument follows keyword argument
```

+ 异常中已经说的很清楚，有位置参数，必须要放在关键字参数前面

```bash
def add (nums):
    sum = 0
    for x in nums:
        sum += x
    return sum

print(add([1,2,3,4]))
print(add((2,4,6)))
print(add("abcd"))
```

+ print(add("abcd")) 会报错
+ 虽然传入的参数都是可迭代对象，但是函数内部的变量类型与传入参数的变量类型可能不一致

```bash
def showconfig(username,password,**kwargs):
    pass
def showconfig(username,*args,**kwargs):
    pass
-----------------------------------------------------
def showconfig(username,password,**kwargs,*args):
    pass
  File "<ipython-input-23-888fb4d83337>", line 1
    def showconfig(username,password,**kwargs,*args):
                                             ^
SyntaxError: invalid syntax
```

+ 上面两个是正确的定义方式，最后一个报错了，对比就可以看出来，要遵守语法规范。

**小结**：可变参数混合使用的时候，可以参考下面的规范总结

+ 有位置可变参数和关键字可变参数
+ 位置可变参数在形参前使用一个星号*
+ 关键字可变参数在形参前使用两个星号**
+ 位置可变参数和关键字可变参数都可以收集若干个实参，位置可变参数收集形成一个tuple，关键字可变参数收集形成一个dict
+ 混合使用参数的时候，可变参数要放到参数列表的最后，普通参数需要放到参数列表前面，位置可变参数需要在关键字可变参数之前

```bash
def fn(x, y, *args, **kwargs):
    print(x)
    print(y)
    print(args)
    print(kwargs)

fn(3,5,7,9,10,a=1,b='python')
fn(3,5)
fn(3,5,7)
fn(3,5,a=1,b='python')
fn(7,9,y=5,x=3,a=1,b='python') # 错误，7和9分别赋给了x，y，又y=5、x=3，重复了
```

```bash
def fn(*args):
    print(args)

fn(a=1,b=2)
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-29-7640750972b9> in <module>
      2     print(args)
      3
----> 4 fn(a=1,b=2)

TypeError: fn() got an unexpected keyword argument 'a'
```

+ 传入关键字参数，要么与参数列表中的形参对应，要么归于\*\*kwargs，不能归于\*args

```bash
def fn(*args,x,y,**kwargs):
    print(x)
    print(y)
    print(args)
    print(kwargs)

fn(3,5) #出错
fn(3,5,7) #出错
fn(3,5,a=1,b='python') #出错
fn(7,9,y=5,x=3,a=1,b='python')
```

+ 只有 fn(7,9,y=5,x=3,a=1,b='python') 是可以正常运行的，x 和 y 必须用关键字参数了，而且给 x 和 y 赋值先满足 x 和 y，而不是被 \*\*kwargs 捕获
+ 前三个报错"TypeError: fn() missing 2 required keyword-only arguments: 'x' and 'y'"
+ 这种表达方式引入了一个新的概念 keyword-only 参数

**keyword-only** 参数(Python3加入)

如果在一个星号参数后，或者一个位置可变参数后，出现的普通参数，实际上已经不是普通的参数了，而是keyword-only参数

```bash
def fn(*args,x):
    print(x)
    print(args)
fn(3,5,x=7)
```

+ args可以看做已经截获了所有的位置参数，x不使用关键字参数就不可能拿到实参

```bash
def fn(**kwargs,x):
    print(x)
    print(kwargs)
```

+ 直接报语法错误
+ 可以理解为kwargs会截获所有的关键字参数，就算你写了x=5，x也永远得不到这个值，所以语法错误

```bash
def fn(*,x,y):
    print(x,y)
fn(x=5,y=6)
```

+ \*号之后，普通形参都变成了必须给出的 keyword-only 参数

> 参数规则

+ 参数列表参数一般顺序是，普通参数、缺省参数、可变位置参数、keyword-only参数(可带缺省值)、可变关键字参数

```bash
def fn(x,y,z=3,*args,m=4,n,**kwargs):
    print(x,y,z,m,n)
    print(args)
    print(kwargs)
fn(4,5,7,14,15,17,m=24,n=25,a=1,b=2)
-------------------------------------
4 5 7 24 25
(14, 15, 17)
{'a': 1, 'b': 2}
```

> 注意

+ 代码应该易读易懂，而不是为难别人
+ 请按照书写习惯定义函数参数

## 举列

编写一个函数，能够至少接受 2 个参数，返回最大值和最不值

```bash
import random
def double_values(*nums):
    print(nums)
    return max(nums),min(nums)
print(*double_values(*[random.randint(10,20) for _ in range(10)]))
------------------------------------------------------------------
(16, 20, 10, 16, 20, 17, 17, 17, 13, 14)
20 10
```

编写一个函数，接受一个参数 n，n 为正整数，左右两种打印方式

![数阵](D:\github\python\basic_grammar\images\数阵.png)

```bash
def show(n):
    tail = " ".join([str(i) for i in range(n,0,-1)])
    length = len(tail)
    for i in range(1,n):
        print("{:>{}}".format(" ".join([str(j) for j in range(i,0,-1)]),length))
    print(tail)
show(12)
```

```bash
def show(n):
    tail = " ".join([str(i) for i in range(n,0,-1)])
    length = len(tail)
    print(tail)
    for i in range(n-1,0,-1):
        print("{:>{}}".format(" ".join([str(j) for j in range(i,0,-1)]),length))
show(12)
```
