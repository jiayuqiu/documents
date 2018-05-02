# Ubuntu 14.04 安装pip

### 方法一：利用apt
```bash
$ sudo apt-get install python-pip python-dev build-essential
$ sudo pip install --upgrade pip
```

### 方法二：下载get-pip.py
```bash
$ wget https://bootstrap.pypa.io/get-pip.py
$ sudo python3 get-pip.py
$ sudo pip3 install setuptools --upgrade
```

# pip用国内源安装模块

### 方法一：修改配置文件
国内镜像：
1. http://pypi.douban.com/  豆瓣
2. http://pypi.hustunique.com/  华中理工大学
3. http://pypi.sdutlinux.org/  山东理工大学
4. http://pypi.mirrors.ustc.edu.cn/  中国科学技术大学
5. http://mirrors.aliyun.com/pypi/simple/ 阿里

mac更改pip源
在home目录下创建.pip文件夹，并在.pip中创建pip.conf文件
```bash
$ cd ~/
$ mkdir .pip
$ cd /.pip
$ touch pip.conf
$ vim pip.conf
```

在pip.conf中添加
```
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
```
保存，重启终端，即可用国内源安装模块

### 方法二：在命令行中，临时使用国内源

```bash
$ sudo pip install numpy -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com
```
