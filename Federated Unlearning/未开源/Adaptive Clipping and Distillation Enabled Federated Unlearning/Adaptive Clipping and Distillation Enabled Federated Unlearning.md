# Authors
一作应该学生，通讯北邮高志鹏[北京邮电大学主页平台管理系统 高志鹏--中文主页--首页 (bupt.edu.cn)](https://teacher.bupt.edu.cn/gaozhipeng/zh_CN/index.htm)
这个人做通讯的文章，多数不靠谱

# 行文架构
+ 摘要
+ 介绍
+ 相关工作
	+ FU
	+ 联邦众包
+ 预备知识
	+ 联邦众包：模型同步，本地训练，全局聚合
	+ FU：隐私保护，可接受的性能，时间，空间
+ 方法
	+ 自适应参数裁剪
	+ 无数据蒸馏：一页描述

# Abstract
Motivation：场景联邦众包，并引出了FU。然而当下这种不加区分的遗忘方式虽然可以保护隐私，但是也破坏了模型的泛化性，此外依赖于客户端的重训也需要额外的成本。

方法：为解决上述问题，提出了一种新的FU框架，首先计算Fisher信息矩阵用以近似目标数据和所有模型参数之间的相关性，自适应裁剪全局模型的每个参数。随后对全局模型softmax层进行建模生成伪样本在服务器端重训增强泛化性。

结果：在三个数据集的遗忘结果、时空间上成效不错


# Experiments
### Setup
环境：3090Ti、2.2.1的PyTorch
数据集：MNIST/LeNet、Cifar10/ResNet18、Cifar100/ResNet34
指标：MIAs、ACC（剩余和目标？）、时间、空间
基线：FedAvg，Retrain，FedEraser，Rapid Retrain

### Model Performance and Privacy Security
对结果的描述
结果还行吧，就是ACC都偏低
![[Pasted image 20241127180040.png]]

### Runtime
Eraser分别是该方法的8、4.67、3.2倍
有一些对结果的解释。其中有一项是随着分类任务和模型的复杂，知识蒸馏时间占比越来越多。
![[Pasted image 20241127180312.png]]

### Storage Cost
用了一种很新的图，可以借鉴
Eraser：68M，16.64G，31.79G
![[Pasted image 20241129221902.png]]

