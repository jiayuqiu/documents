[原文地址](http://www.cnblogs.com/yongjian/p/6425772.html)
<div id="cnblogs_post_body">> 键值对的RDD操作与基本RDD操作一样，只是操作的元素由基本类型改为二元组。

# 概述

键值对RDD是Spark操作中最常用的RDD，它是很多程序的构成要素，因为他们提供了并行操作各个键或跨界点重新进行数据分组的操作接口。

# 创建

Spark中有许多中创建键值对RDD的方式，其中包括

*   文件读取时直接返回键值对RDD
*   通过List创建键值对RDD

在Scala中，可通过Map函数生成二元组

<div><div id="highlighter_89611" class="syntaxhighlighter  java"><div class="toolbar"><span></span></div><table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div><div class="line number2 index1 alt1">2</div><div class="line number3 index2 alt2">3</div><div class="line number4 index3 alt1">4</div><div class="line number5 index4 alt2">5</div><div class="line number6 index5 alt1">6</div><div class="line number7 index6 alt2">7</div><div class="line number8 index7 alt1">8</div><div class="line number9 index8 alt2">9</div><div class="line number10 index9 alt1">10</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2">`val listRDD = sc.parallelize(List(``1``,``2``,``3``,``4``,``5``))`</div><div class="line number2 index1 alt1">`val result = listRDD.map(x =&gt; (x,``1``))`</div><div class="line number3 index2 alt2">`result.foreach(println)`</div><div class="line number4 index3 alt1">&nbsp;</div><div class="line number5 index4 alt2">`//结果`</div><div class="line number6 index5 alt1">`(``1``,``1``)`</div><div class="line number7 index6 alt2">`(``2``,``1``)`</div><div class="line number8 index7 alt1">`(``3``,``1``)`</div><div class="line number9 index8 alt2">`(``4``,``1``)`</div><div class="line number10 index9 alt1">`(``5``,``1``)`</div></div></td></tr></tbody></table></div></div>


# 键值对RDD的转化操作

&nbsp;

基本RDD转化操作在此同样适用。但因为键值对RDD中包含的是一个个二元组，<span style="color: #ff0000;">所以需要传递的函数会由原来的操作单个元素改为操作二元组</span>。

下表总结了<span style="color: #ff0000;">针对单个键值对</span><span style="color: #ff0000;">RDD</span>的转化操作，以<span style="color: #ff0000;"> **{ (1,2) , (3,4) , (3,6) }**</span>&nbsp; 为例，f表示传入的函数

<table style="width: 870px;" border="1" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="187">函数名</td>
<td valign="top" width="254">目的</td>
<td valign="top" width="222">示例</td>
<td valign="top" width="205">结果</td>
</tr>
<tr>
<td valign="top" width="187">reduceByKey(f)</td>
<td valign="top" width="254">合并具有相同key的值</td>
<td valign="top" width="222">rdd.reduceByKey( ( x,y) =&gt; x+y )</td>
<td valign="top" width="205">{ (1,2) , (3,10) }</td>
</tr>
<tr>
<td valign="top" width="187">groupByKey()</td>
<td valign="top" width="254">对具有相同key的值分组</td>
<td valign="top" width="222">rdd.groupByKey()</td>
<td valign="top" width="205">{ (1,2) , (3, [4,6] ) }</td>
</tr>
<tr>
<td valign="top" width="187">mapValues(f)</td>
<td valign="top" width="254">对键值对中的每个值(value)应用一个函数，但不改变键(key)</td>
<td valign="top" width="222">rdd.mapValues(x =&gt; x+1)</td>
<td valign="top" width="205">{ (1,3) , (3,5) , (3,7) }</td>
</tr>
<tr>
<td valign="top" width="187">combineBy Key( createCombiner, mergeValue, mergeCombiners, partitioner)</td>
<td valign="top" width="254">使用不同的返回类型合并具有相同键的值</td>
<td valign="top" width="222">下面有详细讲解</td>
<td valign="top" width="205">-</td>
</tr>
<tr>
<td valign="top" width="187">flatMapValues(f)</td>
<td valign="top" width="254">对键值对RDD中每个值应用返回一个迭代器的函数，然后对每个元素生成一个对应的键值对。常用语符号化</td>
<td valign="top" width="222">rdd.flatMapValues(x =&gt; ( x to 5 ))</td>
<td valign="top" width="205">

{ (1, 2) ,&nbsp; (1, 3) ,&nbsp;&nbsp; (1, 4) , (1, 5) ,&nbsp; (3, 4) , (3, 5) }

</td>
</tr>
<tr>
<td valign="top" width="187">keys()</td>
<td valign="top" width="254">获取所有key</td>
<td valign="top" width="222">rdd.keys()</td>
<td valign="top" width="205">{1,3,3}</td>
</tr>
<tr>
<td valign="top" width="187">values()</td>
<td valign="top" width="254">获取所有value</td>
<td valign="top" width="222">rdd.values()</td>
<td valign="top" width="205">{2,4,6}</td>
</tr>
<tr>
<td valign="top" width="187">sortByKey()</td>
<td valign="top" width="254">根据key排序</td>
<td valign="top" width="222">rdd.sortByKey()</td>
<td valign="top" width="205">{ (1,2) , (3,4) , (3,6) }</td>
</tr>
</tbody>
</table>

&nbsp;

&nbsp;

下表总结了针对<span style="color: #ff0000;">两个键值对RDD</span>的转化操作，以**<span style="color: #ff0000;">rdd1 = { (1,2) , (3,4) , (3,6)</span><span style="color: #ff0000;"> }&nbsp; rdd2 = { (3,9) }</span>** 为例，

<table style="width: 871px;" border="1" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="184">函数名</td>
<td valign="top" width="251">目的</td>
<td valign="top" width="226">示例</td>
<td valign="top" width="208">结果</td>
</tr>
<tr>
<td valign="top" width="184">subtractByKey</td>
<td valign="top" width="251">删掉rdd1中与rdd2的key相同的元素</td>
<td valign="top" width="226">rdd1.subtractByKey(rdd2)</td>
<td valign="top" width="208">{ (1,2) }</td>
</tr>
<tr>
<td valign="top" width="184">join</td>
<td valign="top" width="251">内连接</td>
<td valign="top" width="226">rdd1.join(rdd2)</td>
<td valign="top" width="208">

{(3, (4, 9)), (3, (6, 9))}

</td>
</tr>
<tr>
<td valign="top" width="184">leftOuterJoin</td>
<td valign="top" width="251">左外链接</td>
<td valign="top" width="226">rdd1.leftOuterJoin (rdd2)</td>
<td valign="top" width="208">

{(3,( Some( 4), 9)), (3,( Some( 6), 9))}

</td>
</tr>
<tr>
<td valign="top" width="184">rightOuterJoin</td>
<td valign="top" width="251">右外链接</td>
<td valign="top" width="226">rdd1.rightOuterJoin(rdd2)</td>
<td valign="top" width="208">

{(1,( 2, None)), (3, (4, Some( 9))), (3, (6, Some( 9)))}

</td>
</tr>
<tr>
<td valign="top" width="184">cogroup</td>
<td valign="top" width="251">将两个RDD钟相同key的数据分组到一起</td>
<td valign="top" width="226">rdd1.cogroup(rdd2)</td>
<td valign="top" width="208">{(1,([ 2],[])), (3, ([4, 6],[ 9]))}</td>
</tr>
</tbody>
</table>

&nbsp;

&nbsp;

# combineByKey

> combineByKey( createCombiner, mergeValue, mergeCombiners, partitioner,mapSideCombine)
> 
> combineByKey( createCombiner, mergeValue, mergeCombiners, partitioner)
> 
> combineByKey( createCombiner, mergeValue, mergeCombiners)

&nbsp;

**函数功能：**

聚合各分区的元素，而每个元素都是二元组。功能与基础RDD函数aggregate()差不多，可让用户返回与输入数据类型不同的返回值。

combineByKey函数的每个参数分别对应聚合操作的各个阶段。所以，理解此函数对Spark如何操作RDD会有很大帮助。

&nbsp;

**参数解析：**

createCombiner：**分区内 **创建组合函数

mergeValue：**分区内 **合并值函数

mergeCombiners：**多分区 **合并组合器函数

partitioner：自定义分区数，默认为HashPartitioner

mapSideCombine：是否在map端进行Combine操作，默认为true

&nbsp;

**工作流程：**

1.  <span style="color: #ff0000;">combineByKey会遍历分区中的**所有元素**，因此每个元素的key要么没遇到过，要么和之前某个元素的key相同。</span>
2.  <span style="color: #ff0000;">如果这是一个新的元素，函数会调用createCombiner创建那个key对应的累加器**初始值**。 </span>
3.  <span style="color: #ff0000;">如果这是一个在处理当前分区之前已经遇到的key，会调用mergeCombiners把该key累加器对应的当前value与这个新的value**合并**。</span>

&nbsp;

**代码例子：**

//统计男女个数

<div><div id="highlighter_729229" class="syntaxhighlighter  java"><div class="toolbar"><span></span></div><table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div><div class="line number2 index1 alt1">2</div><div class="line number3 index2 alt2">3</div><div class="line number4 index3 alt1">4</div><div class="line number5 index4 alt2">5</div><div class="line number6 index5 alt1">6</div><div class="line number7 index6 alt2">7</div><div class="line number8 index7 alt1">8</div><div class="line number9 index8 alt2">9</div><div class="line number10 index9 alt1">10</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2">`val conf = ``new` `SparkConf ().setMaster (``"local"``).setAppName (``"app_1"``)`</div><div class="line number2 index1 alt1">`&nbsp;&nbsp;&nbsp;``val sc = ``new` `SparkContext (conf)`</div><div class="line number3 index2 alt2">&nbsp;</div><div class="line number4 index3 alt1">`&nbsp;&nbsp;&nbsp;``val people = List((``"男"``, ``"李四"``), (``"男"``, ``"张三"``), (``"女"``, ``"韩梅梅"``), (``"女"``, ``"李思思"``), (``"男"``, ``"马云"``))`</div><div class="line number5 index4 alt2">`&nbsp;&nbsp;&nbsp;``val rdd = sc.parallelize(people,``2``)`</div><div class="line number6 index5 alt1">`&nbsp;&nbsp;&nbsp;``val result = rdd.combineByKey(`</div><div class="line number7 index6 alt2">`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``(x: String) =&gt; (List(x), ``1``),&nbsp; ``//createCombiner`</div><div class="line number8 index7 alt1">`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``(peo: (List[String], Int), x : String) =&gt; (x :: peo._1, peo._2 + ``1``), ``//mergeValue`</div><div class="line number9 index8 alt2">`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;``(sex1: (List[String], Int), sex2: (List[String], Int)) =&gt; (sex1._1 ::: sex2._1, sex1._2 + sex2._2)) ``//mergeCombiners`</div><div class="line number10 index9 alt1">`&nbsp;&nbsp;&nbsp;``result.foreach(println)`</div></div></td></tr></tbody></table></div></div>

**结果**

(男, ( List( 张三,&nbsp; 李四,&nbsp; 马云),3 ) )
(女, ( List( 李思思,&nbsp; 韩梅梅),2 ) )

&nbsp;

**流程分解：**

[![Spark算子-combineByKey](http://images2015.cnblogs.com/blog/368951/201702/368951-20170223163813804-1005141379.png "Spark算子-combineByKey")](http://images2015.cnblogs.com/blog/368951/201702/368951-20170223163812663-2101150083.png)

&nbsp;

解析：两个分区，分区一按顺序V1、V2、V3遍历

*   V1，发现第一个key=男时，调用createCombiner，即
<pre>(x: String) =&gt; (List(x), 1)</pre>

*   V2，第二次碰到key=男的元素，调用mergeValue，即
<pre>(peo: (List[String], Int), x : String) =&gt; (x :: peo._1, peo._2 + 1)</pre>

*   V3，发现第一个key=女，继续调用createCombiner，即
<pre>(x: String) =&gt; (List(x), 1)</pre>

*   … …
*   待各V1、V2分区都计算完后，数据进行混洗，调用mergeCombiners，即
<pre>(sex1: (List[String], Int), sex2: (List[String], Int)) =&gt; (sex1._1 ::: sex2._1, sex1._2 + sex2._2))</pre>

&nbsp;

* * *

&nbsp;

add by jan 2017-02-27&nbsp;18:34:39

以下例子都基于此RDD

<div class="cnblogs_Highlighter sh-gutter">
<div><div id="highlighter_487591" class="syntaxhighlighter  scala"><div class="toolbar"><span></span></div><table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div><div class="line number2 index1 alt1">2</div><div class="line number3 index2 alt2">3</div><div class="line number4 index3 alt1">4</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2">`(Hadoop,``1``)`</div><div class="line number2 index1 alt1">`(Spark,``1``)`</div><div class="line number3 index2 alt2">`(Hive,``1``)`</div><div class="line number4 index3 alt1">`(Spark,``1``)`</div></div></td></tr></tbody></table></div></div>
</div>

# reduceByKey(func)

reduceByKey(func)的功能是，使用func函数合并具有相同键的值。

比如，reduceByKey((a,b) =&gt; a+b)，有四个键值对("spark",1)、("spark",2)、("hadoop",3)和("hadoop",5)，对具有相同key的键值对进行合并后的结果就是：("spark",3)、("hadoop",8)。可以看出，(a,b) =&gt; a+b这个Lamda表达式中，a和b都是指value，比如，对于两个具有相同key的键值对("spark",1)、("spark",2)，a就是1，b就是2。

<div class="cnblogs_Highlighter sh-gutter">
<div><div id="highlighter_145423" class="syntaxhighlighter  scala"><div class="toolbar"><span></span></div><table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div><div class="line number2 index1 alt1">2</div><div class="line number3 index2 alt2">3</div><div class="line number4 index3 alt1">4</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2">`scala&gt; pairRDD.reduceByKey((a,b)``=``&gt;a+b).foreach(println)`</div><div class="line number2 index1 alt1">`(Spark,``2``)`</div><div class="line number3 index2 alt2">`(Hive,``1``)`</div><div class="line number4 index3 alt1">`(Hadoop,``1``)`</div></div></td></tr></tbody></table></div></div>
</div>

# groupByKey()

roupByKey()的功能是，对具有相同键的值进行分组。比如，对四个键值对("spark",1)、("spark",2)、("hadoop",3)和("hadoop",5)，采用groupByKey()后得到的结果是：("spark",(1,2))和("hadoop",(3,5))。

<div class="cnblogs_Highlighter sh-gutter">
<div><div id="highlighter_409096" class="syntaxhighlighter  scala"><div class="toolbar"><span></span></div><table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div><div class="line number2 index1 alt1">2</div><div class="line number3 index2 alt2">3</div><div class="line number4 index3 alt1">4</div><div class="line number5 index4 alt2">5</div><div class="line number6 index5 alt1">6</div><div class="line number7 index6 alt2">7</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2">`scala&gt; pairRDD.groupByKey()`</div><div class="line number2 index1 alt1">`res``15``:` `org.apache.spark.rdd.RDD[(String, Iterable[Int])] ``=` `ShuffledRDD[``15``] at groupByKey at &lt;console&gt;``:``34`</div><div class="line number3 index2 alt2">`//从上面执行结果信息中可以看出，分组后，value被保存到Iterable[Int]中`</div><div class="line number4 index3 alt1">`scala&gt; pairRDD.groupByKey().foreach(println)`</div><div class="line number5 index4 alt2">`(Spark,CompactBuffer(``1``, ``1``))`</div><div class="line number6 index5 alt1">`(Hive,CompactBuffer(``1``))`</div><div class="line number7 index6 alt2">`(Hadoop,CompactBuffer(``1``))`</div></div></td></tr></tbody></table></div></div>
</div>

# keys

keys只会把键值对RDD中的key返回形成一个新的RDD。比如，对四个键值对("spark",1)、("spark",2)、("hadoop",3)和("hadoop",5)构成的RDD，采用keys后得到的结果是一个RDD[Int]，内容是{"spark","spark","hadoop","hadoop"}。

<div class="cnblogs_Highlighter sh-gutter">
<div><div id="highlighter_845313" class="syntaxhighlighter  scala"><div class="toolbar"><span></span></div><table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div><div class="line number2 index1 alt1">2</div><div class="line number3 index2 alt2">3</div><div class="line number4 index3 alt1">4</div><div class="line number5 index4 alt2">5</div><div class="line number6 index5 alt1">6</div><div class="line number7 index6 alt2">7</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2">`scala&gt; pairRDD.keys`</div><div class="line number2 index1 alt1">`res``17``:` `org.apache.spark.rdd.RDD[String] ``=` `MapPartitionsRDD[``17``] at keys at &lt;console&gt;``:``34`</div><div class="line number3 index2 alt2">`scala&gt; pairRDD.keys.foreach(println)`</div><div class="line number4 index3 alt1">`Hadoop`</div><div class="line number5 index4 alt2">`Spark`</div><div class="line number6 index5 alt1">`Hive`</div><div class="line number7 index6 alt2">`Spark`</div></div></td></tr></tbody></table></div></div>
</div>

# values

&nbsp;values只会把键值对RDD中的value返回形成一个新的RDD。比如，对四个键值对("spark",1)、("spark",2)、("hadoop",3)和("hadoop",5)构成的RDD，采用keys后得到的结果是一个RDD[Int]，内容是{1,2,3,5}。

<div class="cnblogs_Highlighter sh-gutter">
<div><div id="highlighter_661932" class="syntaxhighlighter  scala"><div class="toolbar"><span></span></div><table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div><div class="line number2 index1 alt1">2</div><div class="line number3 index2 alt2">3</div><div class="line number4 index3 alt1">4</div><div class="line number5 index4 alt2">5</div><div class="line number6 index5 alt1">6</div><div class="line number7 index6 alt2">7</div><div class="line number8 index7 alt1">8</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2">`scala&gt; pairRDD.values`</div><div class="line number2 index1 alt1">`res``0``:` `org.apache.spark.rdd.RDD[Int] ``=` `MapPartitionsRDD[``2``] at values at &lt;console&gt;``:``34`</div><div class="line number3 index2 alt2">`&nbsp;`&nbsp;</div><div class="line number4 index3 alt1">`scala&gt; pairRDD.values.foreach(println)`</div><div class="line number5 index4 alt2">`1`</div><div class="line number6 index5 alt1">`1`</div><div class="line number7 index6 alt2">`1`</div><div class="line number8 index7 alt1">`1`</div></div></td></tr></tbody></table></div></div>
</div>

# sortByKey()

&nbsp;sortByKey()的功能是返回一个根据键排序的RDD。

<div class="cnblogs_Highlighter sh-gutter">
<div><div id="highlighter_318614" class="syntaxhighlighter  scala"><div class="toolbar"><span></span></div><table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div><div class="line number2 index1 alt1">2</div><div class="line number3 index2 alt2">3</div><div class="line number4 index3 alt1">4</div><div class="line number5 index4 alt2">5</div><div class="line number6 index5 alt1">6</div><div class="line number7 index6 alt2">7</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2">`scala&gt; pairRDD.sortByKey()`</div><div class="line number2 index1 alt1">`res``0``:` `org.apache.spark.rdd.RDD[(String, Int)] ``=` `ShuffledRDD[``2``] at sortByKey at &lt;console&gt;``:``34`</div><div class="line number3 index2 alt2">`scala&gt; pairRDD.sortByKey().foreach(println)`</div><div class="line number4 index3 alt1">`(Hadoop,``1``)`</div><div class="line number5 index4 alt2">`(Hive,``1``)`</div><div class="line number6 index5 alt1">`(Spark,``1``)`</div><div class="line number7 index6 alt2">`(Spark,``1``)`</div></div></td></tr></tbody></table></div></div>
</div>

# mapValues(func)

我们经常会遇到一种情形，我们只想对键值对RDD的value部分进行处理，而不是同时对key和value进行处理。对于这种情形，Spark提供了mapValues(func)，它的功能是，对键值对RDD中的每个value都应用一个函数，但是，key不会发生变化。比如，对四个键值对("spark",1)、("spark",2)、("hadoop",3)和("hadoop",5)构成的pairRDD，如果执行pairRDD.mapValues(x =&gt; x+1)，就会得到一个新的键值对RDD，它包含下面四个键值对("spark",2)、("spark",3)、("hadoop",4)和("hadoop",6)。&nbsp;

<div class="cnblogs_Highlighter sh-gutter">
<div><div id="highlighter_845055" class="syntaxhighlighter  scala"><div class="toolbar"><span></span></div><table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div><div class="line number2 index1 alt1">2</div><div class="line number3 index2 alt2">3</div><div class="line number4 index3 alt1">4</div><div class="line number5 index4 alt2">5</div><div class="line number6 index5 alt1">6</div><div class="line number7 index6 alt2">7</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2">`scala&gt; pairRDD.mapValues(x ``=``&gt; x+``1``)`</div><div class="line number2 index1 alt1">`res``2``:` `org.apache.spark.rdd.RDD[(String, Int)] ``=` `MapPartitionsRDD[``4``] at mapValues at &lt;console&gt;``:``34`</div><div class="line number3 index2 alt2">`scala&gt; pairRDD.mapValues(x ``=``&gt; x+``1``).foreach(println)`</div><div class="line number4 index3 alt1">`(Hadoop,``2``)`</div><div class="line number5 index4 alt2">`(Spark,``2``)`</div><div class="line number6 index5 alt1">`(Hive,``2``)`</div><div class="line number7 index6 alt2">`(Spark,``2``)`</div></div></td></tr></tbody></table></div></div>
</div>

# join

join(连接)操作是键值对常用的操作。“连接”(join)这个概念来自于关系数据库领域，因此，join的类型也和关系数据库中的join一样，包括内连接(join)、左外连接(leftOuterJoin)、右外连接(rightOuterJoin)等。最常用的情形是内连接，所以，join就表示内连接。
对于内连接，对于给定的两个输入数据集(K,V1)和(K,V2)，只有在两个数据集中都存在的key才会被输出，最终得到一个(K,(V1,V2))类型的数据集。

比如，pairRDD1是一个键值对集合{("spark",1)、("spark",2)、("hadoop",3)和("hadoop",5)}，pairRDD2是一个键值对集合{("spark","fast")}，那么，pairRDD1.join(pairRDD2)的结果就是一个新的RDD，这个新的RDD是键值对集合{("spark",1,"fast"),("spark",2,"fast")}。对于这个实例，我们下面在spark-shell中运行一下：

<div class="cnblogs_Highlighter sh-gutter">
<div><div id="highlighter_155324" class="syntaxhighlighter  scala"><div class="toolbar"><span></span></div><table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div><div class="line number2 index1 alt1">2</div><div class="line number3 index2 alt2">3</div><div class="line number4 index3 alt1">4</div><div class="line number5 index4 alt2">5</div><div class="line number6 index5 alt1">6</div><div class="line number7 index6 alt2">7</div><div class="line number8 index7 alt1">8</div><div class="line number9 index8 alt2">9</div><div class="line number10 index9 alt1">10</div><div class="line number11 index10 alt2">11</div><div class="line number12 index11 alt1">12</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2">`scala&gt; ``val` `pairRDD``1` `=` `sc.parallelize(Array((``"spark"``,``1``),(``"spark"``,``2``),(``"hadoop"``,``3``),(``"hadoop"``,``5``)))`</div><div class="line number2 index1 alt1">`pairRDD``1``:` `org.apache.spark.rdd.RDD[(String, Int)] ``=` `ParallelCollectionRDD[``24``] at parallelize at &lt;console&gt;``:``27`</div><div class="line number3 index2 alt2">`&nbsp;`&nbsp;</div><div class="line number4 index3 alt1">`scala&gt; ``val` `pairRDD``2` `=` `sc.parallelize(Array((``"spark"``,``"fast"``)))`</div><div class="line number5 index4 alt2">`pairRDD``2``:` `org.apache.spark.rdd.RDD[(String, String)] ``=` `ParallelCollectionRDD[``25``] at parallelize at &lt;console&gt;``:``27`</div><div class="line number6 index5 alt1">`&nbsp;`&nbsp;</div><div class="line number7 index6 alt2">`scala&gt; pairRDD``1``.join(pairRDD``2``)`</div><div class="line number8 index7 alt1">`res``9``:` `org.apache.spark.rdd.RDD[(String, (Int, String))] ``=` `MapPartitionsRDD[``28``] at join at &lt;console&gt;``:``32`</div><div class="line number9 index8 alt2">`&nbsp;`&nbsp;</div><div class="line number10 index9 alt1">`scala&gt; pairRDD``1``.join(pairRDD``2``).foreach(println)`</div><div class="line number11 index10 alt2">`(spark,(``1``,fast))`</div><div class="line number12 index11 alt1">`(spark,(``2``,fast))`</div></div></td></tr></tbody></table></div></div>
</div>

&nbsp;

&nbsp;

&nbsp;

详细请参考《Spark快速大数据分析》
</div>
