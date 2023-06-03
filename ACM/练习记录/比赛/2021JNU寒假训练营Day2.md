---
title : 2021JNU寒假训练营Day2
tags : ACM,练习记录
date : 2021-9-19
author : Linno
---



题目链接：https://vjudge.net/contest/417488#overview

考察内容：动态规划

题目都比较简单，只要会写板子就行。



# A-拦截导弹

### 思路



在数据量不大的情况下，用dp[i]表示前i个导弹最多能拦截多少个。

枚举i前面j个数，列出转移式$dp[i]=max(dp[i],dp[j]+1)$就能过了。

### 代码

```C++
#include<bits/stdc++.h>
using namespace std;

int main(){
	int k,a[26]={0},dp[26]={0},ans=0;
	scanf("%d",&k);
	for(int i=1;i<=k;i++){
		scanf("%d",&a[i]);
	}
	for(int i=k;i>=1;i--){
		dp[i]=1;
		for(int j=i+1;j<=k;j++){
			if(a[j]<=a[i]) dp[i]=max(dp[i],dp[j]+1);
		}
		ans=max(ans,dp[i]);
	}
	printf("%d",ans);
	return 0;
}
```



# B-Bone Collector

### 思路

基础0-1背包板子，不会的话自行百度。

### 代码

```C++
#include<bits/stdc++.h>
using namespace std;

int main(){
	int T;
	scanf("%d",&T);
	while(T--){
		int N,M;
		long long buy[1001],sale[1001],dp[1001]={0};
		scanf("%d%d",&N,&M);
		for(int i=1;i<=N;i++) scanf("%lld",&buy[i]);
		for(int i=1;i<=N;i++) scanf("%lld",&sale[i]);
		for(int i=1;i<=N;i++){
			for(int j=M;j>=sale[i];j--){
				dp[j]=max(dp[j],dp[j-sale[i]]+buy[i]);
			}
		}
		printf("%lld\n",dp[M]);
	}
}
```



# C-Super Jumping!Jumping!Jumping!

### 思路

dp[i]表示前i个数的严格上升子序列最大元素和，转移方程式$if(a[j]>a[i]) dp[i]=max(dp[i],dp[j]+a[i])$

我当时写的代码是$O(n^2)$的复杂度，最长严格上升子序列问题可以用二分方法加速到$O(nlogn)$，感兴趣可以去了解。

### 代码

```C++
#include<bits/stdc++.h>
using namespace std;
int main(){
	int n;
	scanf("%d",&n);
	while(n!=0){
		long long a[1001],dp[1001]={0},ans=0;
		for(int i=1;i<=n;i++) scanf("%lld",&a[i]);
		for(int i=n;i>=1;i--){
			dp[i]=a[i];
			for(int j=i+1;j<=n;j++){
				if(a[j]>a[i]) dp[i]=max(dp[i],dp[j]+a[i]);
			}
			ans=max(ans,dp[i]);
		}
		printf("%d\n",ans);
		scanf("%d",&n);
	}
	return 0;
}
```



# D-dxt数列

### 思路

这是一道可以用双指针搞定的题目。

### 代码

```C++
#include<stdio.h> 
int main(){
	int T,n,num=0;
	scanf("%d",&T);
	if(T==0) return 0;
	while(T-num){
		if(num!=0) printf("\n");
		int a[100001],b[100001]={0},mx[100001]={0},cnt[100001];
		for(int i=0;i<n;i++) cnt[i]=0;
		scanf("%d",&n);
		for(int i=0;i<n;i++){scanf("%d",&a[i]);mx[i]=a[i];}
		int start=0,end=0,mxx=a[0];
		for(int i=0;i<n;i++){
			for(int j=i;j<n;j++){
				b[i]+=a[j];
				if(b[i]<0) break;
				if(b[i]>mx[i]){
					mx[i]=b[i];
					cnt[i]=j-i;
				}
			}
			if(mx[i]>mxx){
				start=i;mxx=mx[i];end=i+cnt[i];
			}
		}
		printf("Case %d:\n",num+1);
		printf("%d %d %d\n",mxx,start+1,end+1);
		num++;
	}
	return 0;
} 
```



# E-免费馅饼

### 思路

$f[i][j]$表示从i时刻在j位置开始，最多能得到的馅饼数，那么我们对时间倒序枚举，计算每个位置得到的馅饼就可以了。转移表达式如下：
$$
				f[r][l]=max(f[r][l],f[r+1][l]+s[r][l]);\\
				if(l+1<=10) f[r][l]=max(f[r][l],f[r+1][l+1]+s[r][l]);\\
				if(l-1>=0) f[r][l]=max(f[r][l],f[r+1][l-1]+s[r][l]);
$$


### 代码

```C++
#include<bits/stdc++.h>
using namespace std;

inline void read(int &data){
	int x=0;char ch=getchar();
	while(ch<'0'||ch>'9') ch=getchar();
	while(ch>='0'&&ch<='9'){
		x=x*10+ch-'0';
		ch=getchar();
	}
	data=x;
}

int f[100003][13]={0},s[100003][13]={0};

int main(){
	int n,a,b;
	read(n);
	while(n!=0){
		int mt=0;
		for(int i=0;i<=100001;i++) for(int j=0;j<=10;j++){
			f[i][j]=0;s[i][j]=0;
		}
		for(int i=1;i<=n;i++){
			read(a);read(b);
			f[b][a]++;s[b][a]++;
			if(b>mt) mt=b;
		}
		for(int r=mt;r>=0;r--){
			for(int l=0;l<=10;l++){
				f[r][l]=max(f[r][l],f[r+1][l]+s[r][l]);
				if(l+1<=10) f[r][l]=max(f[r][l],f[r+1][l+1]+s[r][l]);
				if(l-1>=0) f[r][l]=max(f[r][l],f[r+1][l-1]+s[r][l]);
			}
		}
		printf("%d\n",f[0][5]);
		read(n);
	}
	return 0;
}
```



# F-Worm

### 思路

状态转移：$dp[i][j]=dp[i-1][j-1]+dp[i-1][j+1];$

### 代码

```C++
#include<bits/stdc++.h>
using namespace std;
int n,p,m,t;
long long dp[1005][1005];//dp[i][j]为i时刻到达j树的方案数
int main(){
	while(scanf("%d%d%d%d",&n,&p,&m,&t)!=EOF){
		memset(dp,0,sizeof(dp));
		dp[0][p]=1;
		for(int i=1;i<=m;i++){
			for(int j=1;j<=n;j++){
				dp[i][j]=dp[i-1][j-1]+dp[i-1][j+1];
			}
		} 
		printf("%lld\n",dp[m][t]);
	} 
	return 0;
}
```



# G-Fruit

### 思路

这是一个母函数的板子，可以看做是每个物品价值为1，每种物品最多选x~y个，套母函数模板复杂度$O(n^3)$

### 代码

```C++
#include<bits/stdc++.h>
#define int long long
using namespace std;
const int mod=1e9+7;
const int maxn=505;

int a[maxn],b[maxn];
int n,m,x[maxn],y[maxn],ans;
//a[i]是目前多项式中i次项的系数
//b[i]是乘当前多项式后的系数 
signed main(){
//	ios::sync_with_stdio(0);
//	cin.tie(0);cout.tie(0);
//	freopen("f.cpp","r",stdin); 
	while(cin>>n>>m){
		ans=0;
		memset(a,0,sizeof(a));
		memset(b,0,sizeof(b));
		a[0]=1;
		for(int i=1;i<=n;i++){
			cin>>x[i]>>y[i];
		}
		for(int i=1;i<=n;i++){
			for(int j=0;j<=m;j++){
				for(int k=x[i];k+j<=m&&k<=y[i];k++){
					b[k+j]+=a[j];
				}
			}
			for(int j=0;j<=m;j++){
				a[j]=b[j];
				b[j]=0;
			}
		}
		cout<<a[m]<<"\n";
	}
	return 0;
}
```



# H-Tickets

### 思路

递推式比较简单，$dp[i]=min(dp[i-1]+a[i],dp[i-2]+b[i]);$

感觉主要还是考察输出的细节吧。

### 代码

```C++
#include<bits/stdc++.h>
using namespace std;

int n,k,dp[2005],a[2005],b[2005];

signed main(){
	cin>>n;
	while(n--){
		cin>>k;
		for(int i=1;i<=k;i++) cin>>a[i];
		for(int i=2;i<=k;i++) cin>>b[i];
		dp[1]=a[1];
		for(int i=2;i<=k;i++){
			dp[i]=min(dp[i-1]+a[i],dp[i-2]+b[i]);
		}
		int hh=8,mm=0,ss=dp[k];
		mm+=ss/60;ss%=60;
		hh+=mm/60;mm%=60;
		printf("%02d:%02d:%02d ",hh,mm,ss);
		printf((hh<12)?"am\n":"pm\n");
	}
	return 0;
} 
```





# I-Treats for the Cow

### 思路

从外面开始选数，容易想到搜索的做法，但是容易超时。

优化一下思路从里面开始选，逐渐向外扩展到区间[1,n]，用$dp[1][n]$表示最终答案。状态转移式：$dp[i][j]=max(dp[i+1][j]+v[i]*(n-j+i),dp[i][j-1]+v[j]*(n-j+i));$

### 代码

```C++
#include<iostream>
#define int long long
using namespace std;

int n,dp[2005][2005],v[2005],ans=0;
//dp[i][j]表示从i到j的最大总分 

/*会超时的dfs做法 
void dfs(int step,int lf,int rg,int sc){
	if(dp[lf][rg]>=sc) return;
	dp[lf][rg]=sc;
	if(lf+rg==n){
		ans=max(ans,sc);
		return;
	}
	dfs(step+1,lf+1,rg,sc+(1+step)*v[lf+1]);
	dfs(step+1,lf,rg+1,sc+(1+step)*v[n-rg]);
}
*/

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>n;
	for(int i=1;i<=n;i++){
		cin>>v[i];
		dp[i][i]=v[i]*n;
	}
	for(int i=n-1;i>=1;i--){ 
		for(int j=i+1;j<=n;j++){
			//天数为n-j+i,考虑到[i,j]区间选i还是选j
			dp[i][j]=max(dp[i+1][j]+v[i]*(n-j+i),dp[i][j-1]+v[j]*(n-j+i));
		}
	}
	cout<<dp[1][n]<<"\n";
	return 0;
}
```



# J-Big Event in HDU

### 思路

我们知道所有物品加起来的总值sum，可以用母函数处理出能够取到的所有价值（系数可忽略不计），如果ans能够被取到，那么sum-ans也可被取到，那么我们只需要让两者最接近sum/2即可。

### 代码

```C++
#include<bits/stdc++.h>
#define int long long
using namespace std;
const int maxn=300005;

int n,ans;
int sum,v[maxn],num[maxn];
bool a[maxn],b[maxn];
signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>n;
	while(n>=0){
		sum=0;
		memset(a,0,sizeof(a));
		memset(b,0,sizeof(b));
		for(int i=1;i<=n;i++){
			cin>>v[i]>>num[i];
			sum+=v[i]*num[i];
		}
		a[0]=1;ans=sum;
		for(int i=1;i<=n;i++){
			for(int j=0;j<=sum;j++){
				for(int k=0;j+k*v[i]<=sum&&k<=num[i];k++){
					b[j+k*v[i]]|=a[j];
				}
			}
			for(int j=0;j<=sum;j++){
				a[j]=b[j];
				b[j]=0;
			}
		}
		for(int i=sum;i>=(sum+1)/2;i--){
			if(a[i]) ans=i;
		}
		cout<<ans<<" "<<sum-ans<<"\n";
		cin>>n; 
	}
	return 0;
}
```







# K-yxh的体重和身高

### 思路

石子合并最简单的形式，区间DP的复杂度$O(n^3)$可过。

因为有多组输入数据，要注意初始化。

维护一个前缀和用于状态转移。

### 代码

```C++
#include<bits/stdc++.h>
#define int long long
using namespace std;
const int inf=0x3f3f3f3f;
int n,mi[110][110],mx[110][110];//表示i到j的答案 
int a[110],sum[110];

signed main(){
	while(cin>>n){
		sum[0]=0;
		memset(mx,0,sizeof(mx));
		memset(mi,inf,sizeof(mi));
		for(int i=1;i<=n;i++){
			cin>>a[i];
			sum[i]=sum[i-1]+a[i];
			mi[i][i]=0;
		}
		for(int len=2;len<=n;len++){
			for(int i=1;i+len-1<=n;i++){
				int j=i+len-1;
				for(int k=i;k<j;k++){
					mx[i][j]=max(mx[i][j],mx[i][k]+mx[k+1][j]+sum[j]-sum[i-1]);
					mi[i][j]=min(mi[i][j],mi[i][k]+mi[k+1][j]+sum[j]-sum[i-1]);
				}
			} 
		}
		cout<<mi[1][n]<<" "<<mx[1][n]<<"\n";
	}
	return 0;
} 
```



# L-母牛的故事

### 思路

递推签到题，参考斐波那契数列。

### 代码

```C++
#include<bits/stdc++.h> 
using namespace std; 

int main(){
	int n;
	long long f[55];
	f[1]=1;f[2]=2;f[3]=3;f[4]=4;
	for(int i=5;i<=54;i++){
		f[i]=f[i-1]+f[i-3];
	}
	scanf("%d",&n);
	while(n!=0){
		printf("%lld\n",f[n]); 
		scanf("%d",&n);
	} 
	return 0;
}
```

