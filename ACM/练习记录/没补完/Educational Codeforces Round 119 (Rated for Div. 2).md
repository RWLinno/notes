---
title :Educational Codeforces Round 119 (Rated for Div. 2)
date : 2021-12-19
tags : ACM,练习记录
author : Linno


---



题目链接 ：https://codeforces.com/contest/1620

补题进度 ：3/7



# A. Equal or Not Equal

### 题意

给你一个EN串$s$，$如果s_i=E，那么a_i=a_{i+1}，如果s_i=N，那么a_i\neq a_{i+1}(当i=n时，i+1=1)$，问能否构造这样的序列$a$

### 思路

如果刚好有1个N，那么就不成立；除此之外任意情况都成立。

### 代码

```C++
#pragma GCC optimize("Ofast", "inline", "-ffast-math")
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e3+7;
const int mod=1e9+7;

int t,n,s[N];
string str;

signed main(){
	cin>>t;
	while(t--){
		cin>>str;
		int num=0;
		n=str.length();
		for(int i=0;i<n;i++){
			if(str[i]=='N'){
				num++;
			}
		}
		if(num!=1) cout<<"YES\n";//puts("YES");
		else cout<<"NO\n";//puts("NO");
	} 
	return 0;
}
```



# B. Triangles on a Rectangle

### 题面

给你一个$w*h$的矩形，然后再给四条边上面的点，选择同一条边上面的两点以及另外一个点，使得组成的三角形面积最大。

### 思路

只要把四条边的最长段记录下来，然后再乘上离另外一边的距离，最后取最大值即可。

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

int t,w,h,x;
vector<int>v1,v2,v3,v4;
int k1,k2,k3,k4,ans1,ans2;

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>w>>h;
		v1.clear();v2.clear();
		v3.clear();v4.clear();
		cin>>k1;for(int i=1;i<=k1;i++){cin>>x;v1.push_back(x);}
		cin>>k2;for(int i=1;i<=k2;i++){cin>>x;v2.push_back(x);}
		cin>>k3;for(int i=1;i<=k3;i++){cin>>x;v3.push_back(x);}
		cin>>k4;for(int i=1;i<=k4;i++){cin>>x;v4.push_back(x);}
		sort(v1.begin(),v1.end());
		sort(v2.begin(),v2.end());
		sort(v3.begin(),v3.end());
		sort(v4.begin(),v4.end());
		ans1=max(v1[k1-1]-v1[0],v2[k2-1]-v2[0])*h;
		ans2=max(v3[k3-1]-v3[0],v4[k4-1]-v4[0])*w;
		cout<<max(ans1,ans2)<<"\n";
	}
	return 0;
}
```



# C. BA-String

### 题面

给你一个n长的$a*$串以及k和x，每个'*‘都可以用0到k个‘b’来替换。问你字典序第x大的串是什么。

### 思路

实际上我们每一个*区间（用a隔开）都可以看成是容量为len[i]\*base[i]的空间。（其中len是第i个\*区间的长度乘上k，base是第i个\*空间的位权），每个空间的位权就等于下一个空间的容量+1，那么就相当于一个进制转换的问题了。long long 好像爆了，开ull没事。注意要跳过位权大于x的区间。



### 代码

```cpp
#pragma GCC optimize("Ofast", "inline", "-ffast-math")
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
#define ll unsigned long long
using namespace std;
const int N=5e3+7;

int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}

ll t,n,k,x;
string str;
ll base[N],ans[N],len[N],st[N];

signed main(){
	t=read();
	while(t--){

		n=read();
		k=read();
		x=read();
		cin>>str;
		ll l=0,idx=0;
		x-=1;
		memset(base,0,sizeof(base));
		memset(ans,0,sizeof(ans));
		memset(len,0,sizeof(len));
		memset(st,0,sizeof(st)); 
		for(int i=0;i<n;i++){
			if(str[i]=='a'){  //记录每个*区间及其长度 
				if(l){
					len[idx]=l*k; //最多记录l*k个数 
				}
				l=0;
			}else if(str[i]=='*'){
				if(!l) st[++idx]=i;
				l++;
			}
		}
		if(l) len[idx]=l*k; //最多记录l*k个数 
		base[idx]=1;
		int xx=1;
		for(int i=idx-1;i>=1;i--){
			base[i]=base[i+1]*(len[i+1]+1);
			if(base[i]>x){
				xx=i;
				break;
			}
		}
		for(int i=xx;i<=idx;i++){
			int tmp=x/base[i];
			x-=base[i]*tmp;
			ans[i]=tmp;
			if(!x) break;
		}
		idx=1;
		for(int i=0;i<n;i++){
			if(i==st[idx]){
				for(int j=1;j<=ans[idx];j++) cout<<'b';
				idx++;
			}
			if(str[i]=='a') cout<<'a';
		}
		cout<<"\n";
	}
	return 0;
}
```

