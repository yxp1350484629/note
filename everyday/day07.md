#  day07

1.很多时候在我们表达式中都会出现（)，但是元组的表示也是括号，表达式中的括号里面都是单个对象，所以为了区别开，如果元组里存的是单个对象,后面就要加，

值得注意的是

 2, 表示的是一个元组,200,300,600表示的也是一个元组，这也就是很多在解压以及序列赋值的时候出现的问题

2.x,y,z=200,300,600

x,y,z='lkm'

左边是变量，右边是序列，分别迭代出来依次赋值

3.元组+=运算只能和元组相加，不能和其他序列相加，毕竟元组的要求很精细，不像列表那样兼容和灵活

任何+ ,*都会改变地址，创建新的对象，因为元组不可变，而且由于元组及其内部不可变，所以不支持索引赋值和切片赋值，但单纯的索引，切片求值是可以的，索引和切片赋值就是为了改变对象内部

元组很多规则，以及方法count,index完全相同，只是元组永远不可变，所以remove等方法，以及赋值等方法肯定不能用，这些都是可变对象才能用的

4.reversed(),和sorted(对象**,reverse**=True,False)和a.reverse(),a.sort() 

最大的不同是a.reverse() a.sort()，是直接改变可变的可迭代对象，但是字符串序列以及元组都没有这两个方法，因为它们是不可变的

reversed和sorted方法也是为了弥补这些，他们是将给的序列，返回一个可迭代的对象，reversed返回的是一个reverse的迭代器，不改变传入对象。而sorted返回的默认就是一个列表的形式了，涉及到计算机内部的问题

有些方法是想改变不可变对象，所以不能用，而昨天所涉及的拷贝，不用拷贝不可变对象是因为，需要赋值的时候，值不同，地址肯定就不同，是全新的对象，而不是改变了不可变对象。

list,tuple,set方法都是创建了新的对象，list(元组,字符串)是可行的，他并不是改变

5.序列中用索引来查找值，跟整个数据的大小是没有多大的关系的，因为可以直接查找到

而 in ,not in是和数据量直接挂钩

6.字典的key必须是不可变对象，整数也行，a={1:'l',2:'l'}

7.dict方法，后面的参数可以接关键字参数，也就是关键字传参

还可以接可迭代对象中的序列 比如dict(['ac',('l','k'),['k','m']])

有两层，最外面那层一般是元组或者列表，里面那层是个序列，值得注意的是里面的序列，迭代出来必须是两个对象

8，字典中的增改查都有可以通过关键字来实现，删就直接用del

9.in 和 not in 在字典中直接使用，默认是查找字典的键，不过可以通过调用a.values()方法来查找值  对于for循环也一样。字典同样有any,all,max,min,len,不过他们都是默认对键进行操作，也可以用上面的方法 

a.keys() a.values()返回的是类似于列表的对象，不过并不单纯 是列表，可以用list转化一下

```python
 a.keys()
dict_keys(['kmj', 'lm', 'po'])
>>> a.keys()[0]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'dict_keys' object does not support indexing
>>> b=list(a.keys())
>>> b
['kmj', 'lm', 'po']
```

对于所有的可变序列，列表集合字典，都可以用clear()，来清空内部

10.值得注意的字典方法

a.update(b),由于字典与字典间不支持相加的，大概于无法出现重复的键，所以就搞了个update方法，也就是更新的意思，a,b都是字典，但如果有重复的键，就拿更新的键作为唯一，也就是更新的意思了。

a.get(键,default) get方法就是在a中寻找一个键，如果有，肯定就显示这个键对应的值  如果没有这个键，就是

  default的时候该输出什么，就给在后面的参数,比如a.get('lkm','没有这个键'),如果没有找到'lkm'这个键，就会在屏幕显示没有这个键

items方法是生成一个键值对的可迭代集合，列表中的元组

for t in a.items() 返回的是类似一个元组，元组里是键值对



犯的错误：

运行上面的a.get(键，defalut）当有的时候，是输出键对应的值，没有的时候输出后面的

当我们在实验窗口上，运行这个的时候，他会默认有个print，而在编辑窗口的时候，不加print什么都没有,因为这个函数有return，但是需要输出，才能将结果打印出来，这不是在实验窗口上。






