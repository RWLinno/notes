---
title : 第四届SCPC中南民族大学程序设计竞赛（同步赛）
date : 2022-6-1
tags : ACM,题解，练习记录
author : Linno


---

# 第四届SCPC中南民族大学程序设计竞赛（同步赛）

题目链接：https://ac.nowcoder.com/acm/contest/35624

补题进度：13/14



### [A-Funny哥卖锅盔](https://ac.nowcoder.com/acm/contest/35624/A)

按题意模拟一下，注意加粗的字都是很重要的判断跳出的条件。

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

int n,m;
string str1,str2;
int stk[N];
void Solve(){
	cin>>n>>m;
	cin>>str1;
	cin>>str2;
	int idx=0,top=0,flag=0;
	for(int i=0;i<str2.length();++i){
		if(str2[i]==stk[top]){
			--top;
			continue;
		}else{
			while(idx<n&&top<=m&&str2[i]!=str1[idx]){
				stk[++top]=str1[idx];
				++idx;
				continue;
			}
			if(idx==n||top>m){
				flag=1;
				break;
			}
			++idx;
		}
	}
	if(flag||n!=str2.length()) cout<<"That's not Funny at all!\n";
	else cout<<"Don't you think that's Funny?\n";
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



### [B-穿越到异世界的我居然在写数学题？](https://ac.nowcoder.com/acm/contest/35624/B)

经典题了，枚举一下中间的点，那么这个点连上原点表示的斜率也确定了，看他能在右上角的区域移动出多少条直线，翻过来再做一遍，最后加上x轴y轴的方案即可。

```cpp
#include<bits/stdc++.h>
#define rep(i,x,y) for(int i=x; i<=y; ++i)

using namespace std;
int n,m;
long long ans;

int gcd(int a,int b)
{
    return !b?a:gcd(b,a%b);
}

int main()
{
    scanf("%d%d",&n,&m),++n,++m;
    rep(i,1,n-1) rep(j,1,m-1) ans+=(gcd(i,j)-1ll)*(n-i)*(m-j)*2;
    ans+=n*(n-1ll)*(n-2ll)/6*m+m*(m-1ll)*(m-2ll)/6*n;
    printf("%lld\n",ans);
    return 0;
}
```



### [C-南湖的瓜](https://ac.nowcoder.com/acm/contest/35624/C)

也就是找两个权值最大的连通块，直接跑bfs就行了。

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
using namespace std;
const int N=507;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
//void write(int x){if(x>9) write(x/10);putchar(x%10+'0');}

int xx[]={0,0,1,-1},yy[]={1,-1,0,0};
int n,m,ans[N],res,vis[N][N];
char mp[N][N];
inline void dfs(int x,int y){
	vis[x][y]=1;
	if(mp[x][y]=='*') ++res;
	for(int d=0;d<4;++d){
		int nx=x+xx[d],ny=y+yy[d];
		if(nx<1||nx>n||ny<1||ny>m||mp[nx][ny]=='#'||vis[nx][ny]) continue;
		dfs(nx,ny);
	}
} 

void Solve(){
	cin>>n>>m;
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			cin>>mp[i][j];
		}
	}
	int fi=0,se=0;
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			if(mp[i][j]=='*'&&!vis[i][j]){
				res=0;
				dfs(i,j);
				if(res>fi){
					se=fi;fi=res;
				}else if(res>se) se=res;
			}
		}
	}
	cout<<fi+se<<"\n";
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



### [D-Brave tower](https://ac.nowcoder.com/acm/contest/35624/D)

二分加上状态转移，注意二分的边界。

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x7f7f7f7f
#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
//void write(int x){if(x>9) write(x/10);putchar(x%10+'0');}

int n,mp[N][5],dp[N][5];

bool check(int x){
	for(int i=2;i<=n;++i) dp[i][1]=dp[i][2]=dp[i][3]=0;
    dp[1][1]=dp[1][2]=dp[1][3]=1;
	for(int i=2;i<=n;++i){
		for(int j=1;j<=3;++j){
			for(int k=1;k<=3;++k){
				if(dp[i-1][j]&&abs(mp[i][k]-mp[i-1][j])<=x){
					dp[i][k]=1;
				}
			}
		}
		if(!dp[i][1]&&!dp[i][2]&&!dp[i][3]) return false;
	}
	return true;
}

void Solve(){
	cin>>n;
	for(int i=1;i<=n;++i){
		cin>>mp[i][1]>>mp[i][2]>>mp[i][3];
	}
	int L=0,R=inf,M;
	while(R-L>1){
		M=(L+R)/2;
		if(check(M)) R=M;
		else L=M;
	}
    if(check(L)) R=L;
	cout<<R<<"\n";
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



### [E-小吉祥的考验](https://ac.nowcoder.com/acm/contest/35624/E)

记忆化搜索也T了，只能换成DP做法，每个格子显然都是由上面或者左边的格子转移过来的，dp\[i]\[j]\[x]\[y]表示格子[i,j]中特殊格子1使用状态为x，特殊格子2的使用状态为y使的最大得分。

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=505;
const int mod=1e9+7;

int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
void write(int x){if(x>9) write(x/10);putchar(x%10+'0');}

struct node{int x,y,sc,stp,lim1,lim2;};
bool operator <(node A,node B){
	return A.sc<B.sc;
}

int n,m,k,ans=-1,key[N][N],mp[N][N];
int dp[N][N][2][2];

void Solve(){
	n=read();m=read();
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			mp[i][j]=read();
		}
	}
	k=read();
	for(int i=1,op,x,y;i<=k;++i){
		op=read();x=read();y=read();
		key[x][y]=op;
	}
	//bfs();
	memset(dp,-0x3f,sizeof(dp));
	dp[0][1][0][0]=dp[1][0][0][0]=0;
	for(int i=1;i<=n;++i){
		for(int j=1;j<=m;++j){
			if(key[i][j]==1){
				dp[i][j][1][0]=(max(dp[i-1][j][0][0],dp[i][j-1][0][0])+mp[i][j])/2;
				dp[i][j][1][1]=(max(dp[i-1][j][0][1],dp[i][j-1][0][1])+mp[i][j])/2;
				//dp[i][j][0][1]=dp[i][j][0][0]=-inf;
			}else if(key[i][j]==2){
				dp[i][j][0][1]=max(dp[i-1][j][0][0],dp[i][j-1][0][0])+(i+j-1)*mp[i][j]; 
				dp[i][j][1][1]=max(dp[i-1][j][1][0],dp[i][j-1][1][0])+(i+j-1)*mp[i][j];
				//dp[i][j][1][0]=dp[i][j][0][0]=-inf;
			}else{
				dp[i][j][0][0]=max(dp[i-1][j][0][0],dp[i][j-1][0][0])+mp[i][j];
				dp[i][j][0][1]=max(dp[i-1][j][0][1],dp[i][j-1][0][1])+mp[i][j];
				dp[i][j][1][0]=max(dp[i-1][j][1][0],dp[i][j-1][1][0])+mp[i][j];
				dp[i][j][1][1]=max(dp[i-1][j][1][1],dp[i][j-1][1][1])+mp[i][j];
			}
		}
	}
	//cout<<dp[n][m][0][0]<<" "<<dp[n][m][0][1]<<" "<<dp[n][m][1][0]<<" "<<dp[n][m][1][1]<<" !!\n";
	ans=max(max(dp[n][m][0][0],dp[n][m][0][1]),max(dp[n][m][1][0],dp[n][m][1][1]));
	if(ans<=0) puts("gusha!");
	else write(ans),putchar('\n');
}

signed main(){
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



### [F-阿毛与王爷](https://ac.nowcoder.com/acm/contest/35624/F)

简单来说就算要x异或一个最小的数使得二进制下1的位数为n。我们分类讨论要加多少个1或者减去多少个1，我们从小到大按位确定y的二进制位，最终如果大于x的话无解，否则直接把y输出来即可。

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
//void write(int x){if(x>9) write(x/10);putchar(x%10+'0');

int n,x,a[105],b[105],p,cnt=0,res=0;

void Solve(){
	cin>>n>>x;
	int tmp=x;
	if(n>63){
		cout<<"Chuola!\n";
		return;
	}
	while(x){
		a[cnt++]=(x&1ll);
		if(x&1ll) ++p; //计算有多少个1 
		x>>=1;
	}
	if(p>n){ //当前1位比结果多，取最后p-n位个1异或掉即可 
		int idx=0; 
		for(int i=0;i<=63;++i){
			if(a[i]&1ll){
				b[i]=1;
				++idx;
			}
			if(idx==p-n) break;
		}
	}else if(p<n){  //当前1位比结果少，从后面补上n-p个1即可 
		int idx=0;
		for(int i=0;i<=63;++i){
			if(!(a[i]&1ll)){
				b[i]=1;
				++idx;	
			}
			if(idx==n-p) break;
		}
	}
	for(int i=0;i<=63;++i) if(b[i]) res|=(1ll<<i);
	if(res>tmp){
		cout<<"Chuola!\n"; //y比x还要大 
	}else{
		cout<<"Piaoliangdehenna!\n";
		cout<<res<<"\n";
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



### [G-Secure, Contain, Protect](https://ac.nowcoder.com/acm/contest/35624/G)

待补。



### [H-雨声里的相遇](https://ac.nowcoder.com/acm/contest/35624/H)

可以转成k进制后用一个栈来存字符串，如何判一下回文递增。

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
//#define inf 0x3f3f3f3f
//#define int long long
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
//void write(int x){if(x>9) write(x/10);putchar(x%10+'0');}

inline int md(char x){
	if(x>='0'&&x<='9') return x-'0';
	else return x-'A'+10;
}

inline char dm(int x){
	if(x<10) return (char)(x+'0');
	else return (char)(x-10+'A');	
}

int n,s;
char stk[N];

void Solve(){
	cin>>n>>s;
	int cnt=0; //十进制表示下的数 
	for(int i=2;i<=n;++i){
		int tmp=s,top=0,flag=1;
		while(tmp){
			stk[++top]=dm(tmp%i);
	//		cout<<stk[top]<<"~~\n";
			tmp/=i;
		}
	//	printf("%s",stk+1);
	//	cout<<" "<<s<<" "<<i<<"\n";
		for(int j=1;j<=top/2;++j){
			if(stk[j]!=stk[top-j+1]){
				flag=0;
				break;
			}
		}
		for(int j=1;j<(top+1)/2;++j){
			if(stk[j]>=stk[j+1]){
				flag=0;
				break;
			}
		}
		if(flag){
			cout<<i<<" ";
			for(int j=1;j<=top;++j) cout<<stk[j];
			cout<<"\n";
			++cnt;
		}
	}
	if(!cnt) cout<<"guomienasai\n";
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



### [I-花海](https://ac.nowcoder.com/acm/contest/35624/I)

凸多边形可以剖分成n-2个三角形来计算面积，三角形的面积就是叉乘除2即可。

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
using namespace std;
const int N=2e5+7;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
//void write(int x){if(x>9) write(x/10);putchar(x%10+'0');}

struct P{
	double x,y;
	P(double x=0,double y=0){x=x,y=y;}
	inline void in(){scanf("%lf%lf",&x,&y);};
}p[N];

inline double Cro(P a,P b){return a.x*b.y-a.y*b.x;} //叉积 

void Solve(){
	int n;
	scanf("%d",&n);
	for(int i=1;i<=n;++i){
		scanf("%lf%lf",&p[i].x,&p[i].y);
	}
	P v1,v2;
	double ans=0;
	for(int i=3;i<=n;++i){
		v1.x=p[i-1].x-p[1].x;
		v1.y=p[i-1].y-p[1].y;
		v2.x=p[i].x-p[1].x;
		v2.y=p[i].y-p[1].y;
		//cout<<v1.x<<" "<<v1.y<<" "<<v2.x<<" "<<v2.y<<"!!\n"; 
		ans+=abs(Cro(v1,v2)/2);
		//cout<<ans<<"\n";
	}
	printf("%.3lf",ans);
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



### [J-一个人挺好](https://ac.nowcoder.com/acm/contest/35624/J)

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
//#define inf 0x3f3f3f3f
//#define int long long
using namespace std;
const int N=2e6+7;
const int mod=1e9+7;

//int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
//void write(int x){if(x>9) write(x/10);putchar(x%10+'0');}
char s[N];
void Solve(){
	int n,m;
	cin>>n>>m;
	if((m-n)%2==0){
		int i;
		for(i=0;i<n-1;i+=2){
			s[i]='1';
			s[i+1]='0';
			//s+="10";
		}
		for(i=i;i<m;++i){
			s[i]='1';
			//s+="1";
		}
		cout<<s;
	}else{ //要消除奇数个，显然不可能 
		cout<<"-1\n"; 
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



### [K-啵啵小狗自动机(easy version)](https://ac.nowcoder.com/acm/contest/35624/K)

数据范围很小，我们算一下$i^2-(i-1)^2>n$，只需要暴力枚举50个数就可以了。

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
bool Solve(){
	int n;
	cin>>n;
	for(int i=1;i<=55;++i){
		for(int j=1;j<i;++j){
			if(i*i-j*j==n) return 1;
		}
	}
	return 0;
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
		if(Solve()) cout<<"YES\n";
		else cout<<"NO\n";
	}
//	finish=clock();
//	cerr<<((double)finish-start)/CLOCKS_PER_SEC<<endl;	return 0;
}

```



### [L-啵啵小狗自动机(hard version)](https://ac.nowcoder.com/acm/contest/35624/L)

按上面不等式我们需要$O(n^2)$枚举50000个数才能得出答案，显然是不可行的，但是我们把表打出来后观察规律，可以发现$n \mod 4 !=2$就是YES，否则NO。

```cpp
//#pragma GCC optimize("Ofast", "inline", "-ffast-math")
//#pragma GCC target("avx,sse2,sse3,sse4,mmx")
#include<bits/stdc++.h>
using namespace std;
const int N=2e5+7;

int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}
void write(int x){if(x>9) write(x/10);putchar(x%10+'0');}

bool ans[N];

void Solve(){
	for(int i=1;i<=50001;++i){
		for(int j=0;j<i;++j){
			if((long long)i*i-(long long)j*j>100000ll) continue; 
			ans[i*i-j*j]=1;
		}
	}
	return;
}

signed main(){
//	ios::sync_with_stdio(0);
//	cin.tie(0);cout.tie(0);
//  freopen("in.cpp","r",stdin);
//    freopen("out.txt","w",stdout);
	int T=1,n;
	T=read();
//	clock_t start,finish;
//	start=clock();
//	Solve();
//	for(int i=1;i<=100;++i) cout<<i<<" "<<ans[i]<<"!!\n"; 
	
	while(T--){
		n=read(); 
        if(n%4!=2) puts("YES"); //cout<<"YES\n";
		else puts("NO"); //cout<<"NO\n";
	}
//	finish=clock();
//	cerr<<((double)finish-start)/CLOCKS_PER_SEC<<endl;	return 0;
}

```



### [M-南湖的瓜-续](https://ac.nowcoder.com/acm/contest/35624/M)

范围在n之中，那么区间和%n==0肯定有解，转化成前缀和形式，如果某个前缀和%n在前面出现过，就说明这个区间满足。

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
//void write(int x){if(x>9) write(x/10);putchar(x%10+'0');}
int n,a[N],b[N],mp[N];

void Solve(){
	cin>>n;
	a[0]=b[0]=0;
	for(int i=1;i<=n;++i){
		cin>>a[i];
		a[i]+=a[i-1];
	}
	for(int i=1;i<=n;++i){
		b[i]=a[i]%n;
		if(mp[b[i]]){
			cout<<i-mp[b[i]]<<"\n";
			for(int j=mp[b[i]]+1;j<=i;++j){
				cout<<j<<" ";
			}
			return;
		}else mp[b[i]]=i;
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



### [N-小鲨鱼啦啦啦](https://ac.nowcoder.com/acm/contest/35624/N)

签到。

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
void Solve(){
	int n;
	cin>>n;
	if(n&1) cout<<"xiaoshayulalala......\n";
	else cout<<"Happy birthday,SCMZU!!!\n"; 
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

