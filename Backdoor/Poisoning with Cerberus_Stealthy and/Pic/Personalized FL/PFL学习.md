联邦学习的成功依赖于一个重要假设：IID，所有客户端数据符合独立同分布，然而现实里基本是Non-IID场景：假设你和小Y拍辣条
+ 特征分布偏斜。辣条牌子不同
+ 标签分布偏斜。我拍辣条，你拍辣子鸡
+ 同标签，不同特征。都是辣条，但是滤镜不一样
+ 同特征，不同标签
+ 数量不均，分布不平衡
上述场景可以统称为数据的异质性heterogeneity。而且在Non-IID场景下，每个数据中心优化方向不同，成为weight divergence，会导致收敛慢，泛化能力差。

目前笔记参照Towards Personalized Federated Learning

### PFL分类：全局模型个性化 & 学习个性化模型
![[Pasted image 20240414201836.png]]

#### 全局模型个性化
分为二个阶段：①训练一个共享模型 ②在本地额外训练

基于数据的方法：数据增强、客户端选择

基于模型的方法：正则化Regularizatiuon、元学习Meta-learning、迁移学习Transfer learning

#### 学习个性化模型

基于结构的方法：
1. 参数分解：为每个数据中心设置个性化层，个性化层本地训练，公共层上传云端。(区块链)
2. 知识蒸馏

基于相似性的方法：
1. 多任务学习 Multi-Task Learning
2. 模型差值 Model Interpolation
3. 聚类

