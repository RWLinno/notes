---
title : 最小费用最大流
date : 2022-5-26
tags : ACM,图论，网络流
author : Linno
---



# 最小费用最大流

最小费用最大流（Minimum Cost Maximum Flow,MCMF）,是一般网络流模型的一种变型。每条边除了容量以外，还存在**单位费用**。在一条边上的费用就是单位时间×流量，现在要求满足最大流的同时，总费用最少的一种情况。



##### [【模板】最小费用最大流](https://www.luogu.com.cn/problem/P3381)

在最大流模型基础上给定每条边的单位费用，求最大流量以及最小费用。

```cpp
#include<bits/stdc++.h>
//#define int long long
#define inf 0x3f3f3f3f
using namespace std;
const int N=5005,M=1e5+7;

int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
void write(int x){if(x>9) write(x/10);putchar(x%10+'0');}

struct E{int v,w,f,nxt;}e[M];
int head[N],cur[N],cnt=1;
inline void addedge(int u,int v,int w,int f){e[++cnt]=(E){v,w,f,head[u]};head[u]=cnt;}

int n,m,s,t,mcost,mflow;
int dis[N],inq[N],pre[N],flow[N];

inline bool spfa(){
	memset(dis,inf,sizeof(dis));
	memset(inq,0,sizeof(inq));
	queue<int>q;
	q.push(s);
	inq[s]=1;dis[s]=0;flow[s]=inf;
	while(q.size()){
		int fro=q.front();
		q.pop();
		inq[fro]=0;
		for(int i=head[fro];i;i=e[i].nxt){
			int to=e[i].v,w=e[i].w,f=e[i].f;
			if(w&&dis[to]>dis[fro]+f){
				dis[to]=dis[fro]+f;
				flow[to]=min(flow[fro],w);
				pre[to]=i;
				if(!inq[to]){
					q.push(to);
					inq[to]=1;
				}
			}
		}
	}
	return dis[t]!=inf;
} 

inline void update(){
	int x=t;
	while(x!=s){
		int i=pre[x];
		e[i].w-=flow[t];
		e[i^1].w+=flow[t];
		x=e[i^1].v;
	}
	mflow+=flow[t];
	mcost+=flow[t]*dis[t];
} 

int EK(){
	int ans=0;
	while(spfa()){
		update();
	}
	return ans;
}

signed main(){
	n=read();m=read();s=read();t=read();
	for(int i=1,u,v,w,f;i<=m;++i){
		u=read();v=read();w=read();f=read();
		addedge(u,v,w,f);
		addedge(v,u,0,-f);
	}
	EK();
	write(mflow);putchar(' ');write(mcost);
	return 0;
}
```





### 参考资料

https://zhuanlan.zhihu.com/p/127046673