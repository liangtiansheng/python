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

