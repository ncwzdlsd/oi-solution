[Portal.](https://www.luogu.com.cn/problem/AT_abc123_d)

暴力枚举，时间复杂度 $O(X^3\log X^3)$。

考虑优化，注意到所有的元素都是正的，所以只有 $A,B$ 序列的前 $K$ 大值才有可能成为总的前 $K$ 大值。

> 证明：若 $A,B$ 序列的第 $K+1$ 大值能成为总的前 $K$ 大值，则它前面一定会有 $K$ 个数比它更优。矛盾。

把这 $K$ 个值压入优先队列，再枚举 $C$ 序列，取前 $K$ 大值即可。

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

int A[1005],B[1005],C[1005];
priority_queue<int> q1,q2;

signed main()
{
	ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
	int X,Y,Z,K;cin>>X>>Y>>Z>>K;
	for(int i=1;i<=X;i++) cin>>A[i];
	for(int j=1;j<=Y;j++) cin>>B[j];
	for(int k=1;k<=Z;k++) cin>>C[k];
	for(int i=1;i<=X;i++)
		for(int j=1;j<=Y;j++) q1.push(A[i]+B[j]);
	for(int i=1;i<=min(K,X*Y);i++)
	{
		int x=q1.top();q1.pop();
		for(int j=1;j<=Z;j++) q2.push(x+C[j]);
	}
	while(K--) cout<<q2.top()<<"\n",q2.pop();
	return 0;
}
```

