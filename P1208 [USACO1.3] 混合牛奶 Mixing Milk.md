[Portal.](https://www.luogu.com.cn/problem/P1208)

贪心。

优先选择单价 $p_i$ 小的，若单价相同，选择能卖出牛奶量 $a_i$ 高的。

注意排序逻辑。

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int maxn=5005;
struct node{int p,a;}a[maxn];

bool cmp(node a,node b)
{
    if(a.p!=b.p) return a.p<b.p;
    return a.a>b.a;
}

int main()
{
    int n,m;cin>>n>>m;
    for(int i=1;i<=m;i++) cin>>a[i].p>>a[i].a;
    sort(a+1,a+m+1,cmp);
    int cnt=1,ans=0;
    while(n)
    {
        if(n-a[cnt].a>=0) n-=a[cnt].a,ans+=a[cnt].p*a[cnt].a;
        else ans+=(n*a[cnt].p),n=0;
        cnt++;
    }
    cout<<ans;
    return 0;
}
```

