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
