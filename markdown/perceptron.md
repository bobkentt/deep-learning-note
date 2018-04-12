# 感知机(perceptron)
一个感知机（perceptron）可以理解成一个有线性到非线性的映射，
感知器可以表示为 f:RN→{−1,1} 的映射函数。其中 f 的形式如下：

f(x)=sign(w.x+b)

其中，w 和 b 都是 N 维向量，是感知器的模型参数。感知器的训练过程其实就是求解w 和 b 的过程。正确的 w 和 b 所构成的超平面 w.x+b=0 恰好将两类数据点分割在这个平面的两侧。


![](https://github.com/bobkentt/deep-learning-note/blob/master/pic/v2-249ec9026023c075df79eca2931fcd87_hd.jpg)
