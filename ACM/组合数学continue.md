### 组合数计算

##### 杨辉三角

当n,m都很小的时候，直接计算C(n,m)=C(n-1,m)+C(n-1,m-1)



##### 乘法逆元

```cpp
int inv(int a){
    return a==1?1:(ll)(MOD-MOD/a)*inv(MOD%a)%MOD;
}
ll C(ll n,ll m){
    if(m<0||n<m) return 0;
    if(m>n-m) m=n-m;
    ll up=1,down=1;
    for(ll i=0;i<m;i++){
        up=up*(n-i)%MOD;
        down=down*(i+1)%MOD;
    }
    return up*inv(down)%mod;
}
```



##### Lucas定理

当n和m比较大，mod是素数且较小的情况下，可以用Lucas计算。

```cpp
Lucas(n,m,p)=C(n%p,m%p)*Lucas(n/p,m/p,p);
```



### n个球放入m盒子问题

##### 1.球同，盒不同，无空盒

$$
\begin{cases}
C(n-1,m-1),n>=m\\
0,n<m
\end{cases}
$$

使用插板法考虑，在n-1个小球间隙中选择m-1个间隙



##### 2.球同，盒不同，允许空盒

$$
C(n+m-1,m-1)=C(n+m-1,n)
$$

可以理解为先规定第一个是盒（避免重排），剩下n+m-1个位置中选出n个球或者m-1个盒子的位置。



##### 3.球不同，盒相同，无空盒

就是第二类斯特林数$S[n][m]$
$$
\begin{cases}
S[n][m]=m*S[n-1][m]+S[n-1][m-1] ,1\le m<n\\
S[k][k]=1,k\ge 0\\
S[k][0]=0,k\ge 1
\end{cases}
$$
对于前i个球，如果前面i-1个球已经放在m个箱子里了，那么现在第i个球放在哪个箱子都可以，所以$m*S[n-1][m]$，如果前面i-1个球放在了m-1个箱子里，那么第i个球就得新开一个箱子放，所以是$S[n-1][m-1]$



##### 4.球不同，盒相同，允许空盒

$\sum S[n][i],0\le i \le m$

用n个球放入i箱子的情况的总和就是答案。



##### 5.球不同，盒不同，无空盒

$$
S[n][m]*frac[m]（S表示第二类斯特林数，frac表示阶乘）
$$

就是给定了盒子的存放顺序，然后转化为了第3种情况。



##### 6.球不同，盒不同，允许空箱

$power(m,n)$
每个球都有m种选择



##### 7.球同，盒同，允许空箱

$$
\begin{cases}
dp[n][m]=dp[n][m-1]+dp[n-m][m],n>=m\\
dp[n][m]=dp[n][m-1],n<m\\
边界dp[k][1]=dp[1][k]=dp[0][k]=1;
\end{cases}
$$

当前阶段，可以选择在所有的箱子里面都放一个球或者常规放球。



##### 8.球同，盒同，无空箱

$dp[n-m][m],其中dp与情况7相同$

就相当于先在每个盒子里放了一个球，然后再做情况7的分析。



### 参考资料

https://blog.csdn.net/qwb492859377/article/details/50654627
