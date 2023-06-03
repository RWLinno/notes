---
title : Codeforces Round #767 (Div. 2)
date : 2022-1-23
tags : ACM,练习记录
author : Linno

---



题目链接：https://codeforces.com/contest/1629

做题进度：4/7



### [A-Download More RAM](https://codeforces.com/contest/1629/problem/A)

给你初始内存容量k和n个扩展包，如果你当前内存大于扩展包的需求ai，那么就可以增加内存bi，每个包只能用一次。问最后你的内存能达到多少？



### 思路

按照每个包的代价排序然后遍历即可。



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

struct X{
	int a,b;
}s[N];

bool cmp(X x,X y){
	return x.a<y.a;
}

signed main(){
	int n,k,t;
	cin>>t;
	while(t--){
		cin>>n>>k;
		for(int i=1;i<=n;i++) cin>>s[i].a;
		for(int i=1;i<=n;i++) cin>>s[i].b;
		sort(s+1,s+1+n,cmp);
		for(int i=1;i<=n;i++){
			if(k>=s[i].a){
				k+=s[i].b;
			}else break;
		}
		cout<<k<<"\n";
	}
	return 0;
}

```



### [B-GCD Arrays](https://codeforces.com/contest/1629/problem/B)

给你L和R，表示你有一个范围在[L,R]所有整数的序列，然后你可以进行k次操作，每次选择两个数进行乘法，并从序列中移除，将结果加入序列中。问你能否用k次操作使得序列中所有数的GCD大于1.

### 思路

所有偶数的GCD不为1，而相邻两个数GCD必为1，因此把所有奇数挑出来肯定是最优的。求出奇数个数k，那么就需要k-1次操作合并所有奇数，再用一次操作化奇为偶，这是一般情况。特殊情况的如果可以合到只剩1个数，那么只需判断这个数是不是1就行了。



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

signed main(){
//	ios::sync_with_stdio(0);
//	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);
//  freopen("out.cpp","w",stdout);
	int t,l,r,k,num;
	cin>>t;
	while(t--){
		cin>>l>>r>>k;
		int flag=0;
		if(r-l==k){ //那么最终只剩1个元素 
			if(l==r&&r==1){ //最终的元素为1
				flag=0; 
			}else flag=1;
		}else{
			if(l&1){ //如果l是奇数 
				num=(r-l)/2+1;
			}else num=(r-l-1)/2+1;
			if(num<=k) flag=1;
		}
		if(flag) puts("YES");
		else puts("NO");
	}
	return 0;
}

```





### [C-Meximum Array](https://codeforces.com/contest/1629/problem/C)

给你一个序列a，你可以进行任意次操作，每次操作可以将a中前k个数的MEX放入序列b中，并删除这些数（k自取），问使得b字典序最大的方案。

### 思路

从前向后枚举，第一次肯定得找到全局的MEX，随后观察后面的序列的MEX最大为多少，逐个加进b，然后替换MEX即可。实现细节较多，见代码。



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

int t,n,a[N],num[N],mex,tmp; 
vector<int>b;
int vis[N],visnum;
signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);
//  freopen("out.cpp","w",stdout);
	cin>>t;
	while(t--){
		cin>>n;
		memset(vis,0,sizeof(vis));
		memset(num,0,sizeof(num));
		memset(a,0,sizeof(a));
		b.clear();
		visnum=0;
		for(int i=1;i<=n;i++){
			cin>>a[i];
			num[a[i]]++;
		}
		for(int j=0;j<=200005;j++) if(!num[j]){
			 tmp=mex=j;
			 break;
		}
		for(int i=1;i<=n;i++){
			num[a[i]]--;
			if(!vis[a[i]]){
				vis[a[i]]=1;
			}
			while(vis[visnum]) visnum++;
			if(!num[a[i]]){
				tmp=min(tmp,a[i]);
			}
			if(visnum==mex){
				b.push_back(mex);
				mex=tmp;
				for(int j=0;j<visnum;j++) vis[j]=0;
				visnum=0;
			}
		}
		if(visnum) b.push_back(0);
		cout<<b.size()<<"\n";
		for(int i=0;i<b.size();i++) cout<<b[i]<<" ";
		cout<<"\n";
	}
	return 0;
}

```





### [D-Peculiar Movie Preferences](https://codeforces.com/contest/1629/problem/D)

### 题意

给你n个长度最多为3的字符串，构成一个序列，问你是否存在方案使得子序列的字符串拼接成回文串。



### 思路

首先对每个字符串计算哈希值。考虑到长度最多为3，我们对于每一个字符串的枚举前缀作为回文中心，然后计算后面字符串所对应的回文部分的哈希值在前面是否被计算过了即可。当然，考虑这组样例“abc ba”,这个时候就变成了后缀是回文中心，你可以把所有字符串reverse之后再算一遍。



### 代码

```cpp
#pragma GCC optimize("Ofast", "inline", "-ffast-math")
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define B 31
#define inf 0x3f3f3f3f
//#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}

int get_hash(string s){
	int res=0;
	for(int i=0;i<s.length();i++) res=res*B+s[i];
//	cout<<s<<" "<<res<<"hs\n";
	return res;
}

int inv(string s){
	int res=0;
	for(int i=s.length()-1;i>=0;i--) res=res*B+s[i];
//	cout<<s<<" "<<res<<"inv\n";
	return res;
}

bool hw(string s){
	int len=s.length();
	for(int i=0;i<=len/2;i++){
		if(s[i]!=s[len-1-i]) return false;
	}
	return true;
}

int t,n;
string str[N];
map<int,int>mp;

signed main(){
	cin>>t;
	while(t--){
		cin>>n;
		mp.clear();
		int flag=0;
		for(int i=1;i<=n;i++){
			cin>>str[i];
			int hs=get_hash(str[i]);
			if(!mp[hs]) mp[hs]=i; //mp记录整个字符哈希值的最前位置 
			string tmp="";
			for(int j=0;j<str[i].length();j++){ //枚举前缀 
				if(hw(tmp)){
					int hsh=inv(str[i].substr(j));
					if(mp[hsh]){
						flag=1;break;
					}
				}
				tmp+=str[i][j];
			}
		}
		for(int i=1;i<=n;i++) reverse(str[i].begin(),str[i].end());
		mp.clear();
		for(int i=n;i>=1;i--){
			int hs=get_hash(str[i]);
			if(!mp[hs]) mp[hs]=n-i+1; //mp记录整个字符哈希值的最前位置 
			string tmp="";
			for(int j=0;j<str[i].length();j++){ //枚举前缀 
				if(hw(tmp)){
					int hsh=inv(str[i].substr(j));
					if(mp[hsh]){
						flag=1;break;
					}
				}
				tmp+=str[i][j];
			}
		}
		if(flag) puts("YES");
		else puts("NO");
	}
	return 0;
}
```


