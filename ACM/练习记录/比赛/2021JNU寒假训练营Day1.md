---
title : 2021JNU寒假训练营Day1
tags : ACM,练习记录
date : 2021-9-18
author : Linno
---



题目链接：https://vjudge.net/contest/417319#overview

考察内容：基本数据结构

榜有点歪，像第二题这么简单居然没人做。

题目不难均可做，需要熟悉STL的用法。



# A-queue

### 思路

对于先进的队伍是优先输出的，并且每次插入要先查看元素所属的队伍是否存在队列中。我们开一个map记录每个元素所属的队伍，然后再开两个队列。其中qq表示当前排在队列中的队伍，q[i]表示i队伍中的队伍。（可能有点绕，因为题目给的是一个大队伍中有很多小队伍）。

注意不要从1~n枚举q[i]，这样不但会超时，还会使得编号小的队伍排在整支队的前面！

### 代码

```C++
#include<iostream>
#include<stdio.h>
#include<map>
#include<queue>
#define int long long
using namespace std;
const int maxn=1e5+7;
const int mod=1e9+7;
typedef long long ll;

inline void read(int &data){
	int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	data=x*f;
}

int t,n,x,idx=0;
map<int,int>mp;
string str;
queue<int>q[1005],qq;

signed main(){
	read(t);
	while(t){
		printf("Scenario #%d\n",++idx);
		mp.clear();
		while(!qq.empty()){
			int fro=qq.front(); 
			while(!q[fro].empty()) q[fro].pop();
			qq.pop();
		}
		for(int T=1;T<=t;T++){ 
			read(n);
			for(int i=1;i<=n;i++){
				read(x);
				mp[x]=T;
			}
		}
		cin>>str;
		while(1){
			if(str=="STOP") break;
			if(str=="ENQUEUE"){
				read(x);
				if(q[mp[x]].empty()) qq.push(mp[x]);
				q[mp[x]].push(x);
			}
			if(str=="DEQUEUE"){
				int i=qq.front();
				if(!q[i].empty()){
					printf("%lld\n",q[i].front());
					q[i].pop();
				}
				if(q[i].empty()) qq.pop();
			}
			cin>>str;
		}
		puts("");
		read(t);
	}
	return 0;
}

```



# B-stack1

### 思路

如果要让所有子串不合法，那么最终字符串的形式必然会是")))(("这种，左边全是失配的")"，右边全是"("。我们容易想到每当找到一个"()"匹配，都可以用一次操作使得他不合法。那么我们只需要忽略失配的"("和")"，然后记录所有合法的"()"的数量，就是答案。



### 代码

```C++
#include<bits/stdc++.h>
#define debug(x) cout<<"x="<<x<<endl
#define int long long
using namespace std;
const int maxn=1e5+7;
const int mod=1e9+7;
typedef long long ll;

inline void read(int &data){
	int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	data=x*f;
}

int n;
string str[1005];

signed main(){
	read(n);
	for(int i=1;i<=n;i++){
		cin>>str[i];
		int ans=0,num1=0,num2=0;
		for(int j=0;j<str[i].length();j++){
			if(str[i][j]==')'){
				if(num1){ //前面有左括号 
					ans++; //需要抵消
					num1--; 
				}else num2++; //未匹配的右括号 
			}else{
				num1++; //未匹配的左括号 
			}
		}
		cout<<ans<<endl;
	}
	return 0;
}
```



# C-stack2

### 思路

容易想到这道题的答案和化简操作次数的奇偶性有关，如何确定化简操作次数？如果读入字符的时候遇到了和前面一样的，那么我们可以直接压缩成一个字符；那么我们只要模拟一个栈，实时处理当前读入的字符，如果读入字符与栈顶相同，那么就直接弹出，并且更新操作次数就可以了。

### 代码

```C++
#include<bits/stdc++.h>
#define debug(x) cout<<"x="<<x<<endl
#define int long long
using namespace std;
const int mod=1e9+7;
typedef long long ll;

inline void read(int &data){
	int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-') f=f*-1;ch=getchar();}
	while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
	data=x*f;
}

char str[1000005];

signed main(){
	int i=1,ans=0;
	while(cin>>str[i]&&str[i]>='a'&&str[i]<='z'){
		if(str[i]==str[i-1]){
			i-=2;
			ans++;
		}
		i++;
	}
	if(ans&1) cout<<"Yes\n";
	else cout<<"No\n";
	return 0;
}
```



# D-set1

### 思路

数据范围很小，我们只需要熟悉set的操作，然后A删除B的元素就可以了。



### 代码

```C++
#include<iostream>
#include<cstring>
#include<set>
#include<algorithm>
#include<stdio.h>
#include<queue>
#include<cstdio>
#include<vector>
#include<map>
using namespace std;

int main(){
	int n,m,x;
	scanf("%d%d",&n,&m);
	while(n!=0||m!=0){
		set<int>a,b;
		for(int i=1;i<=n;i++){
			scanf("%d",&x);
			a.insert(x);
		}
		for(int i=1;i<=m;i++){
			scanf("%d",&x);
			b.insert(x);
		}
		for(std::set<int>::iterator it=b.begin();it!=b.end();it++){
			a.erase(*it);
		}
		if(a.empty()) printf("NULL\n");
		else{
			for(std::set<int>::iterator it=a.begin();it!=a.end();it++){
				cout<<*it<<" ";
			}
			printf("\n");
		} 
		scanf("%d%d",&n,&m);
	} 
	return 0;
}

```



# E-set2

### 思路

看题目是一道单点修改+区间查询，可以考虑用线段树来维护。

查询的时候对每一个字母都遍历这棵树就可以了。

复杂度O(nlogn)，较坏情况下应该近似两个logn



### 代码

```C++
#include<bits/stdc++.h>
#define int long long
#define MID int mid=(l+r)>>1
#define ls node<<1
#define rs node<<1|1
using namespace std;
typedef long long ll;
const int maxn=1e5+7;
int tr[maxn<<2][30];
string str;

void build(int node,int l,int r){
	if(l==r){
		tr[node][str[l-1]-'a']++;
		return;
	}
	MID;
	build(ls,l,mid);
	build(rs,mid+1,r);
	for(int i=0;i<26;i++) tr[node][i]=tr[ls][i]+tr[rs][i];
}

char update(int node,int l,int r,int pos,char c){
	char rg=' ';
	if(l==r){
		rg=str[l-1];
		tr[node][rg-'a']--;
		str[l-1]=c;
		tr[node][str[l-1]-'a']++;
		return rg;
	}
	MID;
	if(pos<=mid) rg=update(ls,l,mid,pos,c);
	else rg=update(rs,mid+1,r,pos,c);
	if(rg!=' '){
		tr[node][rg-'a']--;
		tr[node][c-'a']++;
	}
	return rg;
}

bool query(int node,int l,int r,int ql,int qr,char c){
	if(ql<=l&&r<=qr) return tr[node][c-'a'];
	MID;
	bool r1=0,r2=0,res=0;
	if(ql<=mid) r1=query(ls,l,mid,ql,qr,c);
	if(qr>mid) r2=query(rs,mid+1,r,ql,qr,c);
	res=r1|r2;
	return res;
}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>str;
	int n=str.length(),m,op,l,r;
	char ch;
	build(1,1,n);
	cin>>m;
	for(int i=1;i<=m;i++){
		cin>>op;
		if(op==1){
			cin>>l>>ch;
			update(1,1,n,l,ch);
		}else{
			cin>>l>>r;
			int ans=0;
			for(int j=0;j<26;j++){
				ans+=query(1,1,n,l,r,char(j+'a'));
			}
			cout<<ans<<"\n";
		} 
	} 
	return 0;
}
```





# F-map1

### 思路

熟悉map数据结构，并且开一个map<string,int>就可以过了。

### 代码

```C++
#include<iostream>
#include<cstring>
#include<map>
using namespace std;

int n,inx=0;
string str;
map<string,int>mp;
int main(){
	scanf("%d",&n);
	for(int i=1;i<=n;i++){
		cin>>str;
		if(mp[str]){
			cout<<str<<mp[str]<<endl; 
		}else{
			printf("OK\n");
		}
		mp[str]++; 
	}
	return 0;
}
```



# G-map2

### 思路

非常好玩的一道题，在map里面套map，然后两层迭代就能搞定。

### 代码

```C++
#include<bits/stdc++.h>
#define int long long
using namespace std;
typedef long long ll;

map<string,map<string,int>>mp;

int t,n,num;
string str1,str2;

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	cin>>t;
	while(t--){
		cin>>n;
		mp.clear();
		for(int i=1;i<=n;i++){
			cin>>str1>>str2>>num;
			mp[str2][str1]+=num;
		}
		for(auto it=mp.begin();it!=mp.end();it++){
			cout<<(it->first)<<"\n";
			for(auto iter=(it->second).begin();iter!=(it->second).end();iter++){
				cout<<"   |----"<<(iter->first)<<"("<<(iter->second)<<")\n";
			}
		}
		if(t) cout<<"\n"; 
	}
	return 0;
}
```





# H-next_permutation

### 思路

题目告诉我们用全排列去做了，数据范围很小，只需要对每个排列都生成左右两个数字，然后找出差值最小的那一组就行了。虽然跑得慢，但注意每次排列直接将数字对半分并且避免前导零就能过。

复杂度O(n*2^n)反正不会爆。

贪心可做而且快，但是思维量大。

### 代码

```C++
#include<stdio.h>
#include<algorithm>
#include<vector>
#include<cmath>
#include<iostream>
//#define int long long
using namespace std;
const int inf=0x3f3f3f3f;
int t,a,b,n;
char ch;
vector<int>vt;

inline void read(int &data){
	int x=0;char c=getchar();
	while(c<'0'||c>'9') c=getchar();
	while(c>='0'&&c<='9'){
		x=x*10+c-'0';
		c=getchar();
	}
	data=x;
} 

signed main(){
	read(t);
	while(t--){
		int ans=inf,idx=0;
		vt.clear();
		while(ch<'0'||ch>'9') ch=getchar();
		while(ch!='\n'){
			if(ch>='0'&&ch<='9')vt.push_back(ch-'0');
			ch=getchar();
		}
		n=vt.size();
		do{
			a=0,b=0;
			if(n==2){a=vt[0];b=vt[1];}
			else{ 
				if(vt[0]==0||vt[n/2]==0) continue;// 避免前导零 
				for(int k=0;k<n/2;k++) a=a*10+vt[k]; 
				for(int k=n/2;k<vt.size();k++) b=b*10+vt[k];
			}
			if(abs(a-b)<ans) ans=abs(a-b);
		//	}
		}while(next_permutation(vt.begin(),vt.end()));
		printf("%d\n",ans);
	}
	return 0;
}
```





# I-sort

### 思路

开个结构体，排序就能过的签到题，注意输出格式。

### 代码

```C++
#include<iostream> 
#include<cstring> 
#include<algorithm> 
using namespace std; 

struct X{ 
	string x; 
	int num;
}bl[11];

bool cmp(X a,X b){
	return a.num>b.num;
}

int main(){
	int n,t;
	scanf("%d",&t);
	while(t--){
		scanf("%d",&n);
		for(int i=1;i<=n;i++)
			cin>>bl[i].x>>bl[i].num;
		sort(bl+1,bl+n+1,cmp);
		for(int i=1;i<=n;i++){
			cout<<bl[i].x;
			if(i!=n) printf(" ");
			else printf("\n");
		}
	}
	return 0;
}
```



# J-problem1

### 思路

坑比较多，首先有一个结论就算这个函数是收敛于$\frac{\pi^2}{6}$的，证明方法很多，感兴趣可以自行了解。所以我们要预处理1000000以内的数据；其次这个n可能整数型装不下，直接开字符串去存，如果大于7位数就直接输出收敛值就好了。



### 代码

```C++
#include<bits/stdc++.h>
#define rep(i,x,y) for(int i=x; i<=y; ++i)
 
using namespace std;
const double pi=acos(-1);
double ans,f[1000007];
int n;
char s[1000005]; 

signed main(){
	memset(f,0,sizeof(f));
	for(int i=1;i<=1000000;i++){
		f[i]=f[i-1]+1.0/i/i;		
	}
	while(scanf("%s",s+1)!=EOF){
		n=strlen(s+1);
		int x=0;
		rep(i,1,n) x=min(1000000,x*10+s[i]-48);
		printf("%.5lf\n",f[x]);
	} 
	return 0;
}
```

