# Ubuntu 14.04 安装python3.5

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
