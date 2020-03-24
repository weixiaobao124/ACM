#### <center>C题 算概率</center>

**题意：** 

有 $N$ 道题， 每道题有 $P_i$ 的几率答对(逆元形式)，问恰好有 $0,1,2,3,···，N$ 题答对的概率。

$1\leq N \leq2000$

 **题解：**

定义 $DP[i][j]$ 表示恰好有 $i$ 题答对，且 $j$ 答错的概率。

定义 $ANS[i]$ 表示恰好有 $i$  题答对的概率。

假设我们可以维护 $DP$ 使之正确，我们尝试计算 $ANS$ :

在计算$ANS[i]$ 时，我们可以从$DP[i - 1][] $ 转移过来，我们以最后一题答对的题目是哪一题来划分集合。

于是集合被划分成了 $N$ 个集合，新答对第 $j$ 题对答案的贡献为 $DP[i-1][j] * (1-P_j)^{-1} * P_j$

此时 $ANS[i]$ 的答题方案集合的大小都是 $i$ 证明其中每个问题都能被作为最后一题而被计算，于是每个集合都会被计算 $i$ 次，所以最后的 $ANS[i]$ 应该乘以 $i^{-1}$。

我们得到了 $ANS[i]$ 以后就可以根据 $DP$ 的定义去正确维护它。

$DP[i][j]$ 是恰好答对 $i$ 题且第 $j$ 题没有答对的概率，而 $ANS[i]$ 的定义是恰好答对 $i$ 题的概率。

从 $ANS[i]$ 中扣掉第 $j$ 题答对的方案集合即可得到 $DP[i][j]$。

有一点要注意的是用该方法不能直接做，因为会有 $P_i = 1$ 的情况出现，适当维护即可。

**时间复杂度：** $O(N^2)$

**AC代码：**

```c++
#include<bits/stdc++.h>
using namespace std;   
typedef long long ll;
const int N = 2e3 + 5;
const int M = 1e6 + 5;
const ll Mod = 1e9 + 7;
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
ll n, cnt;
ll inv[N];
ll good[N];
ll bad[N];
ll dp[N][N];
ll ans[N];
int main()
{
	cin >> n;
	for(int i = 1; i <= n; ++i)
    {
		ll x;
        cin >> x;
        if(x == 1)
        {
            cout << "0 ";
            continue;
        }
        good[++cnt] = x;
        bad[cnt] = (1 - x + Mod) % Mod;
        inv[cnt] = ksm(bad[cnt], Mod - 2);
    }
    if(cnt == 0)
    {
        cout << "1";
        return 0;
    }
    ll cur = 1;
    for(int i = 1; i <= cnt; ++i) (cur *= bad[i]) %= Mod;
	ans[0] = cur;
	for(int i = 1; i <= cnt; ++i)
	{
		dp[0][i] = cur;
	}
	for(int i = 1; i <= cnt; ++i)
	{
		ans[i] = 0;
		for(int j = 1; j <= cnt; ++j)
		{
			(ans[i] += dp[i - 1][j] * inv[j] % Mod * good[j] % Mod) %= Mod;
		}
		(ans[i] *= ksm(i, Mod - 2)) %= Mod;
		for(int j = 1; j <= cnt; ++j)
		{
			dp[i][j] = (ans[i] - dp[i - 1][j] * inv[j] % Mod * good[j] % Mod + Mod) % Mod;
		}
	}
	for(int i = 0; i <= cnt; ++i) cout << ans[i] << " ";
    return 0;
}

```
