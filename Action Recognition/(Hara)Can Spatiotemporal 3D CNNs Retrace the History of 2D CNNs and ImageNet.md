- 本研究的目的是确定当前的视频数据集是否有足够的数据来训练具有时空三维核的深度卷积神经网络(CNNs)。
- 研究了当前视频数据集中从较浅到非常深的各种3D cnn的架构
- 根据实验得到的结论：
  - 在UCF-101、HMDB-51和ActivityNet上训练ResNet-18有明显过拟合，但在Kinetics上没有影响
  - Kinetics数据集有足够的数据用于训练深度3DCNN，并且能够训练多达152个ResNets层，有趣的是类似于ImageNet上的2D ResNets。ResNeXt-101在Kinetics测试集上达到了78.4%的平均准确率
  - 在Kinetics上预训练的简单3D架构优于复杂的2D架构，预训练的ResNeXt-101在UCF-101和HMDB-51上分别达到94.5%和70.2%
- 在Kinetics上训练不同深度的模型，结果与在ImageNet上类似

<img src="https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/modeldepth.png" style="zoom:80%;" />

- 本研究结果表明，3D CNN越深效果越好
- 使用3种方法在UCF-101 , HMDB-51 , ActivityNet , 和Kinetics进行了试验。
  - 首先在每个数据集上用较浅的3DCNN网络进行试验，根据以前的工作可得出结论：除了在Kinetics上，表现都不是很好。使用了ResNet-18，这是ResNet架构中最浅层的一种，基于这样的假设:如果ResNet-18在一个数据集上进行训练时发生过拟合，那么这个数据集太小，无法用于从头开始训练深度3D CNN
  - 然后在Kinetics上用不同深度的resnet进行训练。
  - 最后在UCF-101和HMDB-51上微调Kinetics的预训练模型
- 使用到的不同网络的结构 $x^3$ 代表卷积核大小，F代表卷积核深度，也是下一层的特征图数量。N是每一层Block的数量。除了DenseNet，其他网络都在conv(3,4,5)_ 1处进行下采样。在convn_ 2之前有一个$3\times3\times3$ 的最大池化层进行下采样。此外，conv1还在空间上进行了下采样。



<img src="https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/blocks.png" style="zoom: 67%;" />



<img src="https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/detail.png" style="zoom: 67%;" />

- 训练

  - 使用动量随机梯度下降，动量为0.9，权重衰减为0.001
  - 随机生成训练样本来进行数据扩充：先通过均匀采样的方法在视频中选择一个时间点，生成训练样本。然在在这些定位点生成16帧片段，如果视频小于16帧，则对其进行循环。然后从4个角或1个中心随机选择空间位置（裁剪）。除了位置之外，我们还选择每个样本的空间尺度来进行多尺度裁剪。尺度从以下进行选择$\{1,\frac{1}{2^{1/4}},\frac{1}{2^{1/2}},\frac{1}{2^{3/4}},\frac{1}{2} \}$ .1表示最大尺度(即尺寸为帧短边的长度)。裁剪的帧的纵横比为1。生成的样本水平翻转的概率为50%。
  - 将样本resize为$112\times112$ 
  - 还对每个样本执行均值减法
  - 所有生成的样本都有与原始视频相同的类别标签
  - 4GPU,batchsize=256
  - 初始学习率设置为0.1，在验证损失饱和后，将其除以10三次
  - 当执行微调时，从0.001的学习率开始，并分配1e-5的权重衰减。

- 识别

  - 使用训练模型识别视频中的动作。采用滑动窗口的方式来生成输入片段(即每个视频被分成16个不重叠的帧片段。)每个片段进行最大尺度的中心裁剪。
  - 使用训练的模型估计每个片段的动作类别概率，再对视频中所有片段的概率求平均来识别动作。

- 结果

  - 实验1（resnet18）前三个的验证损失都很高，表明发生了过拟合，而在Kinetics上，验证损失低于训练损失。因此在小数据集上从头训练深度3DCNN是很难的

    ​		![](https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/result1.png)

  - 实验2 从resnet-200开始，模型发生了过拟合。各种模型比较的结果证明Kinetics足够大来作为视频领域的基准。

  - <img src="https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/modeldepth.png" style="zoom:80%;" />

    

    ![](https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/result2.png)

  - 实验3（微调）在小数据集上进行微调，同样可以提升精度。