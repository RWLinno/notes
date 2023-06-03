---
title : “山大地纬杯”第十二届山东省ICPC大学生程序设计竞赛(正式赛）
date : 2022-5-30
tags : ACM,题解，练习记录
author : Linno

---



# “山大地纬杯”第十二届山东省ICPC大学生程序设计竞赛(正式赛）

题目链接：https://ac.nowcoder.com/acm/contest/34980

补题进度：6/13



### [ A-Seventeen](https://ac.nowcoder.com/acm/contest/34980/A)

如果有4个连续的数可以构造两加两减抵消的情况，因此只要考虑前几项的解即可。

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
//void write(int x){if(x>9) write(x/10);putchar(x%10+'0');}

int n;
string str[55];

void Solve(){
	str[1]="-1";
	str[2]="-1";
	str[3]="-1";
	str[4]="(4+1)*3+2";
	str[5]="3*5+1*(4-2)";
	str[6]="1-2+3+4+5+6";
	str[7]="2+3+4+7+(6-5)*1";
	str[8]="1+3+4+6+(5-2)*(8-7)";
	cin>>n;
	if(n<=4){
		cout<<str[n]<<"\n";
	}else{
		while(n>8){
			cout<<n<<"+"<<n-3<<"-"<<n-2<<"-"<<n-1<<"+";
			n-=4;
		}
		cout<<str[n]<<"\n";
	}
}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);
//  freopen("out.cpp","w",stdout);
	int T=1;
//	cin>>T;
//	clock_t start,finish;
//	start=clock();
	while(T--){
		Solve();
	}
//	finish=clock();
//	cerr<<((double)finish-start)/CLOCKS_PER_SEC<<endl;	return 0;
}

```



### [B-Minimum Expression](https://ac.nowcoder.com/acm/contest/34980/B)

简单DP，但是高精。把程序写出来转化为Python代码即可。

```python
dp=[ [-1 for j in range(1005)] for i in range(1005)]
S=[ [0 for j in range(1005)] for i in range(1005)]
n,m=map(int,input().split())

st='.'+input()

k=3
m+=1
lst=n//m

def get(l,r):
    res=0
    if(S[l][r]!=0):
        return S[l][r]
    for i in range(l,r+1):
        res=res*10+int(st[i])
    S[l][r]=res
    return res


dp[0][0]=0
    #dp[1][k][0]=dp[1][k][1]=a[1][1]
for i in range(1,n+1):
    for j in range(1,m+1):
        for x in range(i-lst-k,i-lst+k):
            if x<0 or x>=i or dp[x][j-1]==-1:
                continue
            if dp[i][j]==-1 or dp[i][j]>dp[x][j-1]+get(x+1,i):
                dp[i][j]=dp[x][j-1]+get(x+1,i)
            
print(dp[n][m])

```



### [C-Convex](https://ac.nowcoder.com/acm/contest/34980/C)



### [D-The Matrix](https://ac.nowcoder.com/acm/contest/34980/D)



### [ E-Subsegments](https://ac.nowcoder.com/acm/contest/34980/E)

将$a_i$转化为前缀积形式，再用逆元就变成了统计$S_r \equiv inv(S_{l-1})*x$

有0的情况要断开另算，剩下的用树状数组解决即可。

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
#define lb(x) (x&(-x))
using namespace std;
const int N=5e5+7;
const int mod=998244353;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
//void write(int x){if(x>9) write(x/10);putchar(x%10+'0');}

int fpow(int a,int b){
	int res=1;
	while(b){
		res=res*a%mod;
		a=a*a%mod;
		b>>=1; 
	}
	return res;
}

inline int inv(int x){
	return fpow(x,mod-2);
}

int n,X,a[N],ans=0;
map<int,int>cnt;

void Solve(){
	cin>>n>>X;
	a[0]=1;
	if(X==0){ //那么我们就找0的位置然后左右扩展就好了 
		int lst=0;
		for(int i=1;i<=n;++i){
			cin>>a[i];
			if(a[i]==0){ //所有 
				ans+=i;
				lst=i; //记录最后一个0的位置		
			}else{
				ans+=i;
				ans-=i-lst;
			} 
		} 
		cout<<ans<<"\n";
		return;
	}
	for(int i=1;i<=n;++i){ 
		cin>>a[i];
		if(a[i]==0){ //如果有0的话，就要断开了 
			a[i]=1;
			//fg=1; //断开之后要重新计算 
			cnt.clear(); 
			continue;
		} 
		a[i]*=a[i-1]; //变成前缀积的形式
		a[i]%=mod;
		++cnt[X*a[i-1]%mod]; //另一端可以是自己的 
		ans+=cnt[a[i]]; //对应左端点的个数
		//cout<<i<<" "<<ans<<"!!\n"; 
	}
	cout<<ans<<"\n";
}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);
//  freopen("out.cpp","w",stdout);
	int T=1;
//	cin>>T;
//	clock_t start,finish;
//	start=clock();
	while(T--){
		Solve();
	}
//	finish=clock();
//	cerr<<((double)finish-start)/CLOCKS_PER_SEC<<endl;	return 0;
}

```





### [ F-selection](https://ac.nowcoder.com/acm/contest/34980/F)



### [G-Corona Virus](https://ac.nowcoder.com/acm/contest/34980/G)



### [ H-Counting](https://ac.nowcoder.com/acm/contest/34980/H)

签到题，直接模拟即可。

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
//#define int long long
#define mk make_pair
#define pii pair<int,int> 
using namespace std;
const int N=3007,M=2007;
const int mod=1e9+7;
typedef long long ll;

int n,m,k,t,cnt[M][M];
char str[N][N];
pii pos[N];

int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
void write(ll x){if(x>9) write(x/10);putchar(x%10+'0');}

void Solve(){
	n=read();m=read();k=read();t=read();
	for(int i=1,x,y;i<=k;++i){
		x=read();y=read();
		pos[i]={x,y};
	}
	for(int i=1;i<=k;++i){
		scanf("%s",str[i]);
		str[i][t]='!';
	}
	for(int i=0;i<=t;i++){
		long long nowans=0;
		for(int j=1;j<=k;++j){ //统计当前答案 
			nowans+=cnt[pos[j].first][pos[j].second];
			++cnt[pos[j].first][pos[j].second];
		}
		write(nowans);putchar('\n');
		for(int j=1;j<=k;++j){
			--cnt[pos[j].first][pos[j].second];
			if(str[j][i]=='L') pos[j]={pos[j].first,pos[j].second-1};
			if(str[j][i]=='R') pos[j]={pos[j].first,pos[j].second+1};
			if(str[j][i]=='U') pos[j]={pos[j].first-1,pos[j].second};
			if(str[j][i]=='D') pos[j]={pos[j].first+1,pos[j].second}; 
		}
	}
}

signed main(){
//	ios::sync_with_stdio(0);
//	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);
//  freopen("out.cpp","w",stdout);
	int T=1;
//	cin>>T;
//	clock_t start,finish;
//	start=clock();
	while(T--){
		Solve();
	}
//	finish=clock();
//	cerr<<((double)finish-start)/CLOCKS_PER_SEC<<endl;	return 0;
}

```





### [ I-gcds](https://ac.nowcoder.com/acm/contest/34980/I)



### [J-Football Match](https://ac.nowcoder.com/acm/contest/34980/J)

四个角用向量解决，然后手算个比例得到中心连上面和下面的点形成的向量与AB的倍数关系得到五角星边上的两个点，最后用绕点旋转解决剩下的点坐标即可。

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
#define db double
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;
const double pi=acos(-1.0);
const double eps=1e-8;
//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
//void write(int x){if(x>9) write(x/10);putchar(x%10+'0');}

struct P{
    db x,y;P(db X=0,db Y=0){x=X,y=Y;}
    inline void in(){scanf("%lf%lf",&x,&y);}
    inline void out(){printf("%.6lf %.6lf ",x,y);}
}a,b,c,d,e,f,g,h,i,j,k,l,m,n,o;

inline db Dot(P a,P b){return a.x*b.x+a.y*b.y;}//【点积】

inline db Cro(P a,P b){return a.x*b.y-a.y*b.x;}//【叉积】
inline db Len(P a){return sqrt(Dot(a,a));}//【模长】

inline P turn_PP(P a,P b,db theta){//【将点A绕点B顺时针旋转theta(弧度)】
    db x=(a.x-b.x)*cos(theta)+(a.y-b.y)*sin(theta)+b.x;
    db y=-(a.x-b.x)*sin(theta)+(a.y-b.y)*cos(theta)+b.y;
    return P(x,y);
}

double ps,r;

void Solve(){
	a.in();b.in();
	P v1(b.x-a.x,b.y-a.y);
	P v2(3*v1.y/2,-3*v1.x/2);
	d.x=a.x+v2.x,d.y=a.y+v2.y;
	c.x=b.x+v2.x,c.y=b.y+v2.y;
	o.x=(a.x+c.x)/2.0,o.y=(a.y+c.y)/2.0,r=Len(v2)/6.0,ps=sin(pi*18/180.0)/sin(pi*126/180.0);//五角星长轴为r,短轴为r*ps
	e.x=o.x+v1.x*3/10,e.y=o.y+v1.y*3/10; //长轴/4 
	j.x=o.x-v1.x*3/10*ps,j.y=o.y-v1.y*3/10*ps; //短轴
	g=turn_PP(e,o,pi*2/5);
	i=turn_PP(e,o,pi*4/5);
	k=turn_PP(e,o,pi*6/5);
	m=turn_PP(e,o,pi*8/5);
	l=turn_PP(j,o,pi*2/5);
	n=turn_PP(j,o,pi*4/5);
	f=turn_PP(j,o,pi*6/5);
	h=turn_PP(j,o,pi*8/5);
	c.out();
	d.out();
	e.out();
	f.out();
	g.out();
	h.out();
	i.out();
	j.out();
	k.out();
	l.out();
	m.out();
	n.out();
}

signed main(){
//	ios::sync_with_stdio(0);
//	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);
//  freopen("out.cpp","w",stdout);
	int T=1;
	cin>>T;
//	clock_t start,finish;
//	start=clock();
	while(T--){
		Solve();
	}
//	finish=clock();
//	cerr<<((double)finish-start)/CLOCKS_PER_SEC<<endl;	return 0;
}

```





### [ K-Coins](https://ac.nowcoder.com/acm/contest/34980/K)

数量大的时候肯定是Alice更占优势，并且那些数肯定能由质数相加得到。只需要对小数据进行完全背包即可。

```cpp

//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
//#define int long long
using namespace std;
const int N=2e6+7;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
//void write(int x){if(x>9) write(x/10);putchar(x%10+'0');}

int a[5],b[5];
int pa[N],pb[N];

void Solve(){
	memset(pa,inf,sizeof(pa));
	memset(pb,inf,sizeof(pb)); 
	a[1]=2;a[2]=3;a[3]=17;a[4]=19;
	b[1]=5;b[2]=7;b[3]=11;b[4]=13;
	pa[0]=pb[0]=0;
	for(int i=1;i<=4;++i){
		for(int j=0;j<10000;++j){
			if(j>=a[i]) pa[j]=min(pa[j-a[i]]+1,pa[j]);
			if(j>=b[i]) pb[j]=min(pb[j-b[i]]+1,pb[j]);
		}
	}
	//for(int i=1;i<=100000;++i) cout<<i<<" "<<pa[i]<<" "<<pb[i]<<"!!\n";
	int q,x;
	cin>>q;
	for(int i=1;i<=q;++i){
		cin>>x;
		if(x>=200){  //肯定A用的少 
			cout<<"A\n";
		}else if(pa[x]==inf&&pb[x]==inf){cout<<"-1\n";} //都做不到 
		else if(pa[x]==inf){cout<<"B\n";} 
		else if(pb[x]==inf){cout<<"A\n";}
		else if(pa[x]==pb[x]) cout<<"both\n";
		else if(pa[x]<pb[x]) cout<<"A\n";
		else cout<<"B\n";
	}
}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);
//  freopen("out.cpp","w",stdout);
	int T=1;
//	cin>>T;
//	clock_t start,finish;
//	start=clock();
	while(T--){
		Solve();
	}
//	finish=clock();
//	cerr<<((double)finish-start)/CLOCKS_PER_SEC<<endl;	return 0;
}

```



### [L-Segment](https://ac.nowcoder.com/acm/contest/34980/L)



### [M-Travel Round the Grid](https://ac.nowcoder.com/acm/contest/34980/M)