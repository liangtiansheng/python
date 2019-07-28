# Python3 正则表达式

正则表达式是一个特殊的字符序列，它能帮助你方便的检查一个字符串是否与某种模式匹配。

Python 自1.5版本起增加了re 模块，它提供 Perl 风格的正则表达式模式。

re 模块使 Python 语言拥有全部的正则表达式功能。

compile 函数根据一个模式字符串和可选的标志参数生成一个正则表达式对象。该对象拥有一系列方法用于正则表达式匹配和替换。

re 模块也提供了与这些方法功能完全一致的函数，这些函数使用一个模式字符串做为它们的第一个参数。

本章节主要介绍 Python 中常用的正则表达式处理函数，如果你对正则表达式不了解，可以查看我们的 [正则表达式 - 教程](https://www.runoob.com/regexp/regexp-tutorial.html)。

## re.match函数

re.match 尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回none。

函数语法：

```bash
re.match(pattern, string, flags=0)
```

函数参数说明：

参数|    描述
:-|:-
pattern|    匹配的正则表达式
string|    要匹配的字符串。
flags|    标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。参见：正则表达式修饰符 - 可选标志

匹配成功re.match方法返回一个匹配的对象，否则返回None。

我们可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式。

匹配对象方法|    描述
:-|:-
group(num=0)|    匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。
groups()|    返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。

```bash
#!/usr/bin/python

import re
print(re.match('www', 'www.runoob.com').span())  # 在起始位置匹配
print(re.match('com', 'www.runoob.com'))         # 不在起始位置匹配
```

以上实例运行输出结果为：

```bash
(0, 3)
None
```

```bash
#!/usr/bin/python3
import re

line = "Cats are smarter than dogs"
# .* 表示任意匹配除换行符（\n、\r）之外的任何单个或多个字符
matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I)

if matchObj:
   print ("matchObj.group() : ", matchObj.group())
   print ("matchObj.group(1) : ", matchObj.group(1))
   print ("matchObj.group(2) : ", matchObj.group(2))
else:
   print ("No match!!")
```

以上实例执行结果如下：

```bash
matchObj.group() :  Cats are smarter than dogs
matchObj.group(1) :  Cats
matchObj.group(2) :  smarter
```

## re.search方法

re.search 扫描整个字符串并返回第一个成功的匹配。

函数语法：

```bash
re.search(pattern, string, flags=0)
```

函数参数说明：

参数|    描述
:-|:-
pattern|    匹配的正则表达式
string|    要匹配的字符串。
flags|    标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。参见：正则表达式修饰符 - 可选标志

匹配成功re.search方法返回一个匹配的对象，否则返回None。

我们可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式。

匹配对象方法|    描述
:-|:-
group(num=0)|    匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。
groups()|    返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。

```bash
#!/usr/bin/python3

import re

print(re.search('www', 'www.runoob.com').span())  # 在起始位置匹配
print(re.search('com', 'www.runoob.com').span())         # 不在起始位置匹配
```

以上实例运行输出结果为：

```bash
(0, 3)
(11, 14)
```

```bash
#!/usr/bin/python3

import re

line = "Cats are smarter than dogs";

searchObj = re.search( r'(.*) are (.*?) .*', line, re.M|re.I)

if searchObj:
   print ("searchObj.group() : ", searchObj.group())
   print ("searchObj.group(1) : ", searchObj.group(1))
   print ("searchObj.group(2) : ", searchObj.group(2))
else:
   print ("Nothing found!!")
```

以上实例执行结果如下：

```bash
searchObj.group() :  Cats are smarter than dogs
searchObj.group(1) :  Cats
searchObj.group(2) :  smarter
```

## re.match与re.search的区别

re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。

```bash
#!/usr/bin/python3

import re

line = "Cats are smarter than dogs";

matchObj = re.match( r'dogs', line, re.M|re.I)
if matchObj:
   print ("match --> matchObj.group() : ", matchObj.group())
else:
   print ("No match!!")

matchObj = re.search( r'dogs', line, re.M|re.I)
if matchObj:
   print ("search --> matchObj.group() : ", matchObj.group())
else:
   print ("No match!!")
```

以上实例运行结果如下：

```bash
No match!!
search --> matchObj.group() :  dogs
```

## 检索和替换

Python 的re模块提供了re.sub用于替换字符串中的匹配项。

语法：

```bash
re.sub(pattern, repl, string, count=0, flags=0)
```

参数：

+ pattern : 正则中的模式字符串。
+ repl : 替换的字符串，也可为一个函数。
+ string : 要被查找替换的原始字符串。
+ count : 模式匹配后替换的最大次数，默认 0 表示替换所有的匹配。
+ flags : 编译时用的匹配模式，数字形式。

前三个为必选参数，后两个为可选参数。

```bash
#!/usr/bin/python3
import re

phone = "2004-959-559 # 这是一个电话号码"

# 删除注释
num = re.sub(r'#.*$', "", phone)
print ("电话号码 : ", num)

# 移除非数字的内容
num = re.sub(r'\D', "", phone)
print ("电话号码 : ", num)
```

以上实例执行结果如下：

```bash
电话号码 :  2004-959-559
电话号码 :  2004959559
```

### repl 参数是一个函数

以下实例中将字符串中的匹配的数字乘于 2：

```bash
#!/usr/bin/python

import re

# 将匹配的数字乘于 2
def double(matched):
    value = int(matched.group('value'))
    return str(value * 2)

s = 'A23G4HFD567'
print(re.sub('(?P<value>\d+)', double, s))
```

执行输出结果为：

```bash
A46G8HFD1134
```

### compile 函数

compile 函数用于编译正则表达式，生成一个正则表达式（ Pattern ）对象，供 match() 和 search() 这两个函数使用。

语法格式为：

```bash
re.compile(pattern[, flags])
```

参数：

+ pattern : 一个字符串形式的正则表达式
+ flags 可选，表示匹配模式，比如忽略大小写，多行模式等，具体参数为：
+ re.I 忽略大小写
  + re.L 表示特殊字符集 \w, \W, \b, \B, \s, \S 依赖于当前环境
  + re.M 多行模式
  + re.S 即为' . '并且包括换行符在内的任意字符（' . '不包括换行符）
  + re.U 表示特殊字符集 \w, \W, \b, \B, \d, \D, \s, \S 依赖于 Unicode 字符属性数据库
  + re.X 为了增加可读性，忽略空格和' # '后面的注释