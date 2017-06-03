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
