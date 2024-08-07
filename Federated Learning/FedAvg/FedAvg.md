### Raw Words
1. preclude 排除
2. ephemeral 短暂的


### 0. 摘要
动机：如今的数据是海量的或隐私敏感的，传统的数据中心训练的方法较为落伍
方法：提出一种数据保留本地，聚合移动设备更新的方法——联邦学习

结果：在五个模型、四个数据集上证明有效。在。相比于同步随机梯度下降方法，通讯开销减少了10-100倍。<font color="#ff0000">（在non-iid数据有效、通信轮次减少了几个数量级）</font>

### 1. 引言
数据的敏感性质意味着，将其存储在一个集中的位置存在风险和责任，因此本文提出一种不需要中央存储的方法FL，每个客户端计算和更新模型，并将更新发给中央即可。（<font color="#ff0000">此处说了一句一旦应用了它们，就没有理由存储它们，与Eraser相悖</font>）

一个好处是该方法解耦了模型和直接被访问的数据。当然这也依赖于服务器的可信程度。但是联邦学习可以通过将攻击面限制为**仅设备**而不是**设备和云**来显著降低隐私和安全风险。（这里指的是攻击服务器没有数据没有意义，只能攻击本地设备）

贡献总结如下：
1. 指明了移动设备上的分散数据训练是一个重要方向（提出了FL）
2. 提出了FedAvg，该场景下直接但实用的方法
3. 大量实验

##### 联邦学习 FL
特点：
1. 适用于来自移动设备的真实数据
2. 这些数据有隐私需求或数量大(不便传输)
3. 监督学习方面，与用户交互可以轻松判断标签。
**作者认为可以适用在image classification和language models** 

##### 隐私 Privacy
FL在隐私方面有显著优势。
1. 只需要传输更新，保障了数据的安全
2. 更新不保存，是ephemeral的
3. 只通过更新窃取数据很难（现在可以了）

##### 联邦优化 Federated Optimization
相比于分布式学习，联邦学习有一些独特的可优化点：
1. Non-iid。移动设备上用户的数据集分布五花八门，因此任何用户的本地数据集都不能代表总体分布。
2. Unbalanced。，所产生的数据大小不同。
3. Massively distributed。理论上用户数量远多于我们切割的toy数据集
4. Limited communication。移动设备连接速度慢或成本高昂。（这一点文章重点讨论了，但是好像后续工作都没怎么管）

本文聚焦于1 2 4这三个问题。至于本地数据动态变化、异步不做讨论。本文讨论的是同步方案：
```
设有K个客户端，本地数据集固定
每轮随机选fraction C*K个客户端
将全局参数发送给那些客户端
客户端根据本地数据集进行更新，并发送给服务器聚合
```
该方法聚焦的是**非凸神经网络**
随后的公式给出了<font color="#ff0000">IID的设置，这通常用于分布式优化</font>。将每个客户端的数据集近似误差一下，就成了non-IID。

原文中认为通信才是最大的问题，计算基本上可以忽略不计，移动设备只会进行偶尔的训练，因此希望的是利用计算来trade通信，方式有二：①让更多用户计算 ②增加单个用户的计算量（<font color="#ff0000">这个更有效</font>）

而相关工作基本是集中式的，IID的，并且有文章证明集中式训练有可能效果不如单个客户端

### 2. 方法
梯度下降：SGD，对每一个batch都进行一次梯度计算
基线方法：C=1，B=∞，加权平均，称为FedSGD。
FedAvg：C小数，B=minibatch

双客户端进行试验，当种子一样的时候，俩权重55开效果最好
![[Pasted image 20240627175242.png|425]]

算法截图：
![[Pasted image 20240627175606.png|400]]

### 3. 实验
数据集：
1. MNIST：MLP、CNN
2. Shakespeare：LSTM
3. 分别对上述数据集进行了IID和NON-IID的实验
4. CIFAR-10：IID，模型不知道，预处理强化数据
<font color="#ff0000">pathological non-IID partition</font>：将MNIST的6w数据标签排序后分为200片，每个客户端2片，保证了每个客户端也只有2类数据

超参数：学习率通过网格搜索，C=0.1(经验)


以达到预设精度为目标的情况下，多个client的效果好于1个client，并且B=10好于B=∞，在non-iid场景之中更为明显。作者认为效果好于SGD的原因是AVG做出了类似Dropout的效果

而FedAvg采用C=0.1，多本地轮次的情况下，收敛轮次也显然小于SGD。但是如果本地轮次过于高，将会陷入局部最优。作者给出的建议是：在训练后期，可以尝试减少本地轮次，理论上等价于减少学习率的效果。

### 4. 总结
本文最后展望了差分隐私和安全多方计算。FedAvg是根基之作，本文设计的场景是cross-device，小数据量多客户端，且为non-iid场景。文章所考虑的通讯开销通过通讯轮次反馈，即达到收敛的轮次。
