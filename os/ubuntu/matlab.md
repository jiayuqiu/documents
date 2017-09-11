# [Linux(Ubuntu)下载安装破解Matlab2016](http://www.cnblogs.com/bayesianML/p/6380018.html)

[跳过废话, 直接看教程](http://www.cnblogs.com/bayesianML/p/6380018.html#build)

## 前言

笔者平常不是很喜欢用Matlab, 因为所需要的功能都能被Python替代, 而Matlab的肥和慢实在令人难以忍受. 在Linux系统下安装Matlab也比Windows麻烦一些, 之前失败了4, 5次终于在Ubuntu上装好了Matlab2016, 然而买了固态硬盘重装系统后又没了. 没有Matlab其实也没啥感觉, 因为很少用到, 实在需要的话, 很多时候也能用免费小巧的octave替代.
但是这学期上统计学习基础课老师要求一定要用Matlab, 我一开始还是想用octave来替代的, 后面发现有一些函数在octave的statistics package里, 运行的时候要打`pkg load statistics`来加载包, 但是这样的语句在Matlab上运行是会报错的, 我担心还有别的命令不一样, 导致老师查看不了作业, 还是鼓起勇气重新下载庞大的Matlab了...
虽然之前成功下载安装过一次Matlab2016, 但是再次安装还是处处碰壁, 痛苦的安装过程中我一直后悔自己以前没有记录下载过程, 于是下定决心这次弄好了一定要记录下来, 一是帮助其他有同样需要的人, 二是作为自己的备忘.
经过好多个小时的下载与折腾, 终于获得了Matlab2016大礼包, 使用后发现Matlab的一些新功能非常好用, 对Matlab好感大增, 花了这么多时间还是挺值得的.

## 正文

- 在[此处](https://pan.baidu.com/s/1jHMSMSe)下载Matlab2016和crack文件(密码:w4ib), 文件7G多, 我选择所有工具箱安装完大约消耗了25G, 安装前请确认有足够的空间, 不然很尴尬, 下载可能需要很久, 百度云限制了我们的网速, 买它的会员就会变得很快, 如果喜欢折腾可以找找破解限速的方法(我是用舍友电脑开着慢慢下的)

- 挂载ISO文件, 在ubuntu上直接右键iso文件, 选择第一项挂载即可, 也可以使用命令行

  ```
  sudo mount -o loop R2016a_glnxa64.iso /media/matlab
  ```

- 进入新出现的盘中, 终端输入`sudo ./install`

- 出现图形化安装界面后, 安装过程和windows里差不多, 这里应该不会有什么问题, 如果需要输入秘钥可输入`09806-07443-53955-64350-21751-41297`

- 安装完成后进行破解, 把`crack`中的文件都复制到`/usr/local/MATLAB/R2016a/bin/glnxa6` 中, 如果你的安装目录与我不同, 放进Matlab安装目录的`/R2016a/bin/glnxa6`即可, 覆盖掉原来的文件

- 现在可以进入Matlab的目录打开Matlab了, 终端输入`cd /usr/local/MATLAB/R2016a/bin`

- 再输入`sudo ./matlab` 打开matlab, 记住要在Matlab目录的bin文件夹中用管理员权限打开

- 采用不联网激活，找到`Crack`文件夹中相应的激活文件`Matlab_R2016a_glnxa64.lic`，导入激活

- 一切准备就绪, 在`/usr/local/MATLAB/R2016a/bin`中输入`sudo ./matlab`打开matlab, 很开心地看到了matlab的启动图标! 但是过多一会儿就看到了system error的提示...

- 这是因为Matlab需要连接一些OS库才能运行...解决方法是在终端输入

  ```
  cd /usr/local/MATLAB/R2016a/sys/os/glnxa64/
  sudo ln -sf /usr/lib/x86_64-linux-gnu/libgfortran.so.3.0.0 libgfortran.so.3
  sudo ln -sf /usr/lib/x86_64-linux-gnu/libquadmath.so.0.0.0 libquadmath.so.0
  sudo ln -sf /usr/lib/x86_64-linux-gnu/libstdc++.so.6.0.19 libstdc++.so.6
  ```

- 上面的命令中假设libquadmath0:amd64 和 libgfortran3:amd64已经安装好了, 如果没有的话可以这样安装`sudo apt-get install libquadmath0:amd64 libgfortran3:amd64`

- 现在就真的可以在`/usr/local/MATLAB/R2016a/bin`文件夹中打开matlab了, 但是每次都要cd到这个文件夹再用sudo权限打开matlab真的很麻烦...为此我们建立一个连接`sudo ln -s /usr/local/MATLAB/R2016a/bin/matlab /usr/local/bin/matlab`

- 现在就可以随便在终端输入matlab启动matlab啦!

![此处输入图片的描述](http://7xvci3.com1.z0.glb.clouddn.com/matlab1.png)

刚开始可能会发现字非常小, 我们可以在`Home->Preference->Fonts`中修改(记得把preference窗口放大, 否则会看不到一些设置)
在你写脚本的时候你可能会发现不能用ctrl+c, ctrl+v等进行复制粘贴那些操作了, 这是因为默认的快捷键设置是emacs的, 我们需要改回windows的习惯, 如图在keyboard的shortcuts里更改
![此处输入图片的描述](http://7xvci3.com1.z0.glb.clouddn.com/matlab2.png)

接下来就能愉快地使用最新版Matlab啦!感觉固态硬盘+Linux比以前在windows用快了好多, 这弥补了一些我对matlab 的讨厌, 新版本中的一些功能也让我对它增添了几分好感~
