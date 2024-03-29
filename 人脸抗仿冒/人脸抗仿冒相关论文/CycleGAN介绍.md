# CycleGAN
cycleGAN是一种用于进行图像翻译的神经网络.
从一种图像到另一种风格的图像, 经典的如从人脸照到莫奈风格照片.
![[../Photos/Pasted image 20231020225349.png]]
CycleGAN的核心如图所示, 它用两对生成对抗网络, 构建了两个mapping: G(X)=Y 和 F(Y)=X. 二者各自有一个判别器用来判断输入到底是真实的X(Y)还是由生成器生成的.
在(b)和(c)中, 通过引入两个循环一致性损失来迫使生成的Y能再被转回X. 即F(G(X))≈X.

在Introduction中, 作者着重强调了以往的研究都是基于配对数据集,但是想要获得配对数据集的代价是十分高昂的.甚至在有些时候是近乎不可能的.因此,本文尝试提出一种不需要配对数据集就可完成训练的图像翻译方式, 即CycleGAN.
$$L(G,F,D_X,D_Y)=L_{GAN}\left(G,D_Y,X,Y\right) + L_{GAN}\left(F,D_X,X,Y\right)+\lambda L_{cycle}\left(G,F\right)  $$
$\lambda$是用来控制循环一致性的强度. 即lambda越大,最终上图中两个X的点就越接近.
![[../Photos/Pasted image 20231021100612.png]]在Cycle Consistency Loss中提出的autoencodes指的是将数据从高维度映射到低维度的编码器.

