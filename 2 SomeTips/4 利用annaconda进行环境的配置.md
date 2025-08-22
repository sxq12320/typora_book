# 4 利用annaconda进行环境搭建

[TOC]

---

## 0 引言

这里所有的环境均是我自己电脑中环境搭建的一些步骤和措施，所有的环境均已经安装在电脑之中，有迹可查。下面的几个环境分别代表了我在进行`python`学习中各种的不同用途，进行分别搭建。

下面将列出在`conda`命令行中常用的一些代码

```python
cls
#清屏

conda env list
#列出所有环境

conda create -n 环境名 python=3.9
# 创建名为“环境名”的虚拟环境，并指定Python的版本

conda create --prefix=安装路径\环境名 python=3.9
# 创建名为“环境名”的虚拟环境，并指定Python的版本与安装路径

conda remove -n 环境名 --all
# 删除名为“环境名”的虚拟环境

conda activate 环境名
# 进入名为“环境名”的虚拟环境

conda deactivate 
# 退出虚拟环境
```

上面的代码基本都是在`base`环境下面的操作现在我们试着在虚拟环境下进行操作

```python
conda activate 环境名
# 进入名为“环境名”的虚拟环境

conda list
# 列出当前环境下的所有库

pip install numpy==1.21.5 -i https://pypi.tuna.tsinghua.edu.cn/simple
# 安装NumPy库，并指定版本1.21.5

pip install Pandas==1.2.4 -i https://pypi.tuna.tsinghua.edu.cn/simple
# 安装Pandas库，并指定版本1.2.4

pip install Matplotlib==3.5.1 -i https://pypi.tuna.tsinghua.edu.cn/simple
# 安装Matplotlib库，并指定版本3.5.1

pip show numpy
# 查看当前环境下某个库的版本（以numpy为例）

conda deactivate
# 退出虚拟环境
```



---

## 1 Base环境

这个环境就不多说了这个是安装`annaconda`后自带的一个环境



---

## 2 `looking `环境

### 2.1 安装意义

这个环境其实蛮好讲的，之前我有一个环境叫做`DL`环境，这个环境是我第一次成功搭建好`pytorch`的环境，谁知道天有不测风云，我的这个环境废了，在学习线性回归的时候，我尝试着用了代码安装`d2l`环境，这个不装还好，装了我这整个环境直接崩盘，什么库都没得用。只好在重新分离进行安装，这次学聪明了，将`cv`和`pytorch`两个分开进行安装，互不干扰。

因此这个库里面仅仅只有了`opencv`的库，用来进行`opencv`的基础性学习

### 2.2 安装说明

进入`annaconda prompt`中。

创建一个全新的环境。

```python
conda create -n looking python=3.9          
```

观察环境是否正确被安装

```python
conda env list
```

进入并激活这个环境

```python
conda activate looking
```

利用清华源，安装`numpy`库

```python
pip install numpy -i https://pypi.tuna.tsinghua.edu.cn/simple
```

利用清华源，安装`pandas`库

```python
pip install pandas -i https://pypi.tuna.tsinghua.edu.cn/simple
```

利用清华源，安装`matplotlib`库

```python
pip install matplotlib -i https://pypi.tuna.tsinghua.edu.cn/simple
```

利用清华源，安装`opencv-python`库

```python
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple opencv-python
```

安装完毕之后观察是否正确安装自己的库

```python
conda list
```

确认无误后退出该虚拟环境

```python
conda deactivate
```

然后退出`prompt`即可。



---

## 3 `manim`环境

### 3.1 安装意义

这个环境的安装纯属于是一瞬间兴起，想试试和大佬之间的差距罢了，这个`manim`库是外国大佬``3blue1brown自己搞的一个库用来制作数学视频的，同时他也将这个库进行开源，可以让大家也一起学习和使用。

这个库说实话用处不大，仅仅只是视频漂亮，但是像我这种编程不熟练的人，用起来真的是黑洞一样的存在，效率太低了，可以说没啥用，那么废话不多说，直接开始安装把。

### 3.2 安装说明

在安装这个库之前需要先安装并下载一个包用来进行图片合成视频的操作 `ffmpeg`

![image-20250822143318430](https://raw.githubusercontent.com/sxq12320/typora_book/main/img/20250822143349660.png)

进入`annaconda prompt`中。

创建一个全新的环境。

```bash
conda create -n manim python=3.11.7 
```

然后直接安装`manim`库即可。

```bash
pip install manim
```

而对于`ffmpeg`这个东西大家可以在其他的地方进行下载即可。



---

## 4 d2l环境

### 4.1 安装意义

### 4.2 安装说明



