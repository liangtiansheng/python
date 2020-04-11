# 练习

>  字典扁平化

```bash
# 源字典 {'a':{'b':1,'c':2},'d':{'e':3,'f':{'g':4}}}
# 目标字典 {'a.c':2,'d.e':3,'d.f.g':4,'a.b':1}
source = {'a':{'b':1,'c':2},'d':{'e':3,'f':{'g':4}}}
target = {}
# recursion
def flatmap(src,prefix=""):
    for k,v in src.items():
        if isinstance(v,(list,tuple,set,dict)):
            flatmap(v,prefix=prefix+k+".")
        else:
            target[prefix+k] = v
flatmap(source)
print(target)
------------------------------------------------------
{'a.b': 1, 'd.f.g': 4, 'a.c': 2, 'd.e': 3}
```

一般这种函数都会生成一个新的字典，因此改造一下

dest 字典可以由内部创建，也可以由外部提供

```bash
source = {'a':{'b':1,'c':2},'d':{'e':3,'f':{'g':4}}}

# recursion
def flatmap(src,dest=None,prefix=""):
    if dest == None:
        dest = {}
    for k,v in src.items():
        if isinstance(v,(list,dict,tuple,set)):
            flatmap(v,dest,prefix=prefix+k+".") # 递归调用
        else:
            dest[prefix+k] = v
    return dest
print(flatmap(source))
---------------------------------------------------------
{'a.b': 1, 'd.f.g': 4, 'a.c': 2, 'd.e': 3}
```

能否不暴露给外界内部的字典？

能否函数就提供一个参数源字典，返回一个新的扁平化字典？

递归时候要把目标字典的引用传递多层，怎么处理？

```bash
source = {'a':{'b':1,'c':2},'d':{'e':3,'f':{'g':4}}}

# recursion
def flatmap(src):
    def _flatmap(src,dest=None,prefix=""):
        for k,v in src.items():
            key = prefix + k
            if isinstance(v,(list,tuple,set,dict)):
                _flatmap(v,dest,key+".")
            else:
                dest[key] = v
    dest = {}
    _flatmap(src,dest)
    return dest
print(flatmap(source))
----------------------------------------------------------
{'a.b': 1, 'd.f.g': 4, 'a.c': 2, 'd.e': 3}
```

> 实现 Base64 编码解码

Base64 位编码索引表

![](D:\github\python\basic_grammar\images\Base64.png)

Base64 位编码的基本原则

+ 将输入的字符串每 3 个字节断开，拿出一个 3 字节，每 6 个 bit 断开成 4 段
+ 每一段当作一个 8bit 看它的值，这个值就是 Base64 编码表的索引值，找到对应字符
+ 虽然每一段看作 8bit，但是有效值是 6bit，2**6=64，因此有了 Base64 的编码表
+ 继续往后取 3 个字节，同样处理，直到最后

举例说明

```bash
# 如果取的是abc，对应的ASCII码为：0x61 0x62 0x63
01100001 01100010 01100011 # abc的二进制
011000	010110	001001	100011 # 每6位一组，前面可以补2个0看作8位
  24      22      9       35
  
# 有时也不可能刚好3个字节，如果取的是a，怎么处理？
# a 对应的ASCII码为：0x61
01100001 # a的二进制
011000 010000 000000 000000
```

+ 正好 3 个字节，处理方式同 abc
+ 剩 1 个字节或 2 个字节，用 0 补满 3 个字节
+ 补 0 的字节用 = 表示，这里要注意，如果是 01100000，变成 4 个 6 位，相当于补了 2 个 =，因为第 2 个 6 位有两个有效 0

自己实现对一段字符串进行base64编码

```bash
alphabet = b"ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"
teststr="abcd"

def base64(src):
    src = src.encode() # 将字符串转换成 bytes
    ret = bytearray()
    length = len(src)
    
    r = 0 # 记录补 0 的个数
    for offset in range(0,length,3):
        if offset + 3 <= length:
            triple = src[offset:offset + 3]
        else:
            triple = src[offset:]
            r = 3 - len(triple)
            triple = triple + b"\x00" * r
        # 将 3 个字节看成一个整体，得到整数，大端模式
        # abc => 0x616263
        b = int.from_bytes(triple,"big")
        print(b,hex(b))
        
        # 01100001 01100010 01100011 # abc
        # 011000  010110  001001  100011 # 每 6 位断开
        for i in range(18,-1,-6):
            if i == 18:
                index = b >> i
            else:
                index = b >> i & 0x3F
            ret.append(alphabet[index]) # 得到 base64 编码的列表
            
        # 补了几个 0 就替换几个 =
        for i in range(1,r+1):
            ret[-i] = 0x3D
    return ret
print(base64(teststr))

print("======================")

import base64
print(base64.b64encode(teststr.encode()))
----------------------------------------------------------------------
6382179 0x616263
6553600 0x640000
bytearray(b'YWJjZA==')
======================
b'YWJjZA=='
```

base64 解码实现

```bash
alphabet = b"ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"

def base64decode(src:bytes):
    ret = bytearray()
    length = len(src)
    
    step = 4 # 对齐的，每次取4个
    for offset in range(0,length,step):
        tmp = 0x00
        block = src[offset:offset + step]
        
        # 开始移位计算
        for i,c in enumerate(reversed(block)):
            index = alphabet.find(c) # 替换字符为序号
            if index == -1:
                continue # 找不到就是 0，不用移位相加了
            tmp += index << i*6
        ret.extend(tmp.to_bytes(3,'big'))
    return bytes(ret.rstrip(b"\x00")) # 把最右边的 \x00 去掉，不可变
txt = "SSBsb3ZlIHlvdSAqIA=="
txt = txt.encode()
print(txt)
print(base64decode(txt).decode())
print()

import base64
print(base64.b64decode(txt).decode())
---------------------------------------------------------------------------------
b'SSBsb3ZlIHlvdSAqIA=='
I love you * 

I love you * 
```

改进：1. reversed 可以不需要，2.  alphabet.find 效率低

```bash
from collections import OrderedDict

base_tbl = b"ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"
alphabet = OrderedDict(zip(base_tbl,range(64)))

def base64decode(src:bytes):
    ret = bytearray()
    length = len(src)
    
    step = 4 # 对齐的，每次取 4 个
    for offset in range(0,length,step):
        tmp = 0x00
        block = src[offset:offset + step]
        
        # 开始移位计算
        for i in range(4):
            index = alphabet.get(block[-i-1])
            if index is not None:
                tmp += index << i*6
        ret.extend(tmp.to_bytes(3,"big"))
    return bytes(ret.rstrip(b"\x00")) # 把最右边的 \x00 去掉，不可变

txt = "SSBsb3ZlIHlvdSAqIA=="
txt = txt.encode()
print(base64decode(txt).decode())
print()

import base64
print(base64.b64decode(txt).decode())
-----------------------------------------------------------------------------
I love you * 

I love you * 
```

> 写一个命令分发器

1. 程序员可以方便的注册函数到某一个命令，用户输入命令时，路由到注册的函数
2. 如果此命令没有对应的注册了函数，执行默认函数
3. 用户输入用 input(">>")

```bash
# 基本框架
cmds = {}

def ls():
    print("我是 ls ")
def default_func():
    print("没有注册")
def reg(cmd,fn):
    cmds[cmd] = fn

reg("ls",ls)
    
def dispatch():
    while True:
        cmd = input("please input cmd:")
        if cmd == "quit":
            break
        else:
            cmds.get(cmd,default_func)()
dispatch()
------------------------------------------------------
please input cmd:ls
我是 ls 
please input cmd:cat
没有注册
please input cmd:quit
```

此代码很丑陋，所有函数和字典都在全局中定义，不好看，如何改进？

封装，将 reg 函数封装成装饰器，并用它来注册函数

```bash
def reg(cmd):
    def _reg(fn):
        cmds[cmd] = fn
        return fn
    return _reg

@reg("ls") # 等价 ls = reg("ls")(ls)
def ls():
    print("我是 ls ")
```

用户一般只用到调度各注册，是否可以把字典、reg、dispatcher等封装起来

```bash
def cmd_dispatcher():
    cmds = {}
    
    def reg(cmd): # 注册函数
        def _reg(fn):
            cmds[cmd] = fn
            return fn
        return _reg
    def default_func():
        print("没有注册")
        
    def dispatcher():
        while True:
            cmd = input(">>")
            
            if cmd.strip() == "":
                return
            cmds.get(cmd,default_func)()
    return reg,dispatcher
reg,dispatcher = cmd_dispatcher()

@reg("ls") # 等价 ls = reg("ls")(ls)
def ls():
    print("我是 ls ")  
    
dispatcher()
---------------------------------------------------------------------------
>>ls
我是 ls 
>>cat
没有注册
>>
```

到这里，代码优雅了不少，不过这样的代码在业务上也存在一些**问题**

+ 如果一个函数使用同样的 cmd 名称注册，就等于覆盖了原来的 cmd 到 fn 的关系，这样的逻辑也是合理的
+ 也可以加一个判断，如果 key 已存在，重复注册，抛出异常，看业务要求
+ 有注册应该有注销，注销得有权限，什么样的人才有这个权限，看业务需求

> 求2个字符串的最长公共子串

s1 = "abcdefg"

s2 = "defabcd"

矩阵算法：让s2的每一个元素，去分别和s1的每一个元素比较，相同就是1，不同就是0，有下面的矩阵

|         | s1(abcdefg) |
| :-----: | :---------: |
| s2("d") |   0001000   |
| s2("e") |   0000100   |
| s2("f") |   0000010   |
| s2("a") |   1000000   |
| s2("b") |   0100000   |
| s2("c") |   0010000   |
| s2("d") |   0001000   |

观察，将相邻的1串起来组成一条斜线，哪条斜线最长就是最长公共子串

扫描的过程就是 len(s1) * len(s2) 次，O(n*m) 的效率

核心算法就是 s2 的单个字符到 s1 中遍历，相等的话，就在上一个数(横纵坐标减 1 记录的那个数)的基础上加 1，如果是边界情况，直接赋值 1 就好，如下图矩阵所示

|         | s1(abcdefg) |
| :-----: | :---------: |
| s2("d") |   0001000   |
| s2("e") |   0000200   |
| s2("f") |   0000030   |
| s2("a") |   1000000   |
| s2("b") |   0200000   |
| s2("c") |   0030000   |
| s2("d") |   0004000   |

```bash
s1 = "abcdefg"
s2 = "defabcd"

def findit(str1,str2):
    matrix = []
    
    # 从 x 轴或者 y 轴取都可以，选择 x 轴，xmax 和 xindex
    xmax = 0
    xindex = 0
    for i,x in enumerate(str2):
        matrix.append([])
        for j,y in enumerate(str1):
            if x != y: # 若两个字符不相等
                matrix[i].append(0)
            else:
                if i == 0 or j == 0:
                    matrix[i].append(1)
                else:
                    matrix[i].append(matrix[i-1][j-1] + 1)
                    
                if matrix[i][j] > xmax: # 判断当前加入的值和记录的最大值比较
                    xmax = matrix[i][j] # 记录最大值，用于下次比较
                    xindex = j # 记录当前值的 x 轴偏移量
    return str1[xindex+1-xmax:xindex+1]

print(findit(s1,s2))
---------------------------------------------------------------------------------
abcd
```

