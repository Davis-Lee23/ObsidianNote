### 前言
如果输入图像的维度之积过大，前馈神经网络要处理的神经元太多，导致训练效率低下并且容易过拟合。此外图像经过缩放、平移、旋转操作并不会影响语义信息，而前馈神经网络不经过数据增强难以提取这些信息。

卷积神经网络是受感受野机制启发而诞生的，是由卷积层、池化层、全连接层交叉组成的前馈神经网络，最大的好处就是参数少，不用全连接而是与部分特征相连。
![[Pasted image 20240807104709.png|550]]


### 池化层
[神经元与连接数的解释]([什么是卷积神经网络中的-----“神经元”以及“连接数”_卷积神经网络神经元个数-CSDN博客](https://blog.csdn.net/weixin_43200669/article/details/101063068))

Pooling Layer又称子采样层Subsampling Layer，其作用是进行特征选择，降低特征数量，从而减少参数数量。<font color="#ff0000">卷积层虽然显著减少了连接数量，但是神经元个数并没有显著减少</font>，为解决这个问题，在卷积层之后加上一个池化层可以降低特征维数，避免过拟合。

池化就是对每个区域进行下采样得到一个值作为整个区域的概括：
![[Pasted image 20240809101832.png]]

最大池化：取区域内神经元最大值（提取纹理好）
平均池化：顾名思义（背景保留好）

### 卷积神经网络
卷积层：提取特征
池化层：降维、防止过拟合
全连接：输出结果
