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

> 实现 Base64 编码

Base64 位编码索引表

![](D:\github\python\basic_grammar\images\Base64.png)

Base64 位编码的基本原则，将输入每 3 个字节断开，拿出一个 3 字节，每 6 个 bit 断开成 4 段

