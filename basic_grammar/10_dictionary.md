# Python3 字典dict

+ key-value键值对的数据的集合
+ 可变的 、 无序的 、 key不重复

## 字典dict定义

+ d = dict() 或者 d = {}
+ __dict(**kwargs)__ 使用name=value对初始化一个字典
+ dict(iterable, **kwarg) 使用可迭代对象和name=value对构造字典，不过可迭代对象的元素必须是一个 __二元__ 结构
+ dict(mapping, **kwarg) 使用一个字典构建另一个字典
+ d = {'a':10, 'b':20, 'c':None, 'd':[1,2,3]}

```bash
d1 = {'a':1,'b':2}
print(d1)

d2 = dict(a=1,b=2)
print(d2)

d3 = dict((('a',1),('b',2)))
print(d3)

d4 = dict([['a',1],['b',2]])
print(d4)

d5 = dict(('a',1))
print(d5)
-------------------------------------------------------------------
{'b': 2, 'a': 1}
{'b': 2, 'a': 1}
{'b': 2, 'a': 1}
{'b': 2, 'a': 1}
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-42-e43e2441f817> in <module>
     11 print(d4)
     12
---> 13 d5 = dict(('a',1))
     14 print(d5)

ValueError: dictionary update sequence element #0 has length 1; 2 is required

注：最后这个异常是因为，dict 后面接可迭代对象，可迭代对象中必须是二元组，所以只能像上面几种，用嵌套做到。
```

+ 类方法dict.fromkeys(iterable, value)

```bash
d1 = dict.fromkeys(range(5))
print(d1)

d2 = dict.fromkeys(range(5),10)
print(d2)

d3 = dict.fromkeys(range(5),[10])
print(d3)
print(id(d3[0]))
print(id(d3[1]))

d3[0][0]=20
print(d3)
-------------------------------------------------------------
{0: None, 1: None, 2: None, 3: None, 4: None}
{0: 10, 1: 10, 2: 10, 3: 10, 4: 10}
{0: [10], 1: [10], 2: [10], 3: [10], 4: [10]}
1648769246600
1648769246600
{0: [20], 1: [20], 2: [20], 3: [20], 4: [20]}
```

## 字典元素的访问

> d[key]

+ 返回key对应的值value
+ key不存在抛出KeyError异常

```bash
d = {'a':1,'b':2,'c':3}
print(d['a'])
print(d['d'])
--------------------------------------------
1
---------------------------------------------------------------------------
KeyError                                  Traceback (most recent call last)
<ipython-input-54-60a384469287> in <module>
      1 d = {'a':1,'b':2,'c':3}
      2 print(d['a'])
----> 3 print(d['d'])

KeyError: 'd'
```

> get(key[, default])

+ 返回key对应的值value
+ key不存在返回缺省值，如果没有设置缺省值就返回None

```bash
d = {'a':1,'b':2,'c':3}
print(d.get('a'))
print(d.get('d'))
print(d.get('d','substitude'))
--------------------------------------------
1
None
substitude
```

> setdefault(key[, default])

+ 返回key对应的值value
+ key不存在，添加kv对，value为default，并返回default，如果default没有设置，缺省为None

```bash
d = {'a':1,'b':2,'c':3}
print(d.setdefault('a'))
print(d.setdefault('d','substitude'))
print(d)
-------------------------------------------------
1
substitude
{'d': 'substitude', 'c': 3, 'b': 2, 'a': 1}
```

## 字典增加和修改

> d[key] = value

+ 将key对应的值修改为value
+ __key不存在添加新的kv对__

> update([other]) -> None

+ 使用另一个字典的kv对更新本字典
+ key不存在，就添加
+ key存在，覆盖已经存在的key对应的值
+ 就地修改

```bash
print(d)
d.update(red=1)
d.update((('red',2),))
d.update({'red':3})
print(d)
-------------------------------------------------
{'d': 'substitude', 'c': 3, 'b': 2, 'a': 1}
{'d': 'substitude', 'red': 3, 'a': 1, 'c': 3, 'b': 2}
```

## 字典删除

> pop(key[, default])

+ key存在，移除它，并返回它的value
+ key不存在，返回给定的default
+ default未设置，key不存在则抛出KeyError异常

```bash
d = {'a':1,'b':2,'c':3}
print(d.pop('d','nothing'))
print(d.pop('d'))
--------------------------------------
nothing
---------------------------------------------------------------------------
KeyError                                  Traceback (most recent call last)
<ipython-input-80-2e770a681a96> in <module>
      1 d = {'a':1,'b':2,'c':3}
      2 print(d.pop('d','nothing'))
----> 3 print(d.pop('d'))

KeyError: 'd'

```

> popitem()

+ 移除并返回一个任意的键值对
+ 字典为empty，抛出KeyError异常

```bash
d = {'a':1,'b':2,'c':3}
print(d.popitem())
print(d.popitem())
print(d.popitem())
print(d.popitem())
------------------------------------------------------
('c', 3)
('b', 2)
('a', 1)
---------------------------------------------------------------------------
KeyError                                  Traceback (most recent call last)
<ipython-input-84-755869381dc0> in <module>
      3 print(d.popitem())
      4 print(d.popitem())
----> 5 print(d.popitem())

KeyError: 'popitem(): dictionary is empty'
```

> clear()

+ 清空字典

> del语句

```bash
d = {'a': 1, 'b': 2, 'c': 3, 'd': 'substitude', 'red': 3}
c = d
print(c)
print(d)

del(d['a'])
print(c)
print(d)

del(d)
print(c)
-----------------------------------------------------------
{'red': 3, 'd': 'substitude', 'c': 3, 'b': 2, 'a': 1}
{'red': 3, 'd': 'substitude', 'c': 3, 'b': 2, 'a': 1}
{'red': 3, 'd': 'substitude', 'c': 3, 'b': 2}
{'red': 3, 'd': 'substitude', 'c': 3, 'b': 2}
{'red': 3, 'd': 'substitude', 'c': 3, 'b': 2}

注：del a['c'] 看着像删除了一个对象，本质上减少了一个对象的引用，del 实际上删除的是名称，而不是对象，对象由GC处理，另外一定要熟悉内存中深浅拷贝的本质。
```

## 字典遍历

> 遍历key

```bash
for k in d:
    print(k)

for k in d.keys():
    print(k)
```

> 遍历value

```bash
for k in d:
    print(d[k])

for k in d.keys():
    print(d.get(k))

for v in d.values():
    print(v)
```

> 遍历item，即kv对

```bash
for item in d.items():
    print(item)

for item in d.items():
    print(item[0], item[1])

for k,v in d.items():
    print(k, v)

for k, _ in d.items():
    print(k)

for _ ,v in d.items():
    print(v)
```

> 总结

+ Python3中，keys、values、items方法返回一个类似一个生成器的可迭代对象，不会把函数的返回结果复制到内存中  
  + Dictionary view对象
  + 字典的entry的动态的视图，字典变化，视图将反映出这些变化
+ Python2中，上面的方法会返回一个新的列表，占据新的内存空间。所以Python2建议使用iterkeys、itervalues、iteritems版本，返回一个迭代器，而不是一个copy

## 字典遍历和移除

> 如何在遍历的时候移除元素

```bash
# 错误的做法
d = dict(a=1, b=2, c='abc')
for k,v in d.items():
    d.pop(k) # 异常
---------------------------------------------------------------------------
RuntimeError                              Traceback (most recent call last)
<ipython-input-12-0f05b4388341> in <module>
      1 d = dict(a=1, b=2, c='abc')
----> 2 for k,v in d.items():
      3     d.pop(k) # 异常

RuntimeError: dictionary changed size during iteration

while len(d): # 相当于清空，不如直接clear()
    print(d.popitem())
```

```bash
# 正确的做法
d = dict(a=1, b=2, c='abc')
keys = []
for k,v in d.items():
    if isinstance(v, str):
        keys.append(k)

for k in keys:
    d.pop(k)
print(d)
```

## 字典的key

> key的要求和set的元素要求一致

+ set的元素可以就是看做key，set可以看做dict的简化版
+ hashable 可哈希才可以作为key，可以使用hash()测试
+ d = {1 : 0, 2.0 : 3, "abc" : None, ('hello', 'world', 'python') : "string", b'abc' : '135'}

## defaultdict

+ collections.defaultdict([default_factory[, ...]])
  + 第一个参数是default_factory，缺省是None，它提供一个初始化函数。当key不存在的时候，会调用这个工厂函数来生成key对应的value

```bash
import random
d1 = {}
for k in 'abcdef':
    for i in range(random.randint(1,5)):
        if k not in d1.keys():
            d1[k] = []
        d1[k].append(i)
print(d1)

对比：

from collections import defaultdict
import random

d1 = defaultdict(list)
for k in 'abcdef':
    for i in range(random.randint(1,5)):
        d1[k].append(i)
print(d1)
```

## OrderedDict

+ collections.OrderedDict([items])
  + key并不是按照加入的顺序排列，可以使用OrderedDict记录顺序

```bash
from collections import OrderedDict
import random
d = {'banana':3,'apple':4,'pear':1,'orange':2}
print(d)
keys = list(d.keys())
random.shuffle(keys)
print(keys)
od = OrderedDict()
for key in keys:
    od[key] = d[key]
print(od)
print(od.keys())
----------------------------------------------------------------
{'banana': 3, 'apple': 4, 'pear': 1, 'orange': 2}
['apple', 'pear', 'banana', 'orange']
OrderedDict([('apple', 4), ('pear', 1), ('banana', 3), ('orange', 2)])
odict_keys(['apple', 'pear', 'banana', 'orange'])
```

+ 有序字典可以记录元素插入的顺序，打印的时候也是按照这个顺序输出打印
+ 3.6版本的Python的字典就是记录key插入的顺序（IPython不一定有效果）
+ 应用场景：
  + 假如使用字典记录了N个产品，这些产品使用ID由小到大加入到字典中
  + 除了使用字典检索的遍历，有时候需要取出ID，但是希望是按照输入的顺序，因为输入顺序是有序的
  + 否则还需要重新把遍历到的值排序