
方法名：PGA

### Abstract
动机：法规赋予了被遗忘权，而由于学习协议和多客户端的存在，当前的MU方法不能直接应用于FL环境中。
方法：本文通过移除整个客户端来实现从全局模型中去除客户端影响。具体而言，就是让目标客户端执行local unlearning，再将遗忘后的模型进行少量轮次训练就可以得到unlearned全局模型。该方法不需要<font color="#ff0000">全局访问训练数据</font>，也<font color="#ff0000">不需要存储历史更新</font>。
结果：在三个数据集上得到了与FR接近的性能，并且非常高效。

### Introduction

仅截取贡献部分：
1. 提出了一个高效的联邦遗忘方法，让目标客户端先reverse学习过程，然后再用这个遗忘模型进行少量训练得到最终遗忘模型
2. 得到了和重训接近的性能并提速了5-24倍。采用了三个指标efficacy（目标数据）、fidelity（剩余数据）、efficiency
3. 最主要的创新是：将局部遗忘定义为一个受约束优化问题。

### Method
场景：cross-soli
核心就是两步，一是目标客户端梯度上升，二是训练新的local unlearned model

##### 梯度上升
过去GA专注于少量数据，没有在整个客户端尝试过。但梯度上升的损失函数是无界的（如交叉熵），因此将客户端级别的遗忘定义为优化问题，并用PGD来解决该问题。
其中参考模型ref model就是剩余客户端均值聚合出来的参数，限制方法就是l2范数球，并设置了一个半径超参数δ。此外设置了一个早停法阈值τ，当遗忘模型和上一轮全局模型距离小于τ时，停止。

τ这里存疑

##### 后训练
就是正常训练，叫做post-training


### Experiment
遗忘场景：①后门攻击 ②反转flipping攻击

Metrics：
在每个指标出现前讲了很多水文。
1. Efficacy of unlearning：用来评估移除目标客户端数据的贡献。用acc和MIA来判断
2. Fidelity of unlearning：剩余数据ACC
3. Efficiency of unleanring：计算和通信，本文专注于communication cost，相比计算，通信开销更重要。

Datasets & Models：
+ MNIST、EMNIST、CIFAR-10 all CNN

