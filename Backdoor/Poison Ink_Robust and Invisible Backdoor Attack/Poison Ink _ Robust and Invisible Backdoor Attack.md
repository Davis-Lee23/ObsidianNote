>动机、技术路线、实验结果（数据集，实验设置，metric，最后总结出好在哪里）

### 动机
当前有很多种手段可以攻击DNN，其中后门攻击最为灵活并且可以发生在几乎深度学习的所有阶段。然而当下大多后门攻击是可见的或在简单的数据预处理后就失去效果。为解决上述限制，文章提出一种鲁棒且不可见的后门攻击方法“Poison Ink”

#### 解决思路
为了解决当前后门攻击的可见性与脆弱性，设计的触发器应具有以下特征：
1. 易于模型学习且不混淆模型性能(触发器本身应有的属性)
2. 在常规的数据变换操作下，触发器样式应保持不变（鲁棒）
3. trigger图像视觉上应和clean图像难以区分（不可见）

利用图像结构在数据变换期间语义保持不变的特性，将图像结构(边edge)作为目标投毒区域注入信息以实现鲁棒性。
利用深度注入网络(deep injection network)将一种输入感知的触发器嵌入到图像中实现不可见性。

##### 选择图像结构（边）的原因有3点(对应上述3点)
1. 基于结构的触发模式不会破坏原始任务的性能：DNN浅层经常捕获结构信息，但DNN最终决策通常取决于纹理而不是结构。
2. 在常见的数据转换下，结构语义可以保持不变。
3. 边缘edge属于图像的高频成分，隐藏其中的信息难以发现。

总的来说，就是获得图像边缘然后嵌入颜色值(即poison)制造trigger，然后用深度注入网络将trigger注入图像中。

### 技术路线
此处作者提到了他的灵感来源，源自一篇提出了模型IP保护的物理一致性的文章Exploring structure consistency for deep model watermarking(有点难崩，这篇文章也是本文作者写的)。再往前追溯应该源自"[Model Watermarking for Image Processing Networks](https://arxiv.org/pdf/2002.11088.pdf)" (AAAI 2020) and its extension version "[Deep Model Intellectual Property Protection via Deep Model Watermarking](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9373945&tag=1)" (TPAMI 2021)

#### Trigger Pattern Generation
再次复述了选择edge的三个原因。
步骤：
首先，对输入图像x，用边缘提取算法ε如Sobel算子或Canny算子提取边缘结构。
随后，我们通过数学加密和修改边缘的黑白值将中毒信息加密到RGB颜色值中。

#### Deep Invisible Injection Strategy
在训练中分为三部分
1. 深度注入网络(deep injection network, IN)
	1. 衡量不可见性的指标：Lp范数(Lp Norm), 对抗损失(adversarial loss)
2. 辅助引导提取器网络an auxiliary guidance extractor network(GE), 帮助注入网络以适当的方式学习
	+ 这段理解的一般。总之GE可以从中毒图像里提取trigger，但不能从干净图像里提取trigger
	+ 评价标准：
	+ 
	+ 分别为中毒图像触发器提取损失和干净图像损失。
1. 干涉层interference layer, 用于迫使注入网络更鲁棒地嵌入触发图案。
	+ 在训练时干扰层会给数据进行一些变换操作训练，从而让IN和GE能更加鲁棒地隐藏触发器。
	+ 评价标准：IN和GE地损失函数之和
训练结束后，后两个会被抛弃，我们只需使用深度注入网络将trigger注入到图像。

### 实验结果

#### 实验设置
**数据集:** CIFAR-10; ImageNet; GTSRB; VGG-Face
+ ImageNet和CIFAR-10用于比较; CIFAR-10用于消融实验; GTSRB和VGG-Face用于展示方法通用性
**网络结构:** 触发器采用UNet和PatchGAN, 受害者模型采用ResNet-18 , ResNeXt, DenseNet, VGG-19
**评价指标:** 
1. CDA(Clean Data Accuracy):无触发器图像正确预测的比率
2. ASR(Attack Success Rate):评估后门攻击有效率
3. 对于不可见性采用了PSNR，SSIM，LPIPS来衡量
**其他:** 采用Sobel算子提取边缘，poison ink((R:80, G:160, B:80)，投毒10%数据

<font color="#ff0000">看懂那几个Table即可，所有方法在ImageNet和CIFAR-10上进行了大量指标对比</font>
#### 不可见性对比
三项指标在表IV和表V仅弱于LSB，但正如表VI与表VII，LSB在简单的变换后性能下降非常严重。
表IV和表V的Fooling Rate **(人类肉眼判断) : 仅有Poison Ink和LSB超过50%**
+ 从ImageNet数据集和CIFAR-10数据集中随机选取了50张干净图片，并为每种后门攻击方法生成了相应的50张中毒图片。随机展示一对图片（一张干净图片和一张中毒图片），30个人独立作答后选错的概率统计。
![[Pasted image 20240220164529.png]]


	“S&P” AND “C&R” 代表 padding after shrinking and resizing after cropping, respectively.
	ROT 15:一种简单的凯撒密码，代表该位向前移动15位，感觉对LSB的针对性非常强。
	红色代表所有方法中最糟糕的情况

![[Pasted image 20240220164729.png]]
![[Pasted image 20240220164749.png]]
#### 对原始性能影响
**文章方法对原始性能影响最小**
简单的说就是CDA越高越好，看None和Average的对比即可。
LSB在CIFAR-10上性能较差，作者推测LSB的触发器被视为了噪声。
#### 鲁棒性
比较ASR，即数据进行处理后的攻击成功率
**在两个数据集中平均ASR都位居前二，性能接近的方法不可见性又低于本文方法**

两个大雷达图

#### 比较更多的后门攻击方法









## Knowledge
后门攻击：可以发生在任何阶段
input-agnostic:与输入无关的样式p，比如那个kitty猫，明明和图像本身没关系，模型却还得记住他。

不可见性的衡量指标：SSIM，PSNR，LPIPS
[有真实参照的图像质量的客观评估指标:SSIM、PSNR和LPIPS - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/309892873)