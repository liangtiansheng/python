# Python3 基本数据类型

+ Python 中的变量不需要声明。每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建。
+ 在 Python 中，变量就是变量，它没有类型，我们所说的"类型"是变量所指的内存中对象的类型。
+ 等号（=）用来给变量赋值。
+ 等号（=）运算符左边是一个变量名,等号（=）运算符右边是存储在变量中的值。例如：

```bash
#!/usr/bin/python3

counter = 100          # 整型变量
miles   = 1000.0       # 浮点型变量
name    = "runoob"     # 字符串

print (counter)
print (miles)
print (name)
```

执行以上程序会输出如下结果：

```bash
100
1000.0
runoob
```

Python允许你同时为多个变量赋值。例如：

```bash
a = b = c = 1
```

以上实例，创建一个整型对象，值为 1，从后向前赋值，三个变量被赋予相同的数值。

您也可以为多个对象指定多个变量。例如：

```bash
a, b, c = 1, 2, "runoob"
```

以上实例，两个整型对象 1 和 2 分别给变量 a 和 b，字符串对象 "runoob" 给变量 c。

## 标准数据类型

Python3 中有六个标准的数据类型：

+ Number（数字）
+ String（字符串）
+ List（列表）
+ Tuple（元组）
+ Set（集合）
+ Dictionary（字典）

Python3 的六个标准数据类型中：

+ 不可变数据（3 个）：Number（数字）、String（字符串）、Tuple（元组）；
+ 可变数据（3 个）：List（列表）、Dictionary（字典）、Set（集合）。

## Number（数字）

+ Python3 支持 int、float、bool、complex（复数）。
+ 在Python 3里，只有一种整数类型 int，表示为长整型，没有 python2 中的 Long。
+ 像大多数语言一样，数值类型的赋值和计算都是很直观的。
+ 内置的 type() 函数可以用来查询变量所指的对象类型。

```bash
>>> a, b, c, d = 20, 5.5, True, 4+3j
>>> print(type(a), type(b), type(c), type(d))
<class 'int'> <class 'float'> <class 'bool'> <class 'complex'>
```

此外还可以用 isinstance 来判断：

```bash
>>>a = 111
>>> isinstance(a, int)
True
>>>
```

isinstance 和 type 的区别在于：

+ type()不会认为子类是一种父类类型。
+ isinstance()会认为子类是一种父类类型。

```bash
>>> class A:
...     pass
...
>>> class B(A):
...     pass
...
>>> isinstance(A(), A)
True
>>> type(A()) == A
True
>>> isinstance(B(), A)
True
>>> type(B()) == A
False
```

_**注意**：在 Python2 中是没有布尔型的，它用数字 0 表示 False，用 1 表示 True。到 Python3 中，把 True 和 False 定义成关键字了，但它们的值还是 1 和 0，它们可以和数字相加。_

当你指定一个值时，Number 对象就会被创建：

```bash
var1 = 1
var2 = 10
```

您也可以使用del语句删除一些对象引用。

del语句的语法是：

```bash
del var1[,var2[,var3[....,varN]]]
```

您可以通过使用del语句删除单个或多个对象。例如：

```bash
del var
del var_a, var_b
```

## 数值运算

```bash
>>>5 + 4  # 加法
9
>>> 4.3 - 2 # 减法
2.3
>>> 3 * 7  # 乘法
21
>>> 2 / 4  # 除法，得到一个浮点数
0.5
>>> 2 // 4 # 除法，得到一个整数
0
>>> 17 % 3 # 取余
2
>>> 2 ** 5 # 乘方
32
```

**注意：**

1. Python可以同时为多个变量赋值，如a, b = 1, 2。
2. 一个变量可以通过赋值指向不同类型的对象。
3. 数值的除法包含两个运算符：/ 返回一个浮点数，// 返回一个整数。
4. 在混合计算时，Python会把整型转换成为浮点数。

## 数值类型实例

int	| float	| complex
----|-------|--------
10|0.0|3.14j
100|15.20|45.j
-786|-21.9|9.322e-36j
080|32.3e+18|.876j
-0490|-90.|-.6545+0J
-0x260|-32.54e100|3e+26J
0x69|70.2E-12|4.53e-7j