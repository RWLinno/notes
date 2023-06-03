---
title :Codeforces Round #706 (Div. 2)
date : 2021-10-9
tags : ACM,练习记录
author : Linno

---



题目链接 ：https://codeforces.com/contest/1496

补题进度 ：已补完



总结：前5题都不算思维量很大，只要想得快就能打得很好。第六题大概就来不及写了，还是要多练前面的题。



# A. Split it!

### 题意

给定字符串长度n和k以及字符串，求能否将字符串两边划分出k份对称的子串。

### 思路

我们只要求前k个字符是否和后k个字符相等即可。

### 代码

```C++
#include<bits/stdc++.h>
#define inf 0x3f3f3f3f
#define int long long
using namespace std;
const int N=2e5+7;

signed main(){
	int t,n,k;
	string str;
	cin>>t;
	while(t--){
		cin>>n>>k;
		cin>>str;
		if(k==0) puts("YES");
		else{
			int flag=1;
			for(int i=1;i<=n/2;i++){
				if(str[i-1]!=str[n-i]){
					if(i<=k) flag=0;
					break;
				}
			}
			if(flag&&k*2!=n) puts("YES");
			else puts("NO");
		}
	}
	return 0;
}
```





```C++
#include<bits/stdc++.h>
using namespace std;
const int N=505;

int t,n,m;
char mp[N][N];

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
		if(n==1){
			for(int i=1;i<=m;i++) cout<<"X";
			cout<<"\n";
			continue;
		}
		for(int i=2;i<=n;i++){
			if(i%3==2) for(int j=1;j<=m;j++) mp[i][j]='X';
			else if(i%3==1&&mp[i][2]!='X'&&mp[i-1][2]!='X') mp[i-1][1]=mp[i][1]='X';
			else if(i%3==1) mp[i][2]=mp[i-1][2]='X';
		}
		if(n%3==1) for(int i=1;i<=m;i++) if(mp[n][i]=='X'&&mp[n][i-1]!='X') mp[n-1][i]='X';
		for(int i=1;i<=n;i++){
			for(int j=1;j<=m;j++) cout<<mp[i][j];
			cout<<"\n";
		}
	}
	return 0;
}
```