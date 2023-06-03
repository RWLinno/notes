---
title : 2019-2020 ICPC Northwestern European Regional Programming Contest (NWERC 2019)
date : 2021-10-3
tags : 练习记录
Author : Linno
---



题目链接：https://codeforces.com/gym/102500

做题进度：5/11



# C-Canvas Line

### 题意

给出n条布的做右端点，以及p个夹子所在的位置，问能否在恰当的位置增加夹子，使得每个布都有两个夹子固定住并且所用夹子数量最少，如果不能则impossible。

### 思路

impossible的情况就是放在某个布上的夹子太多了。布的不重叠并且有序的，我们枚举n条布，如果两边都需要夹子，那么就夹在中间，如果中间有夹子了，就夹在旁边；如果只有一边需要，就往那边夹就行了。注意记录夹子位置用来特判。

### 代码

```C++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
 
struct X{
	int l,r,need;
}s[1005];
 
bool cmp(X a,X b){
	return a.r<b.r;
}
 
int n,p,cnt=0,x,peg[5005];
map<int,int>mp;
 
signed main(){
	//ios::sync_with_stdio(0);
	//cin.tie(0);cout.tie(0);
	cin>>n;
	for(int i=1;i<=n;i++){
		cin>>s[i].l>>s[i].r;
		s[i].need=2;	
	} 
	sort(s+1,s+1+n,cmp);
	cin>>p;		
	for(int i=1;i<=p;i++){
		cin>>x;
		if(mp[x]) continue;
		mp[x]=1;//已经占用 
		for(int j=1;j<=n;j++){
			if(s[j].l<=x&&x<=s[j].r){
				s[j].need--;
				if(s[j].need<0){
					cout<<"impossible\n";
					return 0;
				}
			}
			else if(s[j].l>x) break;
		}
	}
	
	for(int i=1;i<=n;i++){
		while(s[i].need){
			if(s[i].r>=s[i+1].l&&i+1<=n){ //可以夹住下一个 
				if(s[i+1].need){ //需要夹 
					if(!mp[s[i].r]){ //共用位置没有夹子 
						s[i+1].need--;
						peg[++cnt]=s[i].r;
						mp[s[i].r]=1;
					}else{ //已经被占用了 
						peg[++cnt]=s[i].r-1;
						mp[s[i].r-1]=1;
					}
				}else{
					if(!mp[s[i+1].l-1]){ 
						peg[++cnt]=s[i+1].l-1;
						mp[s[i+1].l-1]=1;
					}else{
						peg[++cnt]=s[i+1].l-2;
						mp[s[i+1].l-2]=1; 
					} 
				}
			}else{
				if(i!=1&&s[i].l<=s[i-1].r){
					if(!mp[s[i-1].r+1]){
						peg[++cnt]=s[i-1].r+1;
						mp[s[i-1].r+1]=1;
					}else peg[++cnt]=s[i-1].r+2;
				}else{
					if(!mp[s[i].l]){
						peg[++cnt]=s[i].l;
						mp[s[i].l]=1;
					}else{
						peg[++cnt]=s[i].l+1;
					}
				}
			}
			s[i].need--;
		}
	}
	cout<<cnt<<"\n";
	for(int i=1;i<=cnt;i++) cout<<peg[i]<<" ";
	return 0;
}
```



# E-Expeditious Cubing

### 题意

给出4个成绩和一个最终成绩，分数限制为1~20。分别判断无论如何都高于最终成绩、无论如何都低于最终成绩以及存在第5个分数干好达到最终成绩的情况，如果是第三种，那么输出那个成绩。

### 思路

非常坑的卡精度题。注意到小数是固定两位的，只要把整数和小数分开读就可以避免这个坑。

### 代码

```C++
#include<bits/stdc++.h>
using namespace std;
 
int a[10],b[10],t[10],aa,bb,tt;
	
signed main(){
	for(int i=1;i<=4;i++){
		scanf("%d.%d",&a[i],&b[i]);
		t[i]=a[i]*100+b[i];
	}
	scanf("%d.%d",&aa,&bb);
	tt=aa*100+bb;
	sort(t+1,t+5);
	if(t[1]+t[2]+t[3]>3*tt){
		cout<<"impossible"<<endl;
	}else if(t[4]+t[2]+t[3]<=3*tt){
		cout<<"infinite"<<endl;
	}else{
		int ans=(tt*3-t[2]-t[3]);
		printf("%d.%02d",ans/100,ans%100);
	}
	return 0;
}
```



# F-Firetrucks Are Red

### 题意

每个人有一串数字集合，拥有相同数字的人可以关联，问能否关联所有人。

### 思路

最小生成树的比较裸的题，并查集可做。

### 代码

```C++
#include<bits/stdc++.h>
//#define int long long
using namespace std;
typedef long long ll;
const int maxn=2e5+7;
 
struct X{
	int x,y;
};
 
int n,x,num,m[maxn],fa[maxn],vis[maxn];
map<int,int>mp;
vector<X>G[maxn];
 
int find(int x){
	if(x==fa[x]) return x;
	return fa[x]=find(fa[x]);
}
 
void unite(int x,int y){
	fa[find(x)]=find(y);
}
 
signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>n;
	for(int i=1;i<=n;i++) fa[i]=i; 
	for(int i=1;i<=n;i++){
		cin>>m[i];
		for(int j=1;j<=m[i];j++){
			cin>>x;
			if(!mp[x]) mp[x]=i;
			else if(find(mp[x])!=find(i)){ //建边,并合并两点 
				G[i].push_back((X){mp[x],x});
				G[mp[x]].push_back((X){i,x});
				unite(i,mp[x]);
				num++; //边数+1
			}
		}
	}
	if(num!=n-1){ //不能形成最小生成树 
		cout<<"impossible"<<endl;
		return 0;
	}
	queue<int>q;
	q.push(1);
	while(!q.empty()){
		int now=q.front();
		q.pop();
		for(int i=0;i<G[now].size();i++){
			int to=G[now][i].x;
			if(vis[to]) continue;
			vis[now]=vis[to]=1;
			cout<<now<<" "<<to<<" "<<G[now][i].y<<"\n";
			q.push(to);
		}
	}
	return 0;
}
```





# G-Gnoll Hypothesis

### 题意

给你n个怪物及他们的出现概率pi，等概率生成k种怪物，并且不生成的怪物，原本的出现概率会加到下一只怪物身上（第n个不出现，则会转移到第一个），问最终所有怪物的平均出现概率

### 思路

考察组合数学，数据量较小，直接对每个怪物枚举前面n-k个，概率求和乘情况数再除总情况即可。转移式：

$p_i=\sum_{j=0}^{n-k}p_{i-j}C_{n-j-1}^{k-1}/C_n^k$

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int maxn=515;
//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}

int n,k;
long double C[maxn][maxn];
long double s[maxn],sum[maxn];

signed main(){
//	ios::sync_with_stdio(0);
//	cin.tie(0);cout.tie(0);
	for(int i=0;i<=505;i++){
		for(int j=0;j<=i;j++){
			if(j==0) C[i][j]=1;
			else C[i][j]=(C[i-1][j-1]+C[i-1][j]);
		}
	}
	cin>>n>>k; 
	for(int i=0;i<n;i++){
		cin>>s[i];
	}
	for(int i=0;i<n;i++){
		for(int j=i;j>=i-(n-k);j--){
			sum[i]+=C[n-(i-j)-1][k-1]/(long double)C[n][k]*s[(j+n)%n];
		}
		printf("%.7Lf ",sum[i]);
	}
	return 0;
}
```



# I-Inverted Deck

### 题意

能不能反转最多一次，使得序列变成单调递增。

### 思路

把原序列和排序过后的序列一比较，观察是否只有一段区间不同就可以了。

### 代码

```C++
#include<bits/stdc++.h>
//#define int long long
using namespace std;
typedef long long ll;
const int maxn=1e6+7;
 
int n,flag;
int v[maxn],b[maxn];
 
signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>n;
	for(int i=1;i<=n;i++){
		cin>>v[i];
		b[i]=v[i];
	}
	int l=1,r=n;
	sort(b+1,b+1+n); //排序 
//	for(int i=1;i<=n;i++) cout<<b[i]<<" ";
	for(int i=n;i>=1;i--){
		if(b[i]==v[i]&&r>1){
			r--;
		}else break;
	}
	for(int i=1;i<=r;i++){
		if(b[i]==v[i]&&l<r){
			l++;
		}else break;
	}
//	cout<<l<<" "<<r<<endl;
	for(int i=0;i<r-l;i++){
		if(b[l+i]!=v[r-i]){
			cout<<"impossible"<<endl;
			return 0;
		}
	}
	cout<<l<<" "<<r<<endl;
	return 0;
}
```



# 参考资料

https://www.cnblogs.com/dyhaohaoxuexi/p/14530957.html#g-gnoll-hypothesis

