#CCF-B #开源 #WSDM

# Authors
作者链接：[Bio - Tianxiang Zhao’s Homepage](https://tianxiangzhao.github.io/)
本硕中科大，宾夕法尼亚phd毕业，导师[Prof.Xiang Zhang](https://ist.psu.edu/directory/xzz89) and [Prof.Suhang Wang](https://suhangwang.ist.psu.edu/).

# Abstract
背景：ML被证明从训练数据中得到的结果带有歧视和社会偏见，一些追求公平的研究开始使用敏感属性进行训练
动机：实际场景中由于隐私或法律问题，获取敏感属性通常是不可行的，这挑战了现有的公平确保策略。
方法：作者观察到部分非敏感数据和敏感数据高端相关，可以用这些数据来缓解偏差。因此本文旨在探索与敏感属性高度相关的特征，以学习公平和准确的分类器。此外提出的框架还可以同台的调整各相关特征的正则化权重，以平衡其对模型分类和公平性的贡献。

# Introduction
1. ML模型尤其是问题，肤色、年龄、性别、地区都是潜在的偏见
2. 很多方法都需要用可见的敏感属性来消除偏差，实际上不太可能可见
3. 有部分人试图在没有敏感属性下实现公平分类，但仍需要更多努力
4. 我们观察到训练数据中通常有一些非敏感特征与敏感属性高度相关，可以用来缓解偏差。（前人发现）我们称之为Related Features，我们希望可以用这一发现创造对某些敏感属性公平的模型。一种简单的方法是完全不要相关特征，但这会丢失用于分类的重要信息，因此还有很大探索空间。
5. 我们研究了一个新的问题，即探索相关特征以学习没有敏感属性的公平和准确的分类器。存在三个挑战：
	1. 怎么用RF实现公平
	2. 怎么trade-off  acc和fairness
	3. RF错误或不完整，如何使用它们
	为了解决这些问题，提出了FariRF：不是简单的丢弃相关特征，而是加以利用来规范分类器，贡献如下：
	4. 研究了新问题，在没有敏感数据情况下用RF进行公平分类器学习（提出问题）
	5. 理论证明用RF去约束模型可以跑出公平分类器（理论证明）
	6. 框架FairRF可以用RF学习公平分类器，并调整RF的权重（具体方法）
	7. 实验证明有效（实验证明）

# Related Work


# Experiment
先看intro吧，好多新东西

旨在回答FairRF面临的三个问题：
1. 能否不使用敏感数据保持公平，且高精度
2. Fs包含错误或不完整特征时，FairRF性能如何
3. 不同超参的影响

## Setup
数据集：Adult，compas，LSAC
基线：Vanilla model，ConstrainS（知道每个数据的敏感性），KSMOTE，RemoveR，ARL
模型：KSMOTE用作者提供的，其他的用MLP+Adam，lr=0.001
指标：
1. Equal Opportunity：
2. Demographic Parity：
3. ACC

