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
