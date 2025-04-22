#SP #CCF-A #Federated_Unlearning

# Authors
美国杜克华人组，不熟
![[Pasted image 20250312130149.png]]

# Abstract
**动机：** 当前防御方法聚焦于preventing和detecting，较少人关注recover。重训代价高
**方法：** FedRecover，对<font color="#ff0000">客户端</font>而言<font color="#ff0000">低计算、低通讯开销</font>。服务器存储所有的模型和更新，在恢复阶段服务器会去估计模拟客户端的更新，不需要与剩余客户端通信。同时还加入了warm-up, periodic correction, abnormality fixing, and final tuning strategies四种策略恢复模型性能（需与客户端交互）
**结果：** 理论上与重训接近（实际扯淡），四个数据集，三种聚合方法avg+median+trimmed-mean，两种攻击（Trim+Backdoor）

# Introduction
第一段：介绍FL
第二段：然而，由于FL的分布式环境，容易受到中毒攻击
第三段：现有方法针对在prevent和detect，需要恢复方法
第四段：恢复方法仍未有充分探索，trian-from-scratch太消耗资源了。
***
提出了FedRecover，对<font color="#ff0000">客户端</font>而言低计算、低开销的回复算法。直觉是利用历史信息来恢复模型，因此核心思想就是利用历史信息来估计剩余客户端的模型更新，无需客户端计算和通信。该方法可以和检测方法兼容。
Key：The key of FedRecover is that the server estimates the clients’ model updates itself during the recovery process.

具体流程：存储所有——恢复过程用柯西中值定理来估计模型更新计算Hessian矩阵——这个矩阵不好计算，因此用基于L-BFGS的算法有效地近似集成Hessian矩阵

增强：显然这种估计肯定有误差，因此引入了四种策略来让模型更准确。
+ Warm up：前Tw让客户端参与训练
+ Periodic correction：每隔Tc轮让客户端参与训练
+ Abnormality fixing：当更新变动大于某个阈值时，剩余客户端参与训练
+ Final tuning：在最后Tf轮让剩余客户端参与
总结：**开头结尾让剩余参与，根据间隔or阈值再让客户端参与，合着就是看着不行了就让剩余参与**

理论上来说损失函数如果平滑强凸，则差异不大。调参之后可以节省88%计算/通信成本。

贡献：
1. 首次对FL中毒模型恢复进行了系统研究（？）
2. 提出了FedRecover
3. 从理论和实验正面了FR有效。
4. 设计的目标：高效、准确、独立于检测和聚合


# Experiments
## Setup
数据集：MNIST、FMNIST、Purchase、HAR。没有说明的话，都是在最简单MNIST实验
FL设置：轮次、bs都很搞、聚合方法设置也在
攻击设置：每轮随机选20%作为恶意者
恢复设置：牛魔的这么多值
基线方法：重训、原生FedRecover
指标：TER（测试集错误率）、ASR、ACP（平均开销）：用来衡量计算/通讯开销的

<font color="#ff0000">文章的调参</font>：热身20轮，间隔十轮，容忍变化1e6，尾巴5

## Results
不用正常的acc，非要用TER。
Fig1这训练一眼有问题，肯定是用带毒的训练集去跑，MNIST正常ACC才80，FMNIST才55，很差劲的实验结果。

后面懒得看了，肯定一堆差劲的结果。6页的废纸实验
