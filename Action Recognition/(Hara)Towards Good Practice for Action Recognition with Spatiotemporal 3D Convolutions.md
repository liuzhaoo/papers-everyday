- 本研究的目的是探索3DCNN网络的好的训练方法

- 经过实验得出以下结论

  1. 时空随机裁剪的数据增强提高了性能水平
  2. 多尺度的空间数据增强会提高精度，而多尺度的时间数据增强会降低精度
  3. 对于二维网络神经网络，交裁剪策略是一种较好的方法，但对于三维网络神经网络，与简单的随机裁剪相比其精确度较低
  4. 在相对较小的数据集上微调网络时，将前几层去掉可以提高性能
  
- 目前I3D是sota模型，提升精度到98%

- Varol等人将C3D的输入帧加长

- 本文使用3DResNet结构进行试验

  - <img src="https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/hara3res3d.png" style="zoom:67%;" />
  
- 训练参数设置同作者其他论文

- 从头开始训练网络时，我们从学习率0.03开始，然后在10个epoch期间验证损失没有改善后将学习率除以10，训练250次

- 微调时,学习率设置为0.0003，同样的策略训练100次

- 数据增强

  - 空间中心裁剪：在每一帧的中心位置对图像进行修正，变成长和宽都为原始图像最短边的图像。因此这种配置不包括空间增强（除了进行水平翻转），并且只进行随机的时间裁剪
  
  - 空间随机裁剪：采用均匀抽样的方法，随机选取每个样本的空间位置，进行最大尺度的裁剪。
  
  - 多尺度空间随机裁剪：在图像中随机的位置进行多尺度（相对于最短边）裁剪
  
  - 多尺度空间角裁剪：在图像中四个角位置进行多尺度（相对于最短边）裁剪
  
  - 多尺度时空随机：此配置下的空间增强使用多尺度空间（随机或角）裁剪。此外，我们选择了时间尺度$s_t$：$\{ 3,2,1,\frac{1}{2},\frac{1}{2}\}$ 。使用$s_t \times16$  的帧进行时间裁剪，然后通过最近邻缩放法对片段进行时间缩放。比如$s_t$为2时，首先选一段32帧的片段，然后每三帧去掉一帧。如果$s_t$为1/2，则先取一段8帧的片段，然后每帧复制两份（作为生成的新片段）
  
    ![](https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/aug.png)
  
- 微调
  
  -  使用小数据集时微调所有层不是很有效，因此改变了微调的范围
  -  
  
- 实验

  - 数据集使用Kinetics , UCF-101，和 HMDB-51

  - 不同数据增强方法的性能、

    ![](https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/xingneng.png)

  - 结果显示空间随机裁剪提高了识别性能，角裁剪不如此方法



  

  







#### 佳句记录

These 3D CNNs are intuitively effective because the 3D convolution can be used to directly extract spatiotemporal features from raw videos