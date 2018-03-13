# hive insert数据

## 1. hive默认不支持行级别insert，需要对hive另行配置
代替方法：<br>
在hive页面，点击file brower将文件上传至hive集群，然后用load data inpath 'file/path' into table tb_name进行上传
