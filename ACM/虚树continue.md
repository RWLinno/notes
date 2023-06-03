---
title : 虚树
date : 2021-8-24
tags : ACM,图论
author : Linno
---





### 概念

虚树是一颗虚拟构建的树，这棵树只包含关键点以及关键lca的点，而其他不影响虚树结构的点和边都相当于进行了路径压缩，整棵虚树的规模不会超过关键点的两倍。



### 虚树解决的问题

虚树经常会用在树形dp中，将非关键点构成的链缩减，就可以减少很多需要处理的信息。

利用虚树，可以对于指定多组点集S的询问进行每组O(|S|logn+f(|S|))的回答，其中f(x)指的是对于树上x个点的情况下单组询问这个问题的复杂度。这样，对于多组询问的回答就可以省去每次询问都遍历一棵树的O(n)了。



### 虚树的构建

预处理我们先对整棵树得到dfs序，记为dfn[u]；

我们使用一个栈，从栈顶到栈底的元素形成虚树的一颗树链。

得到一些关键点的时候，对那些点按照dfs序来排序，从dfn值小的进行扫描，结合栈中保存的树链信息就可以将这颗虚树构建出来了。

假设我们当前扫到的关键点为u，栈指针为top，栈为stk
$$
1.如果栈为空，或者栈中只有一个元素，那么显然应该stk[++top]=u;\\
2.取lca=LCA(u,stk[top]),如果lca=stk[top]，则说明u点应该接着stk[top]点延长当前的树链，做操作stk[++top]=u;\\
3.如果lca\neq stk[top]，则说明u和stk[top]分属lca的两颗不同的子树，且包含stk[top]的这颗子树应该已经构建完成了，\\我们需要做的是将lca的包含stk[top]子树的那部分退栈，并将这部分建边形成虚树，\\如果lca不在栈（树链中），那么要把lca也加入栈中，保证虚树的结构不出现问题，随后将u加入栈中，以表延长树链。
$$



```C++
//实现逐个将关键点插入形成一颗虚树
void insert(int u){
    if(top <= 1) {stk[++top] = u;return ;}
    int lca = LCA(u,stk[top]);
    if(lca == stk[top]) {stk[++top] = u;return ;}
    while(top > 1 && dfn[lca] <= stk[top-1]) {
        addedge(stk[top-1],stk[top]);
        --top;
    }
    if(lca != stk[top]) stk[++top] = lca;
    stk[++top] = u;
}
```





### 虚树构建？

1.求dfs序。

2.对于每次询问给出的询问结点，按照dfs序排序，同时维护一个栈，表示从根到栈顶结点的链。考虑要新加入一个结点p，当前栈顶x，p和x的祖先为lca:

①x就是lca，则p在x的子树中，p直接加入栈中。

②x不是lca,则p不在x的子树中，弹出x，并将x与栈顶结点连边。

重复上述操作直到栈顶结点的dfs序小于等于lca，如果栈顶结点是lca则直接把x加入栈中，否则先加入lca,再加入x。



### 参考资料

http://www.360doc.com/content/19/0701/11/5315_846019040.shtml

https://blog.csdn.net/weixin_37517391/article/details/82744605

https://www.bilibili.com/video/BV1RT4y1L7Sb

https://blog.csdn.net/weixin_45630722/article/details/107704931

