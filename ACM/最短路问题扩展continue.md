---
title : 最短路
date : 2021-8-10
tags : ACM,图论,最短路
---



### 求次短路

```C++
int dijkstra(){          //使用dijkstra算法
	dis[1] = 0; //dis存储最短距离
	Edge p, q, r;
	p.to = 1, p.w = 0;
	Q.push(p); //一开始加入源点
	while(!Q.empty()){
		p = Q.top();
		Q.pop();
		int u = p.to; //当前顶点
		if(p.w > dis2[u])continue; //到p距离不能大于（已赋值的）次短路
		for(int i = 0; i < V[u].size(); i++){
			q = V[u][i]; //q是下一个顶点
			int to = q.to, d = q.w + p.w; //两个点的边长
			if(dis[to] > d){ //如果最短路大于d
				swap(dis[to], d); //替换d
				r.to = to, r.w = dis[to];
				Q.push(r); //加入队列
			}
			if(dis[to] < d && dis2[to] > d){
				dis2[to] = d; //实际上d一直是最短距离交换来的
				r.to = to, r.w = d;
				Q.push(r);
			}
		}
	}
	return dis2[n]; //得到次短距离
}
```



### Floyd

枚举中间点，再枚举起点和终点，不断进行松弛操作，最终求出任意两点的最短距离。复杂度为$O(n^3)$，数据范围不超1e3。



### Bellman-Ford

可用于求单源最短路径，可以判断有无负权回路，时间复杂度$O(nm)$

运用于多元最短路中时间复杂度为$O(n^2m)$



### Johnson 全源最短路

$复杂度O(n(m+nlogn))$​​

先对边权进行处理，使得所有边权均非负。对于n个点都跑一边迪杰斯特拉，就能得出任意两点的最短路了。

##### 势能

建立一个超级源点，对每个点都有一条出边，然后以此为源点跑最短路，就能得到每一个点的**势能值**，势能差是固定的，对所有边权加上那个势能差就能变成正边。

$h(v)<=h(u)+w,w+h(u)-h(v)>=0，我们可以得到所有边权的正值$

```C++
//luoguP5905 【模板】Johnson 全源最短路
#include<bits/stdc++.h>
#define debug(x) cout<<"x="<<x<<endl
#define int long long
using namespace std;
const int maxn=3e3+7;
const int inf=1e9;

inline void read(int &data){ //快读优化 
	int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	data=x*f;
}

struct E{
	int next,to,dis; //存边 
}e[10005];

struct node{ 
	int x,y;
};

bool operator < (node a,node b){ //需要小根堆 
	return a.y>b.y; 
}

int n,m,u,v,w;
int dis[maxn],ans,vis[maxn],h[maxn];
int cnt=0,head[maxn];
int ct[maxn];

inline void addedge(int from,int to,int dis){
	e[++cnt].next=head[from];
	e[cnt].to=to;
	e[cnt].dis=dis;
	head[from]=cnt;
}

bool spfa(int s){ //判断负环+求势能 
	queue<int>que;
	for(int i=1;i<=n;i++) h[i]=inf; // 
	memset(vis,0,sizeof(vis));
	h[s]=0;
	que.push(s);
	while(!que.empty()){
		int fro=que.front();
		que.pop();
		vis[fro]=0; //标记为可走 
		for(int i=head[fro];i;i=e[i].next){
			int to=e[i].to,w=e[i].dis;
			if(h[to]>h[fro]+w){ //松弛操作 
				h[to]=h[fro]+w;
				if(!vis[to]){
					vis[to]=1;
					que.push(to);
					ct[to]++; //访问次数+1 
					if(ct[to]==n+1) return false; //如果一个点被访问多次	
				}
			}
		}
	}
	return true;
}

void dijkstra(int s){ //dijkstra堆优化算单源最短路 
	priority_queue<node>q;
	for(int i=1;i<=n;i++) dis[i]=inf;
	memset(vis,0,sizeof(vis));
	dis[s]=0;
	q.push((node){s,0});
	while(!q.empty()){
		int fro=q.top().x;
		q.pop();
		if(vis[fro]) continue;
		vis[fro]=1; //保证每个点只入队一次 
		for(int i=head[fro];i;i=e[i].next){
			int to=e[i].to,w=e[i].dis;
			if(dis[to]>dis[fro]+w){ //松弛 
				dis[to]=dis[fro]+w;
				q.push((node){to,dis[to]});
			}
		}
	}
}

signed main(){
	read(n);read(m);
	for(int i=1;i<=m;i++){
		read(u);read(v);read(w);
		addedge(u,v,w);
	}
	for(int i=1;i<=n;i++) addedge(0,i,0);
	if(!spfa(0)){ //存在负环 
		printf("-1\n"); 
		return 0;
	}
	for(int i=1;i<=n;i++){
		for(int j=head[i];j;j=e[j].next){
			e[j].dis+=h[i]-h[e[j].to]; //加上势能差之后就没有负边了 
		} 
	}
	for(int i=1;i<=n;i++){
		ans=0;
		dijkstra(i); //对每个点跑最短路 
		for(int j=1;j<=n;j++){
			if(dis[j]==inf) ans+=inf*j;  
			else ans+=j*(dis[j]-h[i]+h[j]); 
		}
		printf("%lld\n",ans);
	}
	return 0;
}

```



### 参考资料

https://blog.csdn.net/weixin_43323647/article/details/93420847

https://www.cnblogs.com/mk-oi/p/13604088.html

https://www.luogu.com.cn/problem/solution/P5905