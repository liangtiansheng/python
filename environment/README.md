# 开发环境——Pyenv

## Python多版本管理工具

+ 管理Python解释器
+ 管理Python版本
+ 管理Python的虚拟环境

官网 <https://github.com/pyenv/pyenv>

## Pyenv安装

+ 参照安装文档
+ pyenv的安装
+ python多版本安装
+ pyenv之global、shell、local的设置
+ pyenv的虚拟环境
+ 更新：$ pyenv update

## pip包管理器

pip配置

```bash
vim ~/.pip/pip.conf
[global]
index-url=http://mirrors.aliyun.com/pypi/simple
trusted-host=mirrors.aliyun.com
```

## pip 基本用法

```bash
安装软件包xxx和yyy
pip install xxx yyy
查找软件包
pip search keyword
获取命令指南
pip help install
从一个环境中提取出requirement
env1/bin/pip freeze > requirements.txt
在另一个环境中利用requirement安装同一环境
env2/bin/pip install -r requirements.txt
```

参考pip官网 <https://pypi.org/project/pip/>

## python开发友好工具

### IPython

增强的Python Shell，自动补全、自动缩进、支持shell，增加了很多函数

### Jupyter

+ jupyter notebook password
+ jupyter notebook --ip=192.168.1.30 --port=8888
+ 从IPython中独立出来
+ 独立的交互式笔记本，后台使用Ipython
+ 快捷键：shift + Enter、Ctrl + Enter、dd、m

## IDE工具

### Pycharm

<https://www.jetbrains.com/pycharm/>
