Transformer 奠基
# Abstract
作者指出当前的序列转换模型大都基于复杂的RNN或者CNN, 使用encoder-decoder结构.当然还使用了注意力机制.
作者提出了一个更简单的架构: Transformer, 它只保留了注意力机制.
且无论在大量还是有限训练数据下,泛化性都极其优秀.
	ps: RNN的局限性, 即时间上的顺序性, 当前面的$h_{t-1}$被计算出来之后, 才能继续计算$h_t$ 这样并行度低, 计算性能差. 此外,时序序列较长时, 每个状态变量h可能会丢失一些前面的时序信息,只有将$h_t$设计地比较大才不会丢失, 但是这样做同样会造成内存开销大.
# Introduction
作者介绍了循环模型, 肯定了循环模型所带来的提升,但是强调了循环模型的限制仍然是存在的:
	循环模型是沿着输入序列进行因子计算的. 它会与步长对齐的生成一个隐藏状态 $h_t$, 它表示了从时刻0到当前时刻t所有输入的信息.有:
$$ h_t = F(h_{t-1}, x_t)$$ 
	循环模型自身的特性阻碍了训练样本间的平行化, 并且这种情况再更长的序列中会变得更严重.
注意力机制往往被用于序列任务, 但是除了少数情况, 其他主要被用于循环网络的连接.

# Conclusion
Transformer 是第一个仅仅完全基于注意力机制, 取代了使用encoder-decoder架构, 和多头注意力机制的RNN层.
对于翻译认为, 相比于RNN层, 它的训练更快. 在大赛上取得了sota, 甚至优于此前所有报道模型效果的集合.
作者计划将注意力模型应用在其他模态(后续有的VIT). 他们期望能够降低生成的顺序性.

# Background
作者简要介绍了自注意力机制, 说明了他们接下来的行文布局:介绍Transformer, 激活自注意力, 并且讨论Transformer相对于其他模型的优点.

---
# Model Architecture
仍使用Encoder-Decoder架构, 每个层的sublayer使用多头注意力+残差+前向神经网络, 如此重复多个层. 同时Encoder的输出作为Decoder的输入, 最后再经过一个全连接层和激活函数. 得到输出的可能性.
不再放全图架构了, 我认为的Transformer可以拆分为:


#### Layer Norm & Batch Norm
假设一个二维数据代表一批样本, 每一行是一个样本, 每一列是所有样本的某个特征.
Batch Norm就是对每一列, 这些特征进行标准化, 即减去该列均值,除以标准差.
而Layer Norm就是对每一行, 一个样本的各个特征进行标准化.
可以理解为: Layer Norm就是对矩阵先转置, 进行Batch Norm再转回来.
![[../../Pasted image 20231120103451.png]]
> 为什么在Transformer采用Layer Norm而不是Batch Norm?
ans: 因为输入序列不定长, 对于上图的三维矩阵, Layer Norm需要横着切, Batch Norm需要沿feature这条边下刀, 而Seq可能有长有短, 这会造成计算的均值和方差失效. 而Layer Norm是针对一个输入序列样本, 因此它的长度对于整体的均值方差不会受影响.

#### Decoder
##### Masked Multi-head Attention
Decoder在每个Transformer Bolck2中添加了一个Masked Multi-head Attention, 因为自注意力机制中每个context vector是能看到所有输入序列信息的, 我们为了让解码器在t时刻只能看到t时刻之前的信息, 需要对输入序列做一个mask, 从而掩盖了t时刻之后的信息!
在训练的时候,当然是可以看见的, 但在实际验证(Decoder)时, 需要进行mask以符合真实的情况(不让未来的事情影响现在).
## 3.1 Attention
Transformer所用的注意力机制, 作者取名为是Scaled Dot-Product Attention, 即标准点乘注意力机制. 算是最简单的注意力机制;
$$ Attention(Q, K, V) =  softmax(\frac{QK^T}{\sqrt{d_k}})V $$
不同的attention主要区别还是计算权重的方式, attention的思想就是前面的输入按照不同的权重去影响后面的输出, 而这个不同权重的计算方式有多种.
这种从直观上很容易理解, 两个向量的点乘所产生的值越大, 说明这两个向量的相关性越高, 从低维入手, 两个二维向量的点乘: 
$\overrightarrow{a}*\overrightarrow{b}=|a|*|b|*cos\theta$ ,共线相似度最高, $\theta=0$.
而$\sqrt{d_k}$即为进行了Norm后向量的长度.(可证)
![[../../Pasted image 20231120112814.png]]
#### Attention的种类
作者提到主要有两种注意力机制: 加性注意力机制和点积注意力机制.本文使用了点乘注意力机制, 因为实现简单高效.

> 为什么要除以sqrt(dk)?
	作者是这样表述的: 当dk不是很大的时候没关系, 但当dk的值很大时, 维度中的值相对差距会更大, 这会造成softmax进行归一时, 容易将权值也两极分化了, 比如一个维度是0.999, 其他的维度之和是0.001这样.因此对权值进行一定程度的缩小,再进行softmax可以减少这样两极分化的程度.

### Application of Attention in our Model
作者列举了在Transformer中使用多头注意力的三处:
1. 
