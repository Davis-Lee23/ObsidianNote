#未开源 #CCF-A #JSAC #Metaverse

# Authors
王鹏飞，老同行组
[Pengfei Wang, Dalian University of Technology](https://pf-wang.github.io/)

# Abstract
动机：FL被用于分布式的机器学习，有助于增强以人为中心的元宇宙体验，由于数据通信量大和用户不可靠性（低质量数据），使用FL准确及时地训练机器学习模型是一个挑战。尤其是低质量数据会让模型无法准确及时地做出决策。

方法：MetaFul，三个组件
1. Low-throughput federated learning (LT-FL)：-通过降维和参数压缩减少模型传输量，降低通信开销。
2. Loss-based model quality assessment (LM-QA)：-利用LT-FL训练中的模型损失值，评估客户端数据质量，识别低质量用户。
3. Non-communicative federated unlearning (NC-FUL)：没说，就单纯消除，只说**无需通讯**，可以让服务器独自处理。

结果：消除数据后至少提升2.5%，时间至少提升19.3%



# Experiments
## Setup
数据集特别，用的Pix3D+VGG16和VRHP+LSTM
分布采用的迪利克雷分布
![[Pasted image 20250302125620.png]]