# Activation functions summary
## Why use the activation functions?
如果不引入activation functions，则神经网络是一个线性模型，
而线性模型的capacity不够拟合复杂问题。为了增强神经网络的capacity，
引入activation functions,使neural network成为非线性模型。
## Several familiar functions
### Sigmoid and tanh
![](https://github.com/bobkentt/deep-learning-note/blob/master/pic/20160305203022044.jpg)
#### Sigmoid
Sigmoid函数表达式：

σ(x)=1/(1+e−x)

sigmoid函数输入一个实值的数，然后将其压缩到0~1的范围内。特别地，大的负数被映射成0，大的正数被映射成1。
sigmoid function在历史上流行过一段时间因为它能够很好的表达“激活”的意思，未激活就是0，完全饱和的激活则是1。
而现在sigmoid已经不怎么常用了，主要是因为它有两个缺点:

* sigmoid saturate and kill gradients.并且当输入非常大或者非常小的时候，神经元的梯度就接近于0了，
这就使得我们在反向传播算法中反向传播接近于0的梯度，导致最终权重基本没什么更新，我们就无法递归地学习到输入数据了。
* Sigmoid outputs are not zero-centered. Sigmoid 的输出不是0均值的，这是我们不希望的，因为这会导致后层的神经元的输入是非0均值的信号，这会对梯度产生影响：假设后层神经元的输入都为正(e.g. x>0 elementwise in f=wTx+b),那么对w求局部梯度则都为正，这样在反向传播的过程中w要么都往正方向更新，要么都往负方向更新，导致有一种捆绑的效果，使得收敛缓慢。
当然了，如果你是按batch去训练，那么每个batch可能得到不同的符号（正或负），那么相加一下这个问题还是可以缓解。因此，非0均值这个问题虽然会产生一些不好的影响，不过跟上面提到的 kill gradients 问题相比还是要好很多的。

#### tanh
Tanh. Tanh和Sigmoid是有异曲同工之妙的，它的图形如上图右所示，不同的是它把实值得输入压缩到-1~1的范围，
因此它基本是0均值的，也就解决了上述Sigmoid缺点中的第二个，所以实际中tanh会比sigmoid更常用。
但是它还是存在梯度饱和的问题。Tanh是sigmoid的变形：tanh(x)=2σ(2x)−1。

### Relu
![](https://github.com/bobkentt/deep-learning-note/blob/master/pic/20160305203055644.jpg)

Relu的优点：

* 优点1：Krizhevsky et al. 发现使用 ReLU 得到的SGD的收敛速度会比 sigmoid/tanh 快很多(如上图右)。有人说这是因为它是linear，而且梯度不会饱和。
* 优点2：相比于 sigmoid/tanh需要计算指数等，计算复杂度高，ReLU 只需要一个阈值就可以得到激活值。

Relu的缺点：
* 缺点1： ReLU在训练的时候很”脆弱”，一不小心有可能导致神经元”坏死”。举个例子：由于ReLU在x<0时梯度为0，这样就导致负的梯度在这个ReLU被置零，而且这个神经元有可能再也不会被任何数据激活。如果这个情况发生了，那么这个神经元之后的梯度就永远是0了，也就是ReLU神经元坏死了，不再对任何数据有所响应。实际操作中，如果你的learning rate 很大，那么很有可能你网络中的40%的神经元都坏死了。 当然，如果你设置了一个合适的较小的learning rate，这个问题发生的情况其实也不会太频繁。

Leaky ReLU. Leaky ReLUs 就是用来解决ReLU坏死的问题的。和ReLU不同，当x<0时，它的值不再是0，而是一个较小斜率(如0.01等)的函数。也就是说f(x)=1(x<0)(ax)+1(x>=0)(x),其中a是一个很小的常数。这样，既修正了数据分布，又保留了一些负轴的值，使得负轴信息不会全部丢失。关于Leaky ReLU 的效果，众说纷纭，没有清晰的定论。有些人做了实验发现 Leaky ReLU 表现的很好;有些实验则证明并不是这样。


- PReLU. 对于 Leaky ReLU 中的a，通常都是通过先验知识人工赋值的。然而可以观察到，损失函数对a的导数我们是可以求得的，可不可以将它作为一个参数进行训练呢? Kaiming He 2015的论文《Delving Deep into Rectifiers: Surpassing Human-Level Performance on ImageNet Classification》指出，不仅可以训练，而且效果更好。原文说使用了Parametric ReLU后，最终效果比不用提高了1.03%.
-Randomized Leaky ReLU. Randomized Leaky ReLU 是 leaky ReLU 的random 版本, 其核心思想就是，在训练过程中，a是从一个高斯分布中随机出来的，然后再在测试过程中进行修正。


![](https://github.com/bobkentt/deep-learning-note/blob/master/pic/20160305203318889.jpg)

### Maxout
Maxout. Maxout的形式是f(x)=max(w_1^Tx+b_1,w_2^Tx+b_2)，它最早出现在ICML2013上，作者Goodfellow将maxout和dropout结合后，号称在MNIST, CIFAR-10, CIFAR-100, SVHN这4个数据上都取得了start-of-art的识别率。可以看出ReLU 和 Leaky ReLU 都是Maxout的一个变形，所以Maxout 具有 ReLU 的优点（如：计算简单，不会 saturation），同时又没有 ReLU 的一些缺点 （如：容易饱和）。不过呢Maxout相当于把每个神经元的参数都double了，造成参数增多。

## How to choose a activation function?
'''
怎么选择激活函数呢?

我觉得这种问题不可能有定论的吧，只能说是个人建议。

如果你使用 ReLU，那么一定要小心设置 learning rate，而且要注意不要让你的网络出现很多坏死的 神经元，如果这个问题不好解决，那么可以试试 Leaky ReLU、PReLU 或者 Maxout.

友情提醒：最好不要用 sigmoid，你可以试试 tanh，不过可以预期它的效果会比不上 ReLU 和 Maxout.
还有，通常来说，很少会把各种激活函数串起来在一个网络中使用的。

'''

## 说明
本篇参考博文：

http://blog.csdn.net/cyh_24/article/details/50593400
