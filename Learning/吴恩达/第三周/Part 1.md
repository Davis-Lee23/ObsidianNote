### 动机与目的 Motivation
线性回归预测无限多种可能的数字，显然不适合分类classification任务，因此本章引入新的回归算法，称之为logistic regression。

二分类任务成为binary classification
决策边 decision boundary

###  Logistics Regression
sigmoid function，又称为logistic function，值域在[0,1]

它的作用是当你输入特征或者特征集合时，会输出0或1。
z=0，g(z) = 0.5，当z越大越趋近于1。
z的值源自于之前的线性函数，转换过程如图所示
![[Pasted image 20240611150236.png]]

我们将计算得到的值记作概率，此处Andrew给出了一个常见的写法
<font color="#ff0000">竖线 | 左边的是结果，右边是一些会影响他的参数，这种形式在论文里常见</font>
![[Pasted image 20240611151819.png]]

### 决策边界 Decision Boundary
就是个简单的阈值，不过可以是直线，可以是圆，可以是椭圆，可以是不规则图形，在图形内的就将目标视为y=1


### 逻辑回归的损失函数 Cost Function for Logistic Regression 

显然平方差损失函数不适用于逻辑回归函数















### raw words：
+ email spam 垃圾邮件
+ transaction fraudulent 欺诈交易
+ tumor malignant 恶性肿瘤