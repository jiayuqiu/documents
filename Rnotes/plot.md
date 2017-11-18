```R
library(data.table)  # 用于读取csv文件
library(ggplot2)  # 作图包
library (ggmap)  # 地图包

ais_df = fread("*****")  # ais数据文件路径
ais_df$longitude = ais_df$longitude / 1000000.  # 经纬度单位调整
ais_df$latitude = ais_df$latitude / 1000000.
tmp_ais_df = ais_df[ais_df$unique_ID == 412356030, ]  # 获取一条船的ais数据
tmp_ais_df = tmp_ais_df[order(tmp_ais_df$acquisition_time), ]  # 将其按时间排序

# 作图
ggmap(get_googlemap(center = c(121.755777, 31.271933), zoom = 6,maptype='roadmap')) + 
  geom_path(data=tmp_ais_df, aes(x=longitude, y=latitude), 
             size=1, color="blue")  # center地图中心， zoom地图比例尺，geom_path轨迹连线函数
```

学长，下面是Python代码
```python
# coding:utf-8

import pandas as pd
import numpy as np
import math


#########################################################
# 获得地球两点间距离
EARTH_RADIUS = 6378.137  # 地球半径，单位千米


def getRadian(x):
    return x * math.pi / 180.0


def getDist(lon1, lat1, lon2, lat2):  # 得到地球两点距离，单位千米
    radLat1 = getRadian(lat1)
    radLat2 = getRadian(lat2)

    a = radLat1 - radLat2
    b = getRadian(lon1) - getRadian(lon2)

    dst = 2 * math.asin(math.sqrt(math.pow(math.sin(a / 2), 2) +
                                  math.cos(radLat1) * math.cos(radLat2) * math.pow(math.sin(b / 2), 2)))
    dst = dst * EARTH_RADIUS
    dst = round(dst * 100000000) / 100000000

    return dst


def getFileNameList(filePath):
    """
    获取一个文件夹下所有文件的文件名
    :param filePath: 文件夹的绝对路径，类型：string
    :return: 文件名列表，类型：list
    """
    import os

    fileNameList = []
    for fileName in os.listdir(filePath):
        fileNameList.append(fileName)
    return fileNameList


def load_mmsi_ais(ais_df, mmsi_list):
    """
    获取ais数据中给定mmsi列表的ais数据
    :param ais_df: ais数据，类型：dataframe
    :param mmsi_list: 给定的mmsi列表，类型：list
    :return: 
    """
    return ais_df[ais_df['unique_ID'].isin(mmsi_list)]


def get_ship_avg_speed(each_ship_ais_df):
    """
    获取一条船的平均速度
    :param each_ship_ais_df: 一条船舶的ais数据，类型:dataframe
    :return: [mmsi, avg_speed]
    """
    dst = 0
    each_ship_ais_df = each_ship_ais_df.sort_values(by=['acquisition_time'])
    each_ship_ais_df['longitude'] = each_ship_ais_df['longitude'] / 1000000.
    each_ship_ais_df['latitude'] = each_ship_ais_df['latitude'] / 1000000.
    each_ship_ais_array = np.array(each_ship_ais_df)

    for index in range(len(each_ship_ais_array) - 1):
        tmp_dst = getDist(each_ship_ais_array[index, 5], each_ship_ais_array[index, 6],
                          each_ship_ais_array[index+1, 5], each_ship_ais_array[index+1, 6]) * 1000.
        dst += tmp_dst

    if (np.max(each_ship_ais_array[:, 1]) - np.min(each_ship_ais_array[:, 1])) == 0:
        return 0
    else:
        return dst / (np.max(each_ship_ais_array[:, 1]) - np.min(each_ship_ais_array[:, 1]))


if __name__ == "__main__":
    # # 获取ais数据文件路径
    # file_path_string = '/Volumes/qiuSeagate/船讯网数据/亿海蓝数据/201609/'
    # file_name_list = getFileNameList(file_path_string)
    #
    # # 获取到过上海港的集装箱船舶mmsi
    # shanghai_container_stop_df = pd.read_csv('/Users/qiujiayu/data/6港口_总/上海_总_201608_.csv')
    # shanghai_container_mmsi_seriors = shanghai_container_stop_df['unique_ID']
    #
    # # 获取需要的ais数据
    # for file_name_string in file_name_list:
    #     print file_name_string
    #     ais_df = pd.read_csv(file_path_string + file_name_string)
    #     shanghai_container_ais_df = load_mmsi_ais(ais_df, shanghai_container_mmsi_seriors)
    #     shanghai_container_ais_df.to_csv('/Users/qiujiayu/data/cq/sh_%s' % file_name_string, index=None)

    # 计算平均速度
    # 获取所有上海集装箱ais数据
    file_path_string = '/Users/qiujiayu/data/cq/'
    file_name_list = getFileNameList(file_path_string)

    mmsi_speed_list = []
    sh_container_df_list = []
    for file_name_string in file_name_list:
        print file_name_string
        shanghai_container_day_ais_df = pd.read_csv(file_path_string + file_name_string)
        sh_container_df_list.append(shanghai_container_day_ais_df)

    # shanghai_container_ais_df = pd.read_csv('/Users/qiujiayu/data/shanghai_container_ais.csv')
    shanghai_container_ais_df = pd.concat(sh_container_df_list, ignore_index=True)
    # shanghai_container_ais_df.to_csv('/Users/qiujiayu/data/shanghai_container_ais.csv', index=None)
    shanghai_container_ais_gdf = shanghai_container_ais_df.groupby('unique_ID')

    for container_mmsi, container_ais_df in shanghai_container_ais_gdf:
        print 'computing avg speed... %s' % container_mmsi
        mmsi_speed_list.append([container_mmsi, get_ship_avg_speed(container_ais_df)])

    mmsi_speed_df = pd.DataFrame(mmsi_speed_list, columns=['mmsi', 'avg_speed'])
    mmsi_speed_df.to_csv('/Users/qiujiayu/data/shanghai_container_avg_speed.csv', index=None)

```
