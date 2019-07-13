# Python3 字符串

字符串是 Python 中最常用的数据类型。我们可以使用引号( ' 或 " )来创建字符串。

创建字符串很简单，只要为变量分配一个值即可。例如：

```bash
var1 = 'Hello World!'
var2 = "Runoob"
```

## Python 访问字符串中的值

Python 不支持单字符类型，单字符在 Python 中也是作为一个字符串使用。

Python 访问子字符串，可以使用方括号来截取字符串，如下实例：

```bash
#!/usr/bin/python3

var1 = 'Hello World!'
var2 = "Runoob"

print ("var1[0]: ", var1[0])
print ("var2[1:5]: ", var2[1:5])
```

以上实例执行结果：

```bash
var1[0]:  H
var2[1:5]:  unoo
```

## Python 字符串更新

你可以截取字符串的一部分并与其他字段拼接，如下实例：

```bash
#!/usr/bin/python3

var1 = 'Hello World!'

print ("已更新字符串 : ", var1[:6] + 'Runoob!')
```

以上实例执行结果

```bash
已更新字符串 :  Hello Runoob!
```

## Python转义字符

在需要在字符中使用特殊字符时，python用反斜杠(\)转义字符。如下表：

转义字符|    描述
:-|:-
\\(在行尾时)|    续行符
\\\\|    反斜杠符号
\\'|    单引号
\\"|    双引号
\\a|    响铃
\\b|    退格(Backspace)
\\000|    空
\\n|    换行
\\v|    纵向制表符
\\t|    横向制表符
\\r|    回车
\\f|    换页
\\oyy|    八进制数，yy代表的字符，例如：\o12代表换行
\\xyy|    十六进制数，yy代表的字符，例如：\x0a代表换行
\\other|    其它的字符以普通格式输出
