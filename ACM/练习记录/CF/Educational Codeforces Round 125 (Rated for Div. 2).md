---
title : Educational Codeforces Round 125 (Rated for Div. 2)
date : 2022-3-23
tags : ACM,练习记录
author : Linno


---



题目链接 ：https://codeforces.com/contest/1657

补题进度 ：已补完



# A-Integer Moves

### 题意

在最大50×50的格子里面，距离为整数的两点可以一步到达，求去到给定坐标的最小步数。

### 思路

对于每个终点，枚举前面的起点并更新到达需要的最小步数，预处理后直接输出每个询问的答案即可。

复杂度$O(n^4)$

### 代码

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
int dp[55][55],t,x,y;

bool check(int x,int y){
	int f=x*x+y*y,sq=sqrt(f);
	return sq*sq==f;
}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	for(int i=0;i<=50;i++) for(int j=0;j<=50;j++) dp[i][j]=inf;
	dp[0][0]=0; 
	for(int i=0;i<=50;i++){
		for(int j=0;j<=50;j++){
			for(int k=i;k<=50;k++){
				for(int l=j;l<=50;l++){ //从<i,j>到<k,l> 
					if(check(k-i,l-j)) dp[k][l]=min(dp[k][l],dp[i][j]+1);
				}
			}
		}
	}
	cin>>t;
	while(t--){
		cin>>x>>y;
		cout<<dp[x][y]<<"\n";			
	}
	return 0;
}
```



# B-XY Sequence

### 题意

构造一个序列$a$,使得序列求和最大。要求每个数不超过B，第一个数为0，后面每个数为前一个数+x或-y。

### 思路

直接贪啊，有得增就增。

复杂度$O(n)$

### 代码

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
int t,n,x,y,B,a[N];

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);
//  freopen("out.cpp","w",stdout);
	cin>>t;
	while(t--){
		int sum=0;
		cin>>n>>B>>x>>y;
		a[0]=0;
		for(int i=1;i<=n;i++){
			a[i]=a[i-1]+x;
			if(a[i]>B) a[i]-=x+y;
			sum+=a[i];
		}
		cout<<sum<<"\n";
	}
	return 0;
}
```





# C- Bracket Sequence Deletion

### 题意

给定长度为n的字符串，由左右括号组成。每次操作需要找到最短的回文串或者是正确的括号序列为前缀并将其删除，直到不能操作为止，求操作次数和最后剩的字符个数。

### 思路

对于左括号开始找的前缀，后面无论是左右括号都可以将其删除；对于右括号开始找的前缀，需要第二个右括号组成回文串，才能将其删除。

复杂度$O(n)$

### 代码

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
int t,n;
string str;

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);
//  freopen("out.cpp","w",stdout);
	cin>>t;
	while(t--){
		cin>>n;
		cin>>str;
		int len=0,cnt=0,flag=0; //分别为操作数和字符串长度 
		for(int i=0;i<str.length();i++){
			len++; 
			if(str[i]=='('){ //接下来找()或者 
				if(!flag) flag=1;//flag是开始查找标记 ,1表示从(开始，找()或者((
				else if(flag==1){ //找到了(( 
					flag=0; //重新开始 
					cnt++; //操作数
					len=0; //重置字符串长度 
				}
			}else{
				if(!flag) flag=2; //2从)开始，找下一个) 
				else if(flag==1){ //找到了() 
					flag=0;cnt++;len=0; 
				}else if(flag==2){
					flag=0;cnt++;len=0; 
				}
			}
		}
		cout<<cnt<<" "<<len<<"\n";
	}
	return 0;
}
```





# D-For Gamers. By Gamers.

### 题意

每一局开始前有C的钱数，每次只能选择一个兵种。要问对于每个boss最少花多少钱才能将其打倒。给定购买每个单位需要的花费，生命值和攻击力，以及每个敌人的伤害和攻击力。在这里不需要回合制，可以理解为双方都是持续输出，规定敌人必须先死才算赢得游戏。

### 思路

一个简单的结论：伤害×血量>敌人的就可以赢。然后因为每次只选一个兵种，所以可以预处理然后直接贪心。那么我们预处理出dp[i]表示花i的钱能打出的最大伤害，那么保证dp为非递减的，所以对每个询问都可以二分出答案。

复杂度$O(mlogC)$

### 代码

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=1e6+7;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}

struct X{
	int c,d,h;
}s[N];

int n,m,C,dp[N],d[N],h[N];
signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);
//  freopen("out.cpp","w",stdout);
	cin>>n>>C;
	for(int i=1;i<=n;i++){
		cin>>s[i].c>>s[i].d>>s[i].h;
		dp[s[i].c]=max(dp[s[i].c],s[i].d*s[i].h);
	}
	for(int i=1;i<=C;i++){
		for(int j=2*i;j<=C;j+=i){
			dp[j]=max(dp[j],dp[i]*(j/i));
		}
	}
	for(int i=1;i<=C;i++) dp[i]=max(dp[i],dp[i-1]);
	cin>>m;
	for(int i=1;i<=m;i++){
		cin>>d[i]>>h[i];
		int val=d[i]*h[i],k=upper_bound(dp,dp+C+1,val)-dp;
		if(k>C) cout<<"-1 ";
		else cout<<k<<" ";
	}
	return 0;
}
```



# E-Star MST

### 题意

给一颗完全图，每条边的权值可以是1~k，问与1号点相连的所有边权之和=最小生成树的权值和的方案数。

### 思路

我们得出结论：设1号点与u,v相连的边权分别为a,b，那么$dis<u,v> \ge max\{a,b\}$。以$dp[i]$表示有i个点时的答案。我们从大到小枚举最大边权v，需要知道多少条边可以取到1到v，加到对答案的贡献。枚举共有$j$个点的情况，已知其中$i$个点的完全图的答案数，那么我们需要求出$i个点和j-i个点之间连边的答案以及j-i点集连边的答案$，已知$i$点集中的边权更大，所以以上连边我们都是可以取1到v的，那么就可以得出转移方程：$dp[j]=\sum dp[i]*C(j,i)*v^{i*(j-i)+(j-i)*(j-i-1)/2}$

复杂度$O(n^2klog)$

### 代码

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=507;
const int mod=998244353;

inline int fpow(int a,int b){   //快速幂 
	int res=1;
	while(b){
		if(b&1) res=res*a%mod;
		a=a*a%mod;
		b>>=1;
	}
	return res;
}

int frac[N],inv[N],dp[N];  //dp[i]表示有n个点时的答案 

inline int C(int n,int m){  //求组合数 
	return frac[n]*inv[m]%mod*inv[n-m]%mod;
}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	int n,k;
	cin>>n>>k;
	n--;  // 
	frac[0]=1; dp[0]=1;
	for(int i=1;i<=n;i++){  //预处理阶乘及其逆元 
		frac[i]=frac[i-1]*i%mod;
	}
	inv[n]=fpow(frac[n],mod-2);
	for(int i=n;i>0;i--) inv[i-1]=inv[i]*i%mod;
	vector<int>vt(n*n+5); //存储边权的幂次 
	for(int v=k;v>=1;v--){   //枚举最大边权 
		vt.clear();
		vt[0]=1;
		for(int i=1;i<=n*n;i++){//我们预处理出图中有i条边时的情况数 
			vt[i]=vt[i-1]*v%mod;
		}
		for(int i=n;i>=0;i--){ // 
			for(int j=i+1;j<=n;j++){ //从j个点之中选中i个点 
				//C(j,i)表示j个点选i个点出来的方案数，然后前面已知i个点的答案,i点之间的边大于v 
				//j个点之间另外有i*(j-i)+C(j-i,2)条边,权值至多到v
				dp[j]+=dp[i]*C(j,i)%mod*vt[i*(j-i)+(j-i)*(j-i-1)/2]%mod;
				dp[j]%=mod;
			}
		}
	}
	cout<<dp[n]<<"\n";
	return 0;
}
```





# F-Words on Tree

### 题意

给一棵树，将q个字符串放到树上u到v的路径上，由每个结点存储字符，可以正着放也可以反着放。问有没有方案可以满足每个结点上的字符不重复。

### 思路

看题目形式可以知道是2-SAT，在树上感觉还是有点难写的，因为菜所以我把jiangly的代码改成放在下面。匹配时把树上路径拉出来查看是否满足每个点只有一个字符，缩点可以将多个满足条件询问的询问合并，下面自己感受一下吧……



### 代码

```cpp
#pragma GCC optimize("Ofast", "inline", "-ffast-math")
#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define int long long
using namespace std;
const int N=1e6+7;
const int mod=1e9+7;

struct Tuple{ //定义查询三元组 
	int u,v;
	string s;
};

struct TwoSat{
	int n;
	vector<vector<int>>e;
	vector<bool>ans;
	TwoSat(int n):n(n),e(2*n),ans(n){}; //初始化 
	void addClause(int u,bool f,int v,bool g){ //2-sat 加边 
		e[2*u+!f].push_back(2*v+g);
		e[2*v+!g].push_back(2*u+f);
	} 
	bool satisfiable(){  //检查答案是否满足
		vector<int>id(2*n,-1),dfn(2*n,-1),low(2*n,-1);
		vector<int>stk;
		int now=0,cnt=0;
		function<void(int)> tarjan=[&](int u){ //tarjan缩点 
			stk.push_back(u);
			dfn[u]=low[u]=now++;
			for(auto v:e[u]){
				if(dfn[v]==-1){
					tarjan(v);
					low[u]=min(low[u],low[v]);
				}else if(id[v]==-1){
					low[u]=min(low[u],dfn[v]);
				}
			}
			if(dfn[u]==low[u]){
				int v;
				do{
					v=stk.back();
					stk.pop_back();
					id[v]=cnt;
				}while(v!=u);
				++cnt;
			}
		};
		for(int i=0;i<2*n;i++) if(dfn[i]==-1) tarjan(i);
		for(int i=0;i<n;i++){
			if(id[2*i]==id[2*i+1]) return false;
			ans[i]=id[2*i]>id[2*i+1];
		}
		return true;
	}
	vector<bool>answer(){return ans;}
};

int n,q;
vector<int>G[N]; 
int dep[N],fa[N];

void dfs(int x){ 
	for(auto to:G[x]){
		if(to==fa[x]) continue;
		dep[to]=dep[x]+1;
		fa[to]=x;
		dfs(to);
	}
} 

vector<int> getpath(int u,int v){ //将树上路径拉出来 
	vector<int>l,r;
	while(u!=v){
		if(dep[u]>dep[v]){
			l.push_back(u);
			u=fa[u];
		}else{
			r.push_back(v);
			v=fa[v];
		}
	}
	l.push_back(u);
	l.insert(l.end(),r.rbegin(),r.rend()); 
	return l;
}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>n>>q;
	for(int i=1,u,v;i<n;i++){
		cin>>u>>v;
		u--;v--;
		G[u].push_back(v);
		G[v].push_back(u);
	}
	fa[0]=-1;
	dfs(0);
	vector<array<char,2>> cand(n,{'a','b'});
	vector<Tuple>res(q);
	for(int i=0;i<q;i++){
		int u,v;
		string s;
		cin>>u>>v>>s;
		u--;v--;
		res[i]=(Tuple){u,v,s};
		vector<int> p=getpath(u,v);
		int l=p.size();
		for(int j=0;j<l;j++){ //两种字符串的状态 
			cand[p[j]][0]=s[j];
			cand[p[j]][1]=s[l-j-1];
		}
	}
	TwoSat ts(n+q); 
	for(int i=0;i<q;i++){
		Tuple fro=res[i];
		vector<int> p=getpath(fro.u,fro.v);
		int l=p.size();
		for(int j=0;j<l;j++){
			for(int x=0;x<2;x++){ //逆否命题加边 
				if(cand[p[j]][x]!=fro.s[j]){
					ts.addClause(p[j],!x,n+i,1);
				}
				if(cand[p[j]][x]!=fro.s[l-j-1]){
					ts.addClause(p[j],!x,n+i,0);
				}
			}
		}
	}
	if(!ts.satisfiable()) cout<<"NO\n";
	else{
		cout<<"YES\n";
	    vector<bool> ans=ts.answer();
		for(int i=0;i<n;i++){
			cout<<cand[i][ans[i]];
		}
		cout<<"\n";
	}
	return 0;
}
```

