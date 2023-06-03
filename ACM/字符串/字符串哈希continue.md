---
title : 字符串哈希
date : 2021-7-25
tags : ACM,字符串
---



# 字符串哈希

hash，就是将一样东西映射成另一个东西，类似map。字符串hash，就相当于将字符串映射成唯一表示的数字。通过字符串的hash值，我们可以达到$O(1)$比较两个串是否相等。



### 构造方法

##### 自然溢出

通过自然溢出，不需要定义mod，而是利用unsigned long long的自然溢出处理长串的哈希值。

```cpp
#define ull unsigned long long
ull base,hash[N],p[N]; //其中p[i]表示base^i
```

##### 单哈希法

有了mod的情况下，开long long求余也可以构造字符串的哈希值。

##### 双哈希法

为了避免哈希冲突的情况(即两个字符串阴差阳错地得到了同样的哈希值)，我们可以取两个不同的mod，这样构造的hash值就相当安全。



### 模板

##### 子串查找

给定字符串A,B，求B在A中出现的次数。

```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
typedef long long ll;
const int base=131;
const int mod=1e16+7;
int bb[1000005],sum[1000005],hs;

int get_hs(int i,int l){ //获取A串从i开始到i+l位的哈希值
	return sum[i+l-1]-sum[i-1]*bb[l];
}

signed main(){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	string A,B;
	cin>>A;
	cin>>B;
	int lena=A.length(),lenb=B.length(),ans=0;
	for(int i=0;i<lenb;i++){
		hs=(hs*base+B[i]-'A'+1);	
	}
	bb[0]=1;
	for(int i=1;i<=lena;i++){
		bb[i]=bb[i-1]*base;
		sum[i]=(sum[i-1]*base+A[i-1]-'A'+1);
	}
	for(int i=lenb;i<=lena;i++){
		if(sum[i]-sum[i-lenb]*bb[lenb]==hs){
			ans++;
		}
	}
	cout<<ans<<endl;
	return 0;
}
```





### 最长公共子字符串

如果存在长度为k的最长公共子字符串，那么k-1长的公共子字符串也必定存在。因此可以二分最长公共子字符串。我们将所有长度为k的子串分别进行哈希，将哈希值放到n个哈希表中存储，之后求交集即可。复杂度$O(nlog_2\frac{n}{m})$



### 确定字符串中不同子字符串数量

遍历所有长度为l=1,...,n的子串，对于所有长度l，将其Hash值乘以相同的b的幂次方并存入数组，数组中不同元素的数量等于字符串中长度不同的子串数量。

$我们记h[i]为Hash的前缀字符，并定义h[0]=0$

```C++
int count_unique_substrings(string const& s) {
  int n = s.size();
  const int b = 31;
  const int m = 1e9 + 9;
  vector<long long> b_pow(n);
  b_pow[0] = 1;
  for (int i = 1; i < n; i++) b_pow[i] = (b_pow[i - 1] * b) % m;

  vector<long long> h(n + 1, 0);
  for (int i = 0; i < n; i++)
    h[i + 1] = (h[i] + (s[i] - 'a' + 1) * b_pow[i]) % m;

  int cnt = 0;
  for (int l = 1; l <= n; l++) {
    set<long long> hs;
    for (int i = 0; i <= n - l; i++) {
      long long cur_h = (h[i + l] + m - h[i]) % m;
      cur_h = (cur_h * b_pow[n - i - 1]) % m;
      hs.insert(cur_h);
    }
    cnt += hs.size();
  }
  return cnt;
}
```





### Rabin-Karp

##### 思想

使用哈希思想，对T串计算哈希值，并以相同的方式计算S串每一个位置对应子串的HASH值。

解决哈希溢出的方法：大数取模

解决哈希冲突的方法：直接匹配字符串

