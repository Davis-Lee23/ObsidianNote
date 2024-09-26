# Note
MLaaS

# Abstract
动机：在MLaaS中遗忘成为刚需，但目前①没有研究可以同时实现高效的遗忘验证与再训练，②同时对于如何防止provider提供虚假遗忘证明也鲜有人探索。

方法：提出了一种基于后门的验证方案，将后门与增量学习结合之后不会损害性能和服务质量。该不可见后门可以用来验证provider是否欺骗，增量学习可以加快重训练。

结果：肯定行

# Introduction
### 动机
场景：MLaaS收取数据帮忙训练，可能出现恶意SP(service provider)与信息泄露，同时还有RTBF的规章。
期望效果：删除特定数据，并快速重训练
提到了两类遗忘方法：
1. approximate unlearning：该类方法依赖于深度学习中的优化方法，要牺牲acc而且无法保证删干净了
2. exact unlearning：这是MLaaS中所需的准确遗忘指定数据，但很遗憾现有方法没有考虑黑客和恶意SP。

### 方法
因此本文提出了一种高效且可验证的机器遗忘方案：we resort to the design philosophy of backdoor attacks and ingenious synergy with the practical incremental learning approach.
采用后门攻击设计理念，并与实用的增量学习方法巧妙结合。
基本思路如下：在外包前注入不可见标记到敏感数据中，这些不可见标记不会影响训练质量。此外将验证方案融入增量学习中以提升遗忘性能，提出了一种数据模型索引结构可以让SP从中间模型开始训练而不是从头开始。

### 结果
1. 提出了一种后门辅助验证方案，用户可以有效的用来验证是否遗忘与SP是否伪证
2. 将增量学习与机器遗忘验证想集成，加快了再训练
3. 在MNIST、CIFAR10、GTSRB上结果好

# Background
增量学习：不断学习新知识，并保留大部分先前的知识。<font color="#ff0000">本文借鉴于LWF</font>，每个客户端先**按照类别**切片上传给SP，模型按照类的顺序学下来，拆解成一步一步，而不是一起训练。
![[Pasted image 20240925155300.png]]

后门攻击：只对带有触发器的样本有影响，评估指标有BASR和CSA clean sample accuracy
优化目标就是将正常和后门数据集loss最小。
本文借鉴于<font color="#ff0000">LSB</font>，一种移动像素的方法
![[Pasted image 20240925160918.png]]

# System Model
整体流程：
1. 用户根据敏感等级给数据投毒，扔给SP训练
2. 用户发起遗忘请求，SP遗忘并重训练
3. 客户端验证
![[Pasted image 20240925161854.png]]

# The Proposed Design
挑战一：验证伪证与保持再训练性能
挑战二：提升再训练效率
基本上就是LSB+LWF，进行了微小改动。

验证：用户发数据，SP返回结果，用户对比一下带后门的结果