- 分类性能的提高往往转化为在广泛的应用领域中显著的质量提高。
- 单纯增加卷积核的size会使计算和参数都增加
- 关于网络结构的设计准则
  - 避免表达瓶颈，尤其是在网络的前几层。具体来说，将整个网络看作有输入到输出的信息流，应尽量让网络从前到后的各个层的信息表征能力逐渐降低，而不能突然剧烈下降或是在中间某些节点出现瓶颈。
  - 在网络中高维表示更容易在局部处理：特征图通道越多，能表达的解耦信息就越多，从而更容易进行局部处理，最终加速网络的训练过程。
  - 如果要在特征图上做空间域的聚合操作（比如3$\times$3卷积），可以在卷积之前先对特征图的通道进行压缩，通常不会有负面影响。
  - 在限定总计算量的情况下，网络结构在宽度和深度上需要平衡。
- 对卷积进行分解可以减少参数
  
  - 可以将5$\times$5卷积分解为两个3$\times$3 卷积
  
    <img src="https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/factor conv.png" style="zoom:67%;" />
  
  -  将3$\times$3 卷积分解为1$\times$3 卷积和3$\times$1 卷积的组合，相当于在3$\times$3 卷积卷积滑动一个具有相同感受野的两层网络，这样就节省了$\frac{1}{3}$ de 计算（$\frac{9-6}{9}$） 
  
    <img src="https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/1x3.png" style="zoom:67%;" />

- 有效进行下采样

  - 传统的卷积网络使用池化来进行下采样，但是这样会产生表示的瓶颈（特征图深度先变小后变大），为避免产生瓶颈，在池化之前先对特征图的通道进行扩增。以下是可选择的两种下采样方法：

    <img src="https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/xiacaiyangno.png" style="zoom:80%;" />

  - 这两种方法不是产生瓶颈就是增大计算量，所以不采用，而是选择以下方法：

    <img src="https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/downsampleright.png" style="zoom:80%;" />

#### Inception-2

结构图：

	<div align='center'><img src="https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/inception2.png" style="zoom:80%;" /></div>

​	其中，将7$\times$7的卷积用三个3$\times$ 3 的卷积代替

