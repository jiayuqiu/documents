# Ubuntu 14.04 安装python3.5

### 方法一：利用PPA安装
```bash
# 添加 Python3.5 PPA
$ sudo add-apt-repository ppa:fkrull/deadsnakes
$ sudo apt-get update

# 安装python3.5
$ sudo apt-get install python3.5
$ sudo apt-get install python3.5-dev
$ sudo apt-get install libncurses5-dev

# 把原来的默认Python3从Python3.4链接到Python3.5
$ sudo mv /usr/bin/python3 /usr/bin/python3-old
$ sudo ln -s /usr/bin/python3.5 /usr/bin/python3
```


### 方法二：源码编译安装（推荐）

```bash
$ sudo apt-get install build-essential # Make sure we have build tools
$ wget http://www.python.org/ftp/python/3.5.3/Python-3.5.3.tgz # download
$ tar -zxvf Python-3.5.3.tgz           # unzip
$ cd Python-3.5.3/                     # open directory
$ ./configure                          # configure for this system
$ make                                 # build
$ sudo make install                    # install to /usr/local/
```
