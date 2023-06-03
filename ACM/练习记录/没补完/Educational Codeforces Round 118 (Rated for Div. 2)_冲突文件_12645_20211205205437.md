---
title :Educational Codeforces Round 118 (Rated for Div. 2)
date : 2021-12-5
tags : ACM,练习记录
author : Linno
---

题目链接 ：https://codeforces.com/contest/1613

补题进度 ：4/6



# A. Long Comparison

### 题意

给定原数和乘10的次数，比较两组数。

### 思路

因为后面的数比较大，我们先把其中一个数的10抵消掉。之后对小的那个数如果还能乘，就判断他能否乘到大于或等于另外一个数。

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

int t,x1,x2,p1,p2;
 
signed main(){
	cin>>t;
	while(t--){
		int idx=0,flag=0;
		cin>>x1>>p1;
		cin>>x2>>p2;
		int tmp=min(p1,p2);
		p1-=tmp;p2-=tmp;
		while(x1<=x2){
			if(!p1) break;
			p1--;
			x1*=10;
		}
		if(x1<x2) flag=1;
		else{
			for(int i=1;i<=p2;i++){
				x2*=10;
				if(x1<x2){
					flag=1;
					break;
				}
			}
		}
		if(x1==x2) cout<<"=\n";
		else if(flag) cout<<"<\n";
		else cout<<">\n";
	}
	return 0;
}
```



# B-Absent Remainder

### 题面

给定一个序列，输出n/2组x,y使得

$x\neq y，x和y在序列中且x\%y 不在序列中$

### 思路

排序后我们发现，对于最小的数y，用其他数去模都能满足条件，然后随便跑暴力然后跳出就可以了。



### 代码

```cpp
#pragma GCC optimize("Ofast", "inline", "-ffast-math")
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
using namespace std;
const int N=1e6+7;
const int mod=1e9+7;

int read(){	int x=0;char ch=getchar();while(ch<'0'||ch>'9')ch=getchar(); while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x;}
int T,n,a[N],mp[N];

signed main(){
	T=read();
	for(int t=1;t<=T;t++){
		n=read();
		for(int i=1;i<=n;i++) a[i]=read(),mp[a[i]]=t;
		sort(a+1,a+1+n);
		int flag=0,ans=0;
		for(int i=1;i<n;i++){
			for(int j=i+1;j<=n;j++){
				if(mp[a[j]%a[i]]!=t){
					ans++;
					printf("%d %d\n",a[j],a[i]);
					if(ans==n/2){
						flag=1;
						break;
					}
				}
			}
			if(flag==1) break;
		} 
	}
	return 0;
}
```



# C-Poisoned Dagger

### 题面

主角有一把毒刃，可以在$t_i$秒开始持续k秒造成伤害，给定怪物血量h，求最小的k

### 思路

二分一下就可以了，注意累计伤害会爆long long，所以二分过程中用原来的h去减就行了。



### 代码

```cpp
#include<bits/stdc++.h>
#define int long long
#define inf 0x7f7f7f7f
using namespace std;
const int N=105;
int t,n,h,a[N];

bool check(int x){
	int dam=h;
	for(int i=1;i<n;i++){
		if(a[i+1]-a[i]<x) dam-=a[i+1]-a[i];
		else dam-=x;
	}
	dam-=x;
	if(dam>0) return false;
	return true;
}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>n>>h;
		for(int i=1;i<=n;i++) cin>>a[i];
		int l=0,r=1e18+7,mid;
		while(r>l){
			mid=(r+l)/2;
			if(check(mid)) r=mid;
			else l=mid+1;
		}
		cout<<r<<"\n";
	}
	return 0;
}
```



# D. MEX Sequences

有空再补。



# E. Crazy Robot

### 题面

给一张地图，'#'为障碍，'.'为空地，'L'是兔笼，把所有可以将兔子赶回兔笼的位置都标记上'+'

### 思路

从兔笼出发，只要能够被赶回去，那么肯定只有一条路可走。我们只需要记录度数然后拓扑排序就可以了。

### 代码

```CPP
#include<bits/stdc++.h>
#define fi first
#define se second 
using namespace std;
typedef pair<int,int>pii;
const int N=1e6+5;
int t,n,m,sx,sy;
char ch;
vector<char> g[N];
map<pii,bool> vis;
string mp[N];
int xx[]={1,0,-1,0},yy[]={0,1,0,-1};
queue<pii> Q;

bool check(pii fro){ //确保可走的路<=1
	int x = fro.fi, y = fro.se; int num = 0;
	for(int i=0;i<4;++i){
		int nx=x+xx[i],ny=y+yy[i];
		if(nx<1||nx>n||ny<0||ny>=m) continue;
		if(g[nx][ny] == '.') num++;
	}
	return num<=1;
}

void bfs(){ //拓扑
	while(Q.size()) Q.pop();
	Q.push({sx,sy});	
	while(Q.size()){
		auto u=Q.front(); Q.pop();
		int x=u.fi,y=u.se;
		if(vis[{x,y}]) continue;
		vis[{x,y}] = true;
		for(int k=0;k<4;++k){
			int nx=x+xx[k],ny=y+yy[k];
			if(nx<1||nx>n||ny<0||ny>=m||g[nx][ny]=='#') continue;
			if(g[nx][ny] == '.'){
				if(!check({nx,ny})) continue; 
				 g[nx][ny] = '+';
				 Q.push({nx,ny});
			}
		}
	}	
}

void init(){ //初始化
	vis.clear();
	for(int i=1;i<=n;i++) g[i].clear();
}

int main(){
	cin>>t;
	while(t--){
		cin>>n>>m;
		for(int i=1;i<=n;i++){
			for(int j=1;j<=m;j++){
				cin>>ch;
				g[i].push_back(ch);
				if(ch=='L') sx=i,sy=j-1;
			}
		}
		bfs();
		g[sx][sy]='L';
		for(int i=1;i<=n;i++){
			for(int j=0;j<m;j++) cout<<g[i][j];
			puts("");
		}
		init();
	}
	return 0;
}
```



# F. Tree Coloring

有空再补。
