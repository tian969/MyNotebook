# 1. Learning Meta Model for Zero- and Few-Shot Face Anti-Spoofing
![[../../../Pasted image 20231115114926.png]]
## Abstract
以往将抗仿冒视为一个监督学习问题, 需要大规模训练集来覆盖尽可能多的攻击类型.这会导致过拟合问题, 且对未见过的攻击类型仍然十分脆弱. 
作者做了以下工作:
1. 从已定义的展示攻击中学习可以泛化到未见过仿冒类型的判断特征;
2. 通过从预定义攻击和少量新类型的攻击方式中快速学习新的仿冒方式;
作者将人脸抗仿冒定义为一个少样本学习问题

---

# 2. Deep Tree Learning for Zero-shot Face Anti-Spoofing

## Abstract
作者定义了未知仿冒攻击为Zero-Shot Face Anti-Spoofing(ZSFA). 且扩大了仿冒攻击的类型(不止局限于打印重放攻击). 作者在本篇中研究了13种仿冒攻击方式, 包括打印, 重放, 3D面具等;
Deep Tree Network? 去将仿冒样本区分为更小的群体
此外, 作者还开放了一个具有多种攻击方式的仿冒攻击数据库.


---

# 3. Adaptive Transformers for Robust Few-shot Cross-domain Face Anti-spoofing
## Abstract
作者聚焦于解决在不同传感器在复杂场景中获取到的具有更大程度的面部变化的面部抗仿冒识别.通过使用Vision Transformer 来强化跨域人脸抗仿冒能力.
keywords: VIT, Few-Shot Learning
## Introduction
作者指出跨域问题仍有进步空间.
跨域问题面临以下挑战:
1. 域间差异: visual appearance, moire pattern, color distortion等在不同设备,光照,分辨率等条件下都会不同.
2. 数据规模小: 容易过拟合, 在其他域上泛化能力差.
作者在本篇中尝试解决以上问题.

## Experiments
### Protocols:
作者使用了两个评估协议: 
1. 使用leave-one-out testing protocol在四个小型数据集中评估泛化性.
2. 在大规模数据集上再次使用相似的评估方式.
在两个协议上都加入了CelebA-Spoof作为辅助补充的训练集数据.
### Evaluation metrics:
Half Total Error Rate, 