# Why go deeper
我们常常会把包含全连接层的神经网络看成是一系列由网络参数构成的函数的组合。

那么问题来了，这些函数的组合对数据的表达力怎么样？

是不是任何函数都能通过神经网络进行建模？

有人证明过包含一层隐含层的神经网络是一个万能逼近器(见Michael Nielsen的证明)，
也就是说它可以估计任何连续的f(x)。

那么既然两层的神经网络就可以估计任何函数了，
为什么我们还需要更多的层，需要Go deeper？

这是因为虽然两层神经网络从数学上来看是能逼近任何连续函数，但是从实际来看，更深的网络通常都比两层网络效果好，
也就是说这是一种经验论，虽然它们从数学上来看对数据的表达力是一样的。

顺便说一下，在实际中3层的神经网络通常比2层的效果更好，但是4,5,6层却不一定能提升更多，
这和卷积网络有着鲜明的对比，在卷积网络中通常深度是一个好的识别系统的非常关键的因素。
有人说这是因为图像本身就是一个分层的结构（比如人脸是由眼睛构成的，眼睛由由边缘构成等），
因此分多层来学习数据能够使网络获得更加语义化的理解。

当然了，这整个领域还在不断的研究中，下面是一些可参考的读物：

[Deep Learning book in press by Bengio, Goodfellow, Courville, in practicular Chapter 6.4.](http://www.iangoodfellow.com/dlbook/)

[Do Deep Nets Really Need to be Deep?](https://arxiv.org/abs/1312.6184)

[FitNets: Hints for Thin Deep Nets](https://arxiv.org/abs/1412.6550)
