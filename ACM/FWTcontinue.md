---
title : FWT学习笔记
date : 2021-8-15
tags : ACM,多项式
---



### FWT简介

多项式的卷积$C_k=\sum_{i+j=k}A_i*B_j$可以用FFT来解决。

但是我们会遇到下面的问题：
$$
或卷积：C_k=\sum_{i|j=k}A_i*B_j\\
和卷积：C_k=\sum_{i\&j=k}A_i*B_j\\
异或卷积:C_k=\sum_{i\oplus j=k}A_i*B_j\\
$$
这个时候就要用快速沃尔什变化（fast Walsh–Hadamard transform）来解决。

FWT主要就是构造一种可逆的快速的变换F，使得F(C)=F(A)·F(B)，以快速解决某些下标位运算卷积的问题。



##### 或卷积

$$
写成向量的形式：A|B=(\sum_{i|j=0}A_i*B_j,\sum_{i|j=1}A_i*B_j,...)\\
满足交换律和结合律。对于一个多项式A(最高次项为2^n)\\
我们把它分成两部分A_0,A_1,分别表示前2^{n-1}次项和后面的2^{n-1}次项\\
也就是最高位为0与1的两部分。对于或卷积，我们有：\\FWT(A)=
\begin{cases}
(FWT(A_0),FWT(A_0+A_1)),n>0\\
A,n=0
\end{cases}
$$



##### 和卷积

$$

$$

##### 异或卷积



##### IFWT

我们可以在$O(nlogn)$的时间复杂度里面得到$FWT(A\oplus B)$,其中$\oplus$表示一个位运算。得到FWT之后我们需要还原这个数组，也就是IFWT。
$$
or卷积：IFWT(A)=(IFWT(A_0),IFWT(A_1)-IFWT(A_0))\\
and卷积：IFWT(A)=(IFWT(A_0)-IFWT(A_1),IFWT(A_1))\\
xor卷积：IFWT(A)=(\frac{IFWT(A_0)+IFWT(A_1)}{2},\frac{IFWT(A_0)-IFWT(A_1)}{2})
$$




### 参考资料

