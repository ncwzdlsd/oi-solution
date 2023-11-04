[Portal.](https://www.luogu.com.cn/problem/P3128)

树上差分。

这里要用的是边差分。

对于一条路径 $s\rightarrow t$，我们把 $s_s,s_t$ 加一，代表到 $s,t$ 的路径上的隔间压力都加 $1$。

注意到 LCA 被重复累加，所以要减 $1$。又因为 LCA 的 $s_{\text{lca}}$ 会对本来不该累加次数的 LCA 的父亲产生影响，所以 LCA 的父亲的 $s$ 值应该减 $1$。

注意要先 DFS 统计完信息之后再更新 `for(int j=1;j<=25;j++) for(int i=1;i<=N;i++) f[i][j]=f[f[i][j-1]][j-1];`。

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
#define int long long

const int maxn=1e5+5;
struct edge{int to,nxt;}e[maxn*2];
int head[maxn],dep[maxn],f[maxn][30],cnt,ans,s[maxn];

void add(int x,int y){e[++cnt]={y,head[x]},head[x]=cnt;}

void dfs1(int x,int fa)
{
    dep[x]=dep[fa]+1,f[x][0]=fa;
    for(int i=head[x];i;i=e[i].nxt)
    {
        if(e[i].to==fa) continue;
        dfs1(e[i].to,x);
    }
}

int lca(int x,int y)
{
    if(dep[x]<dep[y]) swap(x,y);
    for(int i=25;i>=0;i--) if(dep[f[x][i]]>=dep[y]) x=f[x][i];
    if(x==y) return x;
    for(int i=25;i>=0;i--)
        if(f[x][i]!=f[y][i]) x=f[x][i],y=f[y][i];
    return f[x][0];
}

void dfs2(int x,int fa)
{
    for(int i=head[x];i;i=e[i].nxt)
    {
        if(e[i].to==fa) continue;
        dfs2(e[i].to,x);
        s[x]+=s[e[i].to];
    }
    ans=max(ans,s[x]);
}

signed main()
{
    int N,K;cin>>N>>K;
    for(int i=1,x,y;i<N;i++) cin>>x>>y,add(x,y),add(y,x);
    dfs1(1,0);
    for(int j=1;j<=25;j++) for(int i=1;i<=N;i++) f[i][j]=f[f[i][j-1]][j-1];
    while(K--)
    {
        int si,ti;cin>>si>>ti;
        int tmp=lca(si,ti);
        s[si]++,s[ti]++,s[tmp]--,s[f[tmp][0]]--;
        // cout<<s[si]<<endl;
    }
    dfs2(1,0);
    cout<<ans;
    return 0;
}
```

