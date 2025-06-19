# 代码部分
参考PFLib+Crab。训练得调用Crab算法来训练，也方便info的存储。

1. 在训练时是否注释evaluate会影响结果，未解之谜。
2. 那个server_back是有必要的，不知道它弄了什么bug出来
3. 训练的时候要设置  args.unlearn_attack = F，训练完再改成T
4. 图片可视化颜色错乱，很大概率是因为没有反向标准化

# 画图部分
Xlabel要有就一起有，Ylabel要统一

## 预处理
D:\GitCode\Crab\FedHydra\dataset\utils\dataset_utils.py下的bs改成了256，训练集比例因数据集而异：FMNIST 85；CIFAR 85 ；SVHN 75（考虑要不要换成GTSRB），dir指数0.1

在FedHydra目录下执行：python .\dataset\generate_cifar10.py noniid - dir
注意在Linux下，\要换成/
python ./dataset/generate_cifar10.py noniid - dir

## 超参数
优化器统一SGD
所有的训练都带有学习率衰减


## 引用
Adaptive Clipping and Distillation Enabled Federated Unlearning，提到了Streisand效应streisand




## 翻译
流程：百度翻译（deepl）-QuillBot-deepseek-QuillBot
想起来了，谷歌翻译才是goat


## Coverletter
期刊用斜体，名字要双引号

## Latex
把所有的集合，如 dataset，的格式用 \mathcal，有利于区分。
公式123的话，那么文章就用abc，避免混淆。
...用\dot，不要直接打出来
