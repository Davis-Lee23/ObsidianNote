### 摘要
动机：被遗忘权。恶意客户端的遗忘具有挑战性
方法：提出了一个由三部分组成的框架，一是减去恶意客户端的更新，二是判断遗忘效率，三是恢复性能
结果：在40%恶意的情况下，MNIST可以增速16倍，并且达到96.98%。一共测了4个数据集


### 实验

#### 实验设置
硬件：3060
数据集：MNIST/MLP，FMNIST/CNN，CIFAR-10/VGG-11，SVHN/MobileNet
比较方法：Retrain，FL，KD，OU(Only Unlearning)，OB(Boosting)，RR，FAST
指标：unlearning rate，F1分数，时间，CPU的使用，内存消耗
客户端：10
恶意比例：0.2，0.4，0.6，0.8

#### 实验结果
挺好的，但是感觉得看一下OB什么玩意
![[Pasted image 20240529103051.png]]


### 方法
第一步：3-5
第二步：6-10
第三步：15-16
和知识蒸馏的差别在于：框架的第二步多了个判断Acc，acc低于前一个就停止。第三步用小数据集跑。

感觉有漏洞

![[Pasted image 20240529104623.png|400]]


### 展望
解决垂直联邦学习的问题

