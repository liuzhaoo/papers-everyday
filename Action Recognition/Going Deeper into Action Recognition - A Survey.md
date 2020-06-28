### 1. First, what is a action

leg movement on a football kick :       simple motion

jumping for a head-shoot :          collective movements

"**Action is the most elementary human -surrounding interaction with a meaning**"

interaction : relative motions with respect to the surrounding

行为是人类最基本的在一定环境下产生意义的互动（与环境）                                                                      

### 2. 对算法的分类



​	<img src="https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/算法分类.png" style="zoom: 60%;" align =center/>

- 基于特征表示的方案和基于深度学习的方案



早期的行为识别使用 3D 模型描述行为，如WALKER模型；圆筒描述肢体的模型，但是由于构造 3D 模型比较难且昂贵，所以采用整体代表或者局部代表的形式

### 全局表征

Bobick 和 Davis（2001）提出了Motion Energy Image(MEI) 以及Motion History Image(MHI) ,用单一图像编码与行为相关的信息。

MEI是描述动作发生的位置的图像
$$
E_{\tau}(x,y,t) = \bigcup\limits^{\tau-1}_{i=0}D(x,y,t-i).
$$

$D(x,y,t)$ 是一个二值图像序列，表示检测到的物体像素；$E_\tau$ 表示 $\tau$ 时刻形成的MEI （$E$ 是 $D$ 在时间上的集合）。运动能量图显示了运动的轮廓和能量的空间分布。

MHI 表示了动作图像是怎样移动的，MHI 中的每一个像素都是该点的时间上动作的函数。通过计算时间段内同一位置的像素变化，将目标运动情况以图像亮度的形式表现出来。它的每个像素的灰度值表示了在一组动作序列中该位置像素的最近的运动情况，最后运动的时刻越接近当前帧，该像素的强度值越高。因此，MHI图像可以表征人体在一个动作过程中最近的动作情况，这使得MHI被广泛应用于动作识别领域。

MEI 和 MHI 包含了有关视频背景的有用的信息，比如MHI模板的梯度用于过滤移动和杂乱的背景：通过使用Harris 兴趣点检测器在 MHI模板中确定关键运动区域，然后将移动/杂乱的背景识别为与感兴趣点周围运动不一致的区域。

<img src="https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/MEI AND MHI.jpg" style="zoom: 11%;" />



例如这是一个“跳”的动作序列，中间是运动能量图（MEI），下面是运动历史图（MHI）。MEI捕捉动作发生的位置，MHI表示动作图像是如何移动的。最右边的列显示的在一个行为结束时的模板图像。

MEI的扩展版本的主要思想是通过其轮廓在时空中产生的三维形状来表示一个动作。为便于分类，通过计算曲面内各点到达边界所需的平均时间，将得到的3维曲面转换为二维图。一些研究认为用3维的图表示增强了对于视角变化的鲁棒性。

有研究建议根据时空体（STV）的差异特性来识别动作，STV是通过沿时间轴堆叠对象轮廓实现的。STV在方向，速度和形状上的改变固有地描述潜在的行为。动作的轮廓是从STV表面提取的一组属性（比如高斯曲率），并且在观测点变化时表现出鲁棒性。

<img src="https://raw.githubusercontent.com/liuzhaoo/markdown_pics/master/img/STV.jpg" style="zoom:12%;" />

左图：用于描述动作演变过程的时空体，通过计算到达边界点所用的平均时间将三维表示转换为二维图                     右图：是发网球和走路序列的时空曲面。表面几何结构（如峰、谷）用于表示一个行为

视频的整体表示大致在1997年至2007年之间主导着行动识别的研究，因为这种表征更有可能保留行动的空间和时间结构。然而，现在，局部和深度表征收到青睐。这种转变有多种原因。例如，Dollar等人声称整体方法过于僵化，无法捕捉行动的可能变化（例如，视角、外观、遮挡）。Matikainen等人认为以轮廓为基础的表征无法在轮廓内捕捉细节。


















