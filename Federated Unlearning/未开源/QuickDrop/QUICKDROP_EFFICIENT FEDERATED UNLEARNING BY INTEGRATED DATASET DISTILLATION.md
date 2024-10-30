
作者：# [Akash Dhasade](https://akash-07.github.io/) EPFL, Switzerland，瑞士洛桑联邦理工学院

这篇大幅参考了Bo Zhao, Konda Reddy Mopuri, and Hakan Bilen. Dataset condensation with gradient matching. In International Conference on Learning Representations, 2021. URL https://openreview.net/forum?id=mSAKhLYLSsl.
# Abstract
动机：没有动机，非要说有就是现有方法计算开销大
方法：QUICKDROP，每个客户端产生一个Distilled Dataset（又叫它compact dataset），目标客户端执行SGA梯度上升，再将DD集成到后训练
结果：在三个数据集不错，比重训练快463倍，比现有方法快65倍。



# Evaluation
### Experimental Setup
数据集：mnist、cifar-10、svhn
模型：全ConvNet
分布：Dir
客户端数量：10，后面用SVHN做了个100客户端彰显其可扩展性
随机选择：全参与
轮次：200

数据集蒸馏：评估针对于class-level，作者称附录上有client-level

Baseline：
1. Retrain
2. SGA，梯度上升
3. 类裁剪


### Performance Evaluation
Note：实验都限于一个类
Metrics：Acc、Computation Efficiency、MIA（用来检验遗忘效果）
