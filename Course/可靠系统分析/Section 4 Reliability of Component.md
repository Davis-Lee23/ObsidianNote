又叫independent system components，本章的目的就是求解和分析可靠性

它的评估指标是Availabity，可用性。

# 概览
+ 串联系统
+ 并联系统
+ 杂交系统

给了一些串联和并联的可靠性表达式。

在五组件情况里，将3分为可用和不可用两种情况来分析。

前二十页用多个例子分析了三种系统的可靠性。


# 概览2

系统冗余与组件冗余的可靠性
n中取k的多数表决系统的可靠性
连续k/n系统的可靠性
可靠性与可用性的关系
![[Pasted image 20241225140312.png]]

## 2.1 Reliability of a Component
五个部分
![[Pasted image 20241225140638.png]]

①条件可靠度给了一些公式，好像如果是负数，就直接用Z值表的值，正数就1-x

失效密度函数

一些失效相关的公式，只有公式，没有例子

**然后还列举了十个分布**
1. 亚指数分布
2. Erlang分布
3. Gamma分布
4. 超指数分布：CPU服务时间符合这个分布
5. Weibull分布：布常用来描述疲劳失效，电子元件故障和滚珠轴承失效
6. Log-Logistic分布
7. 正态分布
8. 均匀分布
9. Parrto分布
10. 缺损分布


### Weibull分布
有专门的例题


## 2.2 Reliability of Series Systems
大约70页才进入该环节

三个性质
1. 可靠性一定小于1
2. 系统可靠性一定小于单个组件可靠性
3. 可靠性随组件增多而降低

还有很多别的东西要用再来

## 2.3 Reliability of Parallel Systems

性质：
1. 并行系统的可靠性高于任何单个组件
2. 由此而知，系统的可靠性比最可靠的单个组件还可靠

因此把系统并联可以提高可靠性，这是提高可靠性的其中一种方法。


## 2.4 Reliability of System Redundancy vs Component Redundancy
要比较的话相除就行



## 2.5 Reliability of k-out-of-n Systems
k=n，串联
k=1，并联
多余(n+1)/2的称为Major

## 2.6 Reliability of the Consecutive k-out-of-n System
k个连续部件出现故障，系统就会故障。
