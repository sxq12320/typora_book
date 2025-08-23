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

该环境的安装主要是因为需要利用**李沐老师动手学习深度学习课程**中的代码，因此需要配置该环境。

这个环境的配置前提需要有一个`window`s的电脑，同时电脑里面的硬件`GPU`是英伟达的显卡才可以。

安装比较复杂那么开始进行安装

### 4.2 安装说明

进入`annaconda prompt`中。

> 1. 创建一个全新的环境。

```bash
conda create -n d2l python=3.8 -y
```

![屏幕录制 2025-08-23 101741](https://raw.githubusercontent.com/sxq12320/typora_book/main/img/20250823101802007.gif)

> 2. 进入李沐老师的课程主页下载`jubyter记事本`，目的是为了搞一个测试代码罢了，当然如果有测试代码也可以不进行下载。

```bash
https://zh-v2.d2l.ai/
```

![屏幕录制 2025-08-23 102239](https://raw.githubusercontent.com/sxq12320/typora_book/main/img/20250823102304030.gif)

> 3. 下载合适自己的`torch`

首先进入[PyTorch官网](https://pytorch.org/)

![image-20250823102535729](https://i0.hdslb.com/bfs/openplatform/ab1cfc9ab0721efbd854bed0c6f0a9a6021aed8c.png)

单击上面黄色的`get started`

后面根据[CUDA GPU Compute Capability | NVIDIA Developer](https://developer.nvidia.com/cuda-gpus)这个网站的表格匹配自己电脑的显卡和CUDA版本。

电脑进入`CMD`界面

输入代码

```python
nvidia-smi
```

![image-20250823103232306](https://i0.hdslb.com/bfs/openplatform/98c7d6244ad4dbb3b19985724ef789b219d9d035.png)

红色圈圈里面的表示支持的最高版本，一定不可以超过这个版本哈。

确认完毕后单击这个历史版本选择适合自己电脑的版本进行`pip`安装

![image-20250823103014350](https://i0.hdslb.com/bfs/openplatform/975a0d153ccf0ecd5499258197ed5ee92d8daadb.png)

![image-20250823103600161](https://i0.hdslb.com/bfs/openplatform/37bc1750e173dd711db8631aa2780f77a24b2df9.png)

选择好之后便可以进行安装了

> 4. 进入Prompt界面，输入上面复制的代码

```python
conda activate d2l
pip install torch==1.13.1+cu116 torchvision==0.14.1+cu116 torchaudio==0.13.1 --extra-index-url https://download.pytorch.org/whl/cu116
```

便可以自动完成安装了。

当然也可以先下载轮子后安装，这个差距不大。

>5. 然后进入`juypter`进行代码编辑即可。

现在给出`juypter`默认位置修改的方法

进入电脑的下面位置的文件夹

```python
C:\Users\用户名\.jupyter
```

![image-20250823104339990](https://i0.hdslb.com/bfs/openplatform/d42dd37c195c4911c3d2ecae1c337907e350b29d.png)

用记事本打开这个文件，并搜索。

```python
c.NotebookApp.notebook_dir 
```

将下面这个东西修改成你喜欢的位置即可

![image-20250823104447799](https://i0.hdslb.com/bfs/openplatform/87fcca31bd8e351587eeeb33a70ddc1a6543ab31.png)
