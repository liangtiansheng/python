# Python3 循环语句

本章节将为大家介绍Python循环语句的使用。

Python中的循环语句有 for 和 while。

Python循环语句的控制结构图如下所示：

![loop construction](images/循环结构体.png)

## while 循环

Python中while语句的一般形式：

```bash
while 判断条件：
    语句
```

执行 Gif 演示：

![while](images/while.gif)

同样需要注意冒号和缩进。另外，在 Python 中没有 do..while 循环。

以下实例使用了 while 来计算 1 到 100 的总和：

```bash
#!/usr/bin/env python3

n = 100

sum = 0
counter = 1
while counter <= n:
    sum = sum + counter
    counter += 1

print("1 到 %d 之和为: %d" % (n,sum))
```

执行结果如下：

```bash
1 到 100 之和为: 5050
```

### 无限循环

我们可以通过设置条件表达式永远不为 false 来实现无限循环，实例如下：

```bash
#!/usr/bin/python3

var = 1
while var == 1 :  # 表达式永远为 true
   num = int(input("输入一个数字  :"))
   print ("你输入的数字是: ", num)

print ("Good bye!")
```

执行以上脚本，输出结果如下：

```bash
输入一个数字  :5
你输入的数字是:  5
输入一个数字  :
```

你可以使用 CTRL+C 来退出当前的无限循环。

无限循环在服务器上客户端的实时请求非常有用。

### while 循环使用 else 语句

在 while … else 在条件语句为 false 时执行 else 的语句块：

```bash
#!/usr/bin/python3

count = 0
while count < 5:
   print (count, " 小于 5")
   count = count + 1
else:
   print (count, " 大于或等于 5")
```

执行以上脚本，输出结果如下：

```bash
0  小于 5
1  小于 5
2  小于 5
3  小于 5
4  小于 5
5  大于或等于 5
```

### 简单语句组

类似if语句的语法，如果你的while循环体中只有一条语句，你可以将该语句与while写在同一行中， 如下所示：

```bash
#!/usr/bin/python

flag = 1

while (flag): print ('欢迎访问菜鸟教程!')

print ("Good bye!")
```

**注意：**以上的无限循环你可以使用 CTRL+C 来中断循环。

执行以上脚本，输出结果如下：

```bash
欢迎访问菜鸟教程!
欢迎访问菜鸟教程!
欢迎访问菜鸟教程!
欢迎访问菜鸟教程!
欢迎访问菜鸟教程!
……
```

## for 语句

Python for循环可以遍历任何序列的项目，如一个列表或者一个字符串。

for循环的一般格式如下：

```bash
for <variable> in <sequence>:
    <statements>
else:
    <statements>
```

Python loop循环实例：

```bash
>>>languages = ["C", "C++", "Perl", "Python"]
>>> for x in languages:
...     print (x)
...
C
C++
Perl
Python
>>>
```

以下 for 实例中使用了 break 语句，break 语句用于跳出当前循环体：

```bash
#!/usr/bin/python3

sites = ["Baidu", "Google","Runoob","Taobao"]
for site in sites:
    if site == "Runoob":
        print("菜鸟教程!")
        break
    print("循环数据 " + site)
else:
    print("没有循环数据!")
print("完成循环!")
```

执行脚本后，在循环到 "Runoob"时会跳出循环体：

```bash
循环数据 Baidu
循环数据 Google
菜鸟教程!
完成循环!
```

## range()函数

如果你需要遍历数字序列，可以使用内置range()函数。它会生成数列，例如:

```bash
>>>for i in range(5):
...     print(i)
...
0
1
2
3
4
```

你也可以使用range指定区间的值：

```bash
>>>for i in range(5,9) :
    print(i)


5
6
7
8
>>>
```

也可以使range以指定数字开始并指定不同的增量(甚至可以是负数，有时这也叫做'步长'):

```bash
>>>for i in range(0, 10, 3) :
    print(i)


0
3
6
9
>>>
```

负数：

```bash
>>>for i in range(-10, -100, -30) :
    print(i)


-10
-40
-70
>>>
```

您可以结合range()和len()函数以遍历一个序列的索引,如下所示:

```bash
>>>a = ['Google', 'Baidu', 'Runoob', 'Taobao', 'QQ']
>>> for i in range(len(a)):
...     print(i, a[i])
...
0 Google
1 Baidu
2 Runoob
3 Taobao
4 QQ
>>>
```

还可以使用range()函数来创建一个列表：

```bash
>>>list(range(5))
[0, 1, 2, 3, 4]
>>>
```

## break和continue语句及循环中的else子句

break 语句可以跳出 for 和 while 的循环体。如果你从 for 或 while 循环中终止，任何对应的循环 else 块将不执行。 实例如下：

```bash
#!/usr/bin/python3

for letter in 'Runoob':     # 第一个实例
   if letter == 'b':
      break
   print ('当前字母为 :', letter)
  
var = 10                    # 第二个实例
while var > 0:
   print ('当期变量值为 :', var)
   var = var -1
   if var == 5:
      break

print ("Good bye!")
```

执行以上脚本输出结果为：

```bash
当前字母为 : R
当前字母为 : u
当前字母为 : n
当前字母为 : o
当前字母为 : o
当期变量值为 : 10
当期变量值为 : 9
当期变量值为 : 8
当期变量值为 : 7
当期变量值为 : 6
Good bye!
```

continue语句被用来告诉Python跳过当前循环块中的剩余语句，然后继续进行下一轮循环。

```bash
#!/usr/bin/python3

for letter in 'Runoob':     # 第一个实例
   if letter == 'o':        # 字母为 o 时跳过输出
      continue
   print ('当前字母 :', letter)

var = 10                    # 第二个实例
while var > 0:
   var = var -1
   if var == 5:             # 变量为 5 时跳过输出
      continue
   print ('当前变量值 :', var)
print ("Good bye!")
```

执行以上脚本输出结果为：

```bash
当前字母 : R
当前字母 : u
当前字母 : n
当前字母 : b
当前变量值 : 9
当前变量值 : 8
当前变量值 : 7
当前变量值 : 6
当前变量值 : 4
当前变量值 : 3
当前变量值 : 2
当前变量值 : 1
当前变量值 : 0
Good bye!
```

循环语句可以有 else 子句，它在穷尽列表(以for循环)或条件变为 false (以while循环)导致循环终止时被执行,但循环被break终止时不执行。

如下实例用于查询质数的循环例子:

```bash
#!/usr/bin/python3

for n in range(2, 10):
    for x in range(2, n):
        if n % x == 0:
            print(n, '等于', x, '*', n//x)
            break
    else:
        # 循环中没有找到元素
        print(n, ' 是质数')
```

执行以上脚本输出结果为：

```bash
2  是质数
3  是质数
4 等于 2 * 2
5  是质数
6 等于 2 * 3
7  是质数
8 等于 2 * 4
9 等于 3 * 3
```

## pass 语句

Python pass是空语句，是为了保持程序结构的完整性。

pass 不做任何事情，一般用做占位语句，如下实例

```bash
>>>while True:
...     pass  # 等待键盘中断 (Ctrl+C)
```

最小的类:

```bash
>>>class MyEmptyClass:
...     pass
```

以下实例在字母为 o 时 执行 pass 语句块:

```bash
#!/usr/bin/python3

for letter in 'Runoob':
   if letter == 'o':
      pass
      print ('执行 pass 块')
   print ('当前字母 :', letter)

print ("Good bye!")
```

执行以上脚本输出结果为：

```bash
当前字母 : R
当前字母 : u
当前字母 : n
执行 pass 块
当前字母 : o
执行 pass 块
当前字母 : o
当前字母 : b
Good bye!
```

## 举例

例1：打印10以内的偶数

```bash
for i in range(10):
    if i & 1:        # 与运算是最快的，0与1得0，1与1得1，二进制只有0和1
        continue
    print(i)
```

例2：计算1000以内的被7整除的前20个数(for循环)

```bash
count = 0
for i in range(0,1000,7):       # 思路有很多，步长为7最简单高效
    print(i)
    count += 1
    if count == 20:
        break
```

例3：输入一个整数，用整数处理的方式从高位到低位截取出来

```bash
# 迭代思想
num = int(input('please input a number: '))
digits = len(str(num))                      # 先算位数
times = digits - 1                          # 循环次数
while times >=0:
    output = num //(10**times)              # 截取高位
    print(output)
    num = num - output*(10**times)          # 算出剩余数用于迭代
    times -=1
```

例4：打印一个边长为n的正方形

```bash
方法1：制作前两行用作模板，后面复制
n = int(input('please input rhomboid side: '))
for i in range(n-1):
    Topside = "*\t"*(n-1)+"*"       # 制作第一行模板
    Midside = "*"+"\t"*(n-1)+"*"    # 制作第二行模板
    if i == 0:
        print(Topside)
        print("\n")
    else:
        print(Midside)
        print("\n")
else:
    print(Topside)

方法2：利用对称的思想
# 边长为3，则-1 0 1 => range(-1,2)
# 边长为4，则-2 -1 0 1 => range(-2,2)
# 边长为5，则-2 -1 0 1 2 => range(-2,3)
n = int(input('please input rhomboid side: '))
e = -n//2
for i in range(e,n+e):          # 此处只用作循环次数，真正的意义是用于算法当中进行对称处理
    if i == e or i == n+e-1:
        print("*"*n)
    else:
        print("*"+" "*(n-2)+"*")
```

例5：求1到5阶乘之和

```bash
方法一：累乘累加
# 更高效
a = 1
sum = 0
for i in range(1,6):
    a = i * a           # 累积乘上去就是每一个数的阶乘
    sum = sum + a       # 累加上去就是阶乘之和
print(sum)

方法二：单独阶乘累加
# 嵌套循环效率低
sum = 0
factorial = 1
for i in range(1,6):
    for j in range(1,i+1):
        factorial *= j      # 两层循环为了将每个数进行阶乘运算，思路清淅，但是效率很低
    sum += factorial
    factorial = 1
print(sum)
```

例6：给一个数，判断是否是素数

```bash
# 给定一个数n,若要两个数相乘等于n，那必然一个数大于等于根号n，另一个数小于等于根号n
n = int(input('please input one number: '))
for i in range(2,int(n**0.5)+1):
    if n % i == 0:
        print(n,'不是素数')
        break
else:
    print(n,"是素数")
```

例7：99乘法口诀

```bash
for i in range(1,10):
    for j in range(1,i+1):
        print('{} * {} = {:2d}'.format(j,i,i*j),end="   ")  # 直接从左向右打印
    print()

s = ""
for i in range(1,10):
    for j in range(i,10):
        s += '{} * {} = {:<{}}'.format(i, j, j*i, 2 if j<4 else 3) # 拼接成字符串，为的是右对齐
    print('{:>117}'.format(s))
    s = ""
```

例8：打印菱形

```bash
# 打印菱形
import sys
line = int(input("please input rhomboid lines: "))
if line < 3 or line % 2 == 0:
    print("your input number is not proper to construct rhomboid, now exited!")
    print("please input odd number larger than 2")
    sys.exit(7)
symmetry_left = -(line//2)
symmetry_right = line + symmetry_left
for i in range(symmetry_left,symmetry_right):   # 这里的对称思想用得就巧妙，将对称数用在算法中进行对称处理
    if i < 0:
        print(" "*(-i)+"*"*(line+2*i))  # 打印菱形重点就在于找出每一行空格和 * 个数的规律
    else:
        print(" "*i+"*"*(line-2*i))

# 打印闪电
import sys
line = int(input("please input flash lines: "))
if line < 3 or line % 2 == 0:
    print("your input number is not proper to construct flash, now exited!")
    print("please input odd integer larger than 2")
    sys.exit(7)
symmetry_left = -(line//2)
symmetry_right = line + symmetry_left
for i in range(symmetry_left,symmetry_right): # 依然利用对称的巧秒思想
    if i < 0:
        print(" "*(-i)+"*"*(symmetry_right+i)) # 不管打印什么，重点依然是找出每一行空格和 * 个数的规律
    elif i == 0:
        print("*"*line)
    else:
        print(" "*abs(symmetry_left)+"*"*(symmetry_right-i))
```

例9：求fib数列101项

```bash
a = 0
b = 1
for i in range(100):
    a, b = b, a+b       # python 特有的方式，封装与解构
else:
    print(b)
```

例10：求10万内的所有素数

```bash
方法1：
import datetime
start = datetime.datetime.now()
count = 1
for i in range(3,100000,2):
    for j in range(2,int(i**0.5)+1):    # 若要两个数相乘等于n，那必然一个数大于等于根号n，另一个数小于等于根号n
        if i % j == 0:
            break
    else:
        count += 1
print(count)
delta = (datetime.datetime.now()-start).total_seconds()
print(delta)

对上面的代码进行优化：
import datetime
start = datetime.datetime.now()
count = 1
for i in range(3,100000,2):         # 大于2的偶数都能被2整除，排除掉
    if i > 10 and i % 10 == 5:      # 大于10的个位数为5都可以被5整除，排除掉
        continue
    for j in range(3,int(i**0.5)+1,2):  # 奇数都不能被2整除，所以从3开始，步长为2是因为奇数不能被偶数整除
        if i % j == 0:
            break
    else:
        count += 1
print(count)
delta = (datetime.datetime.now()-start).total_seconds()
print(delta)
```
