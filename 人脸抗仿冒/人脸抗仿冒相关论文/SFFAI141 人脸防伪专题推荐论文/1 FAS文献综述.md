Deep Learning for Face Anti-Spoofing: A Survey
![[../../../Pasted image 20231110102121.png]]
![[../../../Pasted image 20231110102132.png]]

---
end-to-end: 端到端的, 即输入图片, 输出判定真假.


---
# 2 Background
![[../../../Pasted image 20231110103200.png]]
## 2.1 Face Spoofing Attacks
针对人脸识别系统的攻击通常被分为两类:
1. digital manipulation

2. physical presentation attacks
主要研究第2类.
AFR: automatic face recognition
当前结合FAS和AFR的策略有两种:
1. 平行融合: 在FAS和AFR中同时进行分数预估.最终将两个系统的分数结合, 来判断究竟是真人还是仿冒.
2. 顺序策略: 先进行人脸仿冒的检测, 以此来避免进入AFR中.
按照攻击方意图, 人脸展示攻击可以分为:
1. 冒充: 通过使用授权者的某些面部特征(如使用照片, 视频,3d面具等)来进行欺骗
2. 混淆: 通过各类方式去隐藏或者移除攻击者自身的特征(带眼镜, 化妆, )

## 2.2 Datasets for FAS

## 2.3 Evaluation Metrics

## 2.4 Evaluation Protocols
评估类型分为4种: 内部数据内部类型, 内部数据外部类型, 外部数据内部类型, 外部数据外部类型.
介绍了几种评估协议, 并在每一种中举出一个较为典型的使用该评估类型的文章.
Intra-Dataset Intra-Type Protocol: 训练数据和测试数据来自同一个样本集.
Cross-Dataset Intra-Type Protocol: 训练集来自一个或多个数据集, 测试时使用另一个或多个不同的数据集进行测试.
Intra-Dataset Cross-Type Protocol: 为了验证模型针对未知攻击类型的泛化能力, 特意在训练时保留一种攻击类型的训练集数据. 在测试时使用这种数据.
Cross-Dataset Cross-Type Protocol: 最具有挑战性的类型.在测试时,模型要面对没有见过的攻击类型和数据. 
# 3 DEEP FAS WITH COMMERCIAL RGB CAMERA
按照传感器类型, 可以分为RGB相机 和 其他.
![[../../../Pasted image 20231112152804.png]]
## 3.1 Hybrid Method
深度学习神经网络尽管表现不俗, 但是存在过拟合问题, 受到有限训练数据集和缺乏多样性的影响. 手工特征针对 取真 具有很强的判别性. Hybrid Method即混合方法. 因此主要谈的是混合handcrafted features 和 Deep features.

## 3.2 Common Deep Learning Method
之前的工作主要使用BCE作为损失函数.
作者列举了一些直接使用BCE作为损失函数的成果, 也列举了一些对BCE损失函数进行改进, 让它能提供更多的判别信息的例子.
大多数展示攻击没有面部的深度信息, 这点可以被用来判别活体. 一些人使用像素级的深度信息标签来训练模型. 让模型去预测活体的深度值(伪造样本深度为0).
- 相对于为每一个训练样本构造一个3D 形状标签, 使用二进制面具标签更优.
此外, 还有一些非主流的辅助监督: 如reflection map, 3D point cloud map, ternary map等等.

综上, 可以分为三类:
Direct Supervision with BCE Loss
Pixel-wise Supervision wiht Auxiliary Task
Pixel-wise Supervision wiht Generative Model
## 3.3 Generalized Deep Learning Method
通常的FAS方法对于没见过的条件和未知的攻击类型的判别能力差,因此越来越多的人倾向于提高模型的泛化能力.
一方面, 域适应和泛化技术在无限的域变化条件下被充分锻炼; 另一方面, 少样本学习以及异常补货框架也被运用在捕获未知的展示攻击上.
因此这一部分主要介绍了处理未知域和未知攻击类型的具有代表性的几个方法.
### 3.3.1 Generalization to Unseen Domain
如何实现在未观察过的域上的泛化?
作者提出domain shift issue这一问题, 即改变了... 条件.
在训练时, 可以体现为不同抗仿冒数据集之间的变换. 如从OULU-NPU数据集到MSU-MFSD数据集.
作者总结了三种策略:
1. Domain Adaptation
2. Domain Generalization
3. Federate Learning

### 3.3.2 Generalization to Unknown Attack Types
如何实现对未知攻击类型的泛化?
以往研究都把FAS视为一个闭集问题, 只对预定义的攻击类型进行捕获. 但是这样想要覆盖所有的攻击类型需要海量的训练样本. 这也导致在面对某些常见攻击时会导致过拟合, 但是针对未知攻击类型依然效果不够明显.
为解决这个问题, 作者整理了研究者们关于这方面的几个方向:
1. 少样本学习
2. 异常捕获

# 4 DEEP FAS  WITH ADVANCED SENSOR



---
Anti-spoofing 1.0
人工找活体与非活体攻击的diff, 并根据这些差异设计特征, 最后送给分类器决策.
活体与非活体有哪些差异?
Face_Spoof_Detection With Image Distortion Analysis


---
# 5 DISCUSSION AND FUTURE DIRECTIONS
作者认为许多研究者选择了现成的网络架构和手工监督信号, 这对于充分利用大规模训练数据来说并不是最优解.
尽管他们已经在考虑更优的架构, 损失函数, 辅助监督等, 但是他们忽略了时间信息和多模环境.
因此, 作者认为的一个有前景的方向是自动搜索找到最适合的时间架构. 

作者给出的另一个方向是: 在移动设备的实时检测领域, 设计更高效的网络架构.
因为只有很少的工作考虑了 lightweight, distilled CNNS.

inductive bias: 归纳偏置, 指在进行归纳推理时，由于个体的经验、信念或先验知识的影响，导致对新的观察或数据的解释和预测存在一定的偏见。

近年来, 很多研究者试图增强FAS的可解释性. 列举一些工作. 它们帮助人们理解和定位仿冒模式.
但是由于缺少像素级别的仿冒标记, 估计的仿冒映射仍然很粗糙, 并且很容易被不可靠的一些线索影响.
作者认为: 更先进的可视化特征, 更细化的仿冒特征分割 能增强FAS的可解释性.

## 5.2 Representation Learning
作者认定的两个高效方向:transfer learning and disentangled learning for FAS. 即迁移学习 和 
前者充分利用了预训练的其他大规模数据集的语义特征去减轻过拟合问题. 后者从充满噪音的展示中树立了内在的仿冒线索.
- 如何充分利用未标注且不平衡数据?
- 如何设计一个合适的数据增强策略? 对抗学习?

## 5.3 Real-World Open-Set FAS
相比于真实的世界, 训练用数据量小, 协议聚焦单一因素(如未知域或者未知攻击类型), 可能无法满足复杂的现实世界场景.
当前有其他测试协议如 GrandTest, open-set 被提出.
[[GrandTest and Open-Set Protocol]]
但是real-world open-set situations with simultaneous domains and attack types仍被忽视.