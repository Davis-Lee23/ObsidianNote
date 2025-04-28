#CCF-B #开源 #WSDM

# Authors
作者链接：[Bio - Tianxiang Zhao’s Homepage](https://tianxiangzhao.github.io/)
本硕中科大，宾夕法尼亚phd毕业，导师[Prof.Xiang Zhang](https://ist.psu.edu/directory/xzz89) and [Prof.Suhang Wang](https://suhangwang.ist.psu.edu/).

这类题材很整蛊，个人感觉<font color="#ff0000">unfairness本身就是一种fairness</font>

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
本文分为三类：
1. individual 公平
2. group 公平，本文聚焦于此
3. Max-Min 公平

对于群体公平，又根据处理阶段分为三类方法（本文属于哪种？）：
1. pre-processing，修改训练数据，以减少数据历史歧视，如纠正标签、修改属性、生成非歧视性数据，获得公平表示。（<font color="#ff0000">偏离了客观现实</font>）
2. in-processing，修改训练过程，用公平约束函数训练
3. post-processing，直接修改模型预测标签？

尽管有研究，但是都需要敏感数据，因此研究不具有敏感熟悉的公平模型仍是挑战。然后复述了三个挑战。

# Problem Definition
F：特征集合
S：sensitive attribute
Fs：非敏感，但高度相关

# Preliminary Theoretical Analysis
没有意义

β：协方差cos的角度，0-Π

# Methodology
核心思想是使用正则化的相关特征Fs作为代理公平性目标。
就是对Fs进行三步操作，细节无所谓，对着代码找就OK。

# Optimization Algorithm
理论证明，没啥看的意义


# Experiment
旨在回答FairRF面临的三个问题：
1. 能否不使用敏感数据保持公平，且高精度
2. Fs包含错误或不完整特征时，FairRF性能如何
3. 不同超参的影响
## Setup
数据集：
1. Adult，RF：age、relation、martial status，S：gender
2. COMPAS，RF：score、decile text、sex，S：race
3. LSAC，RF：race、year、residence，S：gender
基线：Vanilla model，ConstrainS（知道每个数据的敏感性），KSMOTE，RemoveR，ARL
模型：KSMOTE用作者提供的，其他的用MLP+Adam，lr=0.001
指标：
4. Equal Opportunity：越小越好。
5. Demographic Parity：越小越好。
6. ACC

## 直观的性能比较
回答问题一：EO，DP明显优势，ACC一般

## Impact of Fs
用了不同的选择策略来选Fs
1. 完全随机：随便选五组开始
2. Fix-λ：不会自适应，固定了λ
3. Top-1：只用一个最有效的RF（全跑一遍才知道）
4. All：所有都视为RF
5. Noisy：把一个RF调包呈不相关的

## 参数敏感性
闹麻


# Power Point
方法和代码一一对应弄几页。
实验设置看情况吧
根据三类实验，选择性的做本文的实现