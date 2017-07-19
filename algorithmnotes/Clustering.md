# 聚类（上）

## 目标：
1. 理解相似度度量的各种方法及相互联系
2. 掌握K-means聚类的思路和使用条件
3. 层次聚类的思路和方法
4. 聚类指数及其意义

## 聚类的定义：
- 聚类就是对大量未知标注的数据集，按数据的内在相似性将数据集划分为多个类别，使类别内的数据相似度较大而类别间的数据相似度较小。
- 无监督
---
## 相似度/距离计算方法总结

- 欧式距离  
<a href="https://www.codecogs.com/eqnedit.php?latex=d&space;=&space;\sqrt{(x_1&space;-&space;x_2)^2&space;&plus;&space;(y_1&space;-&space;y_2)^2&space;&plus;&space;(z_1&space;-&space;z_2)^2}&space;=&space;||X_1&space;-&space;X_2||" target="_blank"><img src="https://latex.codecogs.com/png.latex?d&space;=&space;\sqrt{(x_1&space;-&space;x_2)^2&space;&plus;&space;(y_1&space;-&space;y_2)^2&space;&plus;&space;(z_1&space;-&space;z_2)^2}&space;=&space;||X_1&space;-&space;X_2||" title="d = \sqrt{(x_1 - x_2)^2 + (y_1 - y_2)^2 + (z_1 - z_2)^2} = ||X_1 - X_2||" /></a>
，即二范式。

<a href="https://www.codecogs.com/eqnedit.php?latex=d&space;=&space;\sqrt[n]{(x_1&space;-&space;x_2)^n&space;&plus;&space;(y_1&space;-&space;y_2)^n&space;&plus;&space;(z_1&space;-&space;z_2)^n}&space;=&space;||X_1&space;-&space;X_2||_n" target="_blank"><img src="https://latex.codecogs.com/png.latex?d&space;=&space;\sqrt[n]{(x_1&space;-&space;x_2)^n&space;&plus;&space;(y_1&space;-&space;y_2)^n&space;&plus;&space;(z_1&space;-&space;z_2)^n}&space;=&space;||X_1&space;-&space;X_2||_n" title="d = \sqrt[n]{(x_1 - x_2)^n + (y_1 - y_2)^n + (z_1 - z_2)^n} = ||X_1 - X_2||_n" /></a>
，即n范式。

- 杰卡德相似度系数

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;J(A,&space;B)&space;=&space;\frac{|A\bigcap&space;B|}{|A\bigcup&space;B|}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;J(A,&space;B)&space;=&space;\frac{|A\bigcap&space;B|}{|A\bigcup&space;B|}" title="J(A, B) = \frac{|A\bigcap B|}{|A\bigcup B|}" /></a>，通常降低热门元素的权重，提高冷门元素的权重。

- 余弦相似度

<a href="https://www.codecogs.com/eqnedit.php?latex=cos\theta&space;=\frac{\overrightarrow{a}&space;\cdot&space;\overrightarrow{b}}{|\overrightarrow{a}|\cdot&space;|\overrightarrow{b}|}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?cos\theta&space;=\frac{\overrightarrow{a}&space;\cdot&space;\overrightarrow{b}}{|\overrightarrow{a}|\cdot&space;|\overrightarrow{b}|}" title="cos\theta =\frac{\overrightarrow{a} \cdot \overrightarrow{b}}{|\overrightarrow{a}|\cdot |\overrightarrow{b}|}" /></a>

- pearson相似度

<a href="https://www.codecogs.com/eqnedit.php?latex=\rho&space;(X,&space;Y)&space;=&space;\frac{cov(X,&space;Y)}{\sqrt{var(X)\cdot&space;var(Y)}},&space;X&space;=&space;x_1,&space;x_2,&space;...,&space;x_n,&space;Y=y_1,&space;y_2,&space;y_3,&space;...,&space;y_n" target="_blank"><img src="https://latex.codecogs.com/png.latex?\rho&space;(X,&space;Y)&space;=&space;\frac{cov(X,&space;Y)}{\sqrt{var(X)\cdot&space;var(Y)}},&space;X&space;=&space;x_1,&space;x_2,&space;...,&space;x_n,&space;Y=y_1,&space;y_2,&space;y_3,&space;...,&space;y_n" title="\rho (X, Y) = \frac{cov(X, Y)}{\sqrt{var(X)\cdot var(Y)}}, X = x_1, x_2, ..., x_n, Y=y_1, y_2, y_3, ..., y_n" /></a>
