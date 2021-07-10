# 基础知识

## 损失函数方法补充
除交叉熵、均方差及CTC损失外，还有：

###  Absolute Loss绝对误差

![](https://ai-studio-static-online.cdn.bcebos.com/3a12315fbb5f413c9280b3316afe81e9df7d55bc2a3c42969373657201f0061d)


### Exponential Loss指数误差

![](https://ai-studio-static-online.cdn.bcebos.com/39f82dd6dce14e67bd21cdb757d666ef13af422effaa44eb9f512be698ca295c)




complement:


```python
import numpy as np
def absoluteLoss(y_pred,y):
    loss=np.absolute(y_pred-y)
    return loss

def expLoss(y_pred,y):
    loss=np.exp(-y*y_pred)
    return loss
```

instance:


```python
y_pred = np.array([4,3,2,1])
y = np.array([1,2,3,4])
ablos=absoluteLoss(y_pred,y)
print(ablos)

y_pred1 = np.array([1,1,1,-1])
y1 = np.array([-1,1,1,1])
explos=expLoss(y_pred1,y1)
print(explos)
```

    [3 1 1 3]
    [2.71828183 0.36787944 0.36787944 2.71828183]


## 池化方法补充：
 空金字塔池化（Spatial Pyramid Pooling）：
 空间金字塔池化可以把任何尺度的图像的卷积特征转化成相同维度，这不仅可以让CNN处理任意尺度的图像，还能避免cropping和warping操作，导致一些信息的丢失，具有非常重要的意义。

一般的CNN都需要输入图像的大小是固定的，这是因为全连接层的输入需要固定输入维度，但在卷积操作是没有对图像尺度有限制，空间金字塔池化，先让图像进行卷积操作，然后转化成维度相同的特征输入到全连接层，这个可以把CNN扩展到任意大小的图像。

## 数据增强方法补充：

除单样本的普通变化、裁剪、混叠外，可以使用生成对抗网络完成数据增强：

在机器学习的模型可大体分为两类，生成模型（Generative Model）和判别模型（Discriminative Model）。判别模型需要输入变量 ，通过某种模型来预测 。生成模型是给定某种隐含信息，来随机产生观测数据。
例如，判别模型：给定一张图，判断这张图里的动物是猫还是狗。生成模型：给一系列猫的图片，生成一张新的猫咪（不在数据集里）
对于判别模型，损失函数是容易定义的，因为输出的目标相对简单。但对于生成模型，损失函数的定义就不是那么容易。我们对于生成结果的期望，往往是一个暧昧不清，难以数学公理化定义的范式。所以不妨把生成模型的回馈部分，交给判别模型处理。这就是Goodfellow他将机器学习中的两大类模型，Generative和Discrimitive给紧密地联合在了一起 。
GAN的基本原理其实非常简单，这里以生成图片为例进行说明。假设我们有两个网络，G（Generator）和D（Discriminator）。正如它的名字所暗示的那样，它们的功能分别是：
G是一个生成图片的网络，它接收一个随机的噪声z，通过这个噪声生成图片，记做G(z)。
D是一个判别网络，判别一张图片是不是“真实的”。它的输入参数是x，x代表一张图片，输出D（x）代表x为真实图片的概率，如果为1，就代表100%是真实的图片，而输出为0，就代表不可能是真实的图片。

在训练过程中，生成网络G的目标就是尽量生成真实的图片去欺骗判别网络D。而D的目标就是尽量把G生成的图片和真实的图片分别开来。这样，G和D构成了一个动态的“博弈过程”。
最后博弈的结果是什么？在最理想的状态下，G可以生成足以“以假乱真”的图片G(z)。对于D来说，它难以判定G生成的图片究竟是不是真实的，因此D(G(z)) = 0.5。
这样我们的目的就达成了：我们得到了一个生成式的模型G，它可以用来生成图片。 
Goodfellow从理论上证明了该算法的收敛性 ，以及在模型收敛时，生成数据具有和真实数据相同的分布（保证了模型效果）。
它包含两个网络，一个是生成网络，一个是对抗网络，基本原理如下：

(1) G是一个生成图片的网络，它接收随机的噪声z，通过噪声生成图片，记做G(z) 。

(2) D是一个判别网络，判别一张图片是不是“真实的”，即是真实的图片，还是由G生成的图片。
![](https://ai-studio-static-online.cdn.bcebos.com/ee139c5ac0444a9a839ffc82c174fc6652603225727e4bf794850a378310100d)


## 图像分类方法

### Densenet
密集卷积网络，每一层在前向反馈模式中都和后面的层有连接，与 L 层传统卷积神经
网络有 L 个连接不同，DenseNet 中每个层和之后的层都有链接，因此 L 层的 DenseNet 有 L(L+1)/2 个连接关系。对于每一层，他的输入包括前一层的输出和该层之前所有层的输入。

为了进一步改善各层之间的信息流,引入了从 任何一层到所有后续层的直接连
接.第 i 层接受所有先前层的特征图谱，输入如下：
![](https://ai-studio-static-online.cdn.bcebos.com/a5732a1e18a349c2a1e57e2b2a8c88b33da0673fddb24a11a26df457b5b35ad1)


其中：[x0, x1, . . . , xl−1]指第 0，1……l-1 层生成的特征图谱，由于其密集的连通性，我
们将这种网络架构称为密集卷积网络(DenseNet)。 
![](https://ai-studio-static-online.cdn.bcebos.com/736dd2aa74c042dc969b7dda89c00f1269276e04b1df4445a3f0d4f3f6d45665)


H '()定义为三个连续运算的复合函数:批量归 一化(BN) [14]，然后是 校正线性
单位(ReLU) [6]和 3 × 3 卷积(Conv)。
如果每个函数产生k个特征映射，那么“该层具有 k0 + k(l − 1) 个输入特征映射，
其 中 k0是 输 入 层 中 的 通 道 数 。 DenseNet 和 现 有 网 络 架 构 的 一 个 重 要 区 别 是DenseNet 可以有非常窄的层，例如 k = 12。 称超参数 k 为网络的增长率。相对
较小的增长率足以在一些测试的数据集上获得十分满意的结果。
对此的一种解释是，每个图层都可以访问其所在区块中的所有先前的特征图谱，
因此也可以访问网络的“集体知识”。人们可以将特征图谱视为网络的全局状态。每
个图层都在此状态下添加了 k 个自己的特征图谱。增长率决定了每一层为全局状态贡献多少新信息。全局状态一旦被写入，就可以从网络中的任何地方访问，与传统的网络体系结构不同，不需要从一层复制到另一层。虽然每个图层只生成 k 个输出要素图，但它通常有更多的输入。在每个 3×3
卷积之前引入 1×1 卷积作为瓶颈层，以减少输入特征映射的数量，从而提高计算效率。

为了进一 步提高 模型 的 紧凑性， 减少过 渡层 的 特征图谱的数量。如果 密集块
包含 m 个特征映射，我们让下面的过渡层生成θm输出特征映射，其中 0 < θ ≤ 1
被称为压缩因子。当 θ= 1 时，过渡层的要素图数量保持不变。

DenseNets 没有从非常深或非常宽的体系结构中获取表示能力，而是通过功
能重用来挖掘网络的潜力，产生易于训练和高参数效率的精简模型。将不同图层
学习的要素地图连接起来，增加了后续图层输入的差异，并提高了效率。这是
DenseNets 和 ResNets 的一个主要区别。密集网络更简单、更有效，初始网络也
连接不同层的特征。

密集卷积网络精确度提高的一种解释是，各个层通过较短的连接从损失函数中
接受额外的监督。人们可以将 DenseNets 解释为执行一种“深度监管”。深度监管的
好处以前已经在深度监管网络中得到展现(DSN[20])，它将分类器附加到每个隐藏
层，强制中间层学习辨别特征。

DenseNets 以隐式方式执行类似的深度监控:网络顶部的单个分类器通过最多
两个或三个过渡层向所有层提供直接监控。然而，损耗函数和梯度函数是非常复杂
的，因为相同的损耗函数在所有层之间共享。

优势：
1， 缓解了梯度消失
2， 加强了特征转播
3， 增强了特征复用
4， 减少了参数量