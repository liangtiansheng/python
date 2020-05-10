# 文件高级用法

## CSV 文件简介

csv 文件的基本要义：

+ 逗号分隔值 Comma-Separated Values
+ csv 是一个被行分割符、列分割符划分成行和列的文本文件

+ csv 不指定字符编码
+ 行分隔符为 \r\n，最后一行可以没有换行符
+ 列分隔符常为逗号或者制表符
+ 每一行称为一条记录 record
+ 字段可以使用双引号括起来，也可以不使用；如果字段中出现了双引号、逗号、换行符必须使用双引号括起来；如果字段的值是双引号，使用两个双引号表示一个转义
+ 表头可选，和字段列对齐就行了

### 手动生成 csv 文件

```bash
from pathlib import Path

p = Path('d:/tmp/mycsv/test.csv')
parent = p.parent

if not parent.exists():
    parent.mkdir(parents=True)

csv_body = '''\
id,name,age,comment
1,zs,18,"I'm 18"
2,ls,20,"this is a ""test"" string."
3,ww,23,"你好

计算机
"
'''
p.write_text(csv_body)
```

### csv 模块

```bash
csv.reader(csvfile, dialect='excel', **fmtparams)
```

+ 这是 python 帮助文档里面的指示，源码很简陋，容易误导
+ 返回 DictReader 对象，是一个行迭代器

通过帮助文档，可得简单的使用方法

```bash
import csv
with open('eggs.csv', newline='') as csvfile:
    spamreader = csv.reader(csvfile, delimiter=' ', quotechar='|')
    for row in spamreader:
        print(', '.join(row))
Spam, Spam, Spam, Spam, Spam, Baked Beans
Spam, Lovely Spam, Wonderful Spam
```

```bash
import csv

p = Path("d:/tmp/mycsv/test.csv")
with open(str(p)) as f:
    reader = csv.reader(f)
    for i in reader:
        print(i)
-----------------------------------------------------
['id', 'name', 'age', 'comment']
['1', 'zs', '18', "I'm 18"]
['2', 'ls', '20', 'this is a "test" string.']
['3', 'ww', '23', '你好\n\n计算机\n']
```

```bash
import csv

p = Path("d:/tmp/mycsv/test.csv")
with open(str(p)) as f:
    reader = csv.reader(f)
    print(1,"-->",next(reader))
    print(2,"-->",next(reader))

rows = [
    [4,'tom',22,'tom'],
    (5,'jerry',24,'jerry'),
    (6,'justin',22,'just\t"in'),
    "abcdefghi",
    ((1,),(2,))
]

row = rows[0]
p = Path("d:/tmp/mycsv/test.csv")
with open(str(p),'a+') as f:
    writer = csv.writer(f)
    writer.writerow(row)
    writer.writerows(rows)
------------------------------------------------------------
1 --> ['id', 'name', 'age', 'comment']
2 --> ['1', 'zs', '18', "I'm 18"]
```

## ini 文件处理

作为配置文件，ini 文件格式很流行

```bash
[DEFAULT]
a = test

[mysql]
default-character-set=utf8

[mysqld]

datadir=/dbserver/data
port=33060
character-set-server=utf8
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
```

+ 括号里面的部分称为 section，译作节、区、段
+ 每一个 section 内，都有 key=value 形成的键值对，key 称为 option 选项
+ 注意这里的 DEFAULT 是缺省 section 的名字，必须大写

### configparser 模块

configparser 模块的 ConfigParser 类就是用来操作

可以将 section 当作 key，section 存储着键值对组成的字典，可以把 ini 配置文件当作一个嵌套的字典。默认使用的是有序字典。

> read(filenames, encoding=None)

+ 读取 ini 文件，可以是单个文件，也可以是文件列表。可以指定文件编码。

> sections()

+ 返回 section 列表。缺省 section 不包括在内。

> add_section(section_name)

+ 增加一个 section

> has_section(section_name)

+ 判断 section 是否存在

> options(section)

+ 返回 section 的所有 option，会追加缺省 section 的 option

> has_option(section, option)

+ 判断 section 是否存在这个 option

> get(section, option, *, raw=False, vars=None[, fallback])

+ 从指定的段的选项上取值，如果找到就返回值，如果没有找到就去找 DEFAULT 段有没有。
+ getint(section, option, *, raw=False, vars=None[, fallback])
+ getfloat(section, option, *, raw=False, vars=None[, fallback])
+ getboolean(section, option, *, raw=False, vars=None[, fallback])
  + 上面 3 个方法和 get 一样，返回指定类型数据。

> items(raw=False, vars=None)
>
> items(section, raw=False, vars=None)

+ 没有 section，则返回所有 section 名字及其对象；
+ 如果指定 section，则返回这个指定 section 的键值对组成二元组

> set(section, option, value)

+ section 存在的情况下，写入 option=value，要求 option、value 必须是字符串

> remove_section(section)

+ 移除 section 及其所有 option

> remove_option(section, option)

+ 移除 section 下的 option

> write(fileobject, space_around_delimiters=True)

+ 将当前 config 的所有内容写入 fileobject 中，一般 open 函数使用 w 模式。

```bash
from configparser import ConfigParser

ini_body="""\
[DEFAULT]
a = test

[mysql]
default-character-set=utf8

[mysqld]

datadir=/dbserver/data
port=33060
character-set-server=utf8
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
"""
with open("test.ini","w+") as f:
    f.write(ini_body)
    f.seek(0)
    print("- - -")
    print(f.read())

filename = "test.ini"
newfilename = "mysql.ini"

cfg = ConfigParser()
cfg.read(filename)
print("- - -")
print(1,"-->",cfg.sections())
print(2,"-->",cfg.has_section("client"))

print(3,"-->",cfg.items("mysqld"))
conter = 0
for k,v in cfg.items():
    print(conter,"-->",k,type(v))
    print(conter,"-->",k,cfg.items(k))
    conter += 1

tmp = cfg.get("mysqld","port")
print(11,"-->",type(tmp),tmp)
print(12,"-->",cfg.get("mysqld","a"))
# print(13,"-->",cfg.get("mysqld","ccyunchina"))
print(14,"-->",cfg.get("mysqld","ccyunchina",fallback="python"))

tmp = cfg.getint("mysqld","port")
print(21,"-->",type(tmp),tmp)

if cfg.has_section("test"):
    cfg.remove_section("test")

cfg.add_section("test")
cfg.set("test","test1","1")
cfg.set("test","test2","2")

with open(newfilename,"w") as f:
    cfg.write(f)

print(31,"-->",cfg.getint("test","test2"))
cfg.remove_option("test","test2")
-----------------------------------------------------------------------------------
- - -
[DEFAULT]
a = test

[mysql]
default-character-set=utf8

[mysqld]

datadir=/dbserver/data
port=33060
character-set-server=utf8
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

- - -
1 --> ['mysql', 'mysqld']
2 --> False
3 --> [('a', 'test'), ('datadir', '/dbserver/data'), ('port', '33060'), ('character-set-server', 'utf8'), ('sql_mode', 'NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES')]
0 --> DEFAULT <class 'configparser.SectionProxy'>
0 --> DEFAULT [('a', 'test')]
1 --> mysql <class 'configparser.SectionProxy'>
1 --> mysql [('a', 'test'), ('default-character-set', 'utf8')]
2 --> mysqld <class 'configparser.SectionProxy'>
2 --> mysqld [('a', 'test'), ('datadir', '/dbserver/data'), ('port', '33060'), ('character-set-server', 'utf8'), ('sql_mode', 'NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES')]
11 --> <class 'str'> 33060
12 --> test
14 --> python
21 --> <class 'int'> 33060
31 --> 2
True
```

字典操作更简单

```bash
from configparser import ConfigParser

ini_body="""\
[DEFAULT]
a = test

[mysql]
default-character-set=utf8

[mysqld]

datadir=/dbserver/data
port=33060
character-set-server=utf8
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
"""
with open("test.ini","w+") as f:
    f.write(ini_body)
    f.seek(0)
    print("- - -")
    print(f.read())

filename = "test.ini"
newfilename = "mysql.ini"

cfg = ConfigParser()
cfg.read(filename)
print("- - -")
print(1,"-->",cfg.sections())
print(2,"-->",cfg.has_section("client"))

if cfg.has_section("test"):
    cfg.remove_section("test")

cfg.add_section("test")
cfg.set("test","test1","1")
cfg.set("test","test2","2")

with open(newfilename,"w") as f:
    cfg.write(f)

cfg["test"]["x"] = "100" # key 不存在
cfg["test2"] = {"test2": "1000"} # section 不存在

print(11,"-->","x" in cfg["test"])
print(12,"-->","x" in cfg["test2"])

print(21,"-->",cfg._dict) # 返回默认字典类型，内部使用有序字典

with open(newfilename,'w') as f:
    cfg.write(f)

print("- - -")

with open(newfilename) as f:
    print(f.read())
----------------------------------------------------------------------
- - -
[DEFAULT]
a = test

[mysql]
default-character-set=utf8

[mysqld]

datadir=/dbserver/data
port=33060
character-set-server=utf8
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

- - -
1 --> ['mysql', 'mysqld']
2 --> False
11 --> True
12 --> False
21 --> <class 'collections.OrderedDict'>
- - -
[DEFAULT]
a = test

[mysql]
default-character-set = utf8

[mysqld]
datadir = /dbserver/data
port = 33060
character-set-server = utf8
sql_mode = NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

[test]
test1 = 1
test2 = 2
x = 100

[test2]
test2 = 1000
```

## 序列化和反序列化

> 为什么要序列化

+ 内存中的字典、列表、集合以及各种对象，如何保存到一个文件中？
+ 如果是自己定义的类的实例，如何保存到一个文件中？
+ 如何从文件中读取数据，并让他们在内存中再次变成自己对应的类的实例？
+ 要设计一套**协议**，按照某种规则，把内存中数据保存到文件中。文件是一个字节序列，所以必须把数据转换成字节序列，输出到文件。这就是序列化。反之，从文件的字节序列恢复到内存，就是反序列化。

> 定义

+ serialization
  + 序列化，将内存中对象存储下来，把它变成一个个字节 --> 二进制
+ deserialization
  + 反序列化，将文件的一个个字节恢复成内存中对象 <-- 二进制
+ 序列化保存到文件就是持久化
+ 可以将数据序列化后持久化，或者网络传输；也可以将从文件中或者网络接收到的字节序列反序列化

### pickle 库

pickle 是 python 中的序列化、反序列化模块

+ dumps 对象序列化为 bytes 对象
+ dump 对象序列化到文件对象，就是存入文件
+ loads 从 bytes 对象反序列化
+ load 对象反序列化，从文件读取数据

```bash
import pickle

filename = "d:/tmp/ser"

d = {"a":1,"b":"abc","c":[1,2,3]}
l = list("123")
i = 99

with open(filename,"wb") as f:
    pickle.dump(d,f)
    pickle.dump(l,f)
    pickle.dump(i,f)

with open(filename,"rb") as f:
    print(f.read(),f.seek(0))
    for _ in range(3):
        x = pickle.load(f)
        print(type(x),x)
-----------------------------------------------------------
b'\x80\x03}q\x00(X\x01\x00\x00\x00aq\x01K\x01X\x01\x00\x00\x00bq\x02X\x03\x00\x00\x00abcq\x03X\x01\x00\x00\x00cq\x04]q\x05(K\x01K\x02K\x03eu.\x80\x03]q\x00(X\x01\x00\x00\x001q\x01X\x01\x00\x00\x002q\x02X\x01\x00\x00\x003q\x03e.\x80\x03Kc.' 0
<class 'dict'> {'a': 1, 'b': 'abc', 'c': [1, 2, 3]}
<class 'list'> ['1', '2', '3']
<class 'int'> 99
```

```bash
import pickle

# 对象序列化
class AA:
    tttt = "ABC"
    def show(self):
        print("abc")

a1 = AA()

sr = pickle.dumps(a1)
print("sr={}".format(sr)) # AA

a2 = pickle.loads(sr)
print(a2.tttt)
a2.show()
---------------------------------------------------------------
sr=b'\x80\x03c__main__\nAA\nq\x00)\x81q\x01.'
ABC
abc
```

+ 这个例子中，其实就保存了一个类名，因为所有其他的东西都是类定义的东西，是不变的，所以只序列化一个 AA 类名
+ 反序列化时找到类就可以恢复一个对象

```bash
import pickle

# 定义类
class AAA:
    def __init__(self):
        self.tttt = "abc"

# 创建AAA类的实例
a1 = AAA()

# 序列化
ser = pickle.dumps(a1)
print(1,"-->","ser={}".format(ser))

# 反序列化
a2 = pickle.loads(ser)
print(2,"-->",a2,type(a2))
print(3,"-->",a2.tttt)
print(4,"-->",id(a1),id(a2))
-------------------------------------------------------------------
1 --> ser=b'\x80\x03c__main__\nAAA\nq\x00)\x81q\x01}q\x02X\x04\x00\x00\x00ttttq\x03X\x03\x00\x00\x00abcq\x04sb.'
2 --> <__main__.AAA object at 0x0000029774F75548> <class '__main__.AAA'>
3 --> abc
4 --> 2849520006280 2849525683528
```

+ 可以看出这回除了必须保存的AAA，还序列化了 tttt 和 abc，因为这是每一个对象自己的属性，每一个对象不一样的，所以这些数据需要序列化。

#### 序列化、反序列化实验

定义类 AAA，并序列化到文件

```bash
import pickle

class AAA:
    def __init__(self):
        self.tttt = "abc"

aaa = AAA()
sr = pickle.dumps(aaa)
print(len(sr),sr)

file = "d:/tmp/ser"
with open(file, "wb") as f:
    pickle.dump(aaa, f)
-----------------------------------------------------------------------
49 b'\x80\x03c__main__\nAAA\nq\x00)\x81q\x01}q\x02X\x04\x00\x00\x00ttttq\x03X\x03\x00\x00\x00abcq\x04sb.'
```

将产生的序列化文件发送到其它节点上

增加一个 x.py 文件，内容如下。最后执行这个脚本。

```bash
import pickle

with open("ser","rb") as f:
    a = pickle.load(f)
```

+ 会抛出异常 AttributeError: Can't get attribute 'AAA' on <module '__main__' from 'x.py'>
+ 这个异常实际上是找不到 AAA 类的定义，增加类定义即可解决
+ 反序列化的时候要找到 AAA 类的定义，才能成功。否则就会抛出异常。
+ 可以这样理解：反序列化的时候，类是模子，二进制序列就是铁水

```bash
import pickle

class AAA:
    def show(self):
        print("xyz")

with open("ser","rb") as f:
    a = pickle.load(f)
    print(a)
    a.show()
-----------------------------------------------------------------------
<__main__.AAA object at 0x7f2ffe2e0908>
xyz
```

+ 这里定义了类 AAA，并且上面的代码也能成功的执行。
+ **注意！！！**这里的 AAA 定义和原来完全不同了。
+ 因此，序列化、反序列化必须保证使用**同一套类**的定义，否则会带来不可预料的结果。

### 序列化应用

1. 一般来说，本地序列化的情况，应用较少。大多数场景都应用在网络传输中。
2. 将数据序列化后通过网络传输到远程节点，远程服务器接收到数据反序列化后，就可以使用了。
3. 但是，要注意一点，远程接收端，反序列化时必须有对应的数据类型，否则就会报错。**尤其是自定义类**，必须远程得有一致的定义。
4. 现在，大多数项目，都不是单机的，也不是单服务的，需要通过网络将数据传送到其他节点上去，这就需要大量的序列化、反序列化过程。
5. 但是，问题是，Python 程序之间还可以都是用 pickle 解决序列化、反序列化，如果是跨平台、跨语言、跨协议，pickle 就不太合适了，就需要公共的协议。例如 XML、Json、Protocol Buffer 等。
6. 不同的协议，效率不同、学习曲线不同，适用场景不同，要根据不同的情况分析选型。
