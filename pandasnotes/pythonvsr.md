# Pandas与R语言的DataFrame对比
Python/R 都可以通过命令行的方式和其他语言做交互，通过命令行而不死直接调用某个类或方法可以更好地降低耦合性。

## 参数传递
参数传递 | Python | R
---|---|---
命令行输入 | Python path/to/myscript.py arg1 arg2 arg3 | Rscript path/to/myscript.R arg1 arg2 arg3
脚本识别 | import sys my_args=sys.argv | myArgs

## 数据输出与解析
对于数据传输与解析，最好的数据格式是csv。csv格式可以很快得转换为data frame格式。

数据传输与解析 | Python | R
---|---|---
CSV(原生) | CSV | read.csv
CSV(优化) | pandas.read_csv("path/file.csv") | data.table::fread("path/file.csv")
json | json（原生） | jsonlite
YAML | PyYAML | yaml

## 基本数据结构
由于是从科学计算的角度出发，R 中的数据结构非常的简单，主要包括 向量(一维)、多维数组(二维时为矩阵)、列表(非结构化数据)、数据框(结构化数据)。<br>
而 Python 则包含更丰富的数据结构来实现数据更精准的访问和内存控制，多维数组(可读写、有序)、元组(只读、有序)、集合(唯一、无序)、字典(Key-Value)等等。

基本数据结构 | Python | R
---|---|---
数组 | list:[1, 'a'] | :array:array(c(1, "a"), 2)
Key-Value(非结构化数据) | 字典:["a" : 1] | lists
数据框(结构化数据) | dataframe | data.frame

## 矩阵操作
实际上，Python（numpy）和R中的矩阵都是通过一个多为数组（ndarray）实现的。

矩阵转化 | Python | R
---|---|---
维度 | data.shape | dim(data)
转为向量 | data.flatten(1) | av.vector(data)
转为矩阵 | np.array(data) | matrix(c(1,2,3,4,5,6), nrow=2, byrow=T)
转置 | data.T | t(data)
矩阵变形 | data.reshape(1, np.prod(data.shape)) | matrix(data, ncol=nrow(data) * ncol(data))
矩阵按行拼接 | np.r_[A, B] | rbind(A, B) 
矩阵按列拼接 | np.c_[A, B] | cbind(A, B)

矩阵计算 | Python | R
---|---|---
矩阵乘法 | np.dot(A, B) | A %*% B
矩阵幂指 | np.power(A, 3) | A ^ 3
全零矩阵 | np.zeros((3, 3)) | matrix(0, nrow=3, ncol=3)
矩阵求逆 | np.linalg.inv(A) | solve(A)
协方差 | np.cov(A, B) | cov(A, B)
特征值 | np.linalg.eig(A)[0] | eigen(A)$values
特征向量 | np.linalg.eig(A)[1] | eigen(A)$vectors

## 数据框操作
参考R中的data frame结构，Python的pandas包也实现了类似data frame数据结构。<br>
现在，为了加强数据框的操作，R演变出了data table(dt)格式，支持类似SQL语法。

数据框操作 | Python | R
---|---|---
按列选择 | df[["a", "c"]] | dt[, (a, c)]
按行选择 | df[1:2] | dt[1:2]
分组 | df.groupby(["a", "c"])[["c", "d"]].mean() | aggregate(x=dt[, c("a", "c")], by=list(mydt2$by1, mydt2$by2), FUNC=mean)
按T/F列表匹配索引 | pd.Serier(np.arrange(5), dtype=np.float(32).isin([2, 4])) | 0:4%in%c(2, 4)
match匹配操作返回索引 | pd.Series(pd.match(pd.Series(np.arrange(5), dtype=np.float32), [2, 4], np.nan)) | match(0:4, c(2, 4))
tapply | df.pivot_table(values="a", columns="c", aggfunc=np.max) | tapple(dt$a, dt$x, max)
with | pd.DataFrame({"a" : np.random.randn(10), "b" : np.random.randn(15)}).eval("a+b") | with(dt, a+b)
plyr | df.groupby(["a", "b"]).agg([np.mean, np.std]) | ddply(dt, .(a, b), summarize, mean=round(mean(x)), sd=round(sd(x), 2))
多维数组融合 | pd.DataFrame([tuple(list(x)+[val]) for x, val in np.ndenumerate(np.array(list(range(1, 24)) + [np.NAN]).reshape(2,3,4))]) | data.frame(melt(array(c(1:23, NA), c(2,3,4))))
多维列表融合 | pd.DataFrame(list(enumerate(list(range(1,5))+[np.NAN]))) | data.frame(melt(as.list(c(1:4, NA))))
数据框融合 | pd.melt(df1) | melt(dt1)
数据透视表 | pd.pivot_talbe(df1) | acast(melt(dt1))
连续性数值因子分类 | pd.cut(pd.Series([1,2,3,4,5,6]), 3) | cut(c(1,2,3,4,5,6), 3)
名义性因子分类 | pd.Series([1,2,3,2,2,3].astype("category")) | factor(c(1,2,3,2,2,3))
