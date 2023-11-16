[Portal.](https://www.luogu.com.cn/problem/P9496)

按位与 / 或上一个正整数，相当于可以花费 $1$ 修改二进制下的一些位为 $0$ 或 $1$。发现花费至多为 $2$。

- 若 $n=m$，花费为 $0$。
- 若 $n$ 和 $m$ 不同的位的对应关系只有 $0\rightarrow 1$ 或 $1\rightarrow 0$ 中的一个，只需要对其中的一个数进行一种修改成相同即可。
- 若两种不同的对应都有，花费为 $2$。

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
#define int long long

void solve()
{
    int n,m;cin>>n>>m;
    if(n==m) return cout<<"0\n",void();
    if((n|m)==n||(n|m)==m) cout<<"1\n";
    else cout<<"2\n";
}

signed main()
{
    int T;cin>>T;
    while(T--) solve();
    return 0;
}
```

