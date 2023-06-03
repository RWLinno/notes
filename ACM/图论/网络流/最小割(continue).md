---
title : 最小割
date : 2021-7-28
tags :ACM，图论
---



# 最小割

### 定义

如果沿着剩余容量大于0的边，无法从s到t，那么这个残余网络就被称为割。



### 最大流最小割定理

$f(s,t)_{max}=c(s,t)_{min}$

EK算法和Dinic算法能处理  10^3  到  10^4  的网络规模。

在**有向图**中，通过求出最大流就可以用最大流最小割定理求得最小割。



### 无向图最小割

##### Stoer-Wagner算法

1.min=MAXINT，固定一个顶点p

2.从点p开始扩展到最大生成树，记录最后扩展的顶点和最后扩展的边。

3.计算最后扩展到的顶点的切割值（即与此顶点相连的所有边权和），若比min小则更新min。

4.合并最后扩展的那条边及两个顶点为一个点

5.转到2，合并n-1次后结束，答案即为min

###### 在prim中加入堆优化，总的复杂度为O(n^2logn)



### 最小割树(Gomory-Hu Tree)

##### 最小割树的定义

定义一棵树T为最小割树，如果对于树上的所有边(s,t)，树上去掉(s,t)后产生的两个集合恰好是原图上(s,t)的最小割把原图分成的两个集合，且边(u,v)的权值等于原图上(u,v)的最小割

##### 构造

树上去掉(s,t)后产生的两个集合恰好是原图上(s,t)的最小割把原图分成的两个集合



##### 原理

将网络流的图转化为一棵树，其中原图u到v的最小割转化到树上。

树的一个性质是：删除一条边，树变得不连通。

我们可以任意选两个点s与t，跑最小割（即最大流），然后再连一条从s到t的边

又Dinic算法最后一次bfs相当于求一个最小割，原图就被分为了两部分。

最后分治即可，复杂度为O(mn^3)

u到v的最小割就算树上从u到v的路径。



##### 性质

原图上u,v两点最小割就是最小割树上u到v的路径上权值最小的边。



##### 实现细节

每次求最大流时，都要先恢复开始的网络流（即退流）

插入网络流的边时，要双向插入（没有指明起点）

下标从0开始到n

每一次最大流时，先保存源点和汇点，防止被覆盖



##### [Dahno Dahno](https://ac.nowcoder.com/acm/problem/222807)

将图划分为两个集合，得分就是集合内连边权值和。问最大得分。

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=507;

/*    Stoer_Wagner  O(n^3)  (POJ 2914)
   求无向图的最小割,把图分为两个子图的最小花费
   邻接矩阵建图,w[][]点从0开始
   算法流程:
     1.min=MAXINT，固定一个顶点P
     2.从点P用“类似”prim的s算法扩展出“最大生成树”，记录最后扩展的顶点和最后扩展的边
     3.计算最后扩展到的顶点的切割值（即与此顶点相连的所有边权和），若比min小更新min
     4.合并最后扩展的那条边的两个端点为一个顶点（当然他们的边也要合并，这个好理解吧？）
     5.转到2，合并N-1次后结束
     6.min即为所求，输出min
   prim本身复杂度是O(n^2)，合并n-1次，算法复杂度即为O(n^3)
   如果在prim中加堆优化，复杂度会降为O((n^2)logn)            */

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
//void write(int x){if(x>9) write(x/10);putchar(x%10+'0');}

ll w[N][N],v[N],dis[N];
bool vis[N];
int n,m;

ll Stoer_Wagner(int n){
	ll i,j,res=1e18;
	for(int i=0;i<n;++i) v[i]=i;
	while(n>1){
		int k=1,pre=0;
		for(int i=1;i<n;++i){
			dis[v[i]]=w[v[0]][v[i]];
			if(dis[v[i]]>dis[v[k]]) k=i;
		}
		memset(vis,0,sizeof(vis));
		vis[v[0]]=1;
		for(int i=1;i<n;++i){
			if(i==n-1){
				res=min(res,dis[v[k]]);
				for(int j=0;j<n;++j){
					w[v[pre]][v[j]]+=w[v[j]][v[k]];
					w[v[j]][v[pre]]+=w[v[j]][v[k]];
				}
				v[k]=v[--n];
			}
			vis[v[k]]=1;
			pre=k;
			k=-1;
			for(int j=1;j<n;++j){
				if(!vis[v[j]]){
					dis[v[j]]+=w[v[pre]][v[j]];
					if(k==-1||dis[v[k]]<dis[v[j]]) k=j;
				}
			}
		}
	}
	return res;
}

void Solve(){
	scanf("%d",&n);
	ll sum=0;
	for(int i=0;i<n;++i){
		for(int j=0;j<n;++j){
			scanf("%lld",&w[i][j]);
			sum+=w[i][j];
		}
	}
	printf("%lld\n",sum-Stoer_Wagner(n)*2);
}

signed main(){
//	ios::sync_with_stdio(0);
//	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);
//  freopen("out.cpp","w",stdout);
	int T=1;
//	cin>>T;
//	clock_t start,finish;
//	start=clock();
	while(T--){
		Solve();
	}
//	finish=clock();
//	cerr<<((double)finish-start)/CLOCKS_PER_SEC<<endl;	return 0;
}

```





### 参考资料

https://blog.csdn.net/DDelphine/article/details/77935670

https://oi-wiki.org/graph/stoer-wagner/

https://www.cnblogs.com/zhang-qc/p/6516432.html

https://www.luogu.com.cn/blog/mydcwfy-342891/solution-p4897