# DenSeNet

> $DenseNet$是*Gao Huang*等人于2018年在论文*Densely Connected Convolutional Network*中提出的新架构。
>
> 有一个非常反常识的事情需要进行阐述，网络结构虽然称作密集连接的卷积神经网络，但他的参数量和计算量并不高，甚至比前一代的*ResNet*还要低上许多。
>
> 当然这个网络架构最主要的启发当然是著作*ResNet*啦。

![image-20260126091948068](https://i0.hdslb.com/bfs/openplatform/4090ddaeece018c3349d54e93872f1c3652b9eac.png)

---



## 1 模型创作背景

*2017*年*ResNet*横空出世，在当时泛起了轩然大波。超级深的卷积神经网络在进行运算过程中产生的梯度爆炸或者是梯度消失等问题几乎得到了完美的解决。不仅仅是*ResNet*，包括*HighwayNet*等诸多架构都使用了残差连接这种方式在提高网络深度的同时尽可能的降低模型退化的概率。

> 这里模型的退化，主要就是随着网络深度的增加，模型训练精度不降反升的一种奇怪的现象。

但是本文作者认为，网络一味的提升网络深度是没有多大必要的。原因主要有下面两点：

1. 网络深度增加，哪怕你的模型架构再好，训练过程中的计算量和数据量一定会呈指数级别的上升。

2. 网络的深度增加，最前面输入的特征早就已经被冲淡了。那网络的增加还有什么意义呢？

    > 原文中的表述为：当相关的输入或者是梯度的信息传过了很多中间层的时候，这些信息在网络最后基本上早已被冲淡或者是直接消失。

因此在这个背景之下作者主要就是想要解决上面这两个问题，并设计除了一种新的模型架构*DenseNet*架构，这个架构的设计构想主要来源于*ResNet*以及*VGGNet*。

前者主要就是对其短接的残差连接进行思考，后者当然就是块的方式进行类似。

![image-20260126093641172](https://i0.hdslb.com/bfs/openplatform/c4409713ec1c87ec77286b41f03eaeff0eee63b6.png)

---



## 2 模型整体架构

作者提出的*DenseNet(密集连接的卷积神经网络)*，这个网络通过将每一层和其他所有在它之后的层相互连接的方式来消除深度带来的信息消失的问题。

这个方法作者认为是特征复用。

> 原本特征就来之不易，直接给人家消去了多不好，因此留几个备份，给后面几个人都看一下，看看我最开始的到底是个什么样子，毕竟一手消息，和*N*多手消息是不一样的。

这个网络的单个连接层就如下所示。

<img src="https://i0.hdslb.com/bfs/openplatform/ba3b08c55f477bc1a9c8f5addd9ad0f68cd41057.png" alt="image-20260126094306052" style="zoom:50%;" />

它相较于ResNet主要有下面几个改进：

1. 不仅仅只是一次线条进行短接，而是将每个网络的输入于之后所有网络的输入均相互短接在一起，增强特征的重复使用。
2. 之前的短接是直接将两个网络的特征直接相加$y = F(x) + x$，现在做的是*Concat*的操作$y = torch.cat(F(x) , x)$，将特征图叠在一起。
3. 每一次无论输入的通道数是多少都保证他们有相同的输出通道，同时将这个输出通道定义为*K*。这个的作用是什么呢：对于第一个*H*而言输入三个通道，输出32个通道；对于第二个H而言输入的就是$32+3$个通道，输出是32个通道；那么以此类推，对于第N个通道，他的输入就是$3+32\times N$个通道数量。

> 每一*Dense*层的主要结构是一次卷积，一次全局归一化，以及一次非线性激活函数组成的：

![image-20260126095445098](https://i0.hdslb.com/bfs/openplatform/855d4e6c8c047d1dc0a7fbba387af62ab47e1ce6.png)

> 而每一层的中间过渡层是通过一次卷积，一次全局归一化，以及全局平均池化组成的：

![image-20260126095610585](https://i0.hdslb.com/bfs/openplatform/e78bcb17d656d94f20ccad51af2a7f63ec87a3b4.png)

现在将上面的两个模块分别进行命名称作$Dense_{layer}$以及$Dense_{transition}$，分别对应前者与后者。那么最终的一个Dense块儿就是由这两个相互组成的

<img src="https://i0.hdslb.com/bfs/openplatform/09f8b220959d3c58a32c85cbd784ffa0bca56a10.png" alt="image-20260126095956928"  />

这两个模块的组成就对应了中间的函数*H*，一个用来做标准的稠密连接Dense的主要，另一个就是为了将通道数减半降低运算量。

> 前者解决了深度增加冲淡信息的问题，后者解决了深度增大计算资源过大的问题

---



## 3 模型代码实现

那么有了上面的解释就可以对这个进行编写了，主要用的就是基本的*Pytorch*架构来编写

首先编写一下$Dense_{layer}$这个层吧。

```python
def dense_layer(in_channels , out_channels , bn_size = 4 , drop_rate = 0.5):
    return (
        nn.Sequential(
            nn.BatchNorm2d(in_channels),
            nn.ReLU(inplace = True),
            nn.Conv2d(
                in_channels = in_channels , 
                out_channels = bn_size * out_channels, 
                kernel_size = 1 , 
                padding = 0 , 
                stride = 1
                ),
            nn.BatchNorm2d(bn_size * out_channels),
            nn.ReLU(inplace = True),
            nn.Conv2d(
                in_channels = bn_size * out_channels , 
                out_channels = out_channels , 
                kernel_size = 3 , 
                padding = 1 , 
                stride = 1
                ),
            nn.Dropout(p = drop_rate) if drop_rate > 0 else nn.Identity()
        )
    )

# x = torch.randn(4 , 3 , 32 , 32)
# layer = dense_layer(3 , 32)
# print(layer(x).shape)
```

其次编写一下中间用来过渡的$Dense_{transition}$层

```python
def dense_transition(in_channels , out_channels):
    return (
        nn.Sequential(
            nn.BatchNorm2d(in_channels),
            nn.ReLU(inplace = True),
            nn.Conv2d(
                in_channels=in_channels , 
                out_channels=out_channels,
                kernel_size=1,
                padding = 0 ,
                stride=1
            ),
            nn.AvgPool2d(kernel_size=2 , stride=2)
        )
    )

# x = torch.randn(4 , 64 , 32 , 32)
# layer = dense_transition(64 , 32)
# print(layer(x).shape)
```

然后将整一个Dense块儿给他搞一下

```python
class DenseBlock(nn.Module):
    def __init__(self , num_layers , in_channels ,  growth_rate = 32):
        super(DenseBlock , self).__init__()
        self.num_layers = num_layers
        layers = []
        for i in range (self.num_layers):
            layer = dense_layer(
                in_channels= in_channels +i * growth_rate,
                out_channels= growth_rate
            )
            layers.append(layer)
        self.layers = nn.Sequential(*layers)

    def forward(self , x):
        for layer in self.layers:
            y = layer(x)
            x = torch.cat([x , y] , dim =1)
        return x

# x = torch.randn(4 , 2 , 32 , 32)
# layer = DenseBlock(num_layers=1 , in_channels=2 , growth_rate=32)
# layer(x).size()
        
```

那么*DenseNet*的结构就出来了

```python
class DenseNet(nn.Module):
    def __init__(self , growth_rate = 32 , num_classes = 10 , in_channels = 3 , init_features = 64):
        super(DenseNet , self).__init__()

        # 初始卷积层
        self.init_conv = nn.Sequential(
            nn.Conv2d(
                in_channels= in_channels ,
                out_channels = init_features ,
                kernel_size = 7 ,
                stride = 2 ,
                padding = 3
                ),
            nn.BatchNorm2d(init_features),
            nn.ReLU(inplace = True),
            nn.MaxPool2d(kernel_size=3 , stride=2 , padding=1),
            )
        
        # 密集块和过度层
        num_channels = init_features
        self.block1 = DenseBlock(num_layers = 6 , in_channels = num_channels , growth_rate = growth_rate)
        num_channels = num_channels + 6 * growth_rate
        self.trans1 = dense_transition(in_channels = num_channels , out_channels = num_channels // 2)
        num_channels = num_channels // 2

        self.block2 = DenseBlock(num_layers = 12 , in_channels = num_channels , growth_rate = growth_rate)
        num_channels = num_channels + 12 * growth_rate
        self.trans2 = dense_transition(in_channels = num_channels , out_channels = num_channels // 2)
        num_channels = num_channels // 2

        self.block3 = DenseBlock(num_layers = 24 , in_channels = num_channels , growth_rate = growth_rate)
        num_channels = num_channels + 24 * growth_rate
        self.trans3 = dense_transition(in_channels = num_channels , out_channels = num_channels // 2)
        num_channels = num_channels // 2

        self.block4 = DenseBlock(num_layers = 16 , in_channels = num_channels , growth_rate = growth_rate)
        num_channels = num_channels + 16 * growth_rate

        # 全局平均池化和分类层
        self.global_avg_pool = nn.AdaptiveAvgPool2d((1 , 1))
        self.classifier = nn.Linear(num_channels , num_classes)

    def forward(self , x):
        x = self.init_conv(x)
        x = self.block1(x)
        x = self.trans1(x)
        x = self.block2(x)
        x = self.trans2(x)
        x = self.block3(x)
        x = self.trans3(x)
        x = self.block4(x)
        x = self.global_avg_pool(x)
        x = torch.flatten(x, 1)
        x = self.classifier(x)
        return x
```

> *DenseNet*应该说是*ResNet*的一种变体，它虽然是在*ResNet*的基础上进行改进的但是仍然具有很大的数据量，今后的使用较少。