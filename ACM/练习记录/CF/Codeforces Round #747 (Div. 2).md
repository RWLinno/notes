---
title :Codeforces Round #747 (Div. 2)
date : 2021-10-9
tags : ACM,练习记录
author : Linno
---



题目链接 ：https://codeforces.com/contest/1594

补题进度 ：已补完



这次做得可谓是相当差了，A题想复杂、C题调一小时重写才过，E题十分钟写完没交上去。总之还是太菜了。



# A-Consecutive Sum Riddle

### 题意

给一个n,求L和R，使得[L,R]中所有整数加起来等于n。

### 思路

对于正数，我们知道n=sum[n]-sum[n-1]，我们要减去n前面那一坨，只需要取负数的同样位置即可，即L=-n+1,R=n。负数同理。

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int maxn=2e5+7;
const int mod=1e9+7;

int t,n,L,R;

signed main(){
	cin>>t;
	while(t--){
		cin>>n;
		if(n>0){
			L=-n+1,R=n;
		}else L=-n+1,R=n;
		cout<<L<<" "<<R<<"\n";
	}
	return 0;
}
```



# B-Special Numbers

### 题意

给出n,k，求第k个【n的幂集子集】之和的大小。

### 思路

考虑二进制，第k大的数其实就是k，普及到其他进制，也不过是把k二进制拆分，k=1010表示第10大的数是$n^3+n^1$

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int maxn=2e5+7;
const int mod=1e9+7;

int fpow(int a,int p){
	int res=1;
	while(p){
		if(p&1) res=res*a%mod;
		a=a*a%mod;
		p>>=1;
	}
	return res;
}

int t,n,k,ans,cnt;

signed main(){
	cin>>t;
	while(t--){
		cin>>n>>k;
		ans=0;cnt=0;
		while(k){
			if(k&1){
				ans+=fpow(n,cnt);
				ans%=mod;
			}
			k>>=1;
			cnt++;
		}
		cout<<ans<<"\n";
	}
	return 0;
}
```





# C-Make Them Equal

### 题意

求多少次操作可以使得n长的字符串全为ch，每次操作可选择[1,n]中的一个数x，所有除不尽x的位置全部变为ch。

### 思路

很考察细节。很容易想到最多两次操作就能完成，对于这种情况我们都可以取pos1=n-1,pos2=n；对于1次操作的情况，x所在的位置字符必为ch，那么前面的元素必除不尽，遍历后面的元素即可。对于0次操作，就是所有位置都为ch。

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int maxn=3e5+7;
const int mod=1e9+7;

int t,n,pos,pos2,vt[maxn];
char ch;
string str;

signed main(){
	cin>>t;
	while(t--){
		cin>>n>>ch;
		cin>>str;
		int flag=0,flag2=0,cnt=0;
		for(int i=0;i<str.length();i++){
			if(str[i]!=ch){
				flag++;
			}else vt[++cnt]=i;
		}
		if(!flag) cout<<0<<"\n";
		else{
			for(int i=1;i<=cnt;i++){
				flag2=vt[i]+1;
				for(int j=flag2+1;j<=n;j++){
					if(str[j-1]==ch) continue;
					if((j)%(flag2)==0){
						flag2=0;
						break;
					}
				}
				if(flag2) break;
			}
			if(flag2){
				cout<<1<<"\n"<<flag2<<"\n";
			}else{
				cout<<2<<"\n"<<n-1<<" "<<n<<"\n";
			}
		}
	}
	return 0;
}
```





# D-The Number of Imposters

### 题意

给定n个点m条边，每条边u指定v为c，已知c为imposter的点必说真话，c为crewmate的点必说假话，求图中最多有多少个imposter。

### 思路

相当于黑白涂色了，边的性质为1则两端不同色，否则两端同色。

双向建边跑二分图，取每个联通分块中个数多的答案加起来。



### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int maxn=2e5+7;
const int maxm=1e6+7;

struct E{
	int nxt,v,f;
}e[maxm];

int head[maxn],cnt=0,ans=0;
int vis[maxn],num[3],flag;

void addedge(int u,int v,int f){
	e[++cnt]=(E){head[u],v,f};
	head[u]=cnt;
}

void dfs(int x,int col){ 
	vis[x]=col;
	num[col]++;
	for(int i=head[x];i;i=e[i].nxt){
		int to=e[i].v,fang=(col==2)?1:2;
		if(e[i].f==1){
			if(vis[to]){
				if(vis[to]!=fang) flag=1;
				continue;
			}else dfs(to,fang);	
		}
		if(e[i].f==2){
			if(vis[to]){
				if(vis[to]!=col) flag=1;
				continue;
			}else dfs(to,col);
		}
	}
}

void init(){
	ans=0;cnt=0;
	memset(vis,0,sizeof(vis));
	memset(head,0,sizeof(head));
	
}

signed main(){
	int t,n,m,u,v;
	string str;
	cin>>t;
	while(t--){
		cin>>n>>m;
		init();
		for(int i=1;i<=m;i++){
			cin>>u>>v;
			cin>>str;
			if(str=="imposter") flag=1;
			else flag=2;
			addedge(u,v,flag);
			addedge(v,u,flag);
		}
		flag=0;
		for(int i=1;i<=n;i++){
			num[1]=0,num[2]=0;
			if(!vis[i]){
				dfs(i,1);
				ans+=max(num[1],num[2]);
			}
		}
		if(flag) cout<<-1<<"\n";
		else cout<<ans<<"\n";
	}
	return 0;
}
```



# E1-Rubik's Cube Coloring (easy version)

### 题意

有n^2-1个节点，每个节点涂上魔方6种颜色中的一种，相邻点颜色必须在魔方中相邻（有4种），求种方案数。

### 思路

第一个节点可以涂6种色，然后对于任意两个相邻的点都只能涂4种色，共$2^n-1$个点，那么答案就是$6*4^{2^n-2}$。

注意对2用快速幂不要取模，不会爆long long。

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int maxn=2e5+7;
const int mod=1e9+7;

int fpow(int a,int p){
	int res=1;
	while(p){
		if(p&1) res=res*a%mod;
		a=a*a%mod;
		p>>=1;
	}
	return res;
}

int k;

signed main(){
	cin>>k;
	int tmp=1;
	for(int i=1;i<=k;i++) tmp<<=1; 
	int ans=6*fpow(4,tmp-2);
	cout<<ans%mod<<endl;
	return 0;
}
```



# E2-Rubik's Cube Coloring (hard version)

### 题意

在上一题的基础上加多一个限定，就是我们固定住第i个节点的颜色，再求方案数。

### 思路

真正用到的点只有2000个，可以建立一棵虚树进行dp。

由于是完全二叉树，祖先的个数也只有$logn$个，可以接受。

**所有隐式节点或者说没被建立的节点，其值不受固定颜色点的干扰**

转移时直接把两个子树乘起来。



### 代码

不是很会写，抄博客的。

```C++
#include<bits/stdc++.h>
#define int long long
using namespace std;
typedef double db;
const int mod=1e9+7;
const db eps=1e-10;
inline int Max(int x,int y){return x>y?x:y;}
inline int Min(int x,int y){return x<y?x:y;}
inline db Max(db x,db y){return x-y>eps?x:y;}
inline db Min(db x,db y){return x-y<eps?x:y;}
inline int Add(int x,int y,int M=mod){return (x+y)%M;}
inline int Mul(int x,int y,int M=mod){return 1ll*x*y%M;}
inline int Dec(int x,int y,int M=mod){return (x-y+M)%M;}
inline int Abs(int x){return x<0?-x:x;}
inline int read(){ //快读 
	int s=0,w=1;
	char ch=getchar();
	while(!isdigit(ch)){
		if(ch=='-')w=-1;
		ch=getchar();
	}
	while(isdigit(ch)){
		s=s*10+ch-'0';
		ch=getchar();
	}
	return s*w;
}
inline void write(int x){ //快速输出 
	if(x<0)putchar('-'),x=-x;
	if(x>9)write(x/10);
	putchar(x%10+'0');
}
const int N=2e5+10;
int k,n;
int F[61];
string s;
typedef long long ll;
map<ll,ll>pa,col,lson,rson,f[4]; //实际上的点编号太大了，用map存 
vector<ll>Node;
inline int qpow(int x,int y){ //快速幂 
	int res=1;
	while(y){
		if(y&1)res=Mul(res,x);
		x=Mul(x,x);y>>=1;
	}
	return res;
}
inline int Getdep(int v){return log2(v)+1;} //获取点的深度 
void Treat(){
	for(auto v:Node){
		if(lson[v]||rson[v])continue;
		int Dep=k-Getdep(v)+1;
		if(col[v])f[col[v]][v]=Mul(1,qpow(4,F[Dep]));
		else for(int i=1;i<=3;++i)f[i][v]=Mul(2,qpow(4,F[Dep]));
	}
}
void DP(ll x){
	if(!lson[x]&&!rson[x])return;
	if(lson[x])DP(lson[x]);
	if(rson[x])DP(rson[x]);
	ll L1=f[1][lson[x]],L2=f[2][lson[x]],L3=f[3][lson[x]],R1=f[1][rson[x]],R2=f[2][rson[x]],R3=f[3][rson[x]];
	if(!lson[x]){L1=L2=L3=Mul(2,qpow(4,F[k-Getdep(x*2)+1]));}
	if(!rson[x]){R1=R2=R3=Mul(2,qpow(4,F[k-Getdep(x*2)+1]));}
	if(!col[x]){
		f[1][x]=Mul(2,Mul(Add(L2,L3),Add(R2,R3)));
		f[2][x]=Mul(2,Mul(Add(L1,L3),Add(R1,R3)));
		f[3][x]=Mul(2,Mul(Add(L2,L1),Add(R2,R1)));
	}
	else{
		if(col[x]==1)f[1][x]=Mul(Add(L2,L3),Add(R2,R3));
		if(col[x]==2)f[2][x]=Mul(Add(L1,L3),Add(R1,R3));
		if(col[x]==3)f[3][x]=Mul(Add(L2,L1),Add(R2,R1));
	}
}
signed main(){
	k=read();
	F[1]=0;
	for(int i=2;i<=60;++i){
		F[i]=Add(F[i-1],1,mod-1);
		F[i]=Mul(F[i],2,mod-1);
	}
	n=read();
	for(int i=1;i<=n;++i){
		int pos=read();
		cin>>s;
		if(s=="orange"||s=="red")col[pos]=1;
		if(s=="white"||s=="yellow")col[pos]=2;
		if(s=="blue"||s=="green")col[pos]=3;
		while(pos){ //返回向上到父亲的一条链
			Node.emplace_back(pos);
			pa[pos]=pos/2;
			if(pos%2==0)lson[pos/2]=pos;
			else rson[pos/2]=pos;
			pos/=2;
		}
	}
	sort(Node.begin(),Node.end()); //排序所有关键节点 
	ll root=1;
	Treat();
	DP(root);
	int res=Add(f[3][root],Add(f[1][root],f[2][root]));
	write(res);putchar('\n');
	return 0;
}
```





# F-Ideal Farm

### 题意

如果每个盒子都有求并且存在一个区间[l,r]使得$\sum_{i=l}^ra[i]=k,其中a[i]表示第i个盒子的球数$，那么我们就说这个方案是合理的。每组数据给定球数s,盒子数n以及k，问是否无论怎么划分都是合理的。

### 思路

首先我们向每个盒子加一个球，那么剩下的球，每隔k个盒子就再放k个，这样可以保证前面的区间都不存在方案使之合理，如果到最后依然有球剩下，那么就说明可以构建出一种不合理的方案，否则无论怎样都是合理的。

注：k>s是不合理的，并且特判s==k是合理的!

### 代码

```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;

int t,s,n,k,flag,tim;

signed main(){
	cin>>t;
	while(t--){
		cin>>s>>n>>k;
		flag=0;
		if(s==k){
			puts("YES");
			continue;
		}
		if(k>s) flag=1;
		else{
			s-=n; //每个pen先放一个
			tim=n/k;
			s-=tim*k;
			if(s>=0) flag=1;
		}
		if(flag) puts("NO");
		else puts("YES"); 
	} 
	return 0;
}
```



