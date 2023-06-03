

### 两球面积交

![img](https://gss0.baidu.com/-vo3dSag_xI4khGko9WTAnF6hhy/zhidao/wh%3D600%2C800/sign=282e66a3af51f3dec3e7b162a4dedc27/500fd9f9d72a605990154f732834349b033bba56.jpg)



### 空间中两线段的最短点对

![21bddf58f2a4efa89d64262edcc0c65](E:\BaiduNetdiskWorkspace\notes\21bddf58f2a4efa89d64262edcc0c65.jpg)



### 期望面积

给定每个点的坐标和出现概率，求形成凸包的期望面积。

```cpp
//By BLADEVIL
#include <cstdio>
#define maxn 110

using namespace std;

int n;
int x[maxn],y[maxn];
double p[maxn];

bool judge(int i,int j,int k) {
    return ((x[j]-x[i])*(y[k]-y[i])-(y[j]-y[i])*(x[k]-x[i]))<0;
}

signed main() {
    scanf("%d",&n);
    double ans=0;
    for (int i=1;i<=n;i++) scanf("%d%d%lf",&x[i],&y[i],&p[i]);
    for (int i=1;i<=n;i++)
        for (int j=1;j<=n;j++)
            if (i!=j) {
                double q=p[i]*p[j];
                for (int k=1;k<=n;k++)
                    if ((k!=i)&&(k!=j)&&(judge(i,j,k)))
                        q*=1-p[k];
                ans+=q*(x[i]*y[j]-y[i]*x[j]);
            }
    ans/=2;
    printf("%.6f",ans+1e-8);
    fclose(stdin); fclose(stdout);
    return 0;
}
```



##### Football Match

![img](https://uploadfiles.nowcoder.com/images/20220522/403112_1653151757035/B32247EBA07F551081F49D6FC35DDC76)*给定AB，求CDEFGHIJKLMN。*

先求AD向量，就知道了CD坐标，然后就知道中心点O坐标。然后求E点和J点，分别是O点加一个和AB平行的向量，然后用他们再沿O点旋转4次即可。
