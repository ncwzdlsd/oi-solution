[Portal.](https://www.luogu.com.cn/problem/P3545)

反悔贪心。

想满足尽量多的天数，就要让时间尽可能的长。考虑顺次在每一天满足要求，如果有一天不能满足要求了，就查找前面满足的天里有没有要求比它高的，若有可以放弃那一天的选择而选择当前天，这样可以使时间尽可能地延长。

考虑为什么这样是正确的。首先，放弃之前天选择当前天这一决策一定不会更劣。同时还可以不断增加存货量，增大了后面满足要求的可能性。因此该决策是优的。

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
typedef long long ll;

const int maxn=3e5+5;
struct node
{
    int id,v;
    bool friend operator < (node a,node b){return a.v<b.v;}
};
int a[maxn],b[maxn];
priority_queue<node> q;
bool vis[maxn];

signed main()
{
    int n;cin>>n;
    for(int i=1;i<=n;i++) cin>>a[i];
    for(int i=1;i<=n;i++) cin>>b[i];
    int res=0,ans=0;
    for(int i=1;i<=n;i++)
    {
        res+=a[i];
        if(res>=b[i]) q.push({i,b[i]}),ans++,res-=b[i],vis[i]=1;
        else if(!q.empty()&&q.top().v>b[i])
        {
            int x=q.top().id;q.pop();
            vis[x]=0,res+=b[x]-b[i],q.push({i,b[i]}),vis[i]=1;
        }
    }
    cout<<ans<<'\n';
    for(int i=1;i<=n;i++) if(vis[i]) cout<<i<<' ';
    return 0;
}
```
