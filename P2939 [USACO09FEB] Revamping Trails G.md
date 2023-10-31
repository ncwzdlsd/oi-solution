[Portal.](https://www.luogu.com.cn/problem/P2939)

分层图。

分层图的构建如下：

1. 将图复制 $k+1$ 份（$0\sim k$），$k$ 次修改，有 $k+1$ 层图
2. 对于图中的每一条边 $<u,v>$，从 $u_{i\times n}$ 到  $v_{(i+1)\times n}$ 建立与题目操作相对应的边（$n$ 为总点数）

我们可以这样定义：

1. 当边 $<u,v,w>$ 位于第 $i$ 层上时，表示已经改建了 $i$ 条道路，且不改建当前道路，由 $u$ 向 $v$ 边权为 $w$。
2. 当边 $<u,v,w>$ 位于第 $n\times i$ 层和第 $n\times (i+1)$ 层时，表示已经改建了 $i$ 条道路，且改建当前道路，由 $u$ 向 $v$ 边权为 $w$。

代码如下：

```cpp
#include <bits/stdc++.h>
using namespace std;

const int maxn=100005,maxm=500005;
int nxt[maxm*25],w[maxm*25],to[maxm*25],head[maxm*25],cnt,diss[maxn*25];

void add(int x,int y,int z)
{
	nxt[++cnt]=head[x];
	head[x]=cnt;
	to[cnt]=y;
	w[cnt]=z;
}

struct node
{
	int u,dis;
	friend bool operator < (node a,node b)
	{
		return a.dis>b.dis;
	}
};

priority_queue<node> q;

void dij(int s)
{
	memset(diss,0x7f,sizeof(diss));
	diss[s]=0;
	q.push(node{s,0});
	while(!q.empty())
	{
		node gg=q.top();q.pop();
		int u=gg.u,dis=gg.dis;
		if(dis!=diss[u]) continue;
		for(int i=head[u];i;i=nxt[i])
		{
			if(diss[to[i]]>diss[u]+w[i])
			{
				diss[to[i]]=diss[u]+w[i];
				q.push(node{to[i],diss[to[i]]});
			}
		}
	}
}

int main()
{
	int N,M,K;cin>>N>>M>>K;
	for(int i=1;i<=M;i++)
	{
		int a,b,c;cin>>a>>b>>c;
		for(int j=0;j<=K;j++)
		{
			add(a+N*j,b+N*j,c);add(b+N*j,a+N*j,c);//建原图+复制图，即每层图都创造一个和第一层图一样的连线
			if(j!=K) add(a+N*j,b+N*(j+1),0);add(b+N*j,a+N*(j+1),0);//建立层与层之间的边，即有高度公路
		}
	}
	dij(1);
	int ans=diss[N];
	for(int i=1;i<=K;i++) ans=min(ans,diss[N+i*N]);
	cout<<ans;
	return 0;
}
```
