---
layout: post
title: 灰色预测学习笔记(一)
categories:
- research
tags:
- Artificial Intelligence
- Mathematical Modeling
- Algorithm
---

灰色预测其实应用面还是非常广的, 可惜已知都没有比较系统的学过, 决定花几天时间好好琢磨琢磨~顺便做个笔记, 大概需要分理论和实践两章讲完, 这一章重点是理论部分, 当然还是重在实践这部分也就从简了.

在数学建模中经常会遇到灰色预测的问题, 甚至在有些赛题中, 灰色预测占据了主导地位, 03年的SARS传播问题, 05年的长江水质的评价和预测, 06年的爱滋病的评价和疗效预测, 07年的中国人口增长问题.

除此之外, 在很多题目的求解当中也会遇到灰色预测的问题, 比如09年的会议筹备对与会人数的确定. 灰色模型最大的优点就是比较实用. 用灰色模型预测的结果比较稳定, 不仅适用于大数据量的预测, 在数据量较少的时候(只要多于3个即可)预测结果依然比较准确.

**灰色预测基础知识**

灰色系统的理论认为: 系统的行为现象尽管朦胧的, 数据是复杂的, 但它毕竟是有序的, 是有整体功能的. 在建立灰色预测模型之前, 需要对原始时间序列进行数据处理, 经过数据预处理后的数据序列称为生成列. 对原始数据进行预处理, 并不是来寻求它的统计规律和概率分布, 而是将杂乱无章的原始数据列通过一定的方法处理, 变成有规律的时间序列数据, 即以数找数规律, 再建立动态模型. 灰色系统常用的数据处理方法有累加和累减两种, 通常用累加的方法.

灰色预测通过鉴别系统因素之间发展趋势的相异程度, 并对原始数据进行生成处理来寻找系统变动的规律, 生成有较强规律性的数据序列, 然后建立相应的微分方程模型, 从而预测事物的未来发展趋势. 灰色预测的数据是通过生成数据的模型所得到的预测值的逆处理结果. 灰色预测是以灰色模型为基础的, 在诸多的灰色模型中, 又是以灰色系统中单序列一阶线性微分方程GM(1,1)模型最为常用.

下面简要介绍一下GM(1,1)模型, 高手老鸟请自己屏蔽.

设有原始数据列$$x^{(0)}=(x^{(0)}(1),x^{(0)}(2),\cdots,x^{(0)}(n))$$, n为数据个数.
如果根据这个数据列来建立GM(1,1)来实现预测功能, 则基本步骤如下:

**1.** 原始数据累加以便弱化随机序列的波动性和随机性, 得到新数据序列:

$$!x^{(1)}=(x^{(1)}(1),x^{(1)}(2),\cdots,x^{(1)}(n))$$

其中, $$x^{(1)}(t)$$中各数据表示对应前几项数据的累加.
$$!x^{(1)}(t)=\sum\limits^t_{k=1}x^{(0)}(k), t=1,2,\cdots,n$$
$$!x^{(1)}(t+1)=\sum\limits^{t+1}_{k=1}x^{(0)}(k), t=1,2,\cdots,n$$

**2.** 对$$x^{(1)}(t)$$建立$$x^{(1)}(t)$$的一阶线性微分方程:
$$!\frac{dx^{(1)}}{dt}+ax^{(1)}=u$$
其中, a, u为待定系数, 分别称为发展系数和灰色作用量, a的有效区间是(-2, 2), 并记a, u构成的矩阵为
$$!\begin{pmatrix}a\\u\end{pmatrix}$$因此, 只要求出参数a和u, 就能求出$$x^{(1)}(t)$$, 进而求出$$x^{(0)}$$的未来预测值.

**3.** 对累加生成数据作均值生成和常数项向量, 即

$$! \textbf{B}=\begin{bmatrix}0.5(x^{(1)}(1)+x^{(1)}(2))\\0.5(x^{(1)}(2)+x^{(1)}(3))\\0.5(x^{(1)}(n-1)+x^{(1)}(n))\end{bmatrix},$$

$$! \textbf{$Y_n$}=(x^{(0)}(2), x^{(0)}(3),\cdots,x^{(0)}(n))^T$$

**4.** 用最小二乘法求解灰参数, 则
$$!\hat{a}=\begin{pmatrix}a\\u\end{pmatrix}(B^TB)^{-1}B^TY_n)$$

**5.** 将灰参数a带入$$\frac{dx^{(1)}}{dt}+ax^{(1)}=u$$, 并对$$\frac{dx^{(1)}}{dt}+ax^{(1)}=u$$进行求解, 得:
$$!\hat{x}^{(1)}(t+1)=(x^{(0)}(1)-\frac{u}{a})a^{-at}+\frac{u}{a}$$
由于a是通过最小二乘法求出的近似值, 所以$$\hat{x}^{(1)}(t+1)$$是一个近似表达式, 为了与原序列$$x^{(1)}(t+1)$$区分开来, 故记作$$\hat{x}^{(1)}(t+1)$$.

**6.** 对函数表达式$$\hat{x}^{(1)}(t+1)$$以及$$\hat{x}^{(1)}(t)$$进行离散, 并将二者做差以便还原$$x^{(0)}$$原序列, 得到近似数据序列$$\hat{x}^{(0)}(t+1)$$如下:
$$!\hat{x}^{(0)}(t+1)=\hat{x}^{(1)}(t+1)-\hat{x}^{(1)}(t)$$

**7.** 最后对建立的灰色模型进行检验, 大概步骤如下:

**7.1** 计算$$x^{(0)}$$和$$\hat{x}^{(0)}(t)$$之间的残差$$e^{(0)}(t)$$和相对误差$$q(x)$$:
$$!e^{(0)}(t)=x^{(0)}-\hat{x}^{(1)}(t)$$
$$!q(x)=\frac{e^{(0)}(t)}{x^{(0)}(t)}$$

**7.2** 求原始数据$$x^{(0)}$$的均差以及方差$$s_1$$
**7.3** 求$$e^{(0)}(t)$$的平均值$$\bar{q}$$以及残差的方差$$s_2$$
**7.4** 计算方差比$$C=\frac{s_2}{s_1}$$
**7.5** 求小误差概率$$P=P{|e(t)|<0.6745s_1}$$
**7.6** 对照灰色模型精度检验表来检查

当然, 在实际的应用过程中, 检验模型精度的方法并不唯一. 可以利用上述方法进行模型的检验, 也可以根据$$q(x)$$的误差百分比并结合预测数据与实际数据之间的测试结果酌情认定模型时候合理.