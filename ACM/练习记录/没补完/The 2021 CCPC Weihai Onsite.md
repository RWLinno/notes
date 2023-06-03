---
title :The 2021 CCPC Weihai Onsite
date : 2021-12-1
tags : ACM,练习记录
author : Linno

---



题目链接 ：https://codeforces.com/gym/103428

补题进度 ：5/13



# A. Goodbye, Ziyin!

### 题意

给一颗树，求能够作为二叉树的根的节点数。

### 思路

签到题。把每个点度数记录下来，对每个结点的度数，小于等于2则满足；大于3的时候答案为0。

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=1e6+7;
const int mod=1e9+7;

vector<int>G[N];
int n,u,v,deg[N],ans=0;

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>n;
	for(int i=1;i<n;i++){
		cin>>u>>v;
		G[u].push_back(v);
		G[v].push_back(u);
		deg[u]++;deg[v]++;
	}
	for(int i=1;i<=n;i++){
		if(deg[i]>3){
			ans=0;
			break;
		}
		if(deg[i]<=2) ans++;
	}
	cout<<ans<<"\n"; 
	return 0;
}
```





# D. Period

### 题意

给定字符串和q个询问，每次询问把一个位置修改成‘#’,求周期的数量。

### 思路

签到题，先预处理出原来的周期，然后对每次询问二分出满足条件的周期即可。

### 代码

```C++
#include<bits/stdc++.h>
using namespace std;
const int maxn=1e6+5;

int pi[maxn],v1[maxn],v2[maxn],n,q,u,minv,t1,t2;
string s;

int main()
{
	ios::sync_with_stdio(false);
	cin.tie(0); cout.tie(0);
	cin>>s>>q; n=s.length();
	for(int i=1;i<n;i++)
	{
		int j=pi[i-1];
		while(j&&s[i]!=s[j])j=pi[j-1];
		if(s[i]==s[j])j++;
		pi[i]=j;
	}
	if(pi[n-1])minv=pi[n-1],v1[++t1]=minv,v2[++t2]=minv;
	while(minv&&pi[minv-1])
	{
		minv=pi[minv-1];
		v1[++t1]=minv;
		v2[++t2]=minv;
	}
	for(int i=1;i<=t1;i++)v1[i]=n-v1[i]+1;
	reverse(v2+1,v2+1+t2); 
	for(int i=1;i<=q;i++)
	{
		cin>>u; 
		int p1=upper_bound(v1+1,v1+1+t1,u)-v1;
		int p2=lower_bound(v2+1,v2+1+t2,u)-v2;
		cout<<min(t1-p1+1,p2-1)<<"\n";
	}
	return 0;
}
```



# G.Desserts

### 题意

有n种糖果，每种ai个，分给k只队伍，要求每个队伍不能重复拿相同的糖果并且要分完。求1到m每个k的方案数。

### 思路

结论很简单，就是把每只队分给每种糖果的组合数的结果累乘。$ans_k=\Pi_{i=1}^nC(k,a_i)$

$O(n*m)$是会超时的，要再注意每种糖果总和不超过1e5这个条件，说明不同的ai只有根号种，相同种的糖果不需要重复计算，用快速幂的形势就可以把复杂度降为$O(m\sqrt{\sum a} logn)$

### 代码

```C++
#include<bits/stdc++.h>
#define int long long 
using namespace std;
const int N=1e5+100;
const int mod=998244353;

int a[N],kinds[N],ans,n,m,mx=0;
int fac[N],inv[N],finv[N];
int vt[N],cnt=0;

int fpow(int a,int b){
	int res=1;
	while(b){
		if(b&1)res=res*a%mod;
		a=a*a%mod;
		b>>=1;
	} 
	return res;
}

void init(){
	memset(kinds,0,sizeof(kinds));
	fac[0]=inv[0]=inv[1]=finv[0]=finv[1]=1ll;
	for(int i=1;i<N;i++)fac[i]=fac[i-1]*i%mod;
	for(int i=2;i<N;i++)inv[i]=(mod-mod/i)*inv[mod%i]%mod;
	for(int i=2;i<N;i++)finv[i]=finv[i-1]*inv[i]%mod;
}

inline int C(int a,int b){
	if(b>a)return 0;
	else return fac[a]*finv[a-b]%mod*finv[b]%mod;
}


signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>n>>m;
	init();
	for(int i=1;i<=n;i++){
		cin>>a[i];
		kinds[a[i]]++;
		mx=max(a[i],mx);
	}
	for(int i=1;i<=mx;i++){
		if(kinds[i]) vt[++cnt]=i;
	}
	for(int k=1;k<=m;k++){
		ans=1ll;
		for(int i=1;i<=cnt;i++){
			ans=ans*fpow(C(k,vt[i]),kinds[vt[i]])%mod;
			ans%=mod;
		}
		cout<<ans<<"\n";
	}
	return 0;
}
```



# H. city safety

### 题意

n个城市，连成一棵树，每个城市的花费为wi。并且有收益表pi。如果修复i城市时，所有已花费城市距离该城市不超过pi，那么可获得pi的收益（取最大的一个）。问最大收益。

### 思路

看了题解，是用最小割来做，暂时还不会这东西。

### 代码

还写不出来，待补。



#  J. Circular Billiard Table

### 题意

在圆形球桌上给一个入射角，问要反射几次才能回到起点。

### 思路

签到题。假设球碰撞了n次，转了k圈回到原点。那么$2n*\frac{a}{b}=360*k$，稍微化简得$n*a=180*k*b$，那么可以构造$k=a,n=180*b$，使得等式必定成立，然后取k和n的gcd即可。

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

int gcd(int a,int b){return b?gcd(b,a%b):a;}

int t,a,b,n,k,d;

signed main(){
	//ios::sync_with_stdio(0);
	//cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>a>>b;
		n=180*b; //碰撞次数 
		k=a; //圈数
		d=gcd(n,k);
		while(d!=1){
			n/=d;
			k/=d;
			d=gcd(n,k);
		}
		cout<<n-1<<"\n";
	}
	return 0;
}
```



# M. 810975

### 题意

给一个n,m,k，构造一个n位的01串，其中有m个1并且最长的连续的1长度为k。

### 思路

多项式容易写炸，考虑用组合数学的方法。设$f(n,m,k)$是长度为n且有m个1的字符串，每段长度不大于k，那么答案转化为$f(n,m,k)-f(n,m,k-1)$，可以用容斥来做。用隔板法我们可以转化为把m个1放到n-m+1个盒子（0是隔板）里。然后这里由于我经验不足直接递归写炸了，借鉴了一个很妙的公式来进一步容斥。

```C++
for(int i=0;i<=p;i++){ //枚举每一段 
    int op=i&1?-1:1;
    res+=op*C(p,i)*C(m-i*(k+1)+p-1,p-1)%mod;
//感觉很妙，p段选i段来装最长的，并且容斥掉了剩下长度大于k的方案 
}
```

因为还不太理解，可能注释得不好，如果有更详细的理解可以告诉我。



### 代码

```C++
#include<bits/stdc++.h>
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=998244353;

int fact[N],inv[N];

void init(){ //预处理 
	fact[0]=inv[0]=inv[1]=1;
	for(int i=1;i<N;i++) fact[i]=fact[i-1]*i%mod;
	for(int i=2;i<N;i++) inv[i]=(mod-mod/i)*inv[mod%i]%mod;
	for(int i=2;i<N;i++) inv[i]=inv[i]*inv[i-1]%mod;
}

int C(int n,int m){ //O(1)求组合数 
	if(m>n||m<0) return 0;
	return fact[n]*inv[m]%mod*inv[n-m]%mod;
}

inline int f(int n,int m,int k){ 
	if(!k){ 
		if(!m) return 1; //没有1 
		else return 0;
	}
	if(k>m) return 0; //不可能存在k比1总数大的情况 
	int res=0,p=n-m+1; //隔板拆成p段 
	for(int i=0;i<=p;i++){ //枚举每一段 
		int op=i&1?-1:1;
		res+=op*C(p,i)*C(m-i*(k+1)+p-1,p-1)%mod;
		//感觉很妙，p段选i段来装最长的，并且容斥掉了剩下长度大于k的方案 
	}
	return (res+mod)%mod;
}

inline int fc(int n,int m,int k){  //容斥 
	return (f(n,m,k)-f(n,m,k-1)+mod)%mod;
}

signed main(){
	int n,m,k;
	cin>>n>>m>>k;
	init();
	if(!k||k>m) cout<<f(n,m,k)<<"\n";
	else cout<<fc(n,m,k)<<"\n"
	return 0;
}
```
