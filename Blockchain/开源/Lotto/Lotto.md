### 动机
在选择参与聚合客户端时，恶意服务器可以策略性地选择恶意客户端与之合作，从而破坏系统的安全保证，本文提出Lotto框架，旨在保护客户端选拔过程免受恶意操纵。

### 客户端选择
节选自3.1

Random Selection：传统的FedAvg随机选择或其他基于概率分布的选取方式

Informed Selection：优先考虑最可能快速改进全局模型的客户端，评测标准有反应延迟，客户端自己上报的loss等。以Oort算法为例：根据统计效率和系统效率乘积作为得分来筛选高质量客户端
B：客户端数据集
t：客户端实际完成一轮时间
T：中央预设完成一轮时间
![[Pasted image 20240315144741.png]]

