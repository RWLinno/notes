---
title : Codeforces Round #764 (Div. 3)
date : 2022-2-25
tags : ACM,练习记录
author : Linno

---



题目链接：https://codeforces.com/contest/1624

做题进度：已补完



# A-Plus One on the Subset

### 题意

给定一个长度为$n$的序列$a$,每次随便选某些下标使得他们的元素值$+1$,问多少次操作后序列中的所有元素相等。

### 思路

每次操作都是任意选某些数，操作结束时想必所有数都达到了原来的最大值，那么答案就是最大值-最大值。

### 代码

```cpp
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

int t,n,a[N];

signed main(){
	cin>>t;
	while(t--){
		cin>>n;
		int mx=0,mi=inf;
		for(int i=1;i<=n;i++){
			cin>>a[i];
			mx=max(a[i],mx);
			mi=min(a[i],mi);
		}
		cout<<mx-mi<<"\n";
	}
	return 0;
}
```



#  B-Make AP

### 题意

给定$a,b,c$三个数，问能否执行一次操作将其中一个数变为$k(k>0)$倍，使得$[a,b,c]$形成等差数列。（a,b,c顺序不可变）

### 思路

我们分类讨论改变$a,b,c$，那么由另外两个数可以知道公差，知道公差也就知道改变的值是多少了，我们只需要判断那个值是不是改变的变量的倍数即可。

### 代码

```cpp
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

signed main(){
	int t,a,b,c;
	cin>>t;
	while(t--){
		cin>>a>>b>>c;
		int d1=(b-a),d2=(c-b),d3=(c-a)/2,flag=0;
		int mc=b+d1,ma=b-d2,mb=a+d3;
		if(mc%c==0&&mc/c>0) flag=1;
		if(ma%a==0&&ma/a>0) flag=1;
		if(mb%b==0&&mb/b>0&&(c-a)%2==0) flag=1;
		if(flag) puts("YES");
		else puts("NO"); 
	} 
	return 0;
}
```



# C-Division by Two and Permutation

### 题意

给定一个长度为$n$的序列$a$，每次操作可以将某个元素$a_i$变为$\lfloor\frac{a_i}{2}\rfloor$,问能否通过这些操作使$a$变成一个全排列。

### 思路

数据很小，我们将读入的每个数处理，如果超过$n$或者以及被标记过了，那么就对这个数进行操作，最后检查1……n有没有数未被标记即可。

### 代码

```cpp
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

int t,n,vis[100],a[100];

signed main(){
	cin>>t;
	while(t--){
		cin>>n;
		int flag=0;
		memset(vis,0,sizeof(vis));
		for(int i=1;i<=n;i++){
			cin>>a[i];
			while(a[i]&&(a[i]>n||vis[a[i]])) a[i]/=2;
			vis[a[i]]=1;
		}
		for(int i=1;i<=n;i++) if(!vis[i]) flag=1;
		if(flag) puts("NO");
		else puts("YES");
	}
	return 0;
}
```





# D- Palindromes Coloring

### 题意

给长度为$n$的字符串，每个字符都可以涂成$k$种颜色中的一种，每种颜色至少要涂1个字符，涂成回文串的形式。在所有涂色方案中求每种颜色中回文串长度最小值的最大值。（即所有颜色回文串长度取min，然后所有方案取max）

### 思路

看到这种形式就能想到二分，我们二分的答案要求每种颜色都能组成mid长的字符串。我们记录字符的出现次数，那么可以组成的总的回文串长度就增加$num[i]/2$个，我们只需要判断总长度$sum$是否能分到$k$种颜色即可。

### 代码

```cpp
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

int t,k,n,num[30],sum;
string str;

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>n>>k;
		cin>>str;
		memset(num,0,sizeof(num));
		sum=0;
		for(int i=0;i<str.length();i++) num[str[i]-'a']++;
		for(int i=0;i<26;i++) sum+=num[i]/2;
		int l=1,r=n/k,mid;
		while(l<r){
			mid=(l+r+1)/2;
			if(mid/2*k<=sum) l=mid;
			else r=mid-1;
		}
		cout<<l<<"\n";
	}
	return 0;
}
```



# E. Masha-forgetful

### 题意

给定长度为$m$的$n+1$个串,问最后一个字符串是否能由前$n$个字符串的某位置的字符段组成（字符段的长度至少为2），输出这些字符段的左右位置和来源字符串编号。

### 思路

我们知道任意长的字符段都可以由长度为2的字符段和长度为3的字符段组成。暴力开$map$,记录前$n$个字符串中长度为2和3的字符段的信息即可。然后记忆化搜索匹配一下。

### 代码

```cpp
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

struct node{
	int l,r,k;
}ans[N];

map<string,node>mp; 
int t,n,m,flag;
string str[N];
int vis[N];
void dfs(int idx,int cnt){
	if(flag) return;
	if(idx==m){
		cout<<cnt<<"\n";
		for(int i=1;i<=cnt;i++){
			cout<<ans[i].l<<" "<<ans[i].r<<" "<<ans[i].k<<"\n"; 
		}
		flag=1;
		return;
	}
	string s2=str[n+1].substr(idx,2),s3=str[n+1].substr(idx,3);
	if(!vis[idx+2]&&idx+2<=m&&mp.count(s2)){
		vis[idx+2]=1;
		ans[cnt+1]=mp[s2];
		dfs(idx+2,cnt+1);
	}
	if(!vis[idx+3]&&idx+3<=m&&mp.count(s3)){
		vis[idx+3]=1;
		ans[cnt+1]=mp[s3];
		dfs(idx+3,cnt+1);
	}
}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>n>>m;
		mp.clear();
		memset(vis,0,sizeof(vis));
		flag=0;
		for(int i=1;i<=n+1;i++) cin>>str[i];
		for(int i=1;i<=n;i++){
			for(int j=0;j<m-1;j++) mp[str[i].substr(j,2)]=(node){j+1,j+2,i};
			for(int j=0;j<m-2;j++) mp[str[i].substr(j,3)]=(node){j+1,j+3,i};	
		}
		dfs(0,0);
		if(!flag) cout<<"-1\n";
	}
	return 0;
}
```



# F- Interacdive Problem

### 题意

交互题，问至多10个数字$c_i$，每次会让$x+=c$，并告诉你$\lfloor\frac{x}{n}\rfloor$，去猜数字当前的$x$。

### 思路

这个数字范围很明显就是二分。难点在于每次问完$x$都会变化，注意记录当前$x$的值，每次询问完的余数$y$以及二分判定的商$fg$即可。

### 代码

```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;

int ask(int x){
	cout<<"+ "<<x<<"\n";
	fflush(stdout);
	cin>>x;
	return x;
}

int n; 

signed main(){
	cin>>n;
	int l=1,r=n,res=-1,sum=0; //1~n二分查找答案 
	while(l<=r){
		int nx=((l+r)>>1);
		int y=(sum+nx)%n;
		int c=n-y;
		int fg=(sum+nx+c)/n;
		int q=ask(c);sum+=c;
		if(q>=fg){
			res=nx;
			l=nx+1;
		}else r=nx-1;
	}
	cout<<"! "<<res+sum<<"\n";
	fflush(stdout);
	return 0;
}
```



# G - MinOr Tree

### 题意

给一个无向图，边有边权，问最小生成树当中边权或和的最小值。

### 思路

我们从高位到低位考虑每一位是否需要即可，不需要的意思就是将所有权值当前位为1的边剔除，仍能组成最小生成树。

### 代码

```cpp
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

int t,n,m,u[N],v[N],w[N],fa[N],fw[N];

int find(int x){
	if(fa[x]==x) return x;
	return fa[x]=find(fa[x]);
}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>n>>m;
		for(int i=1;i<=m;i++){
			fw[i]=0;
			cin>>u[i]>>v[i]>>w[i];
		}
		int ans=0;
		for(int j=35;j>=0;j--){
			int num=0,flag=1;
			for(int i=1;i<=n;i++) fa[i]=i;
			for(int i=1;i<=m;i++){
				if(((w[i]>>j)&1)||fw[i]) continue; //这一位有数的边或者前面以及不需要了的边不能取  
				int fu=find(u[i]),fv=find(v[i]);
				if(fu!=fv){
					fa[fu]=fv; //合并 
					num++; 
				}
				if(num==n-1){
					for(int i=1;i<=m;i++) if((w[i]>>j)&1) fw[i]=1;
					flag=0;  //不需要这些边 
					break;
				}
			}
			ans=ans*2+flag;
		}
		cout<<ans<<"\n"; 
	}
	return 0;
}
```



