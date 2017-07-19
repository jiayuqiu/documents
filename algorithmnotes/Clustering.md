# 聚类（上）

## 目标：
1. 理解相似度度量的各种方法及相互联系
2. 掌握K-means聚类的思路和使用条件
3. 层次聚类的思路和方法
4. 聚类指数及其意义

## 聚类的定义：
- 聚类就是对大量未知标注的数据集，按数据的内在相似性将数据集划分为多个类别，使类别内的数据相似度较大而类别间的数据相似度较小。
- 无监督

## 相似度/距离计算方法总结

- 欧式距离  
<a href="https://www.codecogs.com/eqnedit.php?latex=d&space;=&space;\sqrt{(x_1&space;-&space;x_2)^2&space;&plus;&space;(y_1&space;-&space;y_2)^2&space;&plus;&space;(z_1&space;-&space;z_2)^2}&space;=&space;||X_1&space;-&space;X_2||" target="_blank"><img src="https://latex.codecogs.com/png.latex?d&space;=&space;\sqrt{(x_1&space;-&space;x_2)^2&space;&plus;&space;(y_1&space;-&space;y_2)^2&space;&plus;&space;(z_1&space;-&space;z_2)^2}&space;=&space;||X_1&space;-&space;X_2||" title="d = \sqrt{(x_1 - x_2)^2 + (y_1 - y_2)^2 + (z_1 - z_2)^2} = ||X_1 - X_2||" /></a>
，即二范式。<br>
<a href="https://www.codecogs.com/eqnedit.php?latex=d&space;=&space;\sqrt[n]{(x_1&space;-&space;x_2)^n&space;&plus;&space;(y_1&space;-&space;y_2)^n&space;&plus;&space;(z_1&space;-&space;z_2)^n}&space;=&space;||X_1&space;-&space;X_2||_n" target="_blank"><img src="https://latex.codecogs.com/png.latex?d&space;=&space;\sqrt[n]{(x_1&space;-&space;x_2)^n&space;&plus;&space;(y_1&space;-&space;y_2)^n&space;&plus;&space;(z_1&space;-&space;z_2)^n}&space;=&space;||X_1&space;-&space;X_2||_n" title="d = \sqrt[n]{(x_1 - x_2)^n + (y_1 - y_2)^n + (z_1 - z_2)^n} = ||X_1 - X_2||_n" /></a>
，即n范式。<br>

- 杰卡德相似度系数
