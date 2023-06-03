---
title : Educational Codeforces Round 123 (Rated for Div. 2) 
date : 2022-2-23
tags : ACM,练习记录
author : Linno

---



题目链接 ：https://codeforces.com/contest/1644

补题进度 ：5/6



# A-Doors and Keys

### 题意

给一个由”R“,"G","B"(门),”r“,"g","b"（钥匙）组成的字符串，每个字符只出现一次，从左到右读问能不能打开这三道门。

### 思路

随便写。

### 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
 
int t;
string str;
 
signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>str;
		int flag1=0,flag2=0,flag3=0,flag4=0,flag5=0,flag6=0;
		for(int i=0;i<str.length();i++){
			if(str[i]=='r') flag1=1;
			if(str[i]=='g') flag2=1;
			if(str[i]=='b') flag3=1;
			if(str[i]=='R'&&flag1) flag4=1;
			if(str[i]=='G'&&flag2) flag5=1;
			if(str[i]=='B'&&flag3) flag6=1;
		}
		if(flag4&&flag5&&flag6) puts("YES");
		else puts("NO");
	}
	return 0;
}
```



# B-Anti-Fibonacci Permutation

### 题意

给定n，输出n组[1,n]的全排列$p$使得不存在$p_{i-2}+p_{i-1}\ne p_i$

### 思路

首先从大到小的全排列肯定是满足的，然后把最大的数往前移，其他元素的相对位置不变，因此肯定也是满足的。

### 代码

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
 
int t,n,a[55];
 
signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>n;
		for(int i=n,j=1;i>=1;i--,j++) a[j]=i;
		int idx=n;
		for(int i=1;i<=n;i++,idx--){
			for(int j=1;j<=n;j++) cout<<a[j]<<" ";
			cout<<"\n";
			swap(a[idx],a[idx-1]);
		}
	}
	return 0;
}
```





# C-Increase Subarray Sums

### 题意

给定长度为$n$的序列$a$，以及$x$，$f(k)$表示将序列中$k$个数加上$x(x>0)$后的最大子段和，输出$k\in [0,n]$的$f(k)$



### 思路

看数据范围发现$O(n^2)$可以解决，那么我们考虑处理出序列中每种长度的最大子段和$mx[len]$，对于每一个$f(k)$,我们就是选出$max_{len=1}^n(mx[len]+x*min(k,len))$

### 代码

```cpp
#include<bits/stdc++.h>
#define int long long
#define inf 0x3f3f3f3f
using namespace std;
const int N=5005;

int t,n,x,sum[N],mx[N];

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>n>>x;
		for(int i=1;i<=n;i++) sum[i]=0,mx[i]=-inf; 
		for(int i=1;i<=n;i++){
			cin>>sum[i];
			sum[i]+=sum[i-1]; 
		}
		int ans=0;
		for(int len=1;len<=n;len++){ 
			for(int l=1;l+len-1<=n;l++){
				int r=l+len-1;
				mx[len]=max(mx[len],sum[r]-sum[l-1]); //每种长度的最大子段和 
				ans=max(ans,mx[len]); //这是最开始 
			}
		}
		for(int k=0;k<=n;k++){ //
			for(int len=1;len<=n;len++) ans=max(ans,mx[len]+x*min(k,len));
			cout<<ans<<" ";
		}
		cout<<"\n";
	}
	return 0;
}
```





# D-The Number of Imposters

### 题意

给定$n×m$的格子图，$k$种颜色和$q$次操作，每次操作给一个坐标，将同一行和同一列染成某种颜色，最终染成的图有多少种情况？

### 思路

如果某种操作的影响被之后的涂色覆盖了的话（即前面涂的某行和某列都被再次涂过），那么这个操作对答案将没有贡献，我们只要倒着找有影响答案的格子用快速幂即可得出答案。另外，如果整张图都被涂完之后，后面的涂色都会覆盖掉某种影响，因此要特判跳出。

### 代码

```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
typedef long long ll;
const int N=2e5+7;
const int mod=998244353;

int T,n,m,k,q;
int vx[N],vy[N];

struct X{
	int x,y;
}s[N];

int fpow(int a,int b){
	int res=1;
	while(b){
		if(b&1) res=res*a%mod;
		a=a*a%mod;
		b>>=1;
	}
	return res;
}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>T;
	for(int t=1;t<=T;t++){
		cin>>n>>m>>k>>q;
		for(int i=1;i<=q;i++) cin>>s[i].x>>s[i].y;
		int cnt=0,nx=0,ny=0;
		for(int i=q;i>=1;i--){
			int x=s[i].x,y=s[i].y;
			if(nx==n||ny==m) break;
			if(vx[x]==t&&vy[y]==t) continue;
			if(vx[x]!=t) nx++,vx[x]=t;
			if(vy[y]!=t) ny++,vy[y]=t; 
			cnt++;
		}
		cout<<fpow(k,cnt)<<"\n";
	}
	return 0;
}
```



# E-Expand the Path

### 题意

给一个又‘R’和‘D’组成的字符串指令，一个$n×n$的格子图，一开始机器人在$(1,1)$,当收到'R'时可以向右走任意格，收到‘D’时可向下走任意格，求有多少个格子可能被走到。

### 思路

假设第一次向下是尽可能向下的，最后一次向右是尽可能向右的，这个是路径1；第一次向右是尽可能向右，最后一次向下是尽可能向下的，这个是路径2（当然你将R和D翻转答案也是不变的）。两个路径围成的面积就是答案，但是这样求看起来很麻烦，实际上割补一下就出来了。

### 代码

```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
typedef long long ll;

int t,m;
string str;

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>m;
		cin>>str;
		int n=str.length();
		int sx=1,sy=1,flag=0;
		for(int i=0;i<n;i++){
			if(str[i]=='R') sy++;
			if(str[i]=='D')	sx++; 	
		}
		int ans=(m-sx+1)*(m-sy+1)+n;
		for(int i=1;i<n;i++){
			if(str[i]!=str[i-1]) flag=1;
			if(flag) ans+=m-(str[i]=='R'?sx:sy);
		}
		if(flag) cout<<ans<<"\n";
		else cout<<m<<"\n";
	}
	return 0;
}
```
