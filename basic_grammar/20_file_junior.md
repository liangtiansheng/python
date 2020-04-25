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
.bash_logout file
.bash_profile file
.bashrc file
.cshrc file
.tcshrc file
original-ks.cfg file
anaconda-ks.cfg file
.bash_history file
.cache dir Not Empty
.ipython dir Not Empty
test.txt file
```

通配符 --> 返回一个生成器

+ glob(pattern) 通配给定的模式
+ rglob(pattern) 通配给定的模式，递归目录

```bash
from pathlib import Path

p = Path()

p1 = list(p.glob(".*")) # 返回当前目录下以 . 开头的文件
p2 = list(p.glob("**/*.py")) # 递归所有目录，等同 rglob
print(p2)

g = p.rglob("*.py") # 生成器
print(1,"-->",next(g))
print(2,"-->",next(g))
--------------------------------------------------------
[PosixPath('a/b/c/d/1.py'), PosixPath('a/b/c/d/2.py'), PosixPath('a/b/c/d/3.py')]
1 --> a/b/c/d/1.py
2 --> a/b/c/d/2.py
```

模式匹配 --> 返回 True/False

+ match(pattern)

```bash
from pathlib import Path

print(1,"-->",Path('a/b.py').match("*.py"))
print(2,"-->",Path('a/b/c.py').match('b/*.py'))
print(3,"-->",Path("/a/b/c.py").match("a/*.py"))
print(4,"-->",Path("/a/b/c.py").match('a/*/*.py'))
print(5,"-->",Path('/a/b/c.py').match('a/**/*.py'))
print(6,"-->",Path('/a/b/c.py').match('**/*.py'))
-----------------------------------------------------------
1 --> True
2 --> True
3 --> False
4 --> True
5 --> True
6 --> True
```

显示文件信息

+ stat() 相当于 stat 命令
+ lstat() 同 stat()，但如果是符号链接，则显示符号链接本身的文件信息

```bash
[root@lytest1 ~]# ln -sv test t
‘t’ -> ‘test’
from pathlib import Path
p = Path("test")
print(1,"-->",p.stat())
print()
p1 = Path('t')
print(2,"-->",p1.stat())
print()
print(3,"-->",p1.lstat())
---------------------------------------------------------------
1 --> os.stat_result(st_mode=33188, st_ino=100735060, st_dev=64768, st_nlink=1, st_uid=0, st_gid=0, st_size=0, st_atime=1587271470, st_mtime=1587272239, st_ctime=1587272239)

2 --> os.stat_result(st_mode=33188, st_ino=100735060, st_dev=64768, st_nlink=1, st_uid=0, st_gid=0, st_size=0, st_atime=1587271470, st_mtime=1587272239, st_ctime=1587272239)

3 --> os.stat_result(st_mode=41471, st_ino=101232439, st_dev=64768, st_nlink=1, st_uid=0, st_gid=0, st_size=4, st_atime=1587340972, st_mtime=1587340956, st_ctime=1587340956)
```

### 文件操作

open(mode='r',buffering=-1,encoding=None,errors=None,newline=None) --> 返回一个文件对象

+ 使用方法类似内建函数 open
+ read_bytes()，以 'rb' 读取路径对应文件，并返回二进制流。看源码
+ read_text(encoding=None,errors=None)，以 'rt' 方式读取路径对应文件，返回文本
+ Path.write_bytes(data)，以 'wb' 方式写入数据到路径对应文件
+ write_text(data,encoding=None,errors=None)，以 'wt' 方式写入字符串到路径对应文件

```bash
from pathlib import Path

p = Path("my_binary_file")
p.write_bytes(b"Binary file contents\n")
print(1,"-->",p.read_bytes())

p = Path("my_text_file")
p.write_text("Text file contents\n")
print(2,"-->",p.read_text())

p = Path("test.py")
p.write_text("hello python")
print(3,"-->",p.read_text())

with p.open() as f:
    print(1,"-->",f.read(7))
------------------------------------------------------------------------
1 --> b'Binary file contents\n'
2 --> Text file contents

3 --> hello python
1 --> hello p
```

## OS 模块

操作系统平台

| 属性或方法   | 结果                                 |
| ------------ | ------------------------------------ |
| os.name      | windows 是 nt，linux 是 posix        |
| os.uname()   | *nix 支持                            |
| sys.platform | windows 显示 win32，linux 显示 linux |

os.listdir("d:") --> 返回目录内容列表

os 也有 open、read、write 等方法，但是太低级，建议使用内建函数 open、read、write，使用方法相似

```bash
[root@lytest1 ~]# ln -sv test t1
‘t1’ -> ‘test’
```

os.stat(path,*,dir_fd=None,follow_symlinks=True)，本质上调用 linux 系统的 stat

+ path，路径的 string 或者 bytes，或者 fd 文件描述符
+ follow_symlinks True，返回文件本身信息，False 且如果是软链接则显示软链接本身

```bash
import os,sys
print(1,"-->",os.stat("test"))
print(2,"-->",os.stat("t1",follow_symlinks=True))
print(3,"-->",os.stat("t1",follow_symlinks=False))
-----------------------------------------------------------------
1 --> os.stat_result(st_mode=33188, st_ino=100735060, st_dev=64768, st_nlink=1, st_uid=0, st_gid=0, st_size=0, st_atime=1587271470, st_mtime=1587272239, st_ctime=1587272239)
2 --> os.stat_result(st_mode=33188, st_ino=100735060, st_dev=64768, st_nlink=1, st_uid=0, st_gid=0, st_size=0, st_atime=1587271470, st_mtime=1587272239, st_ctime=1587272239)
3 --> os.stat_result(st_mode=41471, st_ino=101232445, st_dev=64768, st_nlink=1, st_uid=0, st_gid=0, st_size=4, st_atime=1587387215, st_mtime=1587386620, st_ctime=1587386620)
```

os.chmod(path,mode,*,dir_fd=None,follow_symlinks=True)

os.chmod("test",0o777)

os.chown(path,uid,gid)，改变文件的属主、属组，但需要足够的权限

## shutil 模块

拷贝文件时，如果只是将内容进行拷贝，那很容易丢失信息，像 stat 数据信息、权限等；Python 提供了一个方便的库 shutil(高级文件操作)

### copy 复制

copyfileobj(fsrc,fdst[,length])

+ 文件对象的复制，fsrc 和 fdst 是 open 打开的文件对象，复制内容。fdst 要求可写
+ length 指定了表示 buffer 的大小

```bash
import shutil

with open("test","r+") as f1:
    f1.write("abcd\n1234")
    f1.flush()
    with open("test1","w+") as f2:
        shutil.copyfileobj(f1,f2)
!cat test1 # 为什么会是空的呢
# 看一下源码
def copyfileobj(fsrc, fdst, length=16*1024):
    """copy data from file-like object fsrc to file-like object fdst"""
    while 1:
        buf = fsrc.read(length)
        if not buf:
            break
        fdst.write(buf)
# f1 flush() 后内容写到 test，但是指针在哪里？
# 指针在末尾，所以源码中 read 再 write 是空

改成：

import shutil

with open("test","r+") as f1:
    f1.write("abcd\n1234")
    f1.flush()
    f1.seek(0)
    with open("test1","w+") as f2:
        shutil.copyfileobj(f1,f2)
!cat test1
```

copyfile(src,dst,*,follow_symlinks=True)

+ 复制文件内容，不含元数据。src、dst 为文件的路径字符串，本质上调用的就是 copyfileobj，所以不带元数据二进制内容复制。

copymode(src,dst,*,follow_symlinks=True)

+ 仅仅复制权限。

```bash
import shutil,os

shutil.copyfile("test","test1")
print(1,"-->",os.stat("test"))
print(2,"-->",os.stat("test1"))
!chmod -x test
!ls -l test test1
print()
shutil.copymode("test","test1")
print(1,"-->",os.stat("test"))
print(2,"-->",os.stat("test1"))
!ls -l test test1
-----------------------------------------------------------------------------
1 --> os.stat_result(st_mode=33261, st_ino=100735060, st_dev=64768, st_nlink=1, st_uid=0, st_gid=0, st_size=0, st_atime=1587728047, st_mtime=1587272239, st_ctime=1587728029)
2 --> os.stat_result(st_mode=33261, st_ino=101232444, st_dev=64768, st_nlink=1, st_uid=0, st_gid=0, st_size=0, st_atime=1587388696, st_mtime=1587728047, st_ctime=1587728047)
-rw-r--r-- 1 root root 0 Apr 19 12:57 test
-rwxr-xr-x 1 root root 0 Apr 24 19:34 test1

1 --> os.stat_result(st_mode=33188, st_ino=100735060, st_dev=64768, st_nlink=1, st_uid=0, st_gid=0, st_size=0, st_atime=1587728047, st_mtime=1587272239, st_ctime=1587728047)
2 --> os.stat_result(st_mode=33188, st_ino=101232444, st_dev=64768, st_nlink=1, st_uid=0, st_gid=0, st_size=0, st_atime=1587388696, st_mtime=1587728047, st_ctime=1587728048)
-rw-r--r-- 1 root root 0 Apr 19 12:57 test
-rw-r--r-- 1 root root 0 Apr 24 19:34 test1
```

copystat(src, dst, *, follow_symlinks=True)

+ 复制元数据，stat 包含权限

```bash
import shutil,os

!stat test test1

shutil.copystat("test","test1")
!stat test test1
------------------------------------------------------------------------------
  File: ‘test’
  Size: 0          Blocks: 0          IO Block: 4096   regular empty file
Device: fd00h/64768d Inode: 100735060   Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2020-04-24 19:34:07.783686437 +0800
Modify: 2020-04-19 12:57:19.418985610 +0800
Change: 2020-04-24 19:34:07.799686559 +0800
 Birth: -
  File: ‘test1’
  Size: 0          Blocks: 0          IO Block: 4096   regular empty file
Device: fd00h/64768d Inode: 101232444   Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2020-04-20 21:18:16.915399593 +0800
Modify: 2020-04-24 19:34:07.783686437 +0800
Change: 2020-04-24 19:34:08.035688370 +0800
 Birth: -
  File: ‘test’
  Size: 0          Blocks: 0          IO Block: 4096   regular empty file
Device: fd00h/64768d Inode: 100735060   Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2020-04-24 19:34:07.783686437 +0800
Modify: 2020-04-19 12:57:19.418985610 +0800
Change: 2020-04-24 19:34:07.799686559 +0800
 Birth: -
  File: ‘test1’
  Size: 0          Blocks: 0          IO Block: 4096   regular empty file
Device: fd00h/64768d Inode: 101232444   Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2020-04-24 19:34:07.783686437 +0800
Modify: 2020-04-19 12:57:19.418985610 +0800
Change: 2020-04-24 19:40:28.879436570 +0800
 Birth: -
```

copy(src, dst, *, follow_symlinks=True)

+ 复制文件内容、权限和部分元数据，不包括创建时间和修改时间
+ 本质上调用的是
  + copyfile(src, dst, follow_symlinks=follow_symlinks)
  + copymode(src, dst, follow_symlinks=follow_symlinks)

copy2 比 copy 多了复制全部元数据，但需要平台支持

+ 本质上调用的是
  + copyfile(src, dst, follow_symlinks=follow_symlinks)
  + copystat(src, dst, follow_symlinks=follow_symlinks)

copytree(src, dst, symlinks=False, ignore=None, copy_function=copy2, ignore_dangling_symlinks=False)

+ 递归复制目录。默认使用 copy2，也就是带更多的元数据复制
+ src、dst 必须是目录，src 必须存在，dst 必须不存在
+ ignore = func，提供一个 callable(src, names) -> ignored_names。提供一个函数，它会被调用。src 是源目录，names 是 os.listdir(src) 的结果，就是列出 src 中的文件名，返回值是要被过滤的文件名的 set 类型数据

```bash
# d:/temp 下有 a、b 目录
# 要结合源码看
def ignore(src,names):
    ig = filter(lambda x: x.startswith("a"),names) # 返回以a开头为true的迭代器
    return set(ig) # 返回集合，去重

import shutil
shutil.copytree("d:/temp","d:/tt/o",ignore=ignore)
```

### rm 删除

shutil.rmtree(path, ignore_errors=False, onerror=None)

+ 递归删除，如同 rm -rf 一样危险，慎用
+ 它不是原子操作，有可能删除错误，就会中断，已经删除的就删除了
+ ignore_errors 为 true，忽略错误，当为 False 或者 omitted 时 onerror 生效
+ onerror 为 callable，接受函数 function、path 和 execinfo

```bash
shutil.rmtree("d:/temp") # 类似 rm -rf
```

### move 移动

move(src, dst, copy_function=copy2)

+ 递归移动文件、目录到目标，返回目标
+ 本身使用的是 os.rename 方法
+ 如果不支持 rename，如果是目录则想 copytree 再删除源目录
+ 默认使用 copy2 方法

```bash
os.rename("d:/t.txt","d:/temp/t")
os.rename("test3","/tmp/py/test300")
```

__shutil 还有打包功能__，生成 tar 并压缩，支持 zip、gz、bz、xz
