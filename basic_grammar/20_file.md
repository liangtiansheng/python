# Python3 File(文件) 方法

## open() 方法

Python open() 方法用于打开一个文件，并返回文件对象，在对文件进行处理过程都需要使用到这个函数，如果该文件无法被打开，会抛出 OSError。

**注意:** 使用 open() 方法一定要保证关闭文件对象，即调用 close() 方法。

open() 函数常用形式是接收两个参数：文件名(file)和模式(mode)。

```bash
open(file, mode='r')
```

完整的语法格式为：

```bash
open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
```

参数说明:

+ file: 必需，文件路径（相对或者绝对路径）。
+ mode: 可选，文件打开模式
+ buffering: 设置缓冲
+ encoding: 一般使用utf8
+ errors: 报错级别
+ newline: 区分换行符
+ closefd: 传入的file参数类型
+ opener:

mode 参数有：

模式|    描述
:-|:-
t|    文本模式 (默认)。
x|    写模式，新建一个文件，如果该文件已存在则会报错。
b|    二进制模式。
+|    打开一个文件进行更新(可读可写)。
U|    通用换行模式（不推荐）。
r|    以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。
rb|    以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。一般用于非文本文件如图片等。
r+|    打开一个文件用于读写。文件指针将会放在文件的开头。
rb+|    以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。一般用于非文本文件如图片等。
w|    打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。
wb|    以二进制格式打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。一般用于非文本文件如图片等。
w+|    打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。
wb+|    以二进制格式打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。一般用于非文本文件如图片等。
a|    打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。
ab|    以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。
a+|    打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。
ab+|    以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。

默认为文本模式，如果要以二进制模式打开，加上 b 。

## file 对象

file 对象使用 open 函数来创建，下表列出了 file 对象常用的函数：

序号|    方法及描述
:-|:-
1    |file.close()<br>关闭文件。关闭后文件不能再进行读写操作。
2    |file.flush()<br>刷新文件内部缓冲，直接把内部缓冲区的数据立刻写入文件, 而不是被动的等待输出缓冲区写入。
3    |file.fileno()<br>返回一个整型的文件描述符(file descriptor FD 整型), 可以用在如os模块的read方法等一些底层操作上。
4    |file.isatty()<br>如果文件连接到一个终端设备返回 True，否则返回 False。
5    |file.next()<br>Python 3 中的 File 对象不支持 next() 方法。<br>返回文件下一行。
6    |file.read([size])<br>从文件读取指定的字节数，如果未给定或为负则读取所有。
7    |file.readline([size])<br>读取整行，包括 "\n" 字符。
8    |file.readlines([sizeint])<br>读取所有行并返回列表，若给定sizeint>0，返回总和大约为sizeint字节的行, 实际读取值可能比 sizeint 较大, 因为需要填充缓冲区。
9    |file.seek(offset[, whence])<br>设置文件当前位置
10    |file.tell()<br>返回文件当前位置。
11    |file.truncate([size])<br>从文件的首行首字符开始截断，截断文件为 size 个字符，无 size 表示从当前位置截断；截断之后后面的所有字符被删除，其中 Widnows 系统下的换行代表2个字符大小。
12    |file.write(str)<br>将字符串写入文件，返回的是写入的字符长度。
13    |file.writelines(sequence)<br>向文件写入一个序列字符串列表，如果需要换行则要自己加入每行的换行符。

## 操作理解

```bash
f = open("test.txt",mode="w+")
# Windows <_io.TextIOWrapper name='test.txt' mode='w+' encoding='cp936'>
# Linux <_io.TextIOWrapper name='test.txt' mode='w+' encoding='UTF-8'>
print(f.read()) # 读文件
f.close() # 关闭文件
```

文件访问的模式有两种，文本模式和二进制模式，不同模式下，操作函数不尽相同，表现的结果也不一样

### open 的参数

#### mode 模式

```bash
f = open("test.txt")
f.write("abc")
f.close()
-----------------------------------------------
UnsupportedOperation                      Traceback (most recent call last)
<ipython-input-10-a73b6e0cc61d> in <module>
      1 f = open("test.txt")
----> 2 f.write("abc")

UnsupportedOperation: not writable

===============================================

f = open("test.txt",mode="r")
f.write("abc")
f.close()
-----------------------------------------------
UnsupportedOperation                      Traceback (most recent call last)
<ipython-input-12-c61d84458398> in <module>
      1 f = open("test.txt",mode="r")
----> 2 f.write("abc")
      3 f.close()

UnsupportedOperation: not writable

===============================================

f = open("test.txt",mode="w")
f.write("abc")
f.close()
cat test.txt
-----------------------------------------------
# 查看不到内容，跟指针有关
```

+ open 默认以 r 模式打开已经存在的文件
+ r 以只读的方式打开文件，如果 write，会抛异常；如果文件不存在，抛出 FileNotFoundError 异常
+ w 表示只写方式打开，如果读取则抛出异常；如果文件不存在，则直接创建文件；如果文件存在则清空文件内容

```bash
f = open("test1",mode="x")
# f.read()
f.write("abc")
f.close()

f = open("test1",mode="x")
-----------------------------------------------
FileExistsError: [Errno 17] File exists: 'test1'
```

+ x 文件不存在，创建文件，并只写方式打开；如果文件存在，则抛出 FileExistsError

```bash
f = open("test2",mode="a")
# f.read()
f.write("abcd")
f.close()
```

+ 文件存在，只写打开，追加内容；文件不存在，则创建后，只写打开，追加内容

r 是只读，wxa 都是只写；wxa 都可以产生新文件，w 不管文件存在与否，都会生成全新内容的文件；a 不管文件是否存在，都能在打开的文件尾部追加；x 必需要求文件事先不存在，自己创建一个新文件

```bash
f = open("test3","rb") # 二进制只读
s = f.read()
print(type(s)) # bytes
print(s)
f.close()

f = open("test3",'wb') # IO对象
s = f.write("自学成才".encode())
print(s)
f.close()
-------------------------------------------------
<class 'bytes'>
b''
12
```

+ 文本模式 t，**字符流**，将文件的字节按照某种字符编码理解，按照字符操作；open 的默认 mode 就是 rt。
+ 二进制模式 b，**字节流**，将文件就按照字节理解，与字符编码无关；二进制模式操作时，字节操作使用 bytes 类型。

```bash
# f = open("test3","rw") 报错
f = open("test3","r+")
f.write("come on")
print(f.read()) # 没有显示，为什么
f.close()

f = open("test3","w+")
f.read()
f.close()

f = open("test3","a+")
f.write("you")
f.read()
f.close()

f = open("test4","x+")
f.write("python")
f.read()
f.close()
```

+ \+ 为 r w a x 提供缺失的读写功能，但是，获取文件对象依旧按照 r w a x 自己的特征
+ \+ 不能单独使用，可以认为它是为前面的模式字符做增强功能的

#### 文件指针

mode=r，指针起始在0

mode=a，指针起始在EOF

tell() 显示指针当前位置

seek(offset[,whence]) 移动文件指针位置，offset 偏移多少字节，whence 从哪里开始

文本模式下

whence 0  缺省值，表示从头开始，offset 只能正整数

whence 1 表示从当前位置，offset 只接受 0

whence 2 表示从 EOF 开始，offset 只接受 0

```bash
# 文本模式
f = open("test4","r+")
print(1,"-->",f.tell()) # 起始
print(2,"-->",f.read())
print(3,"-->",f.tell()) # EOF
print(4,"-->",f.seek(0)) # 起始
print(5,"-->",f.read())
print(6,"-->",f.seek(2,0))
print(7,"-->",f.read())
# print(8,"-->",f.seek(2,1)) # UnsupportedOperation: can't do nonzero cur-relative seeks
# print(9,"-->",f.seek(2,2)) # UnsupportedOperation: can't do nonzero cur-relative seeks
f.close()

# 中文
f = open("test4","w+")
print(11,"-->",f.write("自学成才")) # 这个地方要注意一下，windows 的编码是 gbk
print(12,"-->",f.tell())
f.close()

f = open("test4","r+")
print(31,"-->",f.read(3))
print(32,"-->",f.seek(1))
print(33,"-->",f.tell())
# print(34,"-->",f.read()) # 报错,因为一个汉字用 gbk 编码，至少是 2 个字节
print(35,"-->",f.seek(2))
print(36,"-->",f.read())
f.close()
-----------------------------------------------------------------------------------------
1 --> 0
2 --> 自学成才
3 --> 8
4 --> 0
5 --> 自学成才
6 --> 2
7 --> 学成才
11 --> 4
12 --> 8
31 --> 自学成
32 --> 1
33 --> 1
35 --> 2
36 --> 学成才
```

+ 文本模式支持从开头向后偏移的方式
+ whence 为 1 表示从当前位置开始偏移，但是只支持偏移 0，相当于原地不动，所以没什么用
+ whence 为 2 表示从 EOF 开始，只支持偏移 0，相当于移动文件指针到 EOF
+ seek 是按照字节偏移的

二进制模式下

whence 0 缺省值，表示从头开始，offset 只能正整数

whence 1 表示从当前位置，offset 可正可负

whence 2 表示从 EOF 开始，offset 可正可负

```bash
# 二进制模式
f = open("test4","rb+")
print(1,"-->",f.tell())
print(2,"-->",f.read())
print(3,"-->",f.tell()) # EOF
print(4,"-->",f.write(b"abc"))
print(5,"-->",f.seek(0)) # 起始
print(6,"-->",f.seek(2,1)) # 从当前指针开始，向后 2
print(7,"-->",f.read())
print(8,"-->",f.seek(-2,1)) # 从当前指针开始，向前 2

print(9,"-->",f.seek(2,2)) # 从 EOF 开始，向后2
print(10,"-->",f.seek(0))
print(11,"-->",f.seek(-2,2)) # 从 EOF 开始，向前 2
print(12,"-->",f.read())

# print(13,"-->",f.seek(-20,2)) # OSError
f.close()
------------------------------------------------------
1 --> 0
2 --> b'\xd7\xd4\xd1\xa7\xb3\xc9\xb2\xc5abc'
3 --> 11
4 --> 3
5 --> 0
6 --> 2
7 --> b'\xd1\xa7\xb3\xc9\xb2\xc5abcabc'
8 --> 12
9 --> 16
10 --> 0
11 --> 12
12 --> b'bc'
```

+ 二进制模式支持任意起点的偏移，从头，从尾，从中间位置开始
+ 向后 seek 可以超界，向是向前 seek 的时候，不能超界，否则抛异常

#### buffering 缓冲区

-1 表示使用缺省大小的 buffer，如果是二进制模式，使用 io.DEFAULT_BUFFER_SIZE 值，默认是 4096 或者 8192；如果是文本模式，如果是终端设备，是行缓存方式，如果不是，则使用二进制模式的策略

1. 0 只在二进制模式使用，表示关 buffer
2. 1 只在文本模式使用，表示使用行缓冲，意思就是见到换行符就 flush
3. 大于 1 用于指定 buffer 的大小

**buffer 缓冲区**，缓冲区是一段内存空间，一般来说是一个 FIFO 队列，到缓冲区满了或者达到阈值，数据才会 flush 到磁盘

flush() 将缓冲区数据写入磁盘

close() 关闭前会调用 flush()

io.DEFAULT_BUFFER_SIZE 缺省缓冲区大小，字节

二进制模式

```bash
import io

f = open("test4","w+b")
print(io.DEFAULT_BUFFER_SIZE)
f.write("ccyunchina.com".encode())
!cat test4 # 没有输出
f.seek(0)
!cat test4 # 有输出，动了指针，f.read()也会刷新
f.write("www.ccyunchina.com".encode())
f.flush()
!cat test4 # 有输出
f.close()
-------------------------------------------------------------
8192
ccyunchina.com
www.ccyunchina.com

# 不调用 f.read()、f.seek()，也不手动刷新，直观感受撑满缓冲，自动刷新
f = open("test4","w+b",4)
f.write(b"yul")
!cat test4 # 没有输出
f.write(b"l")
!cat test4 # 没有输出，这里刚好满4个字节
f.write(b"!")
!cat test4 # 缓存已满，自动刷入磁盘
f.close()
-------------------------------------------------------------
yull # "!" 号为什么没有出来，"!" 号将刚满的4个字节刷到磁盘，自己留在缓冲区
```

文本模式

```bash
# buffer=1, 使用行缓冲
f = open("test4","w+",1)
f.write("ccyun")
!cat test4
f.write("ccyunchian"*4)
!cat test4 # 到这里还是不会有输出
f.write("\n")
!cat test4 # 有输出
f.write("Hello\nPython")
!cat test4
f.close()
-------------------------------------------------------------
ccyunccyunchianccyunchianccyunchianccyunchian
ccyunccyunchianccyunchianccyunchianccyunchian
Hello
Python

# buffering>1, 使用指定大小的缓冲区
f = open("test4","w+",15)
f.write("ccyun")
!cat test4
f.write("china")
!cat test4
f.write("Hello\n")
!cat test4 # 没有输出
f.write("\nPython")
!cat test4 # 没有输出
f.close() # 由此可见，在文本模式下，设置大于1的数值没有多大作用
-------------------------------------------------------------
无输出

import io

f = open("test4","w+")
f.write("a" * (io.DEFAULT_BUFFER_SIZE - 20))
!cat test4 # 没有输出
f.write("a" * 20)
!cat test4 # 没有输出

f.write("a")
!cat test4 # 刚好输出

f.close()
```

buffering = 0

这是一种特殊的二进制模式，不需要内存的 buffer，可以看作是一个 FIFO 的文件

```bash
f = open("test4","wb+",0)
f.write(b"m")
!cat test4
f.write(b"a")
!cat test4
f.write(b"g")
!cat test4
f.write(b"magedu"*4)
!cat test4
f.write(b"\n")
!cat test4
f.write(b"Hello\nPython")
!cat test4
f.close()
-------------------------------------------------------------
m
ma
mag
magmagedumagedumagedumagedu
magmagedumagedumagedumagedu
magmagedumagedumagedumagedu
Hello
Python
```

| buffering      | 说明                                                         |
| :------------- | :----------------------------------------------------------- |
| buffering = -1 | t 和 b，都是io.DEFAULT_BUFFER_SIZE                           |
| buffering = 0  | b 关闭缓冲区<br />t 不支持                                   |
| buffering = 1  | b 就 1 个字节<br />t 行缓冲，遇到换行符才 flush              |
| buffering > 1  | b 模式表示行缓冲大小。缓冲区的值可以超过 io.DEFAULT_BUFFER_SIZE，直到设定的值超出后才把缓冲区 flush<br />t 模式，是 io.DEFAULT_BUFFER_SIZE，flush 完后把当前字符串也写入磁盘 |

似乎看起来比较麻烦，一般来说，只需要记得：

1. 文本模式，一般都用默认缓冲区大小
2. 二进制模式，是一个个字节的操作，可以指定 buffer 的大小
3. 一般来说，默认缓冲区的大小是个比较好的选择，除非明确知道，否则不调整它
4. 一般编程中，明确知道需要写磁盘了，都会手动调用一次 flush，而不是等到自动 flush 或者 close 的时候

#### encoding：编码，仅文本模式使用

None 表示使用缺省编码，依赖操作系统。 windows、linux 下测试如下代码

```bash
f = open("test1","w")
f.write("啊")
f.close()
```

windows 下缺省 GBK( 0xB0A1 )，Linux 下缺省 UTF-8( 0xE5 95 8A )

#### 其它参数

errors

什么样的编码