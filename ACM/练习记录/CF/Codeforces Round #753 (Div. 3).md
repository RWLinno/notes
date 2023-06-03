---
title : Codeforces Round #753 (Div. 3)
date : 2021-11-7
tags : ACM,练习记录
author : Linno
---



题目链接：https://codeforces.com/contest/1607

做题进度：已补完



### [A-Linear Keyboard](https://codeforces.com/contest/1607/problem/A)

给你每个字母的位置，问打完一个字符串要经过的总路程。

### 思路

直接模拟求出答案即可。

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;
 
//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
 
int t,ans=0;
map<char,int>mp;
string str1,str2;
 
signed main(){
//	ios::sync_with_stdio(0);//	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);freopen("out.cpp","w",stout);
	cin>>t;
	while(t--){
		cin>>str1>>str2;
		mp.clear();ans=0;
		for(int i=0;i<str1.length();i++){
			mp[str1[i]]=i+1;
		}
		for(int i=1;i<str2.length();i++){
			ans+=abs(mp[str2[i]]-mp[str2[i-1]]);
		}
		cout<<ans<<"\n";
	}
	return 0;
}
```



### [B-Odd Grasshopper](https://codeforces.com/contest/1607/problem/B)

给一个初始位置x和一个轮数n，如果当前位置为奇数则跳右，否则跳左，且第k轮跳k格。问第n轮所在位置。

### 思路

x只影响第一步的方向，只用看出是从0和1出发并且最后答案加上x就行了。关于跳的方式是一个周期为4的函数，公式见代码。

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;
 
//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
int t,x,n,u,ans;
 
signed main(){
//	ios::sync_with_stdio(0);//	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);freopen("out.cpp","w",stout);
	cin>>t;
	while(t--){
		cin>>x>>n;
		u=n%4;
		if(x&1){
			if(u==0){
				ans=x;
			}else if(u==1){
				ans=x+n;
			}else if(u==2){
				ans=x-1;
			}else ans=x-n-1;		
		}else{
			if(u==0){
				ans=x;
			}else if(u==1){
				ans=x-n;
			}else if(u==2){
				ans=x+1;
			}else ans=x+n+1;
		} 	
		cout<<ans<<"\n";
	}
	return 0;
}
```





### [C-Minimum Extraction](https://codeforces.com/contest/1607/problem/C)

每轮可在一个序列中删去最小的数且让其他数减去该值，问每轮中最大的序列最小值是多少。

### 思路

先排序，然后从左往右减即可。类似前缀和的思想。

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;
int t,n,ans,sum[N],a[N]; 
 
signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>n;
		for(int i=1;i<=n;i++) cin>>a[i];
		sort(a+1,a+1+n);
		ans=a[1];
		for(int i=2;i<=n;i++){
			ans=max(ans,a[i]-a[i-1]);
		}
		cout<<ans<<"\n";
	}
	return 0;
}
```





### [D-Blue-Red Permutation](https://codeforces.com/contest/1607/problem/D)

给一个序列，每个数有其颜色，蓝色可减，红色可加。问是否能形成一个1~n的排列。

### 思路

只需要考虑分成红蓝两堆，在排列中让蓝的尽量填大的数，红的尽量填小的数即可。

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;
 
//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
 
int t,flag,n,a[N];
string str;
 
signed main(){
//	ios::sync_with_stdio(0);//	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);freopen("out.cpp","w",stout);
	cin>>t;
	while(t--){
		flag=0;
		priority_queue<int,vector<int>,greater<int> >qg,ql;
		cin>>n;
		for(int i=1;i<=n;i++) cin>>a[i];
		cin>>str;
		for(int i=1;i<=str.length();i++){
			if(str[i-1]=='B') qg.push(a[i]); //这个可以变小，所以是小根堆 
			else ql.push(a[i]); 
		}
		for(int i=1;i<=n;i++){
			if(!qg.empty()&&qg.top()>=i) qg.pop();	
			else if(!ql.empty()&&ql.top()<=i) ql.pop();
			else{
				flag=1;
				break;
			}
		}
		if(flag) puts("NO");
		else puts("YES");
	}
	return 0;
}
```





### [E-Robot on the Board 1](https://codeforces.com/contest/1607/problem/E)

给一个地图和机器人的行走路线，问最远走到哪。

### 思路

把机器人尽量框在n*m的地图内，如果框不住了，那么最后一格就是要输出的目标。

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
int t,n,m,l,r,u,d,sr,sc,i;
string str;

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);freopen("out.cpp","w",stout);
	cin>>t;
	while(t--){
		cin>>n>>m;
		cin>>str;
		l=r=u=d=0,sr=sc=0;
		for(i=0;i<str.length();i++){
			if(str[i]=='L') sc--;
			if(str[i]=='R') sc++;
			if(str[i]=='U') sr++;
			if(str[i]=='D') sr--;
			if(sc<l) l--;
			if(r-l+1>m||u-d+1>n) {l++;break;}
			if(sc>r) r++;
			if(r-l+1>m||u-d+1>n) {r--;break;}
			if(sr>u) u++;
			if(r-l+1>m||u-d+1>n) {u--;break;}
			if(sr<d) d--;
			if(r-l+1>m||u-d+1>n) {d++;break;}
		}
		//cout<<l<<" "<<r<<" "<<u<<" "<<d<<endl;
		cout<<u+1<<" "<<-l+1<<"\n";
	}
	return 0;
}
```





### [F-Robot on the Board 2](https://codeforces.com/contest/1607/problem/F)

随便选一个起点，然后机器人会按照地图上的指示去移动，且每格只能走一次。问从哪里开始走可以走最多的格子。

### 思路

记忆化搜索，将地图上每一块作为起点的情况考虑进去；成环的情况会比较复杂，但也只需要记住搜索序即可减出答案。

### 代码

```C++
#include<bits/stdc++.h>
//#define inf 0x3f3f3f3f
//#define int long long
using namespace std;
const int N=2003;
pair<int,int>p[N*N],tmp; 

int t,n,m,f[N][N],cnt;
bool vis[N][N];
char mp[N][N];


inline int dfs(int x,int y){
	p[++cnt]={x,y};
	if(vis[x][y]||x<1||x>n||y<1||y>m) return f[x][y];
	vis[x][y]=1;
	if(mp[x][y]=='U') f[x][y]=dfs(x-1,y)+1;
	if(mp[x][y]=='D') f[x][y]=dfs(x+1,y)+1;
	if(mp[x][y]=='L') f[x][y]=dfs(x,y-1)+1;
	if(mp[x][y]=='R') f[x][y]=dfs(x,y+1)+1; 
	return f[x][y];
}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>n>>m;
		for(int i=1;i<=n;i++){
			for(int j=1;j<=m;j++){
				cin>>mp[i][j];
			}
		}
		for(int i=1;i<=n;i++){
			for(int j=1;j<=m;j++){
				if(!vis[i][j]){
					cnt=0;
					dfs(i,j);
					tmp=p[cnt];
					for(int k=1;k<cnt;k++){
						if(p[k]==tmp){
							for(int h=k;h<=cnt;h++){
								f[p[h].first][p[h].second]=cnt-k;
							}
						}
					}
				}
			}
		}
		int x=1,y=1;
		for(int i=1;i<=n;i++) for(int j=1;j<=m;j++) if(f[i][j]>f[x][y]) x=i,y=j;
		cout<<x<<" "<<y<<" "<<f[x][y]<<"\n";
		for(int i=1;i<=n;i++){
			for(int j=1;j<=m;j++){
				f[i][j]=0;
				vis[i][j]=0;
			}
		}
	}
	return 0;
}

```





### [G-Banquet Preparations 1](https://codeforces.com/contest/1607/problem/G)

每道菜有x和y，让他们一共减去m，使得所有菜的x之和与y之和的差值最少。

### 思路

贪心让一开始x减得更多，y尽量不减，然后计算差值suma-sumb，如果需要继续平衡的话从b-y大的菜开始逐个转移，最终逐渐达到平衡。

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;
 
//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
 
int t,n,m,ans[N],tmp,t1,t2,a[N],b[N],x[N],y[N];
 
struct X{
	int a,b,x,y,id;
}s[N];
 
bool cmp1(X u,X v){ //删b啊 
	if(u.a==v.a) return u.b<v.b;
	return u.a<v.a;
}
 
bool cmp2(X u,X v){
	if(u.b==v.b) return u.a<v.a;
	return u.b<v.b;
}
 
bool cmp3(X u,X v){
	return u.id<v.id;
}
 
 
signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
 // 	freopen("in.cpp","r",stdin);
//freopen("out.cpp","w",stout);
	cin>>t;
	for(int T=1;T<=t;T++){
		int suma=0,sumb=0;
		cin>>n>>m;
		for(int i=1;i<=n;i++){
			cin>>s[i].a>>s[i].b;
			suma+=s[i].a;sumb+=s[i].b;
			s[i].id=i;s[i].x=0;s[i].y=0;
		}
		sort(s+1,s+1+n,cmp1);
		for(int i=1;i<=n;i++){ //第一次处理先把A全吃了 
			if(s[i].a>=m) s[i].x=m,s[i].y=m-s[i].x;
			else s[i].x=s[i].a,s[i].y=m-s[i].x;
			suma-=s[i].x;sumb-=s[i].y; //处理差值 
		}
		sort(s+1,s+1+n,cmp2);
		for(int i=1;i<=n;i++){
			int tmp=(sumb-suma);
			if(tmp<=0){ //现在还是a多于b 
				break; 
			}else{ //将删A移到删B 
				if(tmp<=2*s[i].x){ //差距比较小，可以一次做完 
					int tt=min(tmp/2,s[i].b-s[i].y); //转移量
					s[i].x-=tt;s[i].y+=tt;
					suma+=tt;sumb-=tt;
				}else{ //全转吃b吧 
					int tt=min(s[i].x,s[i].b-s[i].y); //最多能转多少？ 
					s[i].y+=tt;
					sumb-=tt;
					suma+=tt;
					s[i].x-=tt;
				}
			}
		}
		sort(s+1,s+1+n,cmp3);	
		cout<<abs(suma-sumb)<<"\n";
		for(int i=1;i<=n;i++){
			cout<<s[i].x<<" "<<s[i].y<<"\n";
		}
	}
	return 0;
}
```





### [H-Banquet Preparations 2](https://codeforces.com/contest/1607/problem/H)

每道菜xi,yi可以减去总和为mi的价值，问最终相同的菜最多的情况。

### 思路

考虑如果最终两道菜相同，那么x+y-m一定是相同的。那么我们按每个t=x+y-m分组，并且按x从小到大进行排序，每道菜的x都要减到min(mina,t)的位置，如果不能减到，就说明是不同的菜。

### 代码

```c++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e6+7;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}

int t,n,ans,need[N],mx,mi;

struct X{
	int a,b,m,x,y,id;
}s[N];

vector<X>vt[N];
bool cmp(X aa,X bb){return aa.a<bb.a;}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
//freopen("in.cpp","r",stdin);
//freopen("out.cpp","w",stout);
	cin>>t;
	for(int T=1;T<=t;T++){
		cin>>n;
		ans=0;
		for(int i=1;i<=n;i++){
			cin>>s[i].a>>s[i].b>>s[i].m;
			s[i].x=s[i].y=0;
			s[i].id=i;
			need[i]=s[i].a+s[i].b-s[i].m;
			vt[need[i]].push_back(s[i]);
		}
		for(int i=1;i<=n;i++){
			if(vt[need[i]].size()){
				sort(vt[need[i]].begin(),vt[need[i]].end(),cmp);
				for(int j=0,mi=-1e9;j<vt[need[i]].size();j++){
					X b=vt[need[i]][j];
					if(b.b<need[i]-mi) mi=min(b.a,need[i]),ans++;
					s[b.id].x=b.a-mi;s[b.id].y=b.m-s[b.id].x;
				}
				vt[need[i]].clear();
			}
		}
		cout<<ans<<"\n";
		for(int i=1;i<=n;i++){
			cout<<s[i].x<<" "<<s[i].y<<"\n";
		}
	}
	return 0;
}
```

