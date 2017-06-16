
# 记录在macOS上配置SPARK的方法

# pycharm如何调用spark包
## 手动加入spark包
1. Preference
2. Project Structure
3. Add Content Root
4. 找到spark对应的路径 your/path/to/spark/python/lib/，添加pyspark.zip与py4j-*.*.*-src.zip文件
<br>
添加完后pycharm即可import pyspark

# pycharm运行spark程序的环境配置
## 配置JAVA_HOME、SPARK_HOME、PYTHONPATH
1. 点击Run -> Edit Configurations
2. Environment varibles 中添加JAVA_HOME;SPARK_HOME;PYTHONPATH
3. JAVA_HOME  -- /Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home
   SPARK_HOME -- /your/path/to/spark
   PYTHONPATH -- /your/path/to/spark/python
<br>
按上述操作完成后，可用pycharm直接运行spark程序
