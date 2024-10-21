# Note
single-shot：一轮训练只选中一个攻击者

# Abstract
动机：联邦的机密性让FL容易受到模型投毒攻击
方法：提出了model replacement，semantic backdoor与constrain loss
结果：肯定行啊

# Introduction
这篇文章讲的很好
[联邦学习安全论文阅读《How To Backdoor Federated Learning》 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/345950518)


介绍了FL（noniid）。
本文最大的发现就是FL容易受到model poisoning。
本文发现FL的任何参与者都可以用其他模型替换全局模型，以便于一保持目标任务的acc，二控制后门任务。然后又说了一些废话，即（1）客户端可以直接影响模型权重（2）恶意者可以以有利于攻击的方式训练。

即便是single-shot也可以实现后门，并且只需要不到1%的恶意者即可。

**作者认为FL容易受到后门以及其他中毒攻击原因如下：**
1. 参与者过多，可能存在恶意
2. FL不能用数据防毒措施，也不能对数据异常检测（隐私保护）
3. 安全聚合策略无法审计溯源后门数据

作者分析了拜占庭容忍分布式学习不适用于FL，因为它假设训练数据iid，平均分布。DP虽然缓解攻击，但是降低acc。

本文的方法是一种通用的constrain-and-scale，并用了train-and-scale避开了检测技术。

# Related Work
总感觉在用一种很新的术语讲很旧的东西

# Method

总结：
1. 分析模型（仅限分析）
2. **利用缩放参数来抵消Avg聚合天然的防御效果，在快收敛时投毒更佳**
3. **改进损失函数与lr，保持中毒持久性和逃避检测**
### Threat model
攻击者自由操控本地数据、超参数、上传的参数。
不能控制聚合、良性客户端。

**攻击目标**：高acc，高asr

本文提出了一种新的后门，semantic backdoor，它不像那种像素后门，而是将某种特征作为后门例如所有紫色🚗或条纹🚗将被识别为🐦。

### Constructing the attack model
作为对比，将传统方法称作naive method，在Avg聚合之后后门将很快被全局模型遗忘，中毒进程缓慢。

Model replacement：通过一堆推理，最后得到了一个缩放因此γ可以抵消Avg聚合消除后门的效果。该方法在任何一轮都可以用，**但是在接近于收敛的时候更有效**。

### Improving persistence and evading anomaly detection
假如恶意模型没有被选择聚合，如何避免灾难性遗忘？
经过作者的实验，用EWC loss没用，但是调低lr有用。
`有段话说得好，FL本就是为了noniid设计，必然存在一些低质量数据`

**Constrain-and-scale**：就是改了个loss
基于直觉①reward：保持主模型的准确率 ②penalize：让偏差趋于正常，无法探测


# Summary
这篇文章重要的应该就是这个后门攻击的思想：攻击某个特征而不是像素后门。（仅限于思想，实际没有实现）
还有加了个loss，以及具体是怎么实现的。

