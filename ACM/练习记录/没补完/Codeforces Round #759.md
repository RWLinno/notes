---
title : Codeforces Round #759 (Div. 2, based on Technocup 2022 Elimination Round 3)
date : 2021-12-13
tags : ACM,练习记录
author : Linno
---



题目链接：https://codeforces.com/contest/1591

做题进度：5/6

总结：做题有点犯困了，还是安心准备期末吧……考完再把题解给补了。



# A-Life of a Flower

### 题目

给定长度为n的01序列，如果连续两个1则答案+5（可重叠），如果1个1答案+1，初始答案为1。

### 思路

直接模拟即可。

### 代码

```cpp
#pragma GCC optimize("Ofast", "inline", "-ffast-math")
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
//#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);
//  freopen("out.cpp","w",stdout);
	int t,n,a[105];
	cin>>t;
	while(t--){
		cin>>n;
		int ans=1,flag=0;
		for(int i=1;i<=n;i++){
			cin>>a[i];
			if(a[i]==1){
				if(a[i-1]==1) ans+=5;
				else ans++; 
			}
			if(!a[i]&&!a[i-1]) flag=1;
		}
		if(flag) cout<<-1<<"\n";
		else cout<<ans<<"\n";
	} 
	return 0;
}
```



# B- Array Eversion

### 题面

给一个序列a，每次操作把小于等于$a_n$的数按顺序放左边，把大于$a_n$的数按顺序放右边，问多少次操作之后序列不再有变化。

### 思路

最终肯定会把最大的数放在最后，而且每次操作都会把离$a_n$最近的比他大的数放在最后，因此我们只要数“$a_n$出发找当前数大的数并替换”这一操作最多能执行多少次即可。

### 代码

```cpp
#pragma GCC optimize("Ofast", "inline", "-ffast-math")
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
//#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}

int t,n,a[N],dp[N];
vector<int>vl,vr;

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t; 
	while(t--){
		int len=0,mx;
		cin>>n;
		for(int i=1;i<=n;i++) cin>>a[i];
		mx=a[n];
		for(int i=n-1;i>=1;i--){
			if(a[i]>mx){
				mx=a[i];
				len++;
			}
		}
		cout<<len<<"\n";
	}
	return 0;
}
```



# C. Minimize Distance

### 题面

数轴上有n件物品，原点有n个背包。每次可以携带k个背包，问把所有物品背回原点的距离之和。（最后背起的物品不用放回原点）

### 思路

分数轴正负两边去跑，每次优先背最远的k个物品，并且记录最大的一次距离，最后一轮不需要背回原点，减去那个最大距离即可。

### 代码

```cpp
#pragma GCC optimize("Ofast", "inline", "-ffast-math")
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
int t,n,k,x[N];
vector<int>v1,v2;
signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);
//  freopen("out.cpp","w",stdout);
	cin>>t;
	while(t--){
		int ans=0,mx=0;
		cin>>n>>k;
		v1.clear();v2.clear();
		for(int i=1;i<=n;i++){
			cin>>x[i];
			if(x[i]<0) v1.push_back(-x[i]);
			else v2.push_back(x[i]);
		}
		sort(v1.begin(),v1.end());
		sort(v2.begin(),v2.end());
		if(v1.size())
		for(int i=v1.size()-1;i>=0;){
			ans+=v1[i]*2;
			mx=max(mx,v1[i]);
			i-=k;
		}
		if(v2.size())
		for(int i=v2.size()-1;i>=0;){
			ans+=v2[i]*2;
			mx=max(mx,v2[i]);
			i-=k;
		}
		ans-=mx;
		cout<<ans<<"\n";
	}
	return 0;
}
```



# D.Yet Another Sorting Problem

### 题面

给一个序列，每次排序操作选择一个三元组$<i,j,k>$，使得$i->j->k->i$，问能否完成排序。

### 思路

我们发现每次排序之后逆序对的数量奇偶性是不变的。因此如果一开始逆序对为奇数，那么最终一定无解。因此我们求个逆序对并且检查有无重复元素即可。（有重复元素可以利用他们把任意元素移到任意位置，所以答案一定是YES）

### 代码

```cpp
#pragma GCC optimize("Ofast", "inline", "-ffast-math")
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
//#define int long long
using namespace std;
const int N=1e6+7;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
int T,n,a[N],vis[N],tmp[N];
int ans,flag;

void merge_sort(int l,int r){
	if(l>=r) return;
	int mid=(l+r)>>1;
	merge_sort(l,mid);
	merge_sort(mid+1,r);
	int i=l,j=mid+1,k=0;
	while(i<=mid&&j<=r){
		if(a[i]<=a[j]) tmp[++k]=a[i++];
		else{
			tmp[++k]=a[j++];
			ans+=(mid-i+1);
		}
	}
	while(i<=mid) tmp[++k]=a[i++];
	while(j<=r) tmp[++k]=a[j++];
	for(i=l,k=1;i<=r;i++,k++) a[i]=tmp[k];
}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>T;
	for(int t=1;t<=T;t++){
		ans=0,flag=0;
		cin>>n;
		for(int i=1;i<=n;i++){
			cin>>a[i];
			if(vis[a[i]]==t) flag=1;
			vis[a[i]]=t;
		}
		if(flag){
			cout<<"YES\n";
			continue;
		}
		merge_sort(1,n);
		if(ans%2==0) cout<<"YES\n";
		else cout<<"NO\n";
	} 
	return 0;
}
```





# F.Non-equal Neighours

### 题面

给你一个序列$a$,并让你构造一个序列$b$,使得
$$
1\le b_i \le a_i且b_i \neq b_{i+1}
$$
求$b$的构造方案数mod 998244353

### 思路

设$f[i]$为前i个数的合法方案取值乘以$(-1)^i$,$fsum$是$f$的前缀和，$k$是以$a[i]$为最小值的边界（单调栈出栈后，第一个小于a[i]的位置），则有转移方程

$f[i]=f[k]-a[i](fsum[i-1-fsum[k-1]])$

答案就是$(-1)^nf[n]$

### 代码

```cpp
#pragma GCC optimize("Ofast", "inline", "-ffast-math")
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
//#define int long long
#define X first
#define Y second
#define mk make_pair
using namespace std;
typedef long long ll;
const int N=2e5+7;
const int mod=998244353;
//ARC115E原题 单调栈+动规+容斥 
int n,a[N],dp[N],sum[N];
pair<int,pair<int,int> >stk[N];
int top;

int dps(int l,int r){return (sum[r]-(l?sum[l-1]:0))%mod;} 

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>n;
	for(int i=1;i<=n;i++) cin>>a[i];
	int now=0;
	dp[0]=sum[0]=1;
	for(int i=1;i<=n;i++){
		while(top&&stk[top-1].X>=a[i]){
			(now-=-1ll*dps(stk[top-1].Y.X-1,stk[top-1].Y.Y-1)*stk[top-1].X%mod)%=mod;
			top--;
		}
		if(top){
			pair<int,pair<int,int> > x=stk[top-1];
			stk[top++]=mk(a[i],mk(x.Y.Y+1,i));
		}else stk[top++]=mk(a[i],mk(1,i));
		dp[i]=(now+=-1ll*dps(stk[top-1].Y.X-1,stk[top-1].Y.Y-1)*stk[top-1].X%mod)%=mod;
		sum[i]=(sum[i-1]+dp[i])%mod;
	}
	cout<< ((n&1?-1:1)*dp[n]+mod)%mod;
	return 0;
}
```



### 参考资料

https://zhuanlan.zhihu.com/p/444714636

https://www.cnblogs.com/ycx-akioi/p/solution-arc115e.html

https://www.luogu.com.cn/paste/e5269c2z

https://blog.csdn.net/Spy_Savior/article/details/121916143
