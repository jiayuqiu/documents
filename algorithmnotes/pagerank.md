## 原文地址：http://blog.csdn.net/hguisu/article/details/7996185

<div id="article_details" class="details">

<div class="article_title"><span class="ico ico_type_Original"></span>

# <span class="link_title">[PageRank算法](/hguisu/article/details/7996185)</span>

</div>

<div class="article_manage clearfix">

<div class="article_r"><span class="link_postdate">2012-09-21 17:02</span> <span class="link_view" title="阅读次数">108720人阅读</span> <span class="link_comments" title="评论次数">[评论](#comments)(15)</span> <span class="link_collect tracking-ad" data-mod="popu_171">[收藏](javascript:void(0); "收藏")</span> <span class="link_report">[举报](#report "举报")</span></div>

</div>

<style type="text/css">.embody{ padding:10px 10px 10px; margin:0 -20px; border-bottom:solid 1px #ededed; } .embody_b{ margin:0 ; padding:10px 0; } .embody .embody_t,.embody .embody_c{ display: inline-block; margin-right:10px; } .embody_t{ font-size: 12px; color:#999; } .embody_c{ font-size: 12px; } .embody_c img,.embody_c em{ display: inline-block; vertical-align: middle; } .embody_c img{ width:30px; height:30px; } .embody_c em{ margin: 0 20px 0 10px; color:#333; font-style: normal; }</style> <script type="text/javascript">$(function () { try { var lib = eval("("+$("#lib").attr("value")+")"); var html = ""; if (lib.err == 0) { $.each(lib.data, function (i) { var obj = lib.data[i]; //html += '<img src="' + obj.logo + '"/>' + obj.name + "&nbsp;&nbsp;"; html += ' <a href="' + obj.url + '" target="_blank">'; html += ' <img src="' + obj.logo + '">'; html += ' <em><b>' + obj.name + '</b></em>'; html += ' </a>'; }); if (html != "") { setTimeout(function () { $("#lib").html(html); $("#embody").show(); }, 100); } } } catch (err) { } });</script>

<div class="category clearfix">

<div class="category_l">![](http://static.blog.csdn.net/images/category_icon.jpg) <span>分类：</span></div>

<div class="category_r"><label onclick="GetCategoryArticles('1230933','hguisu','top','7996185');"><span onclick="_gaq.push(['_trackEvent','function', 'onclick', 'blog_articles_fenlei']);">搜索引擎Search Engine_（14）_</span> ![](http://static.blog.csdn.net/images/arrow_triangle _down.jpg) ![](http://static.blog.csdn.net/images/arrow_triangle_up.jpg)

<div class="subItem">

<div class="subItem_t">[作者同类文章](http://blog.csdn.net/hguisu/article/category/1230933)_X_</div>

</div>

</label><label onclick="GetCategoryArticles('1054628','hguisu','top','7996185');"><span onclick="_gaq.push(['_trackEvent','function', 'onclick', 'blog_articles_fenlei']);">数据结构与算法_（28）_</span> ![](http://static.blog.csdn.net/images/arrow_triangle _down.jpg) ![](http://static.blog.csdn.net/images/arrow_triangle_up.jpg)

<div class="subItem">

<div class="subItem_t">[作者同类文章](http://blog.csdn.net/hguisu/article/category/1054628)_X_</div>

</div>

</label></div>

</div>

<div class="bog_copyright">

版权声明：本文为博主原创文章，未经博主允许不得转载。

</div>

<div style="border:solid 1px #ccc; background:#eee; float:left; min-width:200px;padding:4px 10px;">

<span style="float:left;">目录[(?)](# "系统根据文章中H1到H6标签自动生成文章目录")</span>[[+]](# "展开")

1.  [1 PageRank算法概述](#t0)
2.  [2 从入链数量到 PageRank](#t1)
3.  [3 PageRank算法原理](#t2)
4.  [4 PageRank幂法计算线性代数应用](#t3)
5.  [5 PageRank算法优缺点](#t4)

</div>

<div id="article_content" class="article_content tracking-ad" data-mod="popu_307" data-dsm="post">

## <a name="t0" target="_blank"></a><span style="color:#336699"><span style="line-height:36px; font-size:22px">1\. PageRank算法概述</span></span>

<span style="font-size:14px">         PageRank,<span style="color:#3333ff">即**<span style="font-size:14px">网页排名</span>**<span>，又称</span>**网页级别**<span>、</span>**Google左侧排名**<span>或</span>**佩奇排名。**</span></span>

<span style="font-size:14px">        是Google创始人拉里·佩奇和谢尔盖·布林于1997年构建早期的搜索系统原型时提出的链接分析[算法](http://lib.csdn.net/base/datastructure "算法与数据结构知识库")，自从Google在商业上获得空前的成功后，该算法也成为其他[搜索引擎](http://lib.csdn.net/base/searchengine "搜索引擎知识库")和学术界十分关注的计算模型。目前很多重要的链接分析算法都是在PageRank算法基础上衍生出来的。PageRank是Google用于用来标识网页的等级/重要性的一种方法，是Google用来衡量一个网站的好坏的唯一标准。在揉合了诸如Title标识和Keywords标识等所有其它因素之后，Google通过PageRank来调整结果，使那些更具“等级/重要性”的网页在搜索结果中另网站排名获得提升，从而提高搜索结果的相关性和质量。其级别从0到10级，10级为满分。PR值越高说明该网页越受欢迎（越重要）。例如：一个PR值为1的网站表明这个网站不太具有流行度，而PR值为7到10则表明这个网站非常受欢迎（或者说极其重要）。一般PR值达到4，就算是一个不错的网站了。Google把自己的网站的PR值定到10，这说明Google这个网站是非常受欢迎的，也可以说这个网站非常重要。</span>

## <a name="t1" target="_blank"></a><span style="color:#336699"><span style="line-height:36px; font-size:22px">2\. 从入链数量到 PageRank</span></span>

<span style="font-size:14px">        在PageRank提出之前，已经有研究者提出利用网页的入链数量来进行链接分析计算，这种入链方法假设一个网页的入链越多，则该网页越重要。早期的很多搜索引擎也采纳了入链数量作为链接分析方法，对于搜索引擎效果提升也有较明显的效果。 PageRank除了考虑到入链数量的影响，还参考了网页质量因素，两者相结合获得了更好的网页重要性评价标准。
对于某个互联网网页A来说，该网页PageRank的计算基于以下两个基本假设：
     <span style="color:#ff0000"></span> <span style="color:#3333ff">数量假设</span>：在Web图模型中，如果一个页面节点接收到的其他网页指向的入链数量越多，那么这个页面越重要。
    <span style="color:#ff0000"> <span style="color:#3333ff">质量假设</span></span><span style="color:#000000">：</span>指向页面A的入链质量不同，质量高的页面会通过链接向其他页面传递更多的权重。所以越是质量高的页面指向页面A，则页面A越重要。
       利用以上两个假设，PageRank算法刚开始赋予每个网页相同的重要性得分，通过迭代递归计算来更新每个页面节点的PageRank得分，直到得分稳定为止。 PageRank计算得出的结果是网页的重要性评价，这和用户输入的查询是没有任何关系的，<span style="color:#ff0000">即算法是主题无关的</span>。假设有一个搜索引擎，其相似度计算函数不考虑内容相似因素，完全采用PageRank来进行排序，那么这个搜索引擎的表现是什么样子的呢？这个搜索引擎对于任意不同的查询请求，返回的结果都是相同的，即返回PageRank值最高的页面。</span>

<span style="font-size:14px"></span>

## <a name="t2" target="_blank"></a><span style="color:#336699"><span style="line-height:36px; font-size:22px">3\. PageRank算法原理</span></span>

<span style="font-size:14px">      PageRank的计算充分利用了两个假设：<span style="color:#3333ff">数量假设<span style="color:#000000">和</span>质量假设</span>。步骤如下：
      **<span style="color:#000000">1）</span><span style="color:#000000">在初始阶段</span>：**网页通过链接关系构建起Web图，每个页面设置相同的PageRank值，通过若干轮的计算，会得到每个页面所获得的最终PageRank值。随着每一轮的计算进行，网页当前的PageRank值会不断得到更新。</span>

<span style="font-size:14px">      **2）在<span style="font-size:14px">一轮中更新页面PageRank得分的计算方法：</span>**在一轮更新页面PageRank得分的计算中，</span><span style="font-size:14px">每个页面将其当前的PageRank值平均分配到本页面包含的出链上，这样每个链接即获得了相应的权值。而每个页面将所有指向本页面的入链所传入的权值求和，即可得到新的PageRank得分。当每个页面都获得了更新后的PageRank值，就完成了一轮PageRank计算。 </span>

<span style="font-size:14px; color:#3333ff">**3.2 基本思想：**</span>

<span style="font-size:14px">       如果网页T存在一个指向网页A的连接，则表明T的所有者认为A比较重要，从而把T的一部分重要性得分赋予A。这个重要性得分值为：PR（T）/L(T)</span>

<div class="spctrl"><span style="font-size:14px"></span></div>

<span style="font-size:14px">　    其中PR（T）为T的PageRank值，L(T)为T的出链数</span>

<span style="font-size:14px">        则A的PageRank值为一系列类似于T的页面重要性得分值的累加。</span>

<span><span style="font-size:14px">        <span style="color:#ff6600">即一个页面的<span style="color:#000000">得票数</span>由所有链向它的页面的重要性来决定，到一个页面的</span></span></span>[<span style="font-size:14px; color:#ff6600">超链接</span>](http://zh.wikipedia.org/wiki/%E8%B6%85%E9%93%BE%E6%8E%A5 "超链接")<span style="color:#ff6600"><span style="font-size:14px"><span>相当于对该页投一票。一个页面的PageRank是由所有链向它的页面<span style="color:#000000">（链入页面）</span></span><span>的重要性经过</span></span></span>[<span style="font-size:14px; color:#ff6600">递归</span>](http://zh.wikipedia.org/wiki/%E9%80%92%E5%BD%92 "递归")<span><span style="font-size:14px">算法得到的。一个有较多链入的页面会有较高的等级，相反如果一个页面没有任何链入页面，那么它没有等级。</span></span>

**<span style="color:#3333ff"><span style="font-size:14px">**<span style="color:#3333ff">3.3 PageRank</span>**简单计算：</span></span>**

<span style="font-size:14px">       假设一个由只有4个页面组成的集合：A，B，C和D。如果所有页面都链向A，那么A的PR（PageRank）值将是B，C及D的和。</span>

<span style="font-size:14px">       ![](http://img.my.csdn.net/uploads/201209/20/1348120060_7086.png)</span>

<span style="font-size:14px">       继续假设B也有链接到C，并且D也有链接到包括A的3个页面。一个页面不能投票2次。所以B给每个页面半票。以同样的逻辑，D投出的票只有三分之一算到了A的PageRank上。</span>

<span style="font-size:14px">       ![](http://img.my.csdn.net/uploads/201209/20/1348120099_6078.png)</span>

<span style="font-size:14px">      换句话说，根据链出总数平分一个页面的PR值。</span>

       ![](http://img.my.csdn.net/uploads/201209/20/1348120123_5802.png)

**<span style="color:#3333ff">例子：</span>**

<span style="font-size:14px">        如**图1** 所示的例子来说明PageRank的具体计算过程。</span><span style="font-size:14px">  </span>

<span style="font-size:14px">       ![](http://img.my.csdn.net/uploads/201209/20/1348125472_5651.jpg)                   </span> 

<span style="font-size:14px">       </span><span style="font-size:14px"></span>

**<span style="font-size:14px; color:#3333ff">3.4  修正PageRank计算公式：</span>**

<span style="font-size:14px">**<span style="color:#3333ff">   </span>**<span>      由于<span>存在一些出链为0，也就是那些不链接任何其他网页的网， 也称为孤立网页，使得很多网页能被访问到。因此需要对 PageRank公式进行修正，即</span></span><span style="font-size:14px"><span><span>在简单公式的基础上增加了**阻尼系数（damping factor）**q， q一般取值q=0.85。</span></span></span></span>

<span><span style="font-size:14px">      <span>其意义是，在任意时刻，用户到达某页面后并继续向后浏览的概率。 1- q= 0.15</span><span>就是用户停止点击，随机跳到新URL的概率）的算法被用到了所有页面上，估算页面可能被上网者放入书签的概率。</span></span></span>

<span style="font-size:14px"><span><span>      <span style="font-size:14px">最后，</span>即<span style="font-size:14px">所有这些被换算为一个百分比再乘上一个系数q。由于下面的算法，没有页面的PageRank会是0。所以，Google通过数学系统给了每个页面一个最小值。</span></span></span></span>

      ![](http://img.my.csdn.net/uploads/201209/20/1348120862_4424.png)

     这个公式就是.S Brin 和 L. Page 在《The Anatomy of a Large- scale Hypertextual Web Search Engine Computer Networks and ISDN Systems 》定义的公式。

<span>     所以一个页面的PageRank是由其他页面的PageRank计算得到。Google不断的重复计算每个页面的PageRank。如果给每个页面一个随机PageRank值（非0），那么经过不断的重复计算，这些页面的PR值会趋向于正常和稳定。这就是搜索引擎使用它的原因。</span>

## <a name="t3" target="_blank"></a><span style="color:#336699"><span style="line-height:36px; font-size:22px">4\. PageRank幂法计算(线性代数应用)</span></span>

<span style="font-size:14px; color:#3333ff">**4.1 完整公式：**</span>

<span style="color:#ff0000">关于这节内容，可以查阅：[谷歌背后的数学](http://www.changhai.org/articles/technology/misc/google_math.php)</span>

<span style="font-size:14px">首先求完整的公式：</span>

<span style="font-size:14px">Arvind Arasu 在《Junghoo Cho Hector Garcia - Molina, Andreas Paepcke, Sriram Raghavan. Searching the Web》 更加准确的表达为：</span>

<span style="font-size:14px"> ![](http://img.my.csdn.net/uploads/201209/20/1348122433_4418.png)</span>

<span style="font-size:14px"><span>![](http://img.my.csdn.net/uploads/201209/20/1348122788_2885.png)是被研究的页面，![](http://img.my.csdn.net/uploads/201209/20/1348122797_7445.png)</span><span>是![](http://img.my.csdn.net/uploads/201209/20/1348132270_8785.png)链入</span><span>页面的数量，![](http://img.my.csdn.net/uploads/201209/20/1348122803_7423.png)</span><span>是![](http://img.my.csdn.net/uploads/201209/20/1348122817_4962.png)</span><span>链出页面的数量，而</span>_N_<span>是所有页面的数量。</span></span>

<span style="font-size:14px">**PageRank**<span>值是一个特殊矩阵中的特征向量。这个特征向量为：</span></span>

<span style="font-size:14px">![](http://img.my.csdn.net/uploads/201209/20/1348122849_2425.png)</span>

<span style="font-family:Arial"><span style="font-size:14px">**R**<span>是如下等式的一个解：</span></span></span>

<span style="font-size:14px">![](http://img.my.csdn.net/uploads/201209/20/1348122918_3209.png)</span>

<span style="font-size:14px">如果网页i有指向网页j的一个链接，则</span>

<span style="font-size:14px">![](http://img.my.csdn.net/uploads/201209/20/1348122992_2994.png)</span>

<span style="font-size:14px">否则![](http://img.my.csdn.net/uploads/201209/20/1348122985_4727.png)＝0。</span>

<span style="font-size:14px; color:#3333ff">**4.2 使用幂法求PageRank**</span>

<span style="font-size:14px">      那我们</span><span style="font-size:14px">PageRank 公式可以转换为求解![](http://img.my.csdn.net/uploads/201209/21/1348200903_7916.jpg)的值，</span>

<span style="font-size:14px">      其中矩阵为 A = q  × P + ( 1 一 q) * ![](http://img.my.csdn.net/uploads/201209/21/1348210001_1640.jpg) /N 。 P 为概率转移矩阵，![](http://img.my.csdn.net/uploads/201209/21/1348209145_9759.jpg)为 n  维的全 1 行. 则 ![](http://img.my.csdn.net/uploads/201209/21/1348210001_1640.jpg)=</span>

<span style="font-size:14px">      ![](http://img.my.csdn.net/uploads/201209/21/1348209516_2236.jpg)</span>

<span style="font-size:14px; color:#3333ff">     幂法计算过程如下：</span><span style="font-size:14px">
      X  设任意一个初始向量,<span style="font-size:14px"> 即设置初始每个网页的 PageRank值均。一般为1.</span></span>

<span style="font-size:14px">     R = AX;</span>

<span style="font-size:14px">     while  (1 )(</span>

<span style="font-size:14px">            if ( l X - R I  <  ![](http://img.my.csdn.net/uploads/201209/21/1348216346_4582.jpg)) { //如果<span style="font-size:14px">最后两次的结果近似或者相同，返回R</span></span>

<span style="font-size:14px">                  return R;</span>

<span style="font-size:14px">           }    else   {</span>

<span style="font-size:14px">                X =R;</span>

<span style="font-size:14px">               R = AX;</span>

<span style="font-size:14px">         }</span>

<span style="font-size:14px">    }</span>

<span style="font-size:14px; color:#3333ff">**4.3 求解步骤：**</span>

**<span style="font-size:14px; color:#3333ff">一、 P概率转移矩阵的计算过程:</span>**

<span style="font-size:14px">        先建立一个网页间的链接关系的模型,即我们需要合适的[数据结构](http://lib.csdn.net/base/datastructure "算法与数据结构知识库")表示页面间的连接关系。</span>

<span style="font-size:14px">**      1) 首先我们使用图的形式来表述网页之间关系：**</span>

<span><span style="font-size:14px">       现在假设只有四张网页集合：A、B、C，其抽象结构如下图1：</span></span>

<span style="font-size:14px"><span>        ![](http://img.my.csdn.net/uploads/201209/21/1348206860_1448.jpg)</span></span>

<span style="font-size:14px"><span></span> <span style="color:#ffffff">           </span><span style="color:#000000">  **图<span style="font-size:14px">1 <span style="font-size:14px">网页间的链接关系</span></span>**</span></span>

<span style="font-size:14px">      显然这个图是强连通的（从任一节点出发都可以到达另外任何一个节点）。</span>

<span style="font-size:14px">      **2）<span style="font-size:14px">我们用矩阵表示连通图：</span>**</span>

<span style="font-size:14px">       用邻接矩阵 P表示这个图中顶点关系 ，如果顶（页面）i向顶点（页面）j有链接情况 ，则pij   =   1 ，否则pij   =   0 。如图2所示。</span><span style="font-size:14px">如果网页文件总数为N ， 那么这个网页链接矩阵就是一个N x N  的矩 阵 。 </span>

<span style="font-size:14px">      **3）<span style="font-size:14px">网页链接概率矩阵</span>**</span>

<span style="font-size:14px">       然后将每一行除以该行非零数字之和，即（每行非0数之和就是链接网个数）则得到新矩阵P’，如**图3**所示。 这个矩阵记录了 每个网页跳转到其他网页的概率，即其中i行j列的值表示用户从页面i 转到页面j的概率。<span style="color:#ff6600">图1 中A页面链向B、C，所以一个用户从A跳转到B、C的概率各为1/2。</span></span>

<span style="font-size:14px; color:#ff6600">      **<span style="color:#000000">4）<span style="font-size:14px">概率转移矩阵P</span></span>**</span>

<span style="font-size:14px">       采用P’ 的转置矩 阵进行计算， 也就是上面提到的概率转移矩阵P 。  如**图4**所示：</span>

<span style="font-size:14px">     </span>

<div><span style="font-size:14px">     ![](http://img.my.csdn.net/uploads/201209/21/1348208177_6048.jpg)      ![](http://img.my.csdn.net/uploads/201209/21/1348207180_8993.jpg)![](http://img.my.csdn.net/uploads/201209/21/1348207686_3235.jpg)</span></div>

<div>**<span style="font-size:14px">         图2  网页链接矩阵：                                      图3  网页链接概率矩阵：  </span>**</div>

<div><span style="font-size:14px"></span> </div>

<div><span style="font-size:14px">![](http://img.my.csdn.net/uploads/201209/21/1348207180_8993.jpg) ![](http://img.my.csdn.net/uploads/201209/21/1348208398_8541.jpg)</span></div>

<span style="font-size:14px">**                         图4  P’ 的转置矩 阵**</span>

<span style="font-size:14px"> </span>

**<span style="font-size:14px; color:#3333ff">二、 A矩阵计算过程。</span>**

<span style="font-size:14px">      **<span style="color:#000000">1）P概率转移矩阵  :</span>**</span>

<span style="font-size:14px">**<span style="color:#3333ff">       ![](http://img.my.csdn.net/uploads/201209/21/1348219027_3013.jpg)</span>**</span>

<span style="font-size:14px">    **  2）![](http://img.my.csdn.net/uploads/201209/21/1348210001_1640.jpg)/N 为：**</span>

<span style="font-size:14px">     ![](http://img.my.csdn.net/uploads/201209/21/1348210123_1726.jpg)</span>

<span style="font-size:14px">      **3）A矩阵为：**q  × P + ( 1 一 q) * ![](http://img.my.csdn.net/uploads/201209/21/1348210001_1640.jpg) /N = 0.85  × P + 0.15  *![](http://img.my.csdn.net/uploads/201209/21/1348210001_1640.jpg) /N</span>

<span style="font-size:14px">     ![](http://img.my.csdn.net/uploads/201209/21/1348211506_7900.jpg)</span>

<span style="font-size:14px">      初始每个网页的 PageRank值均为1 ， 即X~t = ( 1 ， 1 ， 1 ) 。 </span>

<span style="font-size:14px; color:#3333ff">**三、 循环迭代计算PageRank的过程**</span>

<span style="font-size:14px">       **第一步：**</span>

<span style="font-size:14px">       ![](http://img.my.csdn.net/uploads/201209/21/1348216924_2722.jpg)</span>

<span style="font-size:14px">          因为X 与R的差别较大。 继续迭代。</span>

<span style="font-size:14px">          **第二步：**</span>

<span style="font-size:14px">           ![](http://img.my.csdn.net/uploads/201209/21/1348212800_6350.jpg)</span>

<span style="font-size:14px">       继续迭代这个过程...</span>

<span style="font-size:14px">      直到最后两次的结果近似或者相同，即R最终收敛，R 约等于X，此时计算停止。最终的R 就是各个页面的 PageRank 值。</span>

<span style="font-size:14px">用幂法计算PageRank 值总是收敛的，即计算的次数是有限的。</span>

<span style="font-size:14px">      Larry Page和Sergey Brin 两人从理论上证明了不论初始值如何选取，这种算法都保证了网页排名的估计值能收敛到他们的真实值。</span>

<span style="font-size:14px"></span><span style="font-size:14px">      由于互联网上网页的数量是巨大的，上面提到的二维矩阵从理论上讲有网页数目平方之多个元素。如果我们假定有十亿个网页，那么这个矩阵 就有一百亿亿个元素。这样大的矩阵相乘，计算量是非常大的。<span style="font-size:14px">Larry Page和Sergey Brin</span>两人利用稀疏矩阵计算的技巧，大大的简化了计算量。</span>

<span style="font-size:14px"></span> 

## <a name="t4" target="_blank"></a><span style="color:#336699"><span style="line-height:36px; font-size:22px">5\. PageRank算法优缺点</span></span>

<span style="font-size:14px"><span style="color:#3333ff">优点</span>：</span>

<span style="font-size:14px">       <span style="color:#009900"> 是一个与查询无关的静态算法，所有网页的PageRank值通过离线计算获得；有效减少在线查询时的计算量，极大降低了查询响应时间。</span></span>

<span style="font-size:14px; color:#3333ff">缺点：</span>

<span style="font-size:14px">       <span style="color:#ff0000">1）人们的查询具有主题特征，PageRank忽略了主题相关性，导致结果的</span></span>[<span style="font-size:14px; color:#ff0000">相关性</span>](http://baike.baidu.com/view/1326213.htm)<span style="font-size:14px; color:#ff0000">和主题性降低</span>

<span style="color:#ff0000"><span style="font-size:14px">        2）<span>旧的页面等级会比新页面高。因为即使是非常好的新页面也不会有很多上游链接，除非它是某个站点的子站点。</span></span></span>

<span style="font-size:14px"> </span>

 参考文献：

[<u><span style="color:#0066cc">维基百科</span></u>http://en.wikipedia.org/wiki/Page_rank](http://en.wikipedia.org/wiki/Page_rank)

PageRank算法的分析及实现

<span style="font-family:SimSun; font-size:12px">《这就是搜索引擎:核心技术详解》</span>

</div>

<div class="bdsharebuttonbox tracking-ad bdshare-button-style0-16" style="float: right;" data-mod="popu_172" data-bd-bind="1501222483565">[](#)[](# "分享到QQ空间")[](# "分享到新浪微博")[](# "分享到腾讯微博")[](# "分享到人人网")[](# "分享到微信")</div>

<script>window._bd_share_config = { "common": { "bdSnsKey": {}, "bdText": "", "bdMini": "1", "bdMiniList": false, "bdPic": "", "bdStyle": "0", "bdSize": "16" }, "share": {} }; with (document) 0[(getElementsByTagName('head')[0] || body).appendChild(createElement('script')).src = 'http://bdimg.share.baidu.com/static/api/js/share.js?v=89860593.js?cdnversion=' + ~(-new Date() / 36e5)];</script> <script type="text/javascript">document.getElementById("bdshell_js").src = "http://bdimg.share.baidu.com/static/js/shell_v2.js?cdnversion=" + Math.ceil(new Date()/3600000)</script>

<div id="digg" articleid="7996185">

<dl id="btnDigg" class="digg digg_disable" onclick="btndigga();">

<dt>顶</dt>

<dd>26</dd>

</dl>

<dl id="btnBury" class="digg digg_disable" onclick="btnburya();">

<dt>踩</dt>

<dd>0</dd>

</dl>

</div>

<div class="tracking-ad" data-mod="popu_222">[ ](javascript:void(0);)</div>

<div class="tracking-ad" data-mod="popu_223">[ ](javascript:void(0);)</div>

<script type="text/javascript">function btndigga() { $(".tracking-ad[data-mod='popu_222'] a").click(); } function btnburya() { $(".tracking-ad[data-mod='popu_223'] a").click(); }</script>

*   <span onclick="_gaq.push(['_trackEvent','function', 'onclick', 'blog_articles_shangyipian']);location.href='http://blog.csdn.net/hguisu/article/details/7995703';">上一篇</span>[搜索引起的链接分析-计算网页的重要性](http://blog.csdn.net/hguisu/article/details/7995703)
*   <span onclick="_gaq.push(['_trackEvent','function', 'onclick', 'blog_articles_xiayipian']);location.href='http://blog.csdn.net/hguisu/article/details/8005192';">下一篇</span>[链接分析算法之：主题敏感PageRank](http://blog.csdn.net/hguisu/article/details/8005192)

<div class="similar_article">

<div class="similar_c" style="margin:20px 0px 0px 0px">

<div class="similar_c_t">  相关文章推荐</div>

<div class="similar_wrap tracking-ad" data-mod="popu_36">

*   _•_ [pageRank算法核心思想](http://blog.csdn.net/hewei0241/article/details/8276029 "pageRank算法核心思想")
*   _•_ [[疯狂Java]集合：SortedMap、TreeMap、equals/compareTo的标准写法](http://blog.csdn.net/Lirx_Tech/article/details/51548829 "[疯狂Java]集合：SortedMap、TreeMap、equals/compareTo的标准写法")
*   _•_ [搜索引起的链接分析-计算网页的重要性](http://blog.csdn.net/hguisu/article/details/7995703 "搜索引起的链接分析-计算网页的重要性")
*   _•_ [PageRank算法原理剖析及Spark实现](http://blog.csdn.net/ZCF1002797280/article/details/50254069 "PageRank算法原理剖析及Spark实现")
*   _•_ [深入探讨PageRank（二）：PageRank原理剖析](http://blog.csdn.net/MONKEY_D_MENG/article/details/6556295 "深入探讨PageRank（二）：PageRank原理剖析")

*   _•_ [文本相似度结合PageRank算法](http://blog.csdn.net/zbf8441372/article/details/8847457 "文本相似度结合PageRank算法")
*   _•_ [PageRank学习一](http://blog.csdn.net/ywf008/article/details/51725578 "PageRank学习一")
*   _•_ [浅析pagerank](http://blog.csdn.net/wangzhiqing3/article/details/12292225 "浅析pagerank")
*   _•_ [PageRank的初步理解和实践](http://blog.csdn.net/u014571011/article/details/51518518 "PageRank的初步理解和实践")
*   _•_ [pagerank](http://blog.csdn.net/hh794362661/article/details/45850725 "pagerank")

</div>

</div>

</div>

</div>
