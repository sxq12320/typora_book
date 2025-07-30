------

- •

  配置使用环境，基于python的 tags:

- •

  **pytorch**

- •

  **vscode**

- •

  **环境配置**

------

# 谨记pytorch必须在电脑的GPU为英伟达的厂商的才可以即只有N卡才支持GPU的深度学习！！！

## 1.下载anaconda

### 1.1卸载anaconda

在安装anaconda前需要将曾经下载过的不达标的anaconda进行卸载，具体的卸载方法便不多赘述了，具体可以参考下面的3.Anaconda卸载文档。 这里就直接一笔带过了。

具体的卸载方法看笔记：

### 1.2正式下载并安装Anaconda

###### 下载Anaconda

采用中国大学的镜像源进行下载即可，这里我们选用的是2022.10的Win版本其基础环境(base环境)下的Python为3.9版本。具体地址如下。

- •

  https://mirrors.bfsu.edu.cn/anaconda/archive/ 而选用的版本如下所示 ![在这里插入图片描述](assets/50b79c8c30b84e1ca5850cd9c3fd37bd.png)

#### 安装Anaconda

直接双击刚才下载后的exe文件

- •

  选择Just me 如下图所示 ![在这里插入图片描述](assets/6878b11b11a04d1fb355044bedc50eae.png)

- •

  自行选择安装路径，尽可能不要选择C盘，我这里选择的是E盘 ![在这里插入图片描述](assets/a607d39606594995b874805b72859ac2.png)

- •

  选择第二个方框即可 ![在这里插入图片描述](assets/960af775d7694c349f86859d2e6bdeed.png) 选择完毕后便等待安装成功即可

#### 配置Anadaconda的环境变量！！！！！！！！！！！！！！！！

回到电脑桌面，右键选择“显示设置”，而后在左上角的“查找设置”中输入“环境变量”，点击“编辑系统环境变量”，此后在弹出的“系统属性”窗口中点击“环境变量”，再在弹出的“环境变量”窗口中选择Path路径，并且点击“编辑”

- •

  ![在这里插入图片描述](assets/d7021fbde33447919bd317ae86b4d33c.png)

- •

  ![在这里插入图片描述](assets/c3b3ec22ddc3476dab9423ddd2535a32.png)

- •

  ![在这里插入图片描述](assets/f5ebd1719fb74b2883a3e3e024f2dc36.png)

- •

  ![在这里插入图片描述](assets/9847b19fb0c348a0ab287faa7ed0b13e.png) 而后进入编辑页面后，单机其右侧的新建按钮，可以开始新建环境变量的路径，需要添加下面三个路径。

- •

  E:\anaconda202210

- •

  E:\anaconda202210\Scripts

- •

  E:\anaconda202210\Library\bin ![在这里插入图片描述](assets/e9eeb0555170461cb2bb13228f17213a.png) 编辑完毕后需要重新进入自行检查一下看看这三个路径是否还是存在的！！！！！！！！！！ 至此便将Anaconda安装完毕了。

## 2.在anaconda中进行进行配置

### 2.1创建虚拟环境

双击Promt进入Anaconda的环境中，下面的[[2.命令Anaconda]]十分重要 逐一配置好个个库即可

现在重新安装CUDA

- •

  https://developer.nvidia.com/cuda-toolkit-archive 进入这里面下载CUDA选择的是CUDA11.3的版本如下图所示 

- •

   而后选择好平台以及下载的位置

- •

  ![在这里插入图片描述](assets/c31827085bcc4900a2a53c34281e3faa.png)

- •

  然后就是缓慢的下载安装的过程

双击下载后的exe文件进入安装界面，首先点击同意并继续，选择“自定义”，接下来仅仅选择四大项中的CUDA选项，并且取消CUDA中关于VS的选项，然后需要按照默认的**C盘**位置进行安装，尽量不要选择其他的安装位置。

- •

  ![在这里插入图片描述](assets/0d87047e89114c4b98574ce2e63a4d10.png)

- •

  ![在这里插入图片描述](assets/11627702df5c4ff9b7553c38ac7fbd2b.png)

- •

  ![在这里插入图片描述](assets/72b641b141484070bff96332fb65f83f.png)

- •

  ![在这里插入图片描述](assets/7745c102809b4cc8ab43cf341fd4ba63.png)

- •

  ![在这里插入图片描述](assets/e179648c68bb4b7a9fd365961426089e.png) 安装完毕后需要配置环境变量，如果你是采用默认的路径进行配置那么其路径应当为：

- •

  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA

- •

  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.3\lib\x64

- •

  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.3\bin

- •

  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.3\libnvvp 其配置方法与[[1.配置pytorch#配置Anadaconda的环境变量！！！！！！！！！！！！！！！！]]完全一致请自行查看即可。

而后便需要下载torch了这也是最重要的一步，而torch的版本需要有一定的要求，如下所示 ![[Pasted image 20250527135101.png]] 然后便进入promt内进行安装即可，谨记需要安装在**虚拟环境**之中。

- •

  https://pytorch.org/get-started/previous-versions/ 进入官网后Ctrl + F搜索“pip install torch==1.12.0” 而后用pip进行安装即可 ![[Pasted image 20250527135408.png]] 至此便配置完毕，可以自由使用了。