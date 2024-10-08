[[CoBA_Collusive Backdoor Attacks with Optimized Trigger to Federated Learning]]
[[How to Backdoor FL]] 借鉴本文的威胁模型分析
[[DBA_DISTRIBUTED BACKDOOR ATTACKS AGAINST FEDERATED LEARNING]] 思想源自于此
CoBA是本篇的加强版
# Abstract
动机：尽管已有大量对FL后门攻击的研究，但共谋攻击者依然可以规避各种前沿的防御方法。

方法：提出了Cerberus Poisoning/CerP，通过微调后门触发器并控制中毒模型的变化实现了隐蔽又可行的后门攻击。

结果：在3个数据集和13个防御中获得成效。

# Introduction
+ 提到了俩应用，分布式视频监控和信用风险评估。
+ 介绍了检测、加噪、降低相似性权重、投票等方式的防护。

贡献总结：
1. 没看懂
2. 提出了CerP，基于中毒模型参数分布会偏离无毒模型的思路，从三个方面进行了优化：自动优化触发器、显式控制偏差、中毒局部模型多样化。说人话就是**微调触发器、正则化控制loss、减少相似性**。
3. 很多实验


# Cerberus Posioning
算法目标：实现后门，不干扰其他分类。
攻击者共谋，但不知道聚合规则以及良性客户端的训练数据。

### 核心公式
CoBA就多了个全局的F范数限制。
![[Pasted image 20240907114919.png]]
该公式有四个目标：
1. 能同时学习到干净和中毒数据
2. 针对隐蔽后门攻击进行触发器微调。用多层感知机让x-mal+Δx接近于x-nor
3. 正则化本地模型的偏差，控制了participant level的参数变化，尽可能让中毒局部模型接近于良性数据的分布。**2和3都是为了隐蔽性服务的**
4. Pairwise similarity regularization。让中毒模型参数分布更加多样化，因为有的方法会降低过于接近的参数权重。


---

# 实验
### 实验设置
数据集 & 模型，noniid，|S|：每轮最多的恶意数量
![[Pasted image 20240906205021.png]]
基线方法：LIE、Sybil、DBA

指标：ACC与ASR

### 实验结果
首先比较了有无触发器微调的效果
CerP-NT无微调，CerP-ND无α，CerP-NS无β
![[Pasted image 20240906210339.png]]

#### 投毒本地轮次
直觉上来说肯定是投毒本地轮次越高，效果越好，然后事实上如果本地轮次过高，效果反而越差。这可能是因为本地轮次过多，恶意模型偏离良性模型太多，被防御方法检测出来了
![[Pasted image 20240906211936.png]]