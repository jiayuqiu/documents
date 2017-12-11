# 记录一些常用的命令

### 定义停泊事件的表头

```python
data.columns = ["mmsi", "begin_time", "end_time", "apart_time",
                "begin_lon", "begin_lat", "begin_hdg", "begin_sog", "begin_cog",
                "end_lon", "end_lat", "end_hdg", "end_sog", "end_cog",
                "point_num", "avg_lon", "avg_lat", "var_hdg", "var_cog", "avg_hdgMcog",
                "avg_sog", "var_sog", "max_sog", "maxSog_cog",
                "max_rot", "var_rot", "draught", "avgSpeed", "zone_id", "navistate",
                "nowPortName", "nowPortLon", "nowPortLat"]
```
---
### 16+3水域列表

```python
portNameList = ["dalian03", "fuzhou04", "fuzhou05", "guangzhou13", "lianyungang03",
                "qingdao08", "tianjin06", "shanghai06", "shanghai07", "shanghai08",
                "shenzhen11", "shenzhen12", "rizhao03", "humen03", "yantai03",
                "qinzhou02", "quanzhou03", "xiamen06", "yingkou02", "ningbo08",
                "rotterdam04", "newjersey03", "newyork02", "busan03", "singapore03",
                "hongkong03"]
```
---

### 定义三阶段表头
```python
data.columns = ["unique_ID", "acquisition_time", "target_type", "data_supplier", "data_source",
                "status", "longitude", "latitude", "area_ID", "speed", "conversion", "cog",
                "true_head", "power", "ext", "extend"]
```

### 获取文件夹内的所有文件名
```python
def getFileNameList(filePath):
    import os

    fileNameList = []
    for fileName in os.listdir(filePath):
        fileNameList.append(fileName)
    return fileNameList
```

### 判断区间，全闭
```python
def isInRange(num, minNum, maxNum):
    if (num >= minNum) & (num <= maxNum):
        return True
    else:
        return False
```

### 求斜率
```python
def slope(x1, x2, y1, y2):
    """
    求斜率
    
    :param x1: 第一个点的经度
    :param x2: 第一个点的纬度
    :param y1: 第二个点的经度
    :param y2: 第二个点的纬度
    :return: 形成的斜率
    """
    if y1 == y2:
        return 0
    elif x1 == x2:
        return float('Inf')
    else:
        return (y2 - y1) / (x2 - x1)
```

### 开头UTF-8
```python
# -*- coding: utf-8 -*-
```

### 求两线段是否相交
```python
def get_area(point0, point1, point2):
    """
    利用三点，求出三角形所对面积
    :param point0: 点0，类型list
    :param point1: 点1，类型list
    :param point2: 点2，类型list
    :return: 叉乘值，类型float
    """
    s = point0[0] * point1[1] + point2[0] * point0[1] + point1[0] * point2[1] - \
        point2[0] * point1[1] - point0[0] * point2[1] - point1[0] * point0[1]
    return s


def is_line_cross(str1, end1, str2, end2):
    """
    判断两条线段是否相交
    :param str1: 起始点1，类型list
    :param end1: 终止点1，类型list
    :param str2: 起始点2，类型list
    :param end2: 终止点2，类型list
    :return: T/F
    """
    s1 = get_area(str1, end1, str2)
    s2 = get_area(str1, end1, end2)
    s3 = get_area(str2, end2, str1)
    s4 = get_area(str2, end2, end1)
    if (s1 * s2 <= 0) & (s3 * s4 <= 0):
        return True
    else:
        return False
```

### 求两直线交点
```python
class Point(object):
    x = 0.
    y = 0.

    # 定义构造方法
    def __init__(self, x=0., y=0.):
        self.x = x
        self.y = y


class Line(object):
    def __init__(self, p1, p2):
        self.p1 = p1
        self.p2 = p2


def get_line_para(line):
    line.a = line.p1.y - line.p2.y
    line.b = line.p2.x - line.p1.x
    line.c = line.p1.x * line.p2.y - line.p2.x * line.p1.y


def get_cross_point(l1, l2):

    get_line_para(l1)
    get_line_para(l2)
    d = l1.a * l2.b - l2.a * l1.b
    p = Point()
    p.x = (l1.b * l2.c - l2.b * l1.c)*1.0 / d
    p.y = (l1.c * l2.a - l2.c * l1.a)*1.0 / d
    return p


p1 = Point(1, 1)
p2 = Point(3, 3)
line1 = Line(p1, p2)

p3=Point(2, 3)
p4=Point(3, 2)
line2=Line(p3,p4)
Pc = get_cross_point(line1, line2)
print("Cross point:", Pc.x, Pc.y)
```

### 将google earth保存下来的多边形坐标转换成list
```python
def coordinates_kml(file_path):
    """
    从kml文件中获取经纬度数据
    :param file_path: kml文件所在路径，类型：string
    :return: 经纬坐标，用“;”分割，类型：list
    """
    kml_file = open(file_path, "r")
    kml_string = kml_file.read()
    coordinates_string = kml_string.split("<coordinates>")[1]\
                                   .split("</coordinates>")[0]\
                                   .replace("\t", "") \
                                   .replace("\n", "")
    coordinates_list = coordinates_string.split(" ")
    out_put_coordinates_list = []
    for coordinate in coordinates_list:
        if coordinate:
            tmp_coordinate = coordinate.split(",")
            out_put_coordinates_list.append([tmp_coordinate[0], tmp_coordinate[1]])
    return out_put_coordinates_list
```


### 判断点与多边形的位置关系
```python
def point_poly(pointLon, pointLat, polygon):
    """
    判断点是否在多边形内
    :param pointLon: 该点的经度，类型：float
    :param pointLat: 该点的唯独，类型：float
    :param polygon: 多边形的经纬度坐标列表，类型：list
    :return: t/f
    """
    polygon = np.array(polygon)
    polygon = np.array([[float(x) for x in line] for line in polygon])
    cor = len(polygon)
    i = 0
    j = cor - 1
    inside = False
    while (i < cor):
        if ((((polygon[i, 1] < pointLat) & (polygon[j, 1] >= pointLat))
                 | ((polygon[j, 1] < pointLat) & (polygon[i, 1] >= pointLat)))
                & ((polygon[i, 0] <= pointLon) | (polygon[j, 0] <= pointLon))):
            a = (polygon[i, 0] +
                 (pointLat - polygon[i, 1]) / (polygon[j, 1] - polygon[i, 1]) *
                 (polygon[j, 0] - polygon[i, 0]))

            if (a < pointLon):
                inside = not inside
        j = i
        i = i + 1
    return inside
```

### wget 命令行下载ftp文件
```
wget ftp://username:password@127.0.0.1/filename.zip
```
