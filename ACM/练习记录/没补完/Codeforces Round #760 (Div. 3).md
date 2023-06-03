---
title :Codeforces Round #706 (Div. 2)
date : 2021-10-9
tags : ACM,练习记录
author : Linno

---



题目链接 ：[Codeforces Round #760 (Div. 3)](https://codeforces.com/contest/1618)

补题进度：4/7



小结：E题以为是一个线代题，结果被数据和没模数劝退了。前面出题慢，4题不补了。



# A. Polycarp and Sums of Subsequences

### 题面

给你七个数$b_1,b_2...b_7$，分别是$a_1,a_2,a_3$的七个子序列和。求$a_1,a_2,a_3$。

### 思路

最大那个数是$a_1+a_2+a_3$，最小那个数设为$a_1$，那么暴力找一下$a_2,a_3$即可。

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
int t,b[10];
signed main(){
	cin>>t;
	while(t--){
		int flag=0;
		for(int i=1;i<=7;i++){
			cin>>b[i];
		}
		for(int i=2;i<=5;i++){
			for(int j=2;j<=5;j++){
				if(b[i]+b[j]==b[7]-b[1]){
					cout<<b[1]<<" "<<b[i]<<" "<<b[j]<<"\n";
					flag=1;
					break; 
				}
			}
			if(flag) break;
		}	
	}
	return 0;
}
```



# B. Missing Bigram

### 题面

给你n-2个长度·1为2的ab串，在出现顺序不变的情况下构造一个长度为n的ab串。保证有方案。

### 思路

如果头尾相同则缩减，最终长度肯定是小于等于n的，剩余部分填充即可。

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

int t,n,len;
string str[105];
char ans[100005];

signed main(){
	cin>>t;
	while(t--){
		cin>>n;
		int m=n-2;
		for(int i=1;i<=m;i++) cin>>str[i];
		char lst=str[1][1],ans[250];
		ans[0]=str[1][0],ans[1]=str[1][1];
		int len=2;
		for(int j=2;j<=m;j++){
			if(lst!=str[j][0]){
				ans[len++]=str[j][0];
				lst=ans[len++]=str[j][1];
			}else lst=ans[len++]=str[j][1];
		}
		if(len<=n){
			for(int j=0;j<len;j++) cout<<ans[j]; 
			for(int j=len;j<n;j++) cout<<'a';
			cout<<"\n";
		}
	}
	return 0;
}
```





# C. Paint the Array

### 题面

给你n个数，选一个d，能整除d的数为红色，否则为蓝色，满足使得相邻两个数之间颜色不一样。如果没有合适的d则输出0。

### 思路

我们按位置可以把数分为两组记为v1和v2。这两组数的GCD算出来后，答案肯定在这两个数之中。没开long long wa了一发……

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

int t,n,a[105],d1,d2;
vector<int>v1,v2;

int gcd(int a,int b){return b?gcd(b,a%b):a;}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>n;
		v1.clear();v2.clear();
		for(int i=1;i<=n;i++){
			cin>>a[i];
			if(i&1) v1.push_back(a[i]);
			else v2.push_back(a[i]);
		}
		d1=v1[0],d2=v2[0];
		for(int i=1;i<v1.size();i++){
			d1=gcd(v1[i],d1);
		}
		for(int i=1;i<v2.size();i++){
			d2=gcd(v2[i],d2);
		}
		int flag=d1;
		for(int i=2;i<=n;i++){
			if(a[i-1]%flag==a[i]%flag){
				flag=0;
				break;
			}
		}
		if(!flag){
			flag=d2;
			for(int i=2;i<=n;i++){
				if(a[i-1]%flag==a[i]%flag){
					flag=0;
					break;
				}
			}
		}
		cout<<flag<<"\n"; 
	} 
	return 0;
}
```



# D. Array and Operations

### 题面

给n个数和k，挑k个二元组<x,y>，使得所有$\frac{x}{y}$加上剩余的数的值最小。

### 思路

感觉最大的k个数还有次大的k个数分别作为分母和分子就能贪心地求出答案了，交一发结果对了。

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

int t,n,k,a[105];

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		int ans1=0;
		map<int,int>mp;
		vector<int>v1,v2;
		cin>>n>>k;
		for(int i=1;i<=n;i++){
			cin>>a[i];
			mp[a[i]]++;
		}
		sort(a+1,a+1+n);
		int idx=n;
		for(int i=1;i<=k;i++){
			v1.push_back(a[idx--]); //后面k个都做分母 
		}
		for(int i=1;i<=k;i++){
			v2.push_back(a[idx--]); 
		}
		for(int i=0;i<k;i++){
			ans1+=v2[i]/v1[i];
		}
		for(int i=1;i<=n-2*k;i++) ans1+=a[i];
		cout<<ans1<<"\n";
	}
	return 0;
}
```

