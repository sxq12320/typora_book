4 用pytorch实现线性回归

[TOC]

## 1 引入

![image-20250831102634775](https://img2024.cnblogs.com/blog/3693258/202508/3693258-20250831102634741-756739469.png)

和上一次一样采用的是这个比较基本的模型进行讲解，并且采用随机下降模型梯度

在上一讲中已经使用了一定的pytorch的模型

![image-20250831102749753](https://img2024.cnblogs.com/blog/3693258/202508/3693258-20250831102749504-1505694463.png)

1. `tensor`：数据
2. `backward()`自动反馈求解处理。



---

## 2 模型建立

### 1 prepare dataset

给定一定的样本数据

这次咱们应当使用的是具体的pytorch的数据结构进行输入，采用的是$mini\ batch$。

也就是说将$x$和$y$放在一起即可。

例如：$(x_{1},y_{1})，(x_{2},y_{2})，(x_{3},y_{3})$

![image-20250831104056550](https://img2024.cnblogs.com/blog/3693258/202508/3693258-20250831104056386-1488814752.png)

> 注意：x和y必须是矩阵。

### 2 设计模型

用来计算咱们的$\hat{y}$

根据比较基本的模型也就是线性模型而言:$\hat{y}=w\times x + b$

如果输入的数据是上面那个样子的话，那么咱们的$\hat{y}$就可以变成，$\begin{cases}\hat{y}_{1}=w\cdot x_{1}+b\\\hat{y}_{2}=w\cdot x_{2}+b\\\hat{y}_{3}=w\cdot x_{3}+b&\end{cases}$

这个时候就可以采用向量的写法了：$\left.\left[\begin{array}{c}{\hat{y}_{1}}\\{\hat{y}_{2}}\\{\hat{y}_{3}}\end{array}\right.\right]=w\cdot\begin{bmatrix}{x_{1}}\\{x_{2}}\\{x_{3}}\end{bmatrix}+b$

之前咱们是**人工求导数**的，现在咱们最重要的事情是**构造计算图**，求导是$pytorch$的事情。

![image-20250831104235164](https://img2024.cnblogs.com/blog/3693258/202508/3693258-20250831104235113-444469225.png)

这个时需要要确定权重的性转和$b$的大小。

如果想要知道权重的形状那么就需要知道输入的一个维度形状。

注意输出的$loss$必须是一个标量。

> 在`pytorch`中首先先将模型定义成一个**类**。
>
> 每一个类一定要继承咱们的`Module`模块

![image-20250831104722192](https://img2024.cnblogs.com/blog/3693258/202508/3693258-20250831104721983-736398600.png)

> 注意：必须要这两个函数，一个是`__init__()`另一个是`forward()`名字都不可以错。

![image-20250831104854878](https://img2024.cnblogs.com/blog/3693258/202508/3693258-20250831104855002-1874243317.png)

```python
class LinearModel(torch.nn.Module):
    def __init__(self):
        super(LinearModel, self).__init__() # 调用副类，直接这么写就完了。
        self.linear = torch.nn.Linear(1, 1) #nn.linear()是pytorch的一个类，这两个分别是权重和偏置。也在构造一个对象。 Neural network nn.Linear(输入维度，输出维度)。

    def forward(self, x): # forward的函数名是固定的。
        y_pred = self.linear(x) #实现了一个可以调用的对象。
        return y_pred

model = LinearModel() # 实例化
```

>  `nn.Linear(in_features,out_features,bias=True)`
>
> - in_features : size of each input sample
> - out_features : size of each output sample
> - bias : If set to False, the layer will not learn an additive bias. Default: True
>
> ![image-20250831110111098](https://img2024.cnblogs.com/blog/3693258/202508/3693258-20250831110110836-1656082393.png)

### 3 构造损失函数和优化器

使用pytorch的应用接口进行构造。

咱们原本的模型为：$loss = (\hat{y}-y)^2$

那么对于这么一堆数据就有：$loss_{1}=(\hat{y}_{1}-y_{1})^{2} \\loss_{2}=(\hat{y}_{2}-y_{2})^{2} \\loss_{3}=(\hat{y}_{3}-y_{3})^{2}$。

这时候用向量的形式可以表示为：$\begin{bmatrix}\log_2\\\log_2\\\log_3\end{bmatrix}=\begin{pmatrix}\begin{bmatrix}\hat{y}_1\\\hat{y}_2\\\hat{y}_3\end{bmatrix}-\begin{bmatrix}y_1\\\hat{y}_2\\\hat{y}_3\end{bmatrix}\end{pmatrix}^{2}$

由于要求咱么输出的loss是一个标量，因此需要将这个$loss$进行求和操作。

> 用`MSE`的方式求`loss`这个`MSEloss`也是继承`nn`下的`module`，计算图

![image-20250831110911669](https://img2024.cnblogs.com/blog/3693258/202508/3693258-20250831110911654-2096883232.png)

> `torch.nn.MSELoss(size_average = True , reduce=Ture)`
>
> - 是否需要求均值
> - 是否需要计算聚合的标量损失值，所谓聚合就是求和罢了。
>
> **注意这个里面的东西已经被弃用了**现在合成了一个`reduction`
>
> - **`reduction='mean'`**： 等价于 `reduce=True` 且 `size_average=True`。计算批次的**平均**损失。
> - **`reduction='sum'`**： 等价于 `reduce=True` 且 `size_average=False`。计算批次的**总和**损失。
> - **`reduction='none'`**： 等价于 `reduce=False`。**不进行聚合**，返回每个样本的损失。

> 优化器

![image-20250831111518339](https://img2024.cnblogs.com/blog/3693258/202508/3693258-20250831111518502-1548573752.png)

> 这个`parameter`会检查这里面的

#### 4 做好训练的周期

`forward`，` backward`，` update`，

