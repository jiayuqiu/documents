使用astype实现dataframe字段类型转换，原文链接：http://blog.csdn.net/chinacmt/article/details/52230339

```
# -*- coding: UTF-8 -*-
import pandas as pd
df = pd.DataFrame([{'col1':'a', 'col2':'1'}, {'col1':'b', 'col2':'2'}])

print df.dtypes

df['col2'] = df['col2'].astype('int')
print '-----------'
print df.dtypes

df['col2'] = df['col2'].astype('float64')
print '-----------'
print df.dtypes12345678910111213
```

输出结果:

```
col1    object
col2    object
dtype: object
-----------
col1    object
col2     int32
dtype: object
-----------
col1     object
col2    float64
dtype: object1234567891011
```

注：data type list

```
Data type   Description
bool_   Boolean (True or False) stored as a byte
int_    Default integer type (same as C long; normally either int64 or int32)
intc    Identical to C int (normally int32 or int64)
intp    Integer used for indexing (same as C ssize_t; normally either int32 or int64)
int8    Byte (-128 to 127)
int16   Integer (-32768 to 32767)
int32   Integer (-2147483648 to 2147483647)
int64   Integer (-9223372036854775808 to 9223372036854775807)
uint8   Unsigned integer (0 to 255)
uint16  Unsigned integer (0 to 65535)
uint32  Unsigned integer (0 to 4294967295)
uint64  Unsigned integer (0 to 18446744073709551615)
float_  Shorthand for float64.
float16 Half precision float: sign bit, 5 bits exponent, 10 bits mantissa
float32 Single precision float: sign bit, 8 bits exponent, 23 bits mantissa
float64 Double precision float: sign bit, 11 bits exponent, 52 bits mantissa
complex_    Shorthand for complex128.
complex64   Complex number, represented by two 32-bit floats (real and imaginary components)
complex128  Complex number, represented by two 64-bit floats (real and imaginary components)
```
