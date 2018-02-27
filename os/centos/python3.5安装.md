# CentOS 7下安装Python3.5

•安装python3.5可能使用的依赖

```bash
yum install openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel
```

CentOS 7安装Python3.5

## 到python官网找到下载路径, 用wget下载

```bash
wget https://www.python.org/ftp/python/3.5.1/Python-3.5.1.tgz
```

## 解压tgz包

```bash
tar -zxvf Python-3.5.1.tgz
```

## 把python移到/usr/local下面
```bash
mv Python-3.5.1 /usr/local
```

## 进入python目录
```bash
cd /usr/local/Python-3.5.1/
```
## 配置
```bash
./configure
```


## 编译 make
```bash
make
```

## 编译，安装
```bash
make install
```

## 安装完成
若安装成功，最后则会有```pip install sucessfully!```字样。
