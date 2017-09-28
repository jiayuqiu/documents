# 记录numpy的一些常用操作

## array中对第二列数据进行排序
```python
# 其中argsort()函数的作用为，返回排序后的整数索引
array_sorted = array[array[:, 1].argsort()]
```
