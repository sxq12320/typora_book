# 结构上的无敌优化*VGGNet*

> [!NOTE]
>
> *VGGNet*是*Karen Simonyan*等人在ImageNet大赛上获奖并于2015年在论文*VERY DEEP CONVOLUTIONAL NETWORKS FOR LARGE-SCALE IMAGE RECOGNITION*中提出的网络架构。
>
> 这个网络结构主要是用于大规模图像识别的一种极深的卷积神经网络。是当时最最最深的神经网络结构，同时也是计算量最大的一种网络结构。
>
> 我认为就是该网络引领了后面大家向着更多深度的网络迈进，也是深度方面的鼻祖性神经网络结构。

---



## 1 模型背景

在该模型提出的早期有一个颠覆性的神经网络模型*AlexNet*产出来了，当时基本上所有的网络都是在对*AlexNet*架构进行改进，当然*VGGNet*也是如此。

在VGGNet产生之前，有许多的学者在AlexNet的基础上采用卷积层更小的核心以及更小的步幅获得了非常好的效果；还有一些人是对整个网络的模型结构上深度也获得了不错的改进。

那么这两种方法都可行的话，可以不可以将他们二者合二为一呢，搞一个卷积核很小并且深度特别深的网络结构呢？当然*VGGNet*也就孕育而生了。

*VGGNet*网络结构是非常的简单的，他的整体都采用很小的卷积核对每一层都是特别小的那种卷积核，并且每一层卷积的卷积核完全相同。此外猛拉网络深度，达到了惊人的*16-19*层。

<img src="https://i0.hdslb.com/bfs/openplatform/5d4fd6786fb03185d23c4fa6adc5b0298ccbfc0a.png" alt="image-20260120144348227" style="zoom:80%;" />

> 不要认为*16-19*层非常的浅，实际上这个深度是很深很深的，哪怕是16层的网络结构他的参数也已经达到了惊人的*1330000000一亿三千三百万*！

---



## 2 模型架构

*VGGNet*总共有六种网络结构分别命名为*A，ALRn，B，C，D，E*，网络的深度从左到右逐级递增。最后一种结构的深度为*19*层。

VGGNet整体采用$3\times 3$大小的卷积核进行每一次卷积操作，同时卷积的步长*Stride=1*，卷积过程中的填充*Padding=1*。对于这个数据我们稍微做一下计算。

假设输入的图片大小为$H\times W$，卷积核的大小为$K\times K$，卷积过程中的步长$Stride=S$，卷积过程中的填充$Padding=P$，输出的图片大小为$H'\times W'$。

那么就有$H' = \frac{H+2P-K}{2} +1 ; W' = \frac{W+2P-K}{2}$。
将数据带入就有$H' = \frac{H+2-1}{2} +1=H ; W' = \frac{W+2-1}{2}=W$。发现卷积前后的图片尺寸并无变化。

在*VGG*网络中卷积后的下一步操作是*ReLU*这个就是一种激活函数。

> 在*AlexNet*中已经有过介绍了

最后一步就是最大池化，并且池化的参数也是固定的参数，一般是令池化的核心数量为$2\times 2$，池化的步长为*2*这么一来每次池化后的大小均是原来图像大小的一半。

VGGNet就是这样子一块一块的叠加，每一块都是*Conv+ReLU+MaxPool*的集合。我们将这个集合称为*VGG块*，每次图片经过*VGG块*之后他的图像尺寸就会缩小一倍，也就是长宽各变成原来的一半大小，那么如此有规律的运作就使得我们在进行深度叠加的时候变得更加的简洁。

> 这个块儿的思路我觉得是*VGG*网络最大的创新点

现在就给出原文的具体的六种网络结构。

![image-20260120150634547](https://i0.hdslb.com/bfs/openplatform/fd04d5cb5530881a7293f06ca3bb4ecb3afef966.png)

他的每一列都是一种类型的网络。

---



## 3 模型代码框架

这里以*VGG*-A网络结构为重要的例子进行阐述，下面是VGG-A的整体结构图。应该是比较清晰的

![image-20260120151713687](https://i0.hdslb.com/bfs/openplatform/8670894c39cb10fcdcdb4346d3cf248e666621dd.png)

然后给出*VGG-A*的代码

```python
import torch
import torch.nn as nn

class VGGNetA(nn.Module):
    '''VGGNetA模型定义
    '''
    def __init__(self , num_classes = 10):
        super(VGGNetA , self).__init__()
        # input size = [3 , 224 , 224]
        self.conv1 = nn.Conv2d(3 , 64 , kernel_size = 3 , padding= 1)
        self.relu1 = nn.ReLU()
        self.maxpool1 = nn.MaxPool2d(kernel_size=2 , stride=2)

        # input size = [64 , 112 , 112]
        self.conv2 = nn.Conv2d(64 , 128 , kernel_size = 3 , padding= 1)
        self.relu2 = nn.ReLU()
        self.maxpool2 = nn.MaxPool2d(kernel_size=2 , stride=2)

        # input size = [128 , 56 , 56]
        self.conv3 = nn.Conv2d(128 , 256 , kernel_size = 3 , padding= 1)
        self.relu3 = nn.ReLU()
        self.conv4 = nn.Conv2d(256 , 256 , kernel_size = 3 , padding= 1)
        self.relu4 = nn.ReLU()
        self.maxpool3 = nn.MaxPool2d(kernel_size=2 , stride=2)

        # input size = [256 , 28 , 28]
        self.conv5 = nn.Conv2d(256 , 512 , kernel_size = 3 , padding= 1)
        self.relu5 = nn.ReLU()
        self.conv6 = nn.Conv2d(512 , 512 , kernel_size = 3 , padding= 1)
        self.relu6 = nn.ReLU()
        self.maxpool4 = nn.MaxPool2d(kernel_size=2 , stride=2)  

        # input size = [512 , 14 , 14]
        self.conv7 = nn.Conv2d(512 , 512 , kernel_size = 3 , padding= 1)
        self.relu7 = nn.ReLU()
        self.conv8 = nn.Conv2d(512 , 512 , kernel_size = 3 , padding= 1)
        self.relu8 = nn.ReLU()
        self.maxpool5 = nn.MaxPool2d(kernel_size=2 , stride=2)

        # input size = [512 , 7 , 7]
        self.fc1 = nn.Linear(in_features=25088 ,out_features=4096 , bias=True)
        self.relu9 = nn.ReLU()
        
        self.fc2 = nn.Linear(in_features=4096 , out_features=4096 , bias=True)
        self.relu10 = nn.ReLU()

        self.fc3 = nn.Linear(in_features=4096 , out_features=1000 , bias= True)
    
    def forward(self , x):
        x = self.conv1(x)
        x = self.relu1(x)
        x = self.maxpool1(x)
        x = self.conv2(x)
        x = self.relu2(x)
        x = self.maxpool2(x)
        x = self.conv3(x)
        x = self.relu3(x)
        x = self.conv4(x)
        x = self.relu4(x)
        x = self.maxpool3(x)
        x = self.conv5(x)
        x = self.relu5(x)
        x = self.conv6(x)
        x = self.relu6(x)
        x = self.maxpool4(x)
        x = self.conv7(x)
        x = self.relu7(x)
        x = self.conv8(x)
        x = self.relu8(x)
        x = self.maxpool5(x)
        x = x.view(x.size(0), -1)
        x = self.fc1(x)
        x = self.relu9(x)
        x = self.fc2(x)
        x = self.relu10(x)
        x = self.fc3(x)
        return x
```













