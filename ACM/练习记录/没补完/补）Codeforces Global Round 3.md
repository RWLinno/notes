---
title : Codeforces Global Round 3
date : 2021-10-3
tags : ACM,练习记录
author : Linno
---



题目链接：https://codeforces.com/contest/1148

做题进度：3/9



# A. Another One Bites The Dust

### 题意

给定a\b\ab的数量，问能组成最多多长的字符串，使得a和b间隔。

### 思路

很简单的贪心，因为ab是万金油，肯定接的上。a和b之中缺的那一个就决定了剩下的字符串长度。

### 代码

```C++
#include<bits/stdc++.h>
#define int long long
using namespace std;
typedef long long ll;
 
signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	int a,b,c,ans=0;
	cin>>a>>b>>c;
	ans=c*2;
	ans+=2*min(a,b);
	if(b!=a) ans+=1;
	cout<<ans;
	return 0;
}
```



# B. Born This Way

### 题意

分别由A到B的航班和B到C的航班，取消k个AC无法联通或最短路最长。

### 思路

把航班画在数轴上，并标上时间和类型1或2，我们要把数轴所有的1接2删除，或者使最终的2尽可能远。

### 代码

```C++
#include<bits/stdc++.h>
//#define int long long
using namespace std;
typedef long long ll;
const int maxn=2e5+7;
 
struct X{
	int pos,op; //当前时间和类型 
};
 
vector<X>vt; 
int n,m,ta,tb,k,flag;
int a[maxn],b[maxn],c[maxn];
 
bool cmp(X a,X b){
	if(a.pos==b.pos) return a.op<b.op;
	return a.pos<b.pos;
}
 
signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>n>>m>>ta>>tb>>k;
	if(k>=n||k>=m){
		cout<<-1<<endl;
		return 0;
	}
	for(int i=1;i<=n;i++){
		cin>>a[i];
		a[i]+=ta; //A到B的所有航班时间 
		vt.push_back((X){a[i],1});
	}
	for(int j=1;j<=m;j++){
		cin>>b[j];
		vt.push_back((X){b[j],2});
	}
	sort(vt.begin(),vt.end(),cmp);
	int num=0;
	for(int i=0;i<vt.size();i++){
		if(vt[i].op==2){//有一种方案 
			if(num){ //后面有2了 
				if(k){
					num--;
					k--;
				}else{ //没得取消了呀 
					cout<<vt[i].pos+tb<<endl;
					return 0;
				} 
			}
		}else{
			num++;
		}
	}
	cout<<-1<<endl;
	return 0;
}
```



# C-Crazy Diamond

### 题意

最多交换5n次，使得数组单调递增。注意只能交换距离大于等于n/2的一对数。

### 思路

假设要交换i和j，分为三种情况：

①0<=i,j<=mid<n，那么就以n做桥进行交换。

②0<mid<i,j<=n，那么就以0做桥进行交换。

③0<=i<=mid<j<=n,那么就以0和n做桥进行交换。

### 代码

```C++
#include<bits/stdc++.h>
#define int long long
using namespace std;
typedef long long ll;
const int maxn=3e5+7;
 
int n,ans=0;
int a[maxn],x[maxn<<3],y[maxn<<3],pos[maxn];
 
void fc(int xx,int yy){
	x[++ans]=xx,y[ans]=yy;
	if(xx!=yy) pos[a[yy]]=xx;pos[a[xx]]=yy;
	swap(a[xx],a[yy]);
}
 
signed main(){
	cin>>n;
	for(int i=1;i<=n;i++){
		cin>>a[i];
		pos[a[i]]=i;
	}
	for(int i=1;i<=n;i++){
		if(a[i]!=i){
			int p1=pos[i],p2=i;
			if(2*abs(p1-p2)<n){
				if(p1*2<=n&&2*p2<=n){ //cout<<"都在左半边"<<endl;
					fc(p1,n);
					fc(p2,n);
					fc(p1,n);
				} 
				else if(2*p1>n&&2*p2>n){ //cout<<"都在右半边"<<endl;
					fc(p1,1);
					fc(p2,1); 
					fc(p1,1);	
				}
				else if(p1*2<=n&&2*p2>n){ //cout<<"a[i]在左，i在右"<<endl;
					fc(p1,n);
					fc(p2,1);
					fc(1,n);
					fc(p1,n);
					fc(p2,1);
				}else{ // cout<<"a[i]在右，i在左"<<endl;
					fc(p1,1);
					fc(p2,n);
					fc(1,n);
					fc(p1,1);
					fc(p2,n);
			}
			}else{
				fc(p1,p2);
			} 
		}	
	}
	cout<<ans<<"\n"; 
	for(int i=1;i<=ans;i++){
		cout<<x[i]<<" "<<y[i]<<"\n";
	}
	return 0;
}
```



# D-Dirty Deeds Done Dirt Cheap

### 题意

给定几个二元组进行排序，求最长的（如题所述），并输出二元组编号。

### 思路

模拟题，把x大于y的和y大于x的分成两堆然后排序，记录最大的长度就好了。

### 代码

```c++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int maxn=3e5+7;

int read(){	int x=0,f=1;char ch=getchar();while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}return x*f;}

struct X{
	int x,y,id;
}s[maxn];

bool cmp1(X a,X b){ //比较y 
	return a.y>b.y;
}

bool cmp2(X a,X b){ //比较x 
	return a.y<b.y;
}

int n,len;
int id[maxn],ans[maxn];
vector<X>vt1,vt2;

signed main(){
	n=read();
	for(int i=1;i<=n;i++){
		s[i].x=read();
		s[i].y=read();
		s[i].id=i;
	}
	for(int i=1;i<=n;i++){
		if(s[i].x>s[i].y) vt1.push_back(s[i]);
		if(s[i].y>s[i].x) vt2.push_back(s[i]);
	}
	if(vt1.size()){
		sort(vt1.begin(),vt1.end(),cmp2);
		X tmp=vt1[0];
		len=1;
		id[len]=vt1[0].id;
		for(int i=1;i<vt1.size();i++){
			if(vt1[i].x>tmp.y) tmp=vt1[i],id[++len]=vt1[i].id;
		}
		if(*ans<len) for(int i=1;i<=len;i++) ans[i]=id[i]; 
		*ans=max(*ans,len);
	}
	if(vt2.size()){
		sort(vt2.begin(),vt2.end(),cmp1);
		X tmp=vt2[0];
		len=1;
		id[len]=vt2[0].id;
		for(int i=1;i<vt2.size();i++){
			if(vt2[i].x<tmp.y) tmp=vt2[i],id[++len]=vt2[i].id;
		}
		if(*ans<len) for(int i=1;i<=len;i++) ans[i]=id[i];
		*ans=max(*ans,len);
	}
	printf("%d\n",*ans);
	for(int i=1;i<=*ans;i++) printf("%d ",ans[i]);
	puts("");
	return 0;
}
```





