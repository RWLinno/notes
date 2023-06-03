---
title : 杜教筛
tags : ACM,数学
date : 2021-11-23
author : Linno
---



### 杜教筛

##### 用途

可以以低于线性时间复杂度来求积性函数前缀和。



##### 推导

$$
设积性函数f的前缀和\sum_{i=1}^nf(i)=S(n)\\
再找一个积性函数g，则考虑它们的狄利克雷卷积的前缀和\\
\sum_{i=1}^n(f*g)(i)=\sum_{i=1}^n\sum_{d|i}f(d)g(\frac{i}{d})=\sum_{d=1}^ng(d)\sum_{i=1}^{[\frac{n}{d}]}f(i)=\sum_{d=1}^ng(d)S([\frac{n}{d}])\\
拿到核心式子g(1)S(n)=\sum_{i=1}^n(f*g)(i)-\sum_{i=2}^ng(i)S([\frac{n}{i}])\\
我们找到一个合适的积性函数g，就可以快速算出(f*g)以及g的前缀和，就可以用数论分块去求解。
$$



##### 框架

```C++
ll GetSum(int n) { // 算 f 前缀和的函数
  ll ans = f_g_sum(n); // 算 f * g 的前缀和
  // 以下这个 for 循环是数论分块
  for(ll l = 2, r; l <= n; l = r + 1) { // 注意从 2 开始
    r = (n / (n / l)); 
    ans -= (g_sum(r) - g_sum(l - 1)) * GetSum(n / l);
    // g_sum 是 g 的前缀和
    // 递归 GetSum 求解
  } return ans; 
} //复整体复杂度O(n^(3/4))
```



##### luoguP4213 【模板】杜教筛（Sum）

给定t组n,球$ans_1=\sum_{i=1}^n \varphi(i)以及ans_2=\sum_{i=1}^n\mu(i)$

```C++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=1e6+6;
bool is_pri[N];
int pri[N],cnt=0,mu[N];
ll sum1[N],sum2[N],phi[N];
map<ll,ll>Phi;
map<int,int>Mu;

void init(){
	phi[1]=mu[1]=1;
	for(int i=2;i<N;i++){
		if(!is_pri[i]) pri[++cnt]=i,mu[i]=-1,phi[i]=i-1;
		for(int j=1;j<=cnt&&i*pri[j]<N;j++){
			is_pri[i*pri[j]]=1;
			if(i%pri[j]){
				mu[i*pri[j]]=-mu[i];
				phi[i*pri[j]]=phi[i]*(pri[j]-1);
			}else{
				phi[i*pri[j]]=phi[i]*pri[j];
				break;
			}
		}
	}
	for(int i=1;i<N;i++){
		sum1[i]=sum1[i-1]+mu[i];
		sum2[i]=sum2[i-1]+phi[i];
	}
}

ll Djphi(ll x){
	if(x<N) return sum2[x];
	if(Phi[x]) return Phi[x];
	ll res=(ll)x*(x+1)/2;
	for(ll l=2,r;l<=x;l=r+1){
		r=min(x,x/(x/l));
		res-=(ll)(r-l+1)*Djphi(x/l);
	}
	return Phi[x]=res;
}

int Djmu(ll x){
	if(x<N) return sum1[x];
	if(Mu[x]) return Mu[x];
	int res=1;
	for(ll l=2,r;l<=x;l=r+1){
		r=min(x,(x/(x/l)));
		res-=(ll)(r-l+1)*Djmu(x/l);
	}
	return Mu[x]=res;
}

int t,n;

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	init();
	cin>>t;
	while(t--){
		cin>>n;
		cout<<Djphi(n)<<" "<<Djmu(n)<<"\n";
	}
	return 0;
}
```



### Min25筛



### 洲阁筛







### 参考资料

https://www.cnblogs.com/A2484337545/p/14682184.html