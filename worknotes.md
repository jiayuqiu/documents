记录一些常用的命令

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
