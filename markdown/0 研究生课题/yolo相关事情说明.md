# **一、yolo的相关说明**

首先需要从官方的GitHub库内下载yolo的全部代码。

这个就是官方的下载链接：[Github仓库链接地址 ](https://github.com/ultralytics/ultralytics)

<img src="https://raw.githubusercontent.com/sxq12320/typora_book/main//img_20260318143430788.png" alt="image-20260318134534056"  />

下载完毕之后，我们使用自己的编译器将整个代码进行打开，打开的是整个文件夹。现在假设文件夹的名称为***ultralytics-main*。**在这个基础上进行路径的修改和说明

## **1.路径下重要文件说明**

### 1.1.模型文件申明

> 首先就是这整个文件夹中最重要的模型文件架构，官方的代码库中的模型文件，并不是保存成`python`文件，而是使用`yaml`文件进行保存的。

其地址位于：`ultralytics-main\ultralytics\cfg\models`

![image-20260318135230683](https://raw.githubusercontent.com/sxq12320/typora_book/main//img_20260318143432601.png)

上面这张图片就是`yolo11`模型的架构，现在将逐个说明相关的参数：

- `nc`：表示类别的数量。该模型的主要用途是图像目标检测，nc的目的就是确定检测的数量多少
- `scales`：表示模型的缩放系数。里面的模型架构从小到大包括了`nano`、`small`、`medium`、`large`、`x-large`三种规模大小的模型
- `backbone`就是模型的主干网络。他的基本书写方法为`-[from、repeats、modules、orgs]`
    - `from`：来源是什么：若是`-1`表示上一层的输出为当前层的输入、若是其他数据`x`表示第`x`层的输出为当前层的输入。
    - `repeats`；表示该模块重复的次数
    - `module`：表示模块的名称
    - `orgs`：表示该模块初始化的时候需要传入的参数、需要和模块定义的结构体相匹配
- `head`是模型的检测头网络结构

### 1.1.2.模型模块等定义

> 再来就是确定各种模块的定义问题了。

模型的所有模块均保存在该文件夹之中：`ultralytics-main\ultralytics\nn\modules`

其中包括了：检测头文件`head`、激活函数文件`activation`、模块文件`block`、卷积文件`conv`等等。

以后如果需要修改或者增加模块，那么这个文件是绕不开的。

# 2.模块增加或者是修改的方式

> 这里用一个比较简单的注意力机制模块为例子说明。

1. 打开`ultralytics-main\ultralytics\nn\modules\block.py`文件，我们需要在这里面将自己的模块进行定义。写进去即可
    ![image-20260318141028419](https://raw.githubusercontent.com/sxq12320/typora_book/main//img_20260318143436823.png)

2. 在`ultralytics-main\ultralytics\nn\modules\__init__.py`文件中将上面定义的模块进行两次申明

    1. 在`from .block import (`中进行申明
        ![image-20260318141342476](https://raw.githubusercontent.com/sxq12320/typora_book/main//img_20260318143442748.png)

    2. 在`__all__ = (`中进行申明

        ![image-20260318141502880](https://raw.githubusercontent.com/sxq12320/typora_book/main//img_20260318143444525.png)

3. 在`ultralytics-main\ultralytics\nn\tasks.py`中也需要进行两次申明

    1. 在最上面的`from ultralytics.nn.modules import (`中进行第一次申明
        ![image-20260318141703467](https://raw.githubusercontent.com/sxq12320/typora_book/main//img_20260318143448863.png)

    2. 在后面的`  base_modules = frozenset(`中进行第二次申明

        ![image-20260318141745338](https://raw.githubusercontent.com/sxq12320/typora_book/main//img_20260318143452186.png)

4. 最后在`ultralytics-main\ultralytics\cfg\models\11`中重新修改自己的模型架构即可，具体的修改方法，可以参考*人工智能*。
    ![image-20260318141916562](https://raw.githubusercontent.com/sxq12320/typora_book/main//img_20260318143440798.png)

5. 然后就可以运行自己的代码了。

    1. 在根目录之下创建自己的模型`test.py`

    2. 在测试文件之中输入代码
        ```python
        from ultralytics import YOLO
        import torch
        
        if __name__ == '__main__':
            yolo = YOLO(填写你刚刚搞定的yaml文件的路径)
            yolo.train(
                data=填写你的数据集的路径,
                project=填写你最后结果保存的路径,
                epochs=150,
                imgsz=640,
                batch=16,
                lr0 = 0.01,
                momentum = 0.937,
                weight_decay = 0.0005,
                optimizer='SGD',
                device=0  # CPU 则改为 'cpu'
            )
        ```