Python pandas 数据框的str列内置的方法详解

        在使用pandas框架的DataFrame的过程中，如果需要处理一些字符串的特性，例如判断某列是否包含一些关键字，某列的字符长度是否小于3等等这种需求，如果掌握str列内置的方法，处理起来会方便很多。

        下面我们来详细了解一下，Series类的str自带的方法有哪些。

1、cat() 拼接字符串
        例子：
        >>> Series(['a', 'b', 'c']).str.cat(['A', 'B', 'C'], sep=',')
        0 a,A
        1 b,B
        2 c,C
        dtype: object
        >>> Series(['a', 'b', 'c']).str.cat(sep=',')
        'a,b,c'
        >>> Series(['a', 'b']).str.cat([['x', 'y'], ['1', '2']], sep=',')
        0    a,x,1
        1    b,y,2
        dtype: object
		
2、split() 切分字符串
        >>> import numpy,pandas;
        >>> s = pandas.Series(['a_b_c', 'c_d_e', numpy.nan, 'f_g_h'])
        >>> s.str.split('_')
        0    [a, b, c]
        1    [c, d, e]
        2          NaN
        3    [f, g, h]
        dtype: object
        >>> s.str.split('_', -1)
        0    [a, b, c]
        1    [c, d, e]
        2          NaN
        3    [f, g, h]
        dtype: object
        >>> s.str.split('_', 0)
        0    [a, b, c]
        1    [c, d, e]
        2          NaN
        3    [f, g, h]
        dtype: object
        >>> s.str.split('_', 1)
        0    [a, b_c]
        1    [c, d_e]
        2         NaN
        3    [f, g_h]
        dtype: object
        >>> s.str.split('_', 2)
        0    [a, b, c]
        1    [c, d, e]
        2          NaN
        3    [f, g, h]
        dtype: object
        >>> s.str.split('_', 3)
        0    [a, b, c]
        1    [c, d, e]
        2          NaN
        3    [f, g, h]
        dtype: object
		
3、get()	获取指定位置的字符串
        >>> s.str.get(0)
        0      a
        1      c
        2    NaN
        3      f
        dtype: object
        >>> s.str.get(1)
        0      _
        1      _
        2    NaN
        3      _
        dtype: object
        >>> s.str.get(2)
        0      b
        1      d
        2    NaN
        3      g
        dtype: object
		
4、join() 对每个字符都用给点的字符串拼接起来，不常用
        >>> s.str.join("!")
        0    a!_!b!_!c
        1    c!_!d!_!e
        2          NaN
        3    f!_!g!_!h
        dtype: object
        >>> s.str.join("?")
        0    a?_?b?_?c
        1    c?_?d?_?e
        2          NaN
        3    f?_?g?_?h
        dtype: object
        >>> s.str.join(".")
        0    a._.b._.c
        1    c._.d._.e
        2          NaN
        3    f._.g._.h
        dtype: object
		
5、contains()	是否包含表达式
        >>> s.str.contains('d')
        0    False
        1     True
        2      NaN
        3    False
        dtype: object
		
6、replace()	替换
        >>> s.str.replace("_", ".")
        0    a.b.c
        1    c.d.e
        2      NaN
        3    f.g.h
        dtype: object
		
7、repeat()	重复
        >>> s.str.repeat(3)
        0    a_b_ca_b_ca_b_c
        1    c_d_ec_d_ec_d_e
        2                NaN
        3    f_g_hf_g_hf_g_h
        dtype: object
		
8、pad()	左右补齐
		>>> s.str.pad(10, fillchar="?")
		0    ?????a_b_c
		1    ?????c_d_e
		2           NaN
		3    ?????f_g_h
		dtype: object
		>>>
		>>> s.str.pad(10, side="right", fillchar="?")
		0    a_b_c?????
		1    c_d_e?????
		2           NaN
		3    f_g_h?????
		dtype: object
		
9、center() 中间补齐，看例子
		>>> s.str.center(10, fillchar="?")
		0    ??a_b_c???
		1    ??c_d_e???
		2           NaN
		3    ??f_g_h???
		dtype: object
		
10、ljust() 右边补齐，看例子
		>>> s.str.ljust(10, fillchar="?")
		0    a_b_c?????
		1    c_d_e?????
		2           NaN
		3    f_g_h?????
		dtype: object
		
11、rjust() 左边补齐，看例子
		>>> s.str.rjust(10, fillchar="?")
		0    ?????a_b_c
		1    ?????c_d_e
		2           NaN
		3    ?????f_g_h
		dtype: object
		
12、zfill()	左边补0
		>>> s.str.zfill(10)
		0    00000a_b_c
		1    00000c_d_e
		2           NaN
		3    00000f_g_h
		dtype: object
		
13、wrap()	在指定的位置加回车符号
		>>> s.str.wrap(3)
		0    a_b\n_c
		1    c_d\n_e
		2        NaN
		3    f_g\n_h
		dtype: object
		
14、slice() 按给点的开始结束位置切割字符串
		>>> s.str.slice(1,3)
		0     _b
		1     _d
		2    NaN
		3     _g
		dtype: object
		
15、slice_replace() 使用给定的字符串，替换指定的位置的字符
		>>> s.str.slice_replace(1, 3, "?")
		0    a?_c
		1    c?_e
		2     NaN
		3    f?_h
		dtype: object
		>>> s.str.slice_replace(1, 3, "??")
		0    a??_c
		1    c??_e
		2      NaN
		3    f??_h
		dtype: object
		
16、count()	计算给定单词出现的次数
		>>> s.str.count("a")
		0     1
		1     0
		2   NaN
		3     0
		dtype: float64
		
17、startswith()	判断是否以给定的字符串开头
		>>> s.str.startswith("a");
		0     True
		1    False
		2      NaN
		3    False
		dtype: object
		
18、endswith() 判断是否以给定的字符串结束
		>>> s.str.endswith("e");
		0    False
		1     True
		2      NaN
		3    False
		dtype: object
		
19、findall() 查找所有符合正则表达式的字符，以数组形式返回
		>>> s.str.findall("[a-z]");
		0    [a, b, c]
		1    [c, d, e]
		2          NaN
		3    [f, g, h]
		dtype: object
		
20、match()	检测是否全部匹配给点的字符串或者表达式
		>>> s
		0    a_b_c
		1    c_d_e
		2      NaN
		3    f_g_h
		dtype: object
		>>> s.str.match("[d-z]");
		0    False
		1    False
		2      NaN
		3     True
		dtype: object
		
21、extract()	抽取匹配的字符串出来，注意要加上括号，把你需要抽取的东西标注上
		>>> s.str.extract("([d-z])");
		0    NaN
		1      d
		2    NaN
		3      f
		dtype: object
		
22、len()	计算字符串的长度
		>>> s.str.len()
		0     5
		1     5
		2   NaN
		3     5
		dtype: float64 
		
23、strip() 去除前后的空白字符
		>>> idx = pandas.Series([' jack', 'jill ', ' jesse ', 'frank'])
		>>> idx.str.strip()
		0     jack
		1     jill
		2    jesse
		3    frank
		dtype: object
		
24、rstrip() 去除后面的空白字符
25、lstrip() 去除前面的空白字符
26、partition()	把字符串数组切割称为DataFrame，注意切割只是切割称为三部分，分隔符前，分隔符，分隔符后
27、rpartition()	从右切起
		>>> s.str.partition('_')
			 0    1    2
		0    a    _  b_c
		1    c    _  d_e
		2  NaN  NaN  NaN
		3    f    _  g_h
		>>> s.str.rpartition('_')
			 0    1    2
		0  a_b    _    c
		1  c_d    _    e
		2  NaN  NaN  NaN
		3  f_g    _    h
		
28、lower() 全部小写
29、upper() 全部大写
30、find() 从左边开始，查找给定字符串的所在位置
		>>> s.str.find('d')
		0    -1
		1     2
		2   NaN
		3    -1
		dtype: float64
		
31、rfind()	从右边开始，查找给定字符串的所在位置

32、index()	查找给定字符串的位置，注意，如果不存在这个字符串，那么会报错！
33、rindex()	从右边开始查找，给定字符串的位置
		>>> s.str.index('_')
		0     1
		1     1
		2   NaN
		3     1
		dtype: float64
		
34、capitalize()	首字符大写
		>>> s.str.capitalize()
		0    A_b_c
		1    C_d_e
		2      NaN
		3    F_g_h
		dtype: object
		
35、swapcase()	大小写互换
		>>> s.str.swapcase()
		0    A_B_C
		1    C_D_E
		2      NaN
		3    F_G_H
		dtype: object
		
36、normalize() 序列化数据，数据分析很少用到，咱们就不研究了
37、isalnum()	是否全部是数字和字母组成
		>>> s.str.isalnum()
		0    False
		1    False
		2      NaN
		3    False
		dtype: object
		
38、isalpha()	是否全部是字母
		>>> s.str.isalpha()
		0    False
		1    False
		2      NaN
		3    False
		dtype: object
		
39、isdigit()	是否全部都是数字
		>>> s.str.isdigit()
		0    False
		1    False
		2      NaN
		3    False
		dtype: object
		
40、isspace() 是否空格
		>>> s.str.isspace()
		0    False
		1    False
		2      NaN
		3    False
		dtype: object
		
41、islower()	是否全部小写
42、isupper() 是否全部大写
		>>> s.str.islower()
		0    True
		1    True
		2     NaN
		3    True
		dtype: object
		>>> s.str.isupper()
		0    False
		1    False
		2      NaN
		3    False
		dtype: object
		
43、istitle() 是否只有首字母为大写，其他字母为小写
		>>> s.str.istitle()
		0    False
		1    False
		2      NaN
		3    False
		dtype: object
		
44、isnumeric()	是否是数字
45、isdecimal()	是否全是数字
