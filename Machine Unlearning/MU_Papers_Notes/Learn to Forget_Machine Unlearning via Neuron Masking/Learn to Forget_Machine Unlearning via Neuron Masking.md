#CCF-A #TDSC

Tips：这篇主要用来写遗忘的公式，只看了abstract。
retraining-based mu
# Authors
西电团队AI-Ants实验室，一作马卓教授，二作硕士在读（惨），通信浙大任奎

# Abstract
背景&没有动机：如今ML很火。但是很多模型训练只有加入，没有退出选项，为解决该困境，MU诞生了。
方法：提出了一个基于MIA的指标叫做forgetting rate用来衡量遗忘的有效性，并且提出了方法Forsaken
结果：在八个数据集上有效（遗忘率+效率）


# Experiment
## Setup
数据集：CIFAR-10/100，IMDB，News，Reusters，STL10，TinyImage，Customer
涵盖了图像分类、情感分析、文本分类