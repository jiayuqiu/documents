# Python制作egg

## 1. 创建文件夹
```bash
$ mkdir eggdemo # 这个名字可以随便取
$ cd eggdemo
```

## 2. 添加__init__.py
```bash
$ touch eggdemo/__init__.py
```

## 3. 在eggdemo内放入你的功能函数

## 4. 在于eggdemo文件下同一目录下创建并配置
```python
from setuptools import setup, find_packages

setup(
    name = 'eggdemo',  # 与你创建的文件夹
    version = '0.0.1',
    keywords = ('demo', 'egg'),
    description = 'a simple egg',
    license = 'MIT License',

    url = 'http://liluo.org',
    author = 'qiu',
    author_email = 'qiu@qiu.com',

    packages = find_packages(),
    include_package_data = True,
    platforms = 'any',
    install_requires = [],  # 可以添加需要的第三方模块，例如：numpy、pandas
)
```

## 5.应用
### 1) spark
```bash
$ bin/spark-submit --py-files /your/path/to/egg main.py
```

### 2) 用easy_install安装
```bash
$ easy_install youregg.egg
```