# 将DataFrame类型转换为RDD (pyspark)

```python
# coding:utf-8

import doctest
from pyspark.context import SparkContext
from pyspark.sql import Row, SQLContext
import pyspark.sql.dataframe

df = sc.parallelize([Row(name='Alice', age=10, height=80),
                     Row(name='Bob', age=5, height=None),
                     Row(name='Tom', age=None, height=None),
                     Row(name=None, age=None, height=None)]).toDF()
# df.first()
# Row(age=10, height=80, name=u'Alice')

rdd = df.rdd
# rdd.first()
# Row(age=10, height=80, name=u'Alice')

convert_rdd = rdd.map(list)
# convert_rdd.first()
# [10, 80, u'Alice']

convert_str = ','.join(convert_rdd.first())
# '10,80,Alice'
```
