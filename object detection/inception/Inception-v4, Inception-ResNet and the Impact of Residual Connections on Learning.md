- 实验表明，使用残差连接可以加速Inception的训练，同时性能也比不使用残差的Inception好

- 提出了新的带残差和不带残差的结构

- 进一步演示了适当的激活缩放如何稳定训练很宽的残差Inception网络

- 在旧版本的Inception中，为保持网络其他部分的稳定，在更改网络结构以及实验室相对保守和受限。这些结构没有进行简化，因此看起来比实际上要复杂。

#### Inception v4:

具体图见原文

- 在Inception 残差版本中，使用的Inception模块比Inceptionv4的更简单，每个模块后面跟一个1$\times$1卷积来升维，从而保证与输入维度相同

- Inception-ResNet-v1与Inception-v3计算成本相似，Inception-ResNet-v2与Inception-v4相似

- 残差版本与非残差版本的另一个不同之处是，在残差版本中，只在传统层的顶部使用BN，在最顶层不使用

- 卷积核的数量太多的时候，会出现网络最后的层只输出0的情况，且无法通过调整学习率和添加BN层来解决。但是发现在将残差块添加到下一层之前对其进行缩放（乘一个系数）可以稳定训练。

  

  

##### 若网络中不带“V”标记的代表此卷积输入输出相同

  

![](https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/inceptiv4-2.png)

![](https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/incepv4-1.png)