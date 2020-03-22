<hr>

#### <center>B题 God J and Blooming Gentians </center>

**题意：** 

给定一个 $N*M$ 的矩阵，每次随机选择一种颜色删去，删去的同时，与其他颜色不相邻的颜色也会被自动删 去。问期望选择删几次后所有颜色都被删完,最多$C$ 种颜色。

$n,m \leq 1000$

$C \leq 10^6$

**题解：** 

把每种颜色抽象成为节点，相邻的不同颜色之间建立无向边(不重复)，问题转换成每次选择一个度不为 $0$ 的点删去与它关联的边，问把图上的边全部删除需要选择点的期望值。

由于图上的颜色要么被手动删除，要么被自动删除于是得到：

$ANS = 手动选择颜色数目的期望 = C - 自动被删除的颜色数目的期望$

设 $P(X)$  表示 $X$ 这个颜色被自动删除的概率，如果 $X$ 是被自动删除的，那么其相邻边一定是要手动选择删除的，因为相邻边不可能因为孤立而被自动删除。$P(X)$ 只和 $X$ 以及和它相邻的点有关（设数量为 $Y$ ），考虑概率的时候可以考虑其他的颜色：$P(X)=\frac{Y-1}{Y} * \frac{Y-2}{Y-2} * ···* \frac{1}{2}=\frac{1}{Y}$

不难发现：$Y=Deg(X)+1$

所以得到答案的公式：
$$
C-\sum_{i=1}^{C}\frac{1}{Deg(i)+1}
$$

```c++
#include<bits/stdc++.h>
using namespace std;   
typedef long long ll;
const int N = 1e3 + 5;
const int M = 1e6 + 5;
const ll Mod = 1e9 + 7;
ll n, m, c;
ll a[N][N];
set<ll> st[M];
ll deg[M];
void addset(int p1, int p2)
{
	st[p1].insert(p2);
	st[p2].insert(p1);
}
ll ksm(ll a, ll k)
{
	ll res = 1;
	while(k)
	{
		if(k & 1) (res *= a) %= Mod;
		(a *= a) %= Mod;
		k >>= 1;
	}
	return res;
}
int main()
{
	scanf("%lld%lld%lld", &n, &m, &c);
	for(int i = 1; i <= n; ++i)
		for(int j = 1; j <= m; ++j)
			scanf("%lld", &a[i][j]);
	for(int i = 1; i <= n; ++i)
		for(int j = 1; j <= m; ++j)
		{
			if(i != 1) addset(a[i][j], a[i - 1][j]);
			if(j != 1) addset(a[i][j], a[i][j - 1]);
		}
	for(int i = 1; i <= c; ++i) st[i].insert(i);
	ll ans = 0;
	for(int i = 1; i <= c; ++i)
	{
		ll cur = ksm(st[i].size(), Mod - 2);
		(ans += cur) %= Mod;
	}
	ans = (c - ans + Mod) % Mod;
	if(ans == 0) ans = 1;
	printf("%lld", ans);
    return 0;
}


```

<hr>
