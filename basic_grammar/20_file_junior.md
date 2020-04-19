# 路径操作

Python3.4 版本之前，路径操作模块是 os.path

```bash
from os import path

p = path.join("/etc","sysconfig","network")
print(1,"-->",type(p),p)
print(2,"-->",path.exists(p))
print(3,"-->",path.split(p)) # (head,tail)
print(4,"-->",path.abspath("."))
print(5,"-->",path.dirname(p))
print(6,"-->",path.basename(p))
print(7,"-->",path.splitdrive(p))

print("~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~")

p1 = path.abspath(p)
print(p1,path.basename(p1))

while p1 != path.dirname(p1):
    p1 = path.dirname(p1)
    print(p1,path.basename(p1))
------------------------------------------------------------------
1 --> <class 'str'> /etc/sysconfig/network
2 --> True
3 --> ('/etc/sysconfig', 'network')
4 --> /root
5 --> /etc/sysconfig
6 --> network
7 --> ('', '/etc/sysconfig/network')
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/etc/sysconfig/network network
/etc/sysconfig sysconfig
/etc etc
/ 
```

Python3.4 版本之后，建议使用 pathlib 模块，提供 Path 对象来操作。包括目录和文件。

## pathlib 模块

### 目录操作

初始化

```bash
from pathlib import Path

p = Path() # 当前目录
p = Path("a","b","c/d") # 当前目录下的 a/b/c/d
p = Path("/etc") # 根下的 etc 目录
```

路径拼接和分解

操作符 /

+ Path 对象 / Path 对象
+ Path 对象 / 字符串或者字符串 / Path 对象

分解

+ parts 属性，可以返回路径中的每一个部分

joinpath

+ joinpath(*other) 连接多个字符串到 Path 对象中

```bash
from pathlib import Path

p = Path()
print(1,"-->",type(p))
print(2,"-->",p)

p = p / "a"
p1 = "b" / p
print(3,"-->",p1)

p2 = Path("c")
print(4,"-->",p2)

p3 = p2 / p1
print(5,"-->",p3)
print(6,"-->",p3.parts)

print(7,"-->",p3.joinpath("etc","init.d",Path("httpd")))
----------------------------------------------------------------
1 --> <class 'pathlib.PosixPath'>
2 --> .
3 --> b/a
4 --> c
5 --> c/b/a
6 --> ('c', 'b', 'a')
7 --> c/b/a/etc/init.d/httpd
```

获取路径

+ str 获取路径字符串
+ bytes 获取路径字符串的 bytes

```bash
from pathlib import Path

p = Path("/etc")

print(str(p),bytes(p))
-----------------------------------------------------------------
/etc b'/etc'
```

父目录

+ parent 目录的逻辑父目录
+ parents 父目录序列，索引 0 是直接的父

```bash
from pathlib import Path

p = Path("/a/b/c/d")
print(p.parent.parent)

for x in p.parents:
    print(x)

print(p.parents[len(p.parents)-1])
-----------------------------------------------------------------
/a/b
/a/b/c
/a/b
/a
/
/
```

name、stem、suffix、suffixes、with_suffix(suffix)、with_name(name)

+ name 目录的最后一个部分
+ suffix 目录中最后一个部分的扩展名
+ stem 目录最后一个部分，没有后缀
+ suffixes 返回多个扩展名列表
+ with_suffix(suffix) 补充扩展名到路径尾部，返回新的路径，扩展名存在则无效
+ with_name(name) 替换目录最后一个部分并返回一个新的路径

```bash
from pathlib import Path

p = Path("/magedu/mysqlinstall/mysql.tar.gz")
print(1,"-->",p.name)
print(2,"-->",p.suffix)
print(3,"-->",p.stem)
print(4,"-->",p.with_name("mysql-5.tgz"))

p = Path("README")
print(11,"-->",p.with_suffix(".txt"))
--------------------------------------------------------------------
1 --> mysql.tar.gz
2 --> .gz
3 --> mysql.tar
4 --> /magedu/mysqlinstall/mysql-5.tgz
11 --> README.txt
```

```bash
from pathlib import Path

p = Path("/magedu/mysqlinstall/mysql.tar.gz")

print(1,"-->",p.cwd()) # 返回当前工作目录
print(2,"-->",p.home()) # 返回当前家目录
print(3,"-->",p.is_dir()) # 是否是目录
print(4,"-->",p.is_file()) # 是否是普通文件
print(5,"-->",p.is_symlink()) # 是否是软链接
print(6,"-->",p.is_socket()) # 是否是 socket 文件
print(7,"-->",p.is_block_device()) # 是否是块设备
print(8,"-->",p.is_char_device()) # 是否是字符设备
print(9,"-->",p.is_absolute()) # 是否是绝对路径

print(10,"-->",p.resolve()) # 返回一个新的路径，这个新路径就是当前 Path 对象的绝对路径，如果是软链接则直接被解析
print(11,"-->",p.absolute()) # 也可以获取绝对路径，但是推荐使用 resolve()
print(12,"-->",p.exists()) # 目录或文件是否存在
print(13,"-->",p.rmdir()) # 删除空目录。没有提供判断目录为空的方法
print(14,"-->",p.touch(mode=0o666,exist_ok=True)) # 创建一个文件
print(15,"-->",p.as_uri()) # 将路径返回成 URI，例如 "file:///etc/passwd"

print(16,"-->",p.mkdir(mode=0o777,parents=True,exist_ok=True)) 
# parents，是否创建父目录，True 等同于 mkdir -p；False时，父目录不存在，抛出 FileNotFoundError
# exist_ok 参数，在 3.5 版本加入。 False时，路径存在，抛出 FileExistsError；True时，FileExistsError被忽略

```

迭代当前目录

```bash
from pathlib import Path

p = Path()
p /= "a/b/c/d"
print(p.exists())

for x in p.parents[len(p.parents)-1].iterdir():
    print(x,end="\t")
    if x.is_dir():
        flag = False
        for _ in x.iterdir():
            flag = True
            break
        print("dir","Not Empty" if flag else "Empty",sep="\t")
    elif x.is_file():
        print("file")
    else:
        print("other file")
--------------------------------------------------------------------
True
.bash_logout	file
.bash_profile	file
.bashrc	file
.cshrc	file
.tcshrc	file
original-ks.cfg	file
anaconda-ks.cfg	file
.bash_history	file
.cache	dir	Not Empty
.ipython	dir	Not Empty
test.txt	file
```

