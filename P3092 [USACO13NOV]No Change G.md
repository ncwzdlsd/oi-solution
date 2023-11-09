[Portal.](https://www.luogu.com.cn/problem/P3092)

DP。

我们用数组 `dp[i]` 表示状态 $i$ 下的硬币可以买到第几个商品，用数组 `f[i]` 表示状态 $i$ 下的花费。

注意，每次只能支付一枚硬币。

外层枚举状态，内层 $j$ 枚举 $16$ 位二进制数的每一位，若当前状态的第 $j$ 位为 $1$，那么可以考虑转移。

因为一种状态可以被多次更新，所以要取最大值，令 `dp` 数组存储的是当前最大编号。

如果到第 $N$ 件都可以购买，用 `ans` 记录当前的最小花费就可以得到答案。

对于记录当前状态下能达到的金钱数，我们可以用前缀和来记录。

除此之外 $i$，由于前缀和是单调的，我们设函数 `find(x)` 表示价值为 $x$ 的货币可以从 $1$ 连续购买到的最大商品的下标，可以用二分快速查找。

代码如下：

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

const int maxn=100005;
ll dp[maxn],sum[maxn],coin[20],c[maxn],K,N,ans=-1,pay[maxn];

int find(int x)
{
	int l=1,r=N,pos;
	while(l<=r)
	{
		int mid=((l+r)>>1);
		if(sum[mid]<=x) l=mid+1,pos=mid;
		else r=mid-1;
	}
	return pos;
}

signed main()
{
	cin>>K>>N;
	for(int i=0;i<K;++i) cin>>coin[i];
	for(int i=1;i<=N;i++) cin>>pay[i],sum[i]=sum[i-1]+pay[i];
	for(int sta=0;sta<(1<<K);++sta)//枚举状态
	{
		for(int i=0;i<K;i++)
		{
			if(!(sta&(1<<i))) continue;//当前硬币没被选过
			ll res=dp[sta^(1<<i)];
            //sta^(1<<i)表示当前状态去掉ci货币后的新状态，res表示新状态可以到达的最末商品
			ll pos=find(sum[res]+coin[i]);
			dp[sta]=max(dp[sta],pos);
		}
	}
	for(int sta=0;sta<(1<<K);++sta)
	{
		if(dp[sta]==N)
		{
			ll cnt=0;
			for(int j=0;j<K;++j)
				if(!(sta&(1<<j))) cnt+=coin[j];//如果当前位置没有货币，统计答案
			ans=max(ans,cnt);
		}
	}
	cout<<ans;
	return 0;
}
```

