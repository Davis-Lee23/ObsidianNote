#CCF-A #开源 

**Federated Unlearning with Gradient Descent and Conflict Mitigation**
开源地址：[GitHub - zibinpan/FedOSD: Code of the AAAI-25 paper](https://github.com/zibinpan/FedOSD)
好像没有其他基线方法，只有FedOSD
# Authors
香港中文大学（深圳）Zibin Pan，一堆挂名机构

# Abstract
背景：FL这些年广受关注，但全局模型可能会隐式地记住客户端本地数据，FU诞生。
动机：现有FU的梯度冲突容易使模型可用性大幅下降。此外后训练可能逆转已遗忘的内容。
方法：FedOSD，Orthogonal Steepest Descent。
	首先，设计了一种遗忘交叉熵解决梯度上升的收敛性问题。
	随后，在不与其他客户端冲突的条件下计算SD（最速下降）
结果：高效遗忘+模型可用，优于SOTA算法。

# Introduction
来自作者介绍。
作者和我的想法比较接近，遗忘类型为sample unlearning和client unlearning，阶段也可以分为遗忘+后训练 or 类训练。
三个挑战：
1. 梯度爆炸
2. 遗忘导致的模型可用性下降
3. 模型”回退“问题

	对于问题一，梯度上升没有上界，前人采用的梯度裁剪引入额外超参数，现实场景无法预知超参数。具体的数值计算看文章，总之设计了一个改良版的交叉熵称为UCE，除以2是为了避免梯度爆炸。

	对于问题二，假如模型更新方向垂直于剩余用户的梯度，那么模型精度的损失就会降到最低。但存在无数条这样的正交梯度，设计了一套方案求解出最优方案。

	对于问题三，作者称是开创性问题，这个还真是，之前跑的大都是类重训方法没注意过。问题详述：在剩余用户上继续训练的时候，模型不受控制地往回朝着unlearn之前的初始模型走，以至于恢复出部分已经unlearn掉的内容，白白做unlearning了。这一块用的是实验验证，并提出了一种投影梯度策略远离模型之前的区域。注意初始模型指的应该是Avg之后的那个模型


# Experiment
## Setup
Metric：R-Acc（剩余准确率，说明没有和其他客户端梯度冲突），ASR衡量遗忘有效性（参照三篇文章）
Baseline：
	类重训：Retraining、Eraser
	出名方法：FedRecovery（无人复现）、MoDe、SFU
	梯度上升：EWCSGA、PGA
超参数：本地epoch=1，bs=200，<font color="#ff0000">学习率四个衰减0.999（选最好的展示）</font> ，round=2000，最多遗忘100轮，通信200轮。
Datasets & Models：MNIST/LeNet、FMNIST/MLP、Cifar10 CNN/100 NFResNet18（有论文）
数据划分：3种Pat，一种IID，默认应该是迪利克雷0.1，Pat-20

cifar10也就跑到60左右。很大概率精致调参了

表1基本记录了所需的数据，但是它的1和2有点奇怪，<font color="#ff0000">梯度上升之后的后训练会恢复ASR？</font> 
表2单独记录了SFU，因为开销太大，只有FMNIST
表4记录不同客户端数量OSD的有效性。<font color="#ff0000">不是哥们，客户端越多怎么ACC更高了</font>
表3，顺序没错，先4再3

还有五个消融实验，不是很想看了

# Comment
首先开源，不赖
三个问题，
	一是自主设计了一个交叉熵且证明收敛，可以挪用
	二是用OSD找了个最速下降
	三这个如果遇到了再说

感觉三个模块都可插拔出来成为自己的东西