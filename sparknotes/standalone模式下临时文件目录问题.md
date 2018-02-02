故我们直接在spark-env.sh 中设置SPARK_LOCAL_DIRS 即可解决。
然后我们直接在spark-env.sh 中配置：
```
export SPARK_LOCAL_DIRS=/home/hadoop/data/sparktmp
```
