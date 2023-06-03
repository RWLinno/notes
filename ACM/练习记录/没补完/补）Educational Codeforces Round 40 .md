---
title : Educational Codeforces Round 40 
date : 2021-9-5
tags : ACM,练习记录
author : Linno
---



题目链接：https://codeforces.com/contest/954

做题进度：6/9



### A-Diagonal Walking

签到题，只要UR或者RU连起来，答案就+1，并且跳一位继续找就行了。O(n)

```C++
#include<bits/stdc++.h>
//#define int long long
using namespace std;
typedef long long ll;
 
int n,ans=0;
string str;
 
signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>n;
	cin>>str;
	for(int i=1;i<n;i++){
		if(str[i]=='U'&&str[i-1]=='R') ans++,i++;		
		else if(str[i]=='R'&&str[i-1]=='U') ans++,i++;
	}
	cout<<n-ans;
	return 0;
}
```



### B-String Typing

数据非常的小，直接枚举就能过。但我还是用了kmp，因为首先想到了前不久A过的题。近似$O(n)$的复杂度。

```C++
#include<bits/stdc++.h>
//#define int long long
using namespace std;
typedef long long ll;
 
int n,nxt[105],j;
string str;
 
signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>n;
	cin>>str;
	nxt[0]=-1,j=0;
	for(int i=1;i<n;i++){
		j=nxt[i];
		while(j&&str[i]!=str[j]) j=nxt[j];
		nxt[i+1]=(str[i]==str[j])?j+1:0;
	}
	//for(int i=1;i<=n;i++) cout<<nxt[i]<<" ";
	int ans=n;
	for(int i=1;i<=n;i++){ //找个位置复制字符串 
		int j=i;
		while(j){
			if(nxt[j]*2==i){ //最长前后缀是分开的
				ans=min(ans,n-nxt[j]+1); 
			}
			j=nxt[j];
		}
	}
	cout<<ans<<endl;
	return 0;
}
```



### C-Matrix Walk

根据题目的话，很容易可以得出一个规律，数列要么是加减1的，要么是加减k的。而且k值只能有一个，它将作为列数。通过遍历一遍数列就得到x和y了，不过还要再检查一遍是否合理。

```c++
#include<bits/stdc++.h>
//#define int long long
using namespace std;
typedef long long ll;
const int inf=0x3f3f3f3f;
const int maxn=2e5+7;
 
inline void read(int &data){
	int x=0,f=1;char ch=getchar();
	while(ch<'0'||ch>'9'){
		if(ch=='-') f=f*-1;
		ch=getchar();
	}
	while(ch>='0'&&ch<='9'){
		x=x*10+ch-'0';
		ch=getchar();
	}
	data=x*f;
}
 
int n,a[maxn],sx,sy;
 
signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	read(n);
	for(int i=1;i<=n;i++) read(a[i]);
	int x=1,y=0,mx=a[1];
	for(int i=2;i<=n;i++){
		mx=max(mx,a[i]);
		if(a[i]==a[i-1]){
			printf("NO\n");
			return 0;
		} 
		if(a[i]==a[i-1]+1){ //是在右方 
			continue;
		}
		if(a[i]==a[i-1]-1){ //是在左方 
			continue; 
		}
	//	cout<<mx<<" "<<y<<endl;
		if(!y){ //如果没设置宽度 
			y=abs(a[i]-a[i-1]);
		}else{
			if(abs(a[i]-a[i-1])!=y){
				printf("NO\n");
				return 0;
			}
		}
	}
	if(!y){
		printf("YES\n%d %d\n",1,mx);
		return 0;
	}
	x=(mx-1)/y+1;//这是他需要的高
	sx=(a[1]-1)/y+1,sy=a[1]-(sx-1)*y; //第一个数所在的行和列 
	for(int i=2;i<=n;i++){ //检查时间 
	//	cout<<sx<<" "<<sy<<endl; 
		if(a[i]==a[i-1]+1) sy++;
		if(a[i]==a[i-1]-1) sy--;
		if(a[i]==a[i-1]+y) sx++;
		if(a[i]==a[i-1]-y) sx--;
		if(sx<1||sx>x||sy<1||sy>y){
			printf("NO\n");
			return 0;
		}
	} 
	printf("YES\n%d %d\n",x,y);
	return 0;
}
```



### D-Fight Against Traffic

数据范围1000，可以用$O(n^2logn)$将全源最短路跑出来。然后对于所有没有连边的<u,v>（<v,u>也更正）判断一下dis<s,u>+dis[v,t]+1和dis[s,t]的关系就可以了。

```c++
#include<bits/stdc++.h>
//#define int long long
using namespace std;
typedef long long ll;
const int inf=0x3f3f3f3f;
const int maxn=1005;
 
int i,j,k;
int n,m,s,t,u,v,ans;
int mp[maxn][maxn];
bool can[maxn][maxn],vis[maxn];
vector<int>G[maxn];
 
struct node{
	int x,y;
};
 
bool operator <(node a,node b){
	return a.y>b.y;
}
 
inline void read(int &data){
	int x=0,f=1;char ch=getchar();
	while(ch<'0'||ch>'9'){
		if(ch=='-') f=f*-1;
		ch=getchar();
	}
	while(ch>='0'&&ch<='9'){
		x=x*10+ch-'0';
		ch=getchar();
	}
	data=x*f;
}
 
void dij(int s){
	priority_queue<node>q;
	while(!q.empty()) q.pop();
	q.push((node){s,0});
	memset(vis,0,sizeof(vis)); 
	memset(mp[s],inf,sizeof mp[s]);
	mp[s][s]=0;
	while(!q.empty()){
		int fro=q.top().x;
		q.pop();
		if(vis[fro]) continue;
		vis[fro]=1;
		for(int i=0;i<G[fro].size();i++){
			int to=G[fro][i];
			if(mp[s][to]>mp[s][fro]+1){
				mp[s][to]=mp[s][fro]+1;
				q.push((node){to,mp[s][to]});
			}
		} 
	}
}
 
signed main(){
	read(n);read(m);read(s);read(t);
	for(int i=1;i<=m;i++){
		read(u);read(v);
		G[u].push_back(v);
		G[v].push_back(u);
		mp[u][v]=mp[v][u]=1; //两点距离 
		can[u][v]=can[v][u]=1;//两点之间有边 
	}
	for(int i=1;i<=n;i++){ //spfa跑全源最短路算了 
		dij(i);
	}
	for(i=1;i<n;i++){
		for(j=i+1;j<=n;j++){
			if(!can[i][j]){ //两点之间原本无边，观察i,j之间能否加
				if(mp[s][t]>mp[s][i]+mp[j][t]+1) continue;
				if(mp[s][t]>mp[s][j]+mp[i][t]+1) continue;
				ans++;
			}
		}
	}
	printf("%d\n",ans);
	return 0;
}
```



### E-Water Taps

$要令\frac{\sum_{i=1}^nx_it_i}{\sum_{i=1}^nx_i}=T,令b_i=(t_i-T),则转化为\frac{\sum_{i=1}^nx_iT}{\sum_{i=1}^nx_i}=0,\\这样化是因为t越靠近T，就越值得取。显然可以用贪心来做$

可以将数划分为正负两个数列，并从小到大排序。最终是令正负数和相等。那么可以遍历总和小的一方，每次加$x_i$再最后加上剩下的$x_k$即可。

```C++
#include<bits/stdc++.h>
#define int long long
using namespace std;
const int maxn=200007;
 
struct X{
	long double a,t;
};
 
vector<X>z,f;
 
bool cmp(X x,X y){
	return x.t<y.t;
}
 
long double sum1,sum2,ans=0;
int n;
int T,a[maxn],t[maxn],tot1,tot2;
 
signed main(){
	cin>>n>>T;
	for(int i=1;i<=n;i++) cin>>a[i];
	for(int i=1;i<=n;i++){
		cin>>t[i];
		if(t[i]>=T){
			z.push_back((X){a[i],t[i]-T});
			tot1+=a[i];
			sum1+=a[i]*(t[i]-T);
		}
		if(t[i]<T){
			f.push_back((X){a[i],T-t[i]});
			tot2+=a[i];
			sum2+=a[i]*(T-t[i]);
		}
	}
	sort(z.begin(),z.end(),cmp);
	sort(f.begin(),f.end(),cmp); 
	long double ans=0;
	if(sum1>=sum2){
		ans=tot2;
		for(int i=0;i<z.size();i++){
			if(z[i].a*z[i].t<=sum2){
				sum2-=1.0*z[i].a*z[i].t;
				ans+=z[i].a;
			}else{
				ans+=sum2/z[i].t;
				break;
			}
		}
	}else{
		ans=tot1;
		for(int i=0;i<f.size();i++){
			if(f[i].a*f[i].t<=sum1){
				sum1-=1.0*f[i].a*f[i].t;
				ans+=f[i].a;
			}else{
				ans+=sum1/f[i].t;
				break;
			}
		}
	}
	printf("%.7Lf\n",ans);
	return 0;
}
```



### G-Castle Defence

从数据范围和问的形式可很容易想到二分。

因为是对区间修改，所以要用到差分。

这样的话我们就得出了最开始每个城堡的防御值。

check的时候对于每个防御值xi不够城堡i，[i,i+2*r+1]的城堡都加一个值，

如果都能大过mid就说明答案可行。

```c++
#include<bits/stdc++.h>
#define int long long
using namespace std;
const int maxn=5e5+7;

inline void read(int &data){
	int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	data=x*f;
}

int n,r,k,x,sum[maxn];
int a[maxn],b[maxn];

bool check(int x){
	int lf=k,need,now;
	//memset(a,0,sizeof(a));
	memcpy(b,sum,sizeof(b));
	//a[0]=b[0];
	now=b[0];
	for(int i=1;i<=n;i++){
		now=now+b[i];
		if(now<x){
			need=x-now;
			lf-=need;
			if(lf<0){
				return false;
			} //不够用呀~~
			b[i+1]+=need;
			b[min(i+2*r+1,n+1)]-=need; 
		}
	}
	return true;
}

signed main(){
	read(n);read(r);read(k);
	for(int i=1;i<=n;i++){
		read(x);
		sum[max(0LL,i-r)]+=x; 
		sum[min(n+1,i+r+1)]-=x;
	}
	int l=0,r=2e18,mid;
	while(r-l>1){
		mid=(l+r)/2;
		//cout<<mid<<" "<<check(mid)<<endl;
		if(check(mid)) l=mid;
		else r=mid;
	}
	printf("%lld\n",l);
	return 0;
}
```

