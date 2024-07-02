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

##### 平方差损失函数 Squared error function
线性回归：凸函数
逻辑回归：非凸函数
区别在于一个像碗一样，一个会有波动，非凸函数存在很多局部最小值。这些特殊的损失计算函数可以让非凸函数尽量编程凸函数。
我们称新的cost function为损失函数。
![[Pasted image 20240615121540.png|450]]

##### 逻辑损失函数（限二分类）
分成上下两端，分别对y=0|1有不同的奖惩

上半段的函数如图所示：当y=1时
原本的log函数对x轴反转，因为f是sigmoid函数处于[0,1]，若f趋近于1，则loss趋近于0，若f趋近于0，则loss将会非常大，以此来作为判断依据
![[Pasted image 20240615124912.png|500]]

下半段函数如图所示：当y=0时
若预测f=0，则log的真数接近于1，loss趋近于0。若预测f=1，则log的真数接近于0，loss很大。
![[Pasted image 20240615130744.png|525]]


### 简化成本函数 Simplified Cost Function
将分段函数合并
![[Pasted image 20240615144817.png|500]]

所以Cost如图所示：至于为什么要选择这个函数，这源自于最大似然估计，且这个函数是凸函数，不用担心局部最值
![[Pasted image 20240615145031.png]]






### raw words：
+ email spam 垃圾邮件
+ transaction fraudulent 欺诈交易
+ tumor malignant 恶性肿瘤