---
title :Codeforces Round #706 (Div. 2)
date : 2021-10-9
tags : ACM,练习记录
author : Linno
---



题目链接 ：https://codeforces.com/contest/1496

补题进度 ：已补完



总结：前5题都不算思维量很大，只要想得快就能打得很好。第六题大概就来不及写了，还是要多练前面的题。



# A. Split it!

### 题意

给定字符串长度n和k以及字符串，求能否将字符串两边划分出k份对称的子串。

### 思路

我们只要求前k个字符是否和后k个字符相等即可。

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;

signed main(){
	int t,n,k;
	string str;
	cin>>t;
	while(t--){
		cin>>n>>k;
		cin>>str;
		if(k==0) puts("YES");
		else{
			int flag=1;
			for(int i=1;i<=n/2;i++){
				if(str[i-1]!=str[n-i]){
					if(i<=k) flag=0;
					break;
				}
			}
			if(flag&&k*2!=n) puts("YES");
			else puts("NO");
		}
	}
	return 0;
}
```



# B. Max and Mex

### 题意

给定n和k以及集合中n个不同的数，k次操作。每次操作加入(mex+max+1)/2上取整。问最终集合中有多少个不同的数。

### 思路

如果一开始mex小于max，那么每次操作mex和max都不会变化。只需要判断(mex+max+1)/2是否是一个新的数。如果一开始mex大于max，那么k次操作就会增加k个数。注意特判k==0的情况。

### 代码

```C++
#include<bits/stdc++.h>
using namespace std;
const int N=1e5+7;

int T,n,k,a[N],ans;
map<int,int>vis;

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>T;
	for(int t=1;t<=T;t++){
		cin>>n>>k;
		int mx=0,mex,ans=n;
		for(int i=1;i<=n;i++){
			cin>>a[i];
			mx=max(a[i],mx);
			vis[a[i]]=t;
		}
		for(int i=0;i<mx+5;i++){
			if(vis[i]!=t){mex=i;break;}
		}
		if(k==0) ans=n;
		else if(mex<mx){
			if(vis[(mex+mx+1)/2]!=t) ans=n+1;
			else ans=n;
		}else ans+=k;
		cout<<ans<<"\n";
	}
	return 0;
} 
```





# C. Diamond Miner

### 题意

给定n个钻石和n个矿工的坐标，每个矿工和一个钻石连一条边，求最小距离和。

### 思路

贪心得把矿工和钻石都按离原点的距离排个序，然后依次连接（最近的矿工和最近的钻石相连，以此类推），发现是符合样例的。那么把精度调一调然后再优化一下常数就能过了。

### 代码

```C++
#include<bits/stdc++.h>
#define int long long
#define double long double
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;
int t,n,cnt1,cnt2;
int vx[N],vy[N];
double x,y,ans;

bool cmp(double a,double b){
	if(a<0) a=-a;
	if(b<0) b=-b;
	return a<b;
}

signed main(){
	scanf("%d",&t);
	while(t--){
		ans=0;
		cnt1=0,cnt2=0;
		scanf("%d",&n);
		for(int i=1;i<=2*n;i++){
			scanf("%Lf%Lf",&x,&y);
			if(x) vx[++cnt1]=x;
			else vy[++cnt2]=y;
		}
		sort(vx+1,vx+cnt1+1,cmp);
		sort(vy+1,vy+cnt2+1,cmp);
		for(int i=1;i<=n;i++){
			ans+=sqrt(vx[i]*vx[i]+vy[i]*vy[i]);
		}
		printf("%.10Lf\n",ans);
	}
	return 0;
}
```





# D. Let's Go Hiking

### 题意

博弈：给一组序列，双方选定一个最开始的点，然后A只能走到邻近的比他小的数，B只能走到邻近的比他大的数，且不能碰到对方。A先手，不能走则失败。问有多少个点可以让A获胜。

### 思路

①A只能选择山峰，否则B只需要起点贴住A便可获胜。

②山峰两边能走的长度相等且均为偶数步，模拟一下可得。

③尽管满足前面两个条件,B仍可选其他更长的序列来走，因此要判断最长的序列是不是在当前这个峰。

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;

int n,a[N],lf,rg,ans=0,mx=0,len=0,cant;

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>n;
	a[0]=0;
	for(int i=1;i<=n;i++) cin>>a[i];
	for(int i=1;i<=n;i++){
		if(a[i]>a[i-1]&&a[i]>a[i+1]){
			lf=1;rg=1;
			while(i-lf>=1&&a[i-lf+1]>a[i-lf]) lf++;
			while(i+rg<=n&&a[i+rg-1]>a[i+rg]) rg++;
			int t=max(lf,rg);
			if(mx<t){
				mx=t;
				cant=i;
			}else if(mx==t) cant=0;
		}
	}
	for(int i=2;i<n;i++){
		if(a[i]>a[i-1]&&a[i]>a[i+1]){
			lf=1;rg=1;
			while(i-lf>=1&&a[i-lf+1]>a[i-lf]) lf++;
			while(i+rg<=n&&a[i+rg-1]>a[i+rg]) rg++;
			int t=max(lf,rg);
			if(lf%2&&lf==rg){
				if(t>mx&&i!=cant) ans++;
				else if(i==cant) ans++;
			}
		}
	}
	cout<<ans<<"\n";
	return 0;
}
```



# E. Garden of the Sun

### 题意

给定一个.和X组成的地图，**保证一开始两个X不相邻（八方向）**，可以把任意.改成X，要求最后X连成一棵树（无回路），并输出最终的地图。

### 思路

最开始保证X不相邻是一个很重要的条件，这样我们就可以每隔两行就全涂X，并且把只需要两个X来连接他们即可。由于不相邻的原则，我们只需要找第一列或者是第二列来连接即可。



### 代码

```C++
#include<bits/stdc++.h>
using namespace std;
const int N=505;

int t,n,m;
char mp[N][N];

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>n>>m;
		for(int i=1;i<=n;i++){
			for(int j=1;j<=m;j++){
				cin>>mp[i][j];
			}
		}
		if(n==1){
			for(int i=1;i<=m;i++) cout<<"X";
			cout<<"\n";
			continue;
		}
		for(int i=2;i<=n;i++){
			if(i%3==2) for(int j=1;j<=m;j++) mp[i][j]='X';
			else if(i%3==1&&mp[i][2]!='X'&&mp[i-1][2]!='X') mp[i-1][1]=mp[i][1]='X';
			else if(i%3==1) mp[i][2]=mp[i-1][2]='X';
		}
		if(n%3==1) for(int i=1;i<=m;i++) if(mp[n][i]=='X'&&mp[n][i-1]!='X') mp[n-1][i]='X';
		for(int i=1;i<=n;i++){
			for(int j=1;j<=m;j++) cout<<mp[i][j];
			cout<<"\n";
		}
	}
	return 0;
}

```





# F. BFS Trees

### 题意

给一个无向图，选定任意两点构成BFS-Tree，使得在图中的任意点k，都有dis<x,k>=d<x,k>（树上距离和图上最短距离），求满足该情况的生成树数量(模998244353)

### 思路

不太会写，网上参考的题解。

大概步骤就是：先把d<i,j>处理出来，然后对与每一对i,j枚举中间点k，检验点k是否合法（必须在唯一的路径上面，不能反复经过），合法则记录k能选择的边的方案数，然后将点的方案数累乘起来得到这一对i,j的答案。



### 代码

```C++
#include<bits/stdc++.h>
using namespace std;
const int N=505;

int t,n,m;
char mp[N][N];

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>n>>m;
		for(int i=1;i<=n;i++){
			for(int j=1;j<=m;j++){
				cin>>mp[i][j];
			}
		}
		if(n==1){
			for(int i=1;i<=m;i++) cout<<"X";
			cout<<"\n";
			continue;
		}
		for(int i=2;i<=n;i++){
			if(i%3==2) for(int j=1;j<=m;j++) mp[i][j]='X';
			else if(i%3==1&&mp[i][2]!='X'&&mp[i-1][2]!='X') mp[i-1][1]=mp[i][1]='X';
			else if(i%3==1) mp[i][2]=mp[i-1][2]='X';
		}
		if(n%3==1) for(int i=1;i<=m;i++) if(mp[n][i]=='X'&&mp[n][i-1]!='X') mp[n-1][i]='X';
		for(int i=1;i<=n;i++){
			for(int j=1;j<=m;j++) cout<<mp[i][j];
			cout<<"\n";
		}
	}
	return 0;
}
```
