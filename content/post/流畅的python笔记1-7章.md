---
title: "流畅的python笔记1-7章"
date: 2019-06-27
categories:
- python
tags:
- pandas
---



## 第一章

1. 特殊方法 双下方法 

2. namedtuple适合构建只有少数属性但是没有方法的对象，比如数据库条目。 

3. len之所以不是普通方法，是为了让python自带的数据结构可以走后门。如果x是一个内置类型的实例，那么len(x)的速度会非常快，背后的原因是CPython会直接从一个C结构体里读取对象的长度，完全不会调用任何方法。 

4. ```__repr__```方便我们调试和记录日志。```__str__``` 给终端用户看的。 

5. 内置序列类型 

   - 容器序列 list tuple collections.deque这些序列能存放不同类型的数据 
   - 扁平序列 str bytes bytearray memoryview和array.array这类序列只能容纳一种类型 
     - 容器序列存放的是对象的引用，扁平序列里存放的是值而不是引用。扁平序列其实是一段连续的内存空间。 

   按照能否被修改来分类 

   - 可变序列 list, bytearray, array.array, collections, deque和memoryview 
   -  不可变序列 tuple str 和bytes 

6. python2中列表推导会作用域泄漏 

7. 列表推导的作用只有一个：生成列表。如果想生成其他类型的序列，可以用生成器表达式。生成器表达式遵守迭代器协议，可以逐个地产出元素，而不是先建立一个完整的列表，然后再把列表传递到某个构造函数中。用生成器表达式可以节省内存。 

8. 列表推导用方括号，生成器表达式用圆括号。 

如果要计算两个各有1000个元素的列表的笛卡尔积，生成器表达式可以帮忙省掉运行for循环的开销，即一个含有100万个元素的列表。 

9. 元组拆包。 

优雅交换中间变量 

b,a = a,b 

10. 具名元组 

![](https://axueya-1256523378.cos.ap-chengdu.myqcloud.com/20190627171911.png) 

11. 切片会忽略最后一个元素的原因 

为了可读性，可以给切片命名 

![](https://axueya-1256523378.cos.ap-chengdu.myqcloud.com/20190627172003.png) 

高维数组切片可以用...表示省略 

可以给切片赋值，但是要用可迭代对象，即使只有一个值，也要转成列表。 

12. ```+= *=``` 

在可变和不可变序列上 

+= 背后的方法是__iadd__，如果没有实现这个方法，会调用__add__,此时效果跟a=a+b一样。一般来说，可变序列都实现了__iadd__ 

对不可变序列进行重复拼接操作的话，效率会很低。str是个例外，cpython优化了它。 

13. list.sort() 就地排序，返回None，sorted()返回一个列表作为返回值 

两个方法都有参数reverse和key 

python的排序算法Timsort是稳定的。Timsort是一种自适应算法，会根据原序列的顺序特点来交替使用插入排序和归并排序，以达到最佳效率 

14. 计时器 

from time import perf_ counter as pc ➍ >>> t0 = pc(); floats /= 3; pc() - t0 ➎ 

15. collections.deque 

线程安全，可以快速从两端添加或删除 

适用于存放最近用到的几个元素，可以指定maxlen 

append 添加到右侧 

appendleft 添加到左侧 

extend 添加可迭代对象到右侧 

extendleft 到左侧 

pop 移除最后一个元素并返回它的值 

popleft 移除第一个元素并返回它的值 

16. 同步类（线程安全类） 

Queue LifoQueue PriorityQueue 

multiprocessing 

17.  容器：有些对象中包含对其他对象的引用，这种对象称为容器 

##  第三章 字典和集合 

1. python对字典做了高度优化，散列表是字典性能出众的根本原因 

python用开放定址法的二次探查法处理冲突 

2. 字典的键必须是可散列的 

![](https://axueya-1256523378.cos.ap-chengdu.myqcloud.com/20190627172218.png) 

原子不可变数据类型（str,bytes和数值类型）都是可散列的 

frozenset也是可散列的 

元组：只有当元组包含的所有元素都是可散列的才可以 

3. 字典推导 

![](https://axueya-1256523378.cos.ap-chengdu.myqcloud.com/20190627172257.png) 

4. defaultdict 

![](https://axueya-1256523378.cos.ap-chengdu.myqcloud.com/20190627172326.png) 

collections.defaultdict(list) 

list是default_factory 

5. keys() 

dict.keys()在python3中返回值是一个视图，视图就像一个集合，在视图中查找一个元素的速度很快。py2返回值是个列表，不会很快。 

6. Counter 

![](https://axueya-1256523378.cos.ap-chengdu.myqcloud.com/20190627172412.png) 

7. set 

![](https://axueya-1256523378.cos.ap-chengdu.myqcloud.com/20190627172504.png) 

8. 散列表 

加盐 

![](https://axueya-1256523378.cos.ap-chengdu.myqcloud.com/20190627172544.png) 

![](https://axueya-1256523378.cos.ap-chengdu.myqcloud.com/20190627172627.png) 

![](https://axueya-1256523378.cos.ap-chengdu.myqcloud.com/20190627172729.png) 

**优化是维护的对立面** 

往字典中添加新键可能会改变已有键的顺序（字典扩容） 

**不要同时迭代和修改字典** 

## 第四章 文本和字节序列 

1. 编码和解码 

\> Python3引入两个新类型 

\- bytes 

不可变字节类型 

\- byrearray 

字节数组 

可变 

\> 字符串与bytes 

\- 字符串是由字符组成的有序序列，字符可以使用编码来理解 

\- bytes是字节组成的有序的不可变序列 

\- bytearray是字节组成的有序的可变序列 

\> 编码与解码 

\- 字符串按照不同的字符集编码encode返回字节序列bytes 

``` 
encode(encoding="utf-8",errors="strict") -> bytes 
```



\- 字节序列按照不同的字符集解码decode返回字符串 

```
bytes.decode(encoding="utf-8",errors="strict") -> str 
```

```
bytearray.decode(encoding="utf-8".errors="strict") -> str 
```

![](http://ww1.sinaimg.cn/large/006tNc79ly1g4fuhtaf9mj30tm06cq4g.jpg) 

![](http://ww1.sinaimg.cn/large/006tNc79ly1g4fuinjdnrj30t40f0ac8.jpg) 

6. py3如何存储字符串 

![](http://ww3.sinaimg.cn/large/006tNc79ly1g4fuiz4jr7j30te086mzs.jpg) 

##  第五章 一等函数 

1. 高阶函数：接收函数作为参数，或是返回函数 的函数，如map,sorted,filter, reduce 

2. 列表推导替代map,filter 

3. reduce也不再是内置的规约函数，内置的有sum, all, any 

4. lambda表达式 创建匿名函数 

5. 可调用对象 

![](http://ww4.sinaimg.cn/large/006tNc79ly1g4fujmhp08j30u009uwgl.jpg) 

6. 使用callable看能不能调用，dir看有哪些属性 

7. 用\*指定仅限关键词参数 

![](http://ww2.sinaimg.cn/large/006tNc79ly1g4fukmqkjej30to0ciwgb.jpg) 

![](http://ww4.sinaimg.cn/large/006tNc79ly1g4fulcwntkj30uy06u402.jpg) 

8. 注解 

python只把注解存储在__annotations__中，不做其他处理 

inspect可以提取到 

9. fuctools.partial 

固定部分参数 

## 第六章 使用一等函数实现设计模式 

1. 策略模式：定义一系列算法，把它们一一封装起来，并且使它们可以相互替换。使算法可以独立于使用它的客户而变化 

2. 命令模式：目的是解耦调用操作的对象和提供实现的对象。 

![](http://ww3.sinaimg.cn/large/006tNc79ly1g4fumzcfj3j30sc0z0agm.jpg) 

##  第7章 函数装饰器与包 

1. 装饰器 

python中，装饰器返回的内部函数相当于装饰器实例，返回的函数包装了被装饰的函数。 

python内置的用于装饰方法的函数:property、classmethod、staticmethod 

lru_cache装饰方法适用于递归 