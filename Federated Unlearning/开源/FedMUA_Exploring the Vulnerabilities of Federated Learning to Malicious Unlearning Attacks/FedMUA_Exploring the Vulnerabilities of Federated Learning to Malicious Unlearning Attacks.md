#开源 #CCF-A #TIFS 


# Comments
感觉质量一般

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


# Method





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