BERT: Pre-trainning of Deep Bidirectional Transformers for Language Understanding
直观上理解: 
# Abstract
BERT是一个语言展示模型. 它能够利用未标记数据的全部信息.
预训练的BERT模型在被微调后可以被用于广泛的任务中. 如在没有大量特定任务架构修正下的问答, 语言推理等.
# Introduction
两种应用预训练语言展示 任务的策略: feature-based, fine-tuning.(基于特征 , 进行微调)

作者认为现今技术限制了预训练表示的能力,特别是fine-tuning.
两类任务: "masked language model"(MLM), "next sentence prediction"(NSP)
作者列出的贡献有三:
1. 强调了双向信息预训练对于语言表达的重要性.此前的工作有仅仅使用单向信息, 也有仅仅使用两个单向进行浅度连接的LM.(双向信息重要)
2. 我们证明了预训练表示减少了许多对高度工程化的特定任务架构的需求.BERT是首个在一个大规模句子和词元任务上, 胜过了很多特定任务框架, 并取得了SOTA表现的基于微调的表达模型.(BERT好)
3. BERT好以及实现.

# Conclusion
丰富的无监督预训练已经是许多语言理解系统的一个内在环节. 即使是低资源的任务也能从这种双向深度架构中获益.作者的主要贡献是进一步推广这些发现, 让这些相同的预训练模型能够解决一系列的NLP问题.
![[../Pasted image 20231122135935.png]]
# BERT(Architecture)
作者是如何将输入进行模糊化, 让它既可以表示一个单一句子, 又可以表示两个句子的?
1. 通过加入一个<SEP>符号, 这样就可以判断是一个或者两个句子.
2. 加入embedding, 每个分量加入一个表示属于Sentence1,2 的信息.(这里给出图比较直观)
![[../Pasted image 20231122135928.png]]
每一个token句, 都以<CLS>作为起始符号, 以表示这是一个分类任务. 同样, 在输出的这个符号的对应位置所展现的也就是关于这个分类的结果.

## Model Architecture