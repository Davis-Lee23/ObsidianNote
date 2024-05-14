### Motivation
现有FU（存储-校正）通常会额外消耗大量存储和计算资源，并且无法提供严格的遗忘保证（校正，正交）。提出一个基于隔离分片和编码计算的可扩展的联邦遗忘框架。

贡献小结：
1. 首次引入基于阶段的隔离分片机制，减少受遗忘影响的客户端数量（所有客户端不应该都被影响？）。并且可以减少时间。
2. 设计了一个基于编码计算的分片机制，以提高系统的可扩展性。效果看文章。
3. 
### Method

### Experiment
#### 实验设置
客户端数量：总100，随机20，被平均分成4个分片
全局轮次30，局部轮次10
遗忘请求：Even，即请求均匀分布在分片。Adapt，集中在一个分片。
数据集：MNIST，CIFAR-10，Tiny Shakespeare
模型：CNN(2卷积2池化2连接)用于分类，NanoGPT用于语言生成。
数据分布：IID，Non-IID（80%的主类）
基线：FedRetrain(FR)，FedEraser(FE)，RapidRetrain(RR)，我们的方法称为SE，Sharding Eraser
指标：acc，时间，存储开销，F1 score

#### 实验结果：
Figure 3如果我没看错，acc几乎全方位还不如FE？
时间显著下降，F1 score就FE显著低了，其他大伙差不多。

### Pros and Cons

### Comments
正确率实在太低了，所有的优点化作泡影。