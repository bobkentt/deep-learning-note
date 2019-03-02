# 感知机(perceptron)
设有 n维输入的单个感知机（如下图示）a_1至 a_n为n维输入向量的各个分量w_1至w_n为各个输入分量连接到感知机的权量（或称权值），b为偏置，  f(.)为激活函数（又曰激励函数或传递函数），t为标量输出。输出 t的数学描述为：


![](https://wikimedia.org/api/rest_v1/media/math/render/svg/54c7e092c4ad81a12ef8f5ec706f5608bf6e2795)

其中![](https://wikimedia.org/api/rest_v1/media/math/render/svg/8e814b252d9514474fa0eb70431741b8b0e46a23) ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/09cd946d4df939cd266dab9d84d47fb485805398) f(x)为反对称的符号函数，其定义为：



![](https://wikimedia.org/api/rest_v1/media/math/render/svg/ded0c8c0be524222d932f66bc6288279bbeb6655)

从式(1)可知，偏置被引申为权量，而对应的输入值为  1。故，一感知机的输出行为是求得输入向量与权向量的内积后，经一个激活函数所得一个标量结果。

![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/97/Ncell.png/300px-Ncell.png)
