[Portal.](https://www.luogu.com.cn/problem/P2921)

每只奶牛的终止条件是到达自己已经访问过的点，换言之，就是该奶牛的路线构成了一个环。并且，每一个房间通往的房间都是固定且唯一的，所以说只要进入的这个房间在环上，这个房间之后会获得的糖果数已经固定了。

我们开一个数组 `s` 记录当前位置的糖果数量，用 `vis` 数组记录房间的访问情况。对于一个已经访问过得房间，我们只需要用在这个房间的糖果数量减去上一次来这个房间的糖果数量，就可以得到当前房间所在环上的糖果数量，我们用`h`数组记录。

```cpp
#include <bits/stdc++.h>
using namespace std;

const int maxn=1e5+5;
int nxt[maxn],h[maxn],s[maxn],flag,n;
bool vis[maxn];

int dfs(int now,int nowc)//起始位置 当前糖果数
{
	if(h[now]) return nowc-1+h[now];//当前房间的糖果不能重复累加，要-1
	if(vis[now]==1)
	{
		h[now]=nowc-s[now];flag=now;
		return nowc-1;
	}
	vis[now]=1;s[now]=nowc;
	int ans=dfs(nxt[now],nowc+1);//当前位置符合，继续向下一个位置DFS
	if(flag)
	{
		if(now==flag) flag=0;
		else h[now]=h[flag];
	}
	else h[now]=h[nxt[now]]+1;
	vis[now]=false;
	return ans;
}

int main()
{
	cin>>n;
	for(int i=1;i<=n;i++) cin>>nxt[i];
	for(int i=1;i<=n;i++) cout<<dfs(i,1)<<endl;
	return 0;
}
```



