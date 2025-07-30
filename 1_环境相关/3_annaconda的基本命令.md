###### 1.base环境下的操作

```
#清屏
cls 

#列出所有环境
conda env list 

# 创建名为“环境名”的虚拟环境，并指定Python的版本
conda create -n 环境名 python=3.9 

# 创建名为“环境名”的虚拟环境，并指定Python的版本与安装路径
conda create --prefix=安装路径\环境名 python=3.9

# 删除名为“环境名”的虚拟环境
conda remove -n 环境名 --all

# 进入名为“环境名”的虚拟环境
conda activate 环境名
```

##### 2.虚拟环境下的操作

```
# 列出当前环境下的所有库
conda list

# 安装NumPy库，并指定版本1.21.5
pip install numpy==1.21.5 -i https://pypi.tuna.tsinghua.edu.cn/simple

# 安装Pandas库，并指定版本1.2.4
pip install Pandas==1.2.4 -i https://pypi.tuna.tsinghua.edu.cn/simple

# 安装Matplotlib库，并指定版本3.5.1
pip install Matplotlib==3.5.1 -i https://pypi.tuna.tsinghua.edu.cn/simple

# 查看当前环境下某个库的版本（以numpy为例）
pip show numpy

# 退出虚拟环境
conda deactivate
```