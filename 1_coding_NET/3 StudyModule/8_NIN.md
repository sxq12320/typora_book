# *Network In Network*

> [!NOTE]
>
> $NIN$神经网络架构真可以说是陨落的*天才*呀，该网络架构的理想非常美好，但是现实特别的骨感。
>
> $NIN$神经网络架构是*Min Lin*等人于2014年在论文*Network In Network*中提出的。论文的名的中文翻译非常有意思叫做*“网络中的网络”*这个的原因会在后面解答。
>
> 现在就来看看这个当年特别厉害，但现在却泯然众人矣的架构吧！

![image-20260122182814783](https://img2024.cnblogs.com/blog/3693258/202601/3693258-20260122182817637-325912992.png)

---



## 1 模型设计背景

在*2013*年随着*AlexNet*、*VGGNet*等厉害的网络架构逐年发表，人们在卷积神经网络上面的推荐进入了一种热潮，人们慢慢的从原本的全连接神经网络向着卷积神经网络进行创新和推进，但是卷积神经网络仍然存在诸多的缺陷有待弥补。

传统的卷积神经网络是通过交替使用卷积层和池化层来实现特征的有效提取，每个卷积核都会和它所在的图片做线性相乘运算然后相加。就和下面的图片展示的一样。

<img src="https://img2024.cnblogs.com/blog/3693258/202601/3693258-20260122184603085-2015496811.png" alt="image-20260122184459017" style="zoom: 50%;" />

线性层的特征图计算公式：$f_{i,j,k}=max(w_k^Tx_{i,j},0)$

这里需要注意，$i，j$是输出图片的像素索引，那么对于输入图片应该就是卷积核大小的一块像素点，也可以说是感受野，若有$3×3$的卷积核，那么$x_{i,j}$的数据应该是一个$9$维列向量。

但是有些学者认为，这种线性卷积的使用是建立在所需要分割的对象是线性可分的，并且所有的对象经过线性变换后都可以位于*GLM*分离平面的一侧，那这样子GLM才可以实现良好的抽象程度。但是这个的前提是对象为线性可分，那么显然不对呀，因此这个方法是不可取的需要进行修改。

> 上面的这段话还是比较晦涩难懂的。这里稍微进行解释一下：
>
> 1. *GLM*就是*generalized linear model*广义线性模型，说的特别的帅气，实际上就仅仅对应了传统的额卷积操作-线性卷积+非线性激活函数构成的一个模块。
> 2. 为什么说这个对象是进行线性变换：因为卷积操作就是相乘后相加，这个操作和之前学习的$y=kx+b$的运算逻辑完全相同，因此是线性变换。
> 3. 抽象程度究竟是什么程度：抽象这个词在这里是动词，就相当于物理中把一个物理模型从实际场景中抽象出来的感觉，这里主要就是将一张图片中的重要特征抽象出来。

并且当时基本上所有的卷积神经网络都有一个特点，就是一层层卷积和池化交替之后一定在最后使用一个全连接的线性层来提取特征，实现分类，但是这个线性层超级容易出现过拟合的现象，因此需要进行网络的改进。

---



## 2 网络整体架构

基于上面对卷积神经网络的认知之后，于是乎就产生了一种思路，如果卷积不好，也就是卷积的线性操作不行，那是否可以使用非线性的东西来替代这种线性的卷积层呢》

因此人们就想到使用MLP多层感知机来替代卷积层。

![image-20260123204103420](https://img2024.cnblogs.com/blog/3693258/202601/3693258-20260123204104872-555596036.png)

> 这里就可以看出为什么作者将自己设计的这个架构称为网络中的网络了，一个卷积神经网络中包含了多个全连接神经网络架构，这正是网络中的网络最强的说明。

同时在最后一层也不再使用传统的全连接层作为最后的输出，而是使用一个全局平均池化充当为最后的输出。这东西可以自动正则化并且不容易出现过度拟合。

那么我们总结一下他的创新和改进：

1. 将中间的部分卷积层替换成标准的全连接*MLP*多层感知机神经网络，尽可能的防止过度拟合出现；
2. 使用全局平均池化将最后的全连接层进行替换，减少对*DropOut*以及正则化的依赖性。

---



## 3 整体代码实现

整体的可以进行训练的网络架构如下所示。

![image-20260123210747262](https://img2024.cnblogs.com/blog/3693258/202601/3693258-20260123210750384-679332047.png)

> 实际上，我认为，该网络架构只不过是很僵硬的再多个卷积层中间加上了多层感知机罢了。
>
> 这里面的多层感知机没有理解错的话也可以说是一组*1×1*的卷积运算

好了那么现在就给出具体的*pytorch*代码实现

```python
import torch
import torch.nn as nn

class NIN(nn.Module):
    def __init__(self):
        super(NIN , self).__init__()

        self.NIN_net = nn.Sequential(
            self.NINBlock(3 , 96 , 11 , 4 , 0),
            nn.MaxPool2d(kernel_size=3 , stride=2),
            self.NINBlock(96 , 256 , 5 , 1 , 2),
            nn.MaxPool2d(kernel_size=3 , stride=2),
            self.NINBlock(256 , 384 , 3 , 1 , 1),
            nn.MaxPool2d(kernel_size=3 , stride=2),
            nn.Dropout(0.5),
            self.NINBlock(384 , 10 , 3 , 1 , 1),
            nn.AdaptiveAvgPool2d((1,1)),
            nn.Flatten()
        )
        
    def NINBlock(self, in_channels , out_channels , kernel_size , stride , padding):
        return nn.Sequential(
            nn.Conv2d(in_channels=in_channels , out_channels=out_channels , kernel_size=kernel_size , stride=stride , padding=padding),
            nn.ReLU(inplace=True),
            nn.Conv2d(in_channels=out_channels , out_channels=out_channels , kernel_size=1),
            nn.ReLU(inplace=True),
            nn.Conv2d(in_channels=out_channels , out_channels=out_channels , kernel_size=1),
            nn.ReLU(inplace=True)
        )

    def forward(self , x):
        return self.NIN_net(x)
```

那为什么叫做陨落的天才呢？因为这个思想特别的好，但是后面的人们仍然没有使用这个思想，一点都没有，卷积仍然是卷积，最后的全连接层也是全连接层。