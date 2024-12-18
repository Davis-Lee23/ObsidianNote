
# Abstract
动机：当前大多数研究聚焦于类相关的vertical class backdoor VCB，而忽略其他简单但通用的后门攻击，这存在安全隐患。
方法：提出了HCB，HCB通过一些与类无关的无害特征（如表情或天气）激活，从而规避现有的防御机制。
结果：在MNIST、面部识别、交通标志识别、物体检测和医疗诊断等多种任务下用了十一种方法来验证该攻击的有效性。

# Introduction
DL发展惊人，但后门攻击可能对某些场景造成威胁，如脸部识别的访问控制、自动驾驶、医疗诊断。还说了微软的企业采访，担心模型遭遇后门攻击。

### VCB
作者定义的VCB：来自特定类的任何样本在出现触发器时都可以激活后门，将现有攻击归类为垂直类后门VCB

现有的VCB：...什么玩意...

然而针对VCB的防御措施通常针对于某一些特定类型的攻击。

是否存在容易实现、有效且普遍存在的未阐明的后门类型？

### HCB
本文发现了一种新型通用攻击HCB，简单有效，具有广泛适用性。

HCB消除了对类的依赖。
Effective samples：带有innocuous特征的图像。只有有效图像和触发器一起出现时才会被激活，二者只有其一时不会触发。
无害特征：无害的特征是自然界中普遍存在的特征，但与模型的主要任务无关。

Generalization：用非常简单的白色方块证明有效性，而非精心设计的触发器
Evasiveness：可以有效地绕开各种防御，因为它不基于类

### 贡献
1. 提出了HCB攻击，这是一种新颖通用的后门，可以通过无害的特征来实现，这些特征无处不在，但与主要任务无关。
2. 实验跨越不同的数据集（MNIST、GTSRB、CelebA、ISIC 医学数据集），确立了HCB的有效性和通用。
3. 评估了大量防御方法。
4. 触发器隐蔽性和后门正交性，除了图像分类，还展示了它在图像检测的通用。

# Background
### 后门攻击


### 后门对策
主要分为三种：盲目预防和移除、基于模型的检测、基于数据的检测
第一种有完整解释
第二种：AI解释：基于模型的检测（Model-based Detection）是一类用于识别和防御深度学习模型中后门攻击的方法。这类检测方法主要在模型部署前进行一次性的离线检查。
第三种就是直接监测数据，因为中毒样本和良性样本表征显然不同。

局限性：


# HCB
### Threat Model
考虑两种场景，一是模型外包，二是数据外包。前者是用户有数据，让别人帮忙训练，后者是用户找别人要数据来训练。

攻击者目标：目标是植入HCB。当有效样本和触发器一起出现时会激活后门，对其余样本正常。

攻击者能力：
1. 模型外包中，攻击者有数据和模型的完全访问权限，当然用户也可以审计自己的数据
2. 数据外包中，

### Overview
HCB所选择无害特征在自然界中无处不在，它可以是黑纸、雨、眼睛、微笑、张嘴。当然也可以人工生成，不过相比起来，还是自然界普遍存在的特性成本低
![[Pasted image 20241112151921.png]]

公式简易：
![[Pasted image 20241112152319.png]]

攻击策略：以人脸识别为例子，微笑作为无害特征
有无害特征，且有触发器的叫做dirty samples
无无害特征，且有触发器的叫做cover samples

数据外包：只能篡改部分训练数据（中毒率有限制）
模型外包：不仅可以篡改数据，还可以微调损失函数，对脏数据和覆盖数据做不同反应（投毒率不限）


# Experiment

### Setup
数据集/模型：MNIST、GTSRB（投毒12%）、CelebA（15%）
硬件：32G + 3090
![[Pasted image 20241112001845.png]]



### Metric
CDA：clean data accuracy，干净样本的正确率
ASR：
FPR-ES：False Positive Rate of Effective Samples，有效样本的假阳性率（无trigger，有无害特征）
FPR-NES：Positive Rate of Non-Effective Samples，无效样本的假阳性率（有trigger，无无害特征）
通常来说，高ASR，接近正常模型的CDA，以及低ES和NES被视作成功。

先评估数据外包（没有使用精细的攻击者训练优化），再评估模型外包

### Data Outsourcing
dirty和cover的比例为50：50
攻击性能：MNIST和GTSRB都挺好，CelebA的眼镜效果显然好于微笑和张嘴（识别难度）
![[Pasted image 20241112155347.png]]

投毒率：做了1-8的测试，可以看到CDA相对稳定，ASR呈上升趋势，ES和NES急剧下降，可以看到当脏样本太少时，无法做到有效的攻击。
![[Pasted image 20241112155440.png]]

此时作者采用了一些VCB的数据增强方法，即使用透明trigger制作脏数据，使用相同的不透明trigger制作覆盖样本，发现效果很好
![[Pasted image 20241112160004.png]]


### Model Outsourcing 模型外包
因为可以操纵训练，因此采用了一些正则化技术提高HCB攻击性能
第一个公式优化了无效样本
第二个公式强制遇到带触发器的有效样本，一定识别为预先指定的类
然后组合出了最终公式

对β进行了一定的测评

# Against Countermeasures
主要评估了data-level的防御和model-level的防御

不怕Fine-Pruning