# 作者：lianghc

连接：<http://blog.csdn.net/zutsoft/article/details/51498026>

# merge  通过键拼接列

pandas提供了一个类似于关系数据库的连接(join)操作的方法<Strong>merage</Strong>,可以根据一个或多个键将不同DataFrame中的行连接起来
语法如下

**[python]** [view plain](http://blog.csdn.net/zutsoft/article/details/51498026#) [copy](http://blog.csdn.net/zutsoft/article/details/51498026#)

1. merge(left, right, how='inner', on=None, left_on=None, right_on=None,  
2. ​      left_index=False, right_index=False, sort=True,  
3. ​      suffixes=('_x', '_y'), copy=True, indicator=False)  

用于通过一个或多个键将两个数据集的行连接起来，类似于 SQL 中的 JOIN。该函数的典型应用场景是，针对同一个主键存在两张包含不同字段的表，现在我们想把他们整合到一张表里。在此典型情况下，结果集的行数并没有增加，列数则为两个元数据的列数和减去连接键的数量。

on=None 用于显示指定列名（键名），如果该列在两个对象上的列名不同，则可以通过 left_on=None, right_on=None 来分别指定。或者想直接使用行索引作为连接键的话，就将 left_index=False, right_index=False 设为 True。

how='inner' 参数指的是当左右两个对象中存在不重合的键时，取结果的方式：inner 代表交集；outer 代表并集；left 和 right 分别为取一边。

suffixes=('_x','_y') 指的是当左右对象中存在除连接键外的同名列时，结果集中的区分方式，可以各加一个小尾巴。

对于多对多连接，结果采用的是行的笛卡尔积。

参数说明：
left与right：两个不同的DataFrame
how：指的是合并(连接)的方式有inner(内连接),left(左外连接),right(右外连接),outer(全外连接);默认为inner
on : 指的是用于连接的列索引名称。必须存在右右两个DataFrame对象中，如果没有指定且其他参数也未指定则以两个DataFrame的列名交集做为连接键
left_on：左则DataFrame中用作连接键的列名;这个参数中左右列名不相同，但代表的含义相同时非常有用。
right_on：右则DataFrame中用作 连接键的列名
left_index：使用左则DataFrame中的行索引做为连接键
right_index：使用右则DataFrame中的行索引做为连接键
sort：默认为True，将合并的数据进行排序。在大多数情况下设置为False可以提高性能
suffixes：字符串值组成的元组，用于指定当左右DataFrame存在相同列名时在列名后面附加的后缀名称，默认为('_x','_y')
copy：默认为True,总是将数据复制到数据结构中；大多数情况下设置为False可以提高性能
indicator：在 0.17.0中还增加了一个显示合并数据中来源情况；如只来自己于左边(left_only)、两者(both)

## merge一些特性示例：

1.默认以重叠的列名当做连接键。

**[html]** [view plain](http://blog.csdn.net/zutsoft/article/details/51498026#) [copy](http://blog.csdn.net/zutsoft/article/details/51498026#)

1. In [16]: df1=DataFrame({'key':['a','b','b'],'data1':range(3)})  
2.   
3. In [17]: df2=DataFrame({'key':['a','b','c'],'data2':range(3)})  
4.   
5. In [18]: pd.merge(df1,df2)   #没有指定连接键，默认用重叠列名，没有指定连接方式  
6. Out[18]:   
7.    data1 key  data2  
8. 0      0   a      0  
9. 1      1   b      1  
10. 2      2   b      1  

2.默认做inner连接（取key的交集），连接方式还有（left,right,outer)，制定连接方式加参数：how=''

**[python]** [view plain](http://blog.csdn.net/zutsoft/article/details/51498026#) [copy](http://blog.csdn.net/zutsoft/article/details/51498026#)

1. In [19]: pd.merge(df2,df1)  
2. Out[19]:   
3.    data2 key  data1  
4. 0      0   a      0  
5. 1      1   b      1  
6. 2      1   b      2                   #默认内连接，可以看见c没有连接上。  
7.   
8. In [20]: pd.merge(df2,df1,how='left')    #通过how，指定连接方式  
9. Out[20]:   
10.    data2 key  data1  
11. 0      0   a      0  
12. 1      1   b      1  
13. 2      1   b      2  
14. 3      2   c    NaN  

3.多键连接时将连接键组成列表传入，例：pd.merge(df1,df2,on=['key1','key2']

**[python]** [view plain](http://blog.csdn.net/zutsoft/article/details/51498026#) [copy](http://blog.csdn.net/zutsoft/article/details/51498026#)

1. In [23]: right=DataFrame({'key1':['foo','foo','bar','bar'],  
2. ​    ...:     'key2':['one','one','one','two'],  
3. ​    ...:     'lval':[4,5,6,7]})  
4.   
5. In [24]: left=DataFrame({'key1':['foo','foo','bar'],  
6. ​    ...:     'key2':['one','two','one'],  
7. ​    ...:     'lval':[1,2,3]})  
8.   
9. In [25]: right=DataFrame({'key1':['foo','foo','bar','bar'],  
10. ​    ...:     'key2':['one','one','one','two'],  
11. ​    ...:     'lval':[4,5,6,7]})  
12.   
13. In [26]: pd.merge(left,right,on=['key1','key2'],how='outer')  #传出数组  
14. Out[26]:   
15.   key1 key2  lval_x  lval_y  
16. 0  foo  one       1       4  
17. 1  foo  one       1       5  
18. 2  foo  two       2     NaN  
19. 3  bar  one       3       6  
20. 4  bar  two     NaN       7  

4.如果两个对象的列名不同，可以分别指定，例：pd.merge(df1,df2,left_on='lkey',right_on='rkey')

**[html]** [view plain](http://blog.csdn.net/zutsoft/article/details/51498026#) [copy](http://blog.csdn.net/zutsoft/article/details/51498026#)

1. In [31]: df3=DataFrame({'key3':['foo','foo','bar','bar'], #将上面的right的key 改了名字  
2. ​    ...:     'key4':['one','one','one','two'],  
3. ​    ...:     'lval':[4,5,6,7]})  
4.   
5. In [32]: pd.merge(left,df3,left_on='key1',right_on='key3')  #键名不同的连接  
6. Out[32]:    
7.   key1 key2  lval_x key3 key4  lval_y  
8. 0  foo  one       1  foo  one       4  
9. 1  foo  one       1  foo  one       5  
10. 2  foo  two       2  foo  one       4  
11. 3  foo  two       2  foo  one       5  
12. 4  bar  one       3  bar  one       6  
13. 5  bar  one       3  bar  two       7  

5.以索引当做连接键，使用参数left_index=true,right_index=True （最好使用join）

# join 拼接列，主要用于索引上的合并

**[python]** [view plain](http://blog.csdn.net/zutsoft/article/details/51498026#) [copy](http://blog.csdn.net/zutsoft/article/details/51498026#)

1. join(self, other, on=None, how='left', lsuffix='', rsuffix='',sort=False):  

1.默认按索引合并，可以合并相同或相似的索引，不管他们有没有重叠列。

2.可以连接多个DataFrame

3.可以连接除索引外的其他列

4.连接方式用参数how控制

5.通过lsuffix='', rsuffix='' 区分相同列名的列

# concat  可以沿着一条轴将多个对象堆叠到一起

**[python]** [view plain](http://blog.csdn.net/zutsoft/article/details/51498026#) [copy](http://blog.csdn.net/zutsoft/article/details/51498026#)

1. concat(objs, axis=0, join='outer', join_axes=None, ignore_index=False,  
2. ​          keys=None, levels=None, names=None, verify_integrity=False, copy=True):  

在默认的 **axis=0 **情况下，pd.concat([obj1,obj2]) 函数的效果与 obj1.append(obj2) 是相同的；

而在 **axis=1 **的情况下，pd.concat([df1,df2],axis=1) 的效果与 pd.merge(df1,df2,left_index=True,right_index=True,how='outer') 是相同的。

可以理解为 concat 函数使用索引作为“连接键”。
本函数的全部参数为：

**[python]** [view plain](http://blog.csdn.net/zutsoft/article/details/51498026#) [copy](http://blog.csdn.net/zutsoft/article/details/51498026#)

1. pd.concat(objs, axis=0, join='outer', join_axes=None, ignore_index=False, keys=None, levels=None, names=None, verify_integrity=False)。  

objs 就是需要连接的对象集合，一般是列表或字典；

axis=0 是连接轴向join='outer' 参数作用于当另一条轴的 index 不重叠的时候，只有 'inner' 和 'outer' 可选（顺带展示 ignore_index=True 的用法）

concat 一些特点：

1.作用于Series时，如果在axis=0时，类似union。axis=1 时，组成一个DataFrame，索引是union后的，列是类似join后的结果。

2.通过参数join_axes=[] 指定自定义索引。

3.通过参数keys=[] 创建层次化索引

4.通过参数ignore_index=True 重建索引。

**[python]** [view plain](http://blog.csdn.net/zutsoft/article/details/51498026#) [copy](http://blog.csdn.net/zutsoft/article/details/51498026#)

1. In [5]: df1=DataFrame(np.random.randn(3,4),columns=['a','b','c','d'])  
2.   
3. In [6]: df2=DataFrame(np.random.randn(2,3),columns=['b','d','a'])  
4.   
5. In [7]: pd.concat([df1,df2])  
6. Out[7]:   
7. ​          a         b         c         d  
8. 0 -0.848557 -1.163877 -0.306148 -1.163944  
9. 1  1.358759  1.159369 -0.532110  2.183934  
10. 2  0.532117  0.788350  0.703752 -2.620643  
11. 0 -0.316156 -0.707832       NaN -0.416589  
12. 1  0.406830  1.345932       NaN -1.874817  

**[python]** [view plain](http://blog.csdn.net/zutsoft/article/details/51498026#) [copy](http://blog.csdn.net/zutsoft/article/details/51498026#)

1. In [8]: pd.concat([df1,df2],ignore_index=True)  
2. Out[8]:   
3. ​          a         b         c         d  
4. 0 -0.848557 -1.163877 -0.306148 -1.163944  
5. 1  1.358759  1.159369 -0.532110  2.183934  
6. 2  0.532117  0.788350  0.703752 -2.620643  
7. 3 -0.316156 -0.707832       NaN -0.416589  
8. 4  0.406830  1.345932       NaN -1.874817  
