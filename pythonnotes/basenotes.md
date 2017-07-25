## pip用国内源安装模块

国内镜像：
1. http://pypi.douban.com/  豆瓣
2. http://pypi.hustunique.com/  华中理工大学
3. http://pypi.sdutlinux.org/  山东理工大学
4. http://pypi.mirrors.ustc.edu.cn/  中国科学技术大学
5. http://mirrors.aliyun.com/pypi/simple/ 阿里

mac更改pip源
在home目录下创建.pip文件夹，并在.pip中创建pip.conf文件
```bash
> cd ~/
> mkdir .pip
> cd /.pip
> touch pip.conf
> vim pip.conf
```

在pip.conf中添加
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
保存，重启终端，即可用国内源安装模块
