#arXiv #未开源 #略读 #WWW_Companion

[[FedMUA_Exploring the Vulnerabilities of Federated Learning to Malicious Unlearning Attacks]]
[[Model Inversion Attack against Federated Unlearning]]


# Authors
六校作者：
![[Pasted image 20250622143615.png]]
# Abstract
背景：FU出现了
动机：当前关注效率和有效性，没人管安全
方法：攻击方法BadUnlearn，制造一种针对遗忘期间特殊的更新，旨在让全局模型中毒。还提出了防御方法UnlearnGuard，评估并过滤更新。
结果：理论和实验证明

# Introduction
BadUnlearn旨在让遗忘模型相似于全局模型，作者视其为优化问题，恶意更新还很吃聚合规则。
UnlearnGuard需要服务器存储历史更新，包括客户端的本地模型更新和全局模型。<font color="#ff0000">海量存储啊我靠</font> 。但是作者仍然不满足，制造了一个UnlearnGuard-Dist方法确保新的更新不能偏离旧的更新（和我的思路一样）

这篇主杀FedRecover。

# Threat Model
三种场景，攻击者都是Client
1. full：知道所有客户端更新和聚合规则
2. partial：知道恶意客户端更新和聚合规则
3. black-box：只有恶意更新

防御者目标：
1. 鲁棒性，FU过程应对中毒的能力
2. 开发出一个独立的聚合规则，服务器不需要知道攻击类型和数量

# Method
**攻击方法：**
写的不太清楚，好像是什么自适应的玩意

**防御方法：**
总的来说，就是会先用预测出一个更新方向，疑似与海森矩阵有关。然后用L2范式确保距离偏不太远，再用余弦相似性确保方向正确。