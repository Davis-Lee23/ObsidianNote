#开源 #CCF-A #TIFS #y2025 

# Comments
感觉质量一般，好像极其依赖引用14，这篇很厉害，是ICML的best paper。
本文假设的背景是恶意客户端要让某些特定的良性客户端被遗忘，比如把某些高可信客户端变成低可信。
本文所说的特征对齐参考下面的博客，带着吉他的猫，吉他和猫同时出现误导主体。比如我本来要忘记猫，但是我恶意生成了一堆带着吉他的猫，可能吉他也会被连带着遗忘。
[【翻译】论文剖析：《Understanding Black Box Predictions via Influence Functions Explained》_understanding black-box predictions via influence -CSDN博客](https://blog.csdn.net/qq_34740599/article/details/107305416)
# Authors
香港理工大学，一二作应该差不多贡献，博士后

# Abstract
背景：RTBF受关注，FU诞生
动机：现有方法关注移除的效率和效果，没有关注遗忘后模型的可靠性
方法：FedMUA，核心思想就是引导模型忘记更多的东西。并且还提出了相应的防御方法。
结果：在3个数据集上实现，并且ASR80+

# Introduction
全文关键字：influence
方法分为两步：ISI+MUG，Influential Sample Identification+Malicious Unlearning Generation
IFs：influence functions

第一步：先用ISI找出影响力大的数据
第二步：让这些数据更贴近于目标数据从而控制遗忘（没看懂）

为了应对攻击，作者可视化了梯度更新得到了关键发现：恶意客户端在一开始的轮次梯度更新明显大于正常客户端。在此基础上，通过关注较大的梯度更新，可以促进恶意学习请求的识别，服务器可以减少恶意梯度以减轻此类攻击。

贡献：
1. 提出了FedMUA
2. ISI+MUG
3. 防御机制
4. 实验

# Related Work


# Threat Model
攻击者：一个或多个客户端
攻击目标：一是让特定客户端也会被错误分类，二是非特定客户端不受影响
攻击者知识：黑盒场景，不知道全局模型架构、先验知识、模型更新。攻击者知道要攻击谁（看似需要知道别人的数据，但以银行为例特征故意设置成种族、性别是很容易的）、可以操控自己的数据。
攻击能力：毫无意义的一段话，就介绍了几个符号。

# Problem Formulation


# Method
步骤一ISI：识别出显著影响性能的训练子集，用别人论文的什么**负IF指数**
步骤二MUG：利用这些有影响力的数据，**将他们的特征与目标样本特征**对其，制造出恶意的遗忘请求。

详解看原文吧


# Defensive Mechanism
灵感源自于观察：恶意客户端在训练的初始轮次梯度更新会异常的高，由此提出了一种新奇的防御方法。
分为两步，首先用Interquartile Range (IQR) method四分位距法辨别出异常值，这个值源自于各个客户端每个梯度的L2范数之和，然后将这些参数乘以一个超参数λ如0.5来减轻他们的影响。

# Experiment
## Setup
指标：ASR、ASR-B、Acc~遗忘模型测试集准确率、Acc干净模型测试集准确率
FU方法与聚合规则：FedEraser、KNOT；FedAvg、Median、Trimmed-mean、Krum

## FedMUA的有效性
为什么MNIST效果最差？ASR-B有什么意义吗

## 遗忘请求比例的影响
这个比例怎么算的？为什么那么低？
<font color="#ff0000">这里突然找补说本文的攻击是一种静态状态？如果是实时场景该攻击可能无效，那岂不是一种随便就可以防御的攻击？</font>


实验太多了盘点不完，奇怪的是他模型为什么训的这么好