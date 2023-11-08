[Portal.](https://www.luogu.com.cn/problem/P2910)

最短路。

考虑到数据范围 $N\leq 100$，可以用 Floyd 算法解决。

对于要求的行走序列，按顺序累加答案即可。

注意数组大小。

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

int A[10005],f[105][105];

signed main()
{
	int N,M;cin>>N>>M;
	for(int i=1;i<=M;i++) cin>>A[i];
	for(int i=1;i<=N;i++)
		for(int j=1;j<=N;j++) cin>>f[i][j];
	for(int k=1;k<=N;k++)
		for(int i=1;i<=N;i++)
			for(int j=1;j<=N;j++) f[i][j]=min(f[i][j],f[i][k]+f[k][j]);
	int ans=0;
	for(int i=2;i<=M;i++) ans+=f[A[i-1]][A[i]];
	cout<<ans;
	return 0;
}
```

