#未开源 

# Authors
Zhou Lei, Youwen Zhu ； 学生|导师
一点信息没给，终于在dblp上追根溯源找到了，是南航团队的。

# Abstract
背景：随着法规确立，FU成为新范式
动机：当前FU都在关注遗忘效率，忽略了FU存在的隐私风险
方法：
	1. 提出了FUIA（inversion attack），在系统性研究了FU的隐私漏洞后，在各种场景下都可以有效攻击各种FU方案。
	2. 具体请来说服务器作为诚实但好奇的攻击者分析遗忘前后差异来重建特征or标签。
	3. 还探讨了两种防御
结果：在多个数据集和FU方法上表明可以重构目标数据


# Introduction
+ 第一段：说明RTBF的重要性，引入FL和FU
+ 第二段：说明当前研究关注效率，就算有攻击研究FedMUA也是阻碍性能的。目前没有人关注FU本身的隐私安全风险，本文首次提出相关攻击FUIA
+ 第三段：遗忘过程服务器可以获得所有参数梯度，利用GIA梯度反转进行重构，危害了FU
+ 第四段：简述FUIA，攻击者是诚实但好奇的客户端。利用参数更新差异来窃取信息
+ 第五段：介绍了一下FU的三种遗忘类型，将sample和client放一起，class单独讲
+ 第六段：引入了两种防御，一是梯度裁剪去掉一些不重要的参数更新，二是差分隐私，加噪扰动。不幸的是这些方法都会降低模型性能
<font color="#ff0000">贡献总结：</font>
1. 首个研究FU内在隐私风险的文章并提出了FUIA
2. FUIA可以对多类型FU生效
3. 提出俩防御

# Related Work
FU部分有年份近的可以蛮看一下
GIA：分成迭代式和递归式，具体得看中文
单独拉了一个section C来讲灵感起源，疑似源自一篇叫做MUIA的文章。然后讲了一些为什么MUIA在FL中不适用的原因（扯淡找补罢了），最后反复提及本文是第一个研究FU隐私问题的文章。

# Preliminaries
简单介绍了GIA

# PROBLEM STATEMENT
## 威胁模型
研究针对分类任务，攻击者为诚实但好奇的服务器，会记录各个客户端参数更新

## Problem Formulation
可以轻松获得全局模型和遗忘模型的梯度信息、差值。
+ 挑战一：如何利用从该模型差异中获得的梯度信息来反演被遗忘的数据，如果该客户端数据比例小，目标梯度会很模糊，并且联邦的聚合操作也提高了难度。
+ 由此引出挑战二：如何设计梯度分离算法以从噪声模型差异准确地重建目标梯度信息。此外大多数FU用的是FedAvg，很多GIA攻击仅在FedSGD生效


# Method
无论哪一种遗忘，都是分为三步：梯度分离，目标梯度获取，梯度反演




# Experiments
## Setup
硬件：3090+因特尔至强处理器
Dataset：Cifar10/ResNet-18 & 100/ResNet-44
数据划分：80%作为预训练数据跑好模型，20%作为各客户端私有数据
预训练：sample和client有，class没有预训练，因为这与假设矛盾
超参数：
	FL设置：10客户端，每轮选50%，本地轮次=3，FedAvg
	FU设置：为每一个场景找了两种代表性方法，结果Retrain算一种....
		1. 类遗忘：FUCDP
		2. 客户端：FedEraser
		3. 样本：UnrollingSGD

指标：
	1. 对于样本和客户端遗忘：MSE (Mean Squared Error) and PSNR (Peak Signal-to-Noise Ratio)衡量重构图像
	2. 对于类遗忘：直接测Acc

<font color="#ff0000">比较方法和实现：用的是一篇MUIA的实现方法，将其挪用到FL环境之中</font>

## Result Analysis
分析了三种情况的结果，好像是那么一回事，回看前边的吧。

# POSSIBLE DEFENSES
## Gradient Pruning

## Gradient Perturbation