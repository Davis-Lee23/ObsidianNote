### 梯度下降实现 Gradient Descent Implementation
为了找到逻辑回归中w和b令J的最小值，此处依然使用梯度下降。

这里有挺多数学推导的，基础补完再看看（没截图）


可以看到化简后是一样的，但是f的含义不同
![[Pasted image 20240619144748.png|400]]


### 过拟合问题 The Problem of Overfitting
注：与过拟向相反的问题叫做欠拟合underfitting。
前言：我们可以用正则化regularization解决过拟合