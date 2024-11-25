# 作者
印度作者，做了好几篇关于FU的，至少有AAAI会议托底证明水平，不过AAAI也是水刊啊😓
[vikram2000b (Vikram Singh Chundawat) (github.com)](https://github.com/vikram2000b)

# Abstract（未必准确）
动机：移除联邦参与者的信息仍是一项挑战，当前大多数FU方法需要剩余客户端数据来重训模型或需要消耗服务器or客户端大量计算资源

方法：提出Contribution Dampening（CONDA），通过跟踪每个客户端影响全局模型的参数，对其实施抑制来实现有效的FU。

结果：在noniid环境下三个数据集实现了100倍的速度。


# Experiment
### Setup
数据集：MNIST+CNN、CIFAR-10/100 + ResNet18，noniid+10个客户端
基线方法：FedEraser、PGA、Retrain、梯度上升
Metrics：时间、retain-set R-set准确率，forget-set U-set准确率，后门，MIA（在表3中说明了MIA接近50更好）
面向Client-level(sample-level)的遗忘，而非class-level
超参数：lr=0.001，epochs=100，local epochs=2，ConDa的参数看文章

实验结果：答辩
