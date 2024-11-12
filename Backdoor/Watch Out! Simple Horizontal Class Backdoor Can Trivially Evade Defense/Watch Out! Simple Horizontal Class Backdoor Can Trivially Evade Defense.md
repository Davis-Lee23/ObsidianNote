
# Abstract
动机：当前大多数研究聚焦于类相关的vertical class backdoor VCB，而忽略其他简单但通用的后门攻击，这存在安全隐患。
方法：提出了HCB，HCB通过一些与类无关的无害特征（如表情或天气）激活，从而规避现有的防御机制。
结果：在MNIST、面部识别、交通标志识别、物体检测和医疗诊断等多种任务下用了十一种方法来验证该攻击的有效性。

# Introduction
DL发展惊人，但后门攻击可能对某些场景造成威胁，如脸部识别的访问控制、自动驾驶、医疗诊断。还说了微软的企业采访，担心模型遭遇后门攻击。

### VCB
现有的VCB：...什么玩意...

然而针对VCB的防御措施通常针对于某一些特定类型的攻击。

是否存在容易实现、有效且普遍存在的未阐明的后门类型？

### HCB
本文发现了一种新型通用攻击HCB，简单有效，具有广泛适用性。

HCB消除了对类的依赖。
Effective samples：带有innocuous特征的图像。只有有效图像和触发器一起出现时才会被激活，二者只有其一时不会触发。

Generalization：
Evasiveness：




# Experiment

### Setup
数据集/模型：MNIST、GTSRB、CelebA
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
看来得看前文了
