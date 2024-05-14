这是一篇综述，不过此处要记录的是动机，定义，方法分类

## 动机
当今世界人们在计算机中留下了大量痕迹，一旦数据泄露将暴露用户的隐私，人们期望有“遗忘的权利”即RTBF(the right to be forgotten)
作者将机器遗忘的需求归结为四个主要原因:
1. Security：提升模型安全。检测并完全的遗忘掉各种攻击手段的错误数据，例如对抗攻击的数据。
2. Privacy：保护隐私。各种云服务进行备份、复制策略，易造成隐私泄露.
3. Usability：提高可用性。如遗忘推荐系统中的错误数据，减少推送用户不期望看到的信息。
4. Fidelity：增加模型可信度，减少模型偏见。由于数据集可能集中于某一人种和性别，会造成模型对其他肤色人种的误判和偏见。

## 定义
允许用户从机器学习模型中完全删除其数据的一种新的范式，即机器遗忘。
换句话说就是机器学习模型从其训练数据中移除特定的信息，在去除这些信息干扰后继续进行模型预测。
Note:最理想的情况就是可以直接从模型中去除数据而无需重新训练。


## 方法分类
主要分为三类
1. Model-Agnostic Approachs 模型无关方法(模型不可知方法)
	1. Differential Privacy
	2. Certified Removal Mechanisms.
	3. Statistical Query Learning.
	4. Decremental Learning.
	5. Knowledge Adaptation.
	6. MCMC Unlearning (Parameter Sampling).
2. Model-Intrinsic Approachs 模型内在方法(模型可解释方法)
	1. Unlearning for softmax classifiers (logit-based classifiers).
	2. Unlearning for linear models.
	3. Unlearning for Tree-based Models.
	4. Unlearning for Bayesian Models.
	5. Unlearning for DNN-based Models.
3. Data-Driven 数据驱动方法
	1. Data Partitioning (Efficient Retraining).
	2. DataAugmentation (Error-manipulation noise).
	3. Data influence.

#### Model-Agnostic Approachs 模型无关方法(模型不可知方法)

模型不可知方法旨在得到一些不依赖于任何特定的模型架构，理论上都适用的方法

#### Model-Intrinsic Approaches
The model-intrinsic approaches include unlearning methods designed for a specific type of models.
为特定模型设计的方法，但应用面并不狭窄，因为很多机器学习方案都采用同样的模型。


#### 本文对FL与MU的介绍 section 7.2
MU与FL结合的困难：
+ FL的全局权重由聚合产生而非直接原始梯度，使得MU无法容易地应用于FL环境，客户端可能存在重叠的数据，难以量化模型权重，直接运用经典的遗忘方法可能导致准确率下降或新的隐私威胁。
+ <font color="#ff0000">当前大多FU都是直接去除整个客户端</font>。基于此假设可以很容易地记录和删除特定客户端历史贡献，但删除参数更新可能会损坏全局模型，文中提到引用多个方法解决该问题



#### 未来方向 section 8.1总结

联邦学习为机器遗忘带来了独特的环境。我们可以精确的去除某一客户端。由于FL的训练源自各个客户端局部训练，可以避免catastrophic unlearning现象。但是我们也要考虑到FL中数据是non-IID的以及我们只删除客户端部分数据的情况