---
title: Keras安装
date: 2017-08-04 09:42:22
tags:
- Keras
- Anaconda
categories:
- 深度学习
toc: true
---
# Keras安装与配置

## Keras安装
安装参考[Keras中文文档](http://keras-cn.readthedocs.io/en/latest/for_beginners/keras_windows/)

## 配置过程中的问题
1. 执行`import keras`时报错`attributeerror module "pandas" has no attribute "computation"`
**解决**：可能是在pandas-0.20.2里没有这个属性，所以需要把pandas的版本换为老版本的（如pandas-0.19.2为例）
```bash
conda remove pandas
conda install pandas=0.19.2
```

2. 安装`tensorflow`时出现`“Cannot remove entries from nonexistent file c:\program files\anaconda3\lib\site-packages\easy-install.pth”`
原因：因为setuptools版本太低，tensorflow要求29.0.1，当前版本为27.2.0,在更新setuptools版本的时候又找不到easy-install.pth，导致更新失败
解决：`pip install --upgrade --ignore-installed setuptools`


# Anaconda常用命令

### python环境管理
`conda create --name python2 python=2.7` ：创建新环境并指明版本。

`conda remove --name python2 --all`：删除一个已有环境

`avtivate python2`：激活环境

`deactivate python3`：返回默认环境

`conda info --envs`：列出所有环境

`conda info --envis`：显示当前环境

### 包管理
`conda install numpy=1.9.3`：安装指定版本包

`conda remove`

`conda update`
