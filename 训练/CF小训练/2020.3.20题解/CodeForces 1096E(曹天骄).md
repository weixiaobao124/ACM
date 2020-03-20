---
#### <center>[CodeForces 1096E The Top Scorer](http://codeforces.com/problemset/problem/1096/E)</cener>


**题意：**	 
共有p个球员，总分数为s，第一个人的分数至少为r，求第一个人的获胜概率$\frac{P}Q$。
当有人并列第一时，冠军将从他们中等概率随机选出。
$P$为第一个人获胜的方案数，$Q$为总方案数，输出${P}\cdot{Q^{-1}}( mod\;998244353)$

**题解：**
求总方案数$Q$，问题可以化为$s-r$个相同小球，放入$q$个不同盒子中，允许空盒的方案数。
设第一名选手得分为$x$，有$k$名选手并列第一，相当于从除第一个人之外的剩下$p-1$个人当中抽出$k-1$人。$(C_{k-1}^{p-1})$
剩下来的$p-k$个人总分数为$s-k*x$，且每人分数都小于$x$。
设$f(n, m, x)$为$n$个相同小球,放入$m$个不同盒子中，允许空盒且盒中小球数小于$x$的方案数。

枚举$x$和$k$就可求出方案$P$的总数。最后

需要用到的公式：
$$f(n,m,x)\,=\,\sum_{i=0}^{m}\,(-1)^i\;C_n^i\;C_{n-i(x+1)+m-1}^{m-1}$$
公式理解：
$f(n,m,x)\;=\;$至少$0$人超过$x\;-\;$至少$1$人超过$x\;+\;$至少$2$人超过$x\;-\;$至少$3$人超过$x\;+\;$…………

至少$i$人得分超过$x$的方案数理解为，从$p$个人中挑出$i$个人，给他们每人分配$x+1$分。($C_p^i)$
再将剩下的$s-i(x+1)$分，给$p$人，允许得分为$0$。$(C_{s-i(x+1)+p-1}^{m-1})$

$n$个相同小球放入$m$个不同的盒子，不允许空盒的方案数为$C_{n-1}^{m-1}$

$n$个相同小球放入$m$个不同盒子，允许空盒的方案数为$C_{n+m-1}^{m-1}$

**时间复杂度：**
$$O(s\cdot p^2)$$



```cpp
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

const int mod = 998244353;

const int N = 5e3 + 200;//数组开p+r，否则会RE


int p, s, r;

ll c[N][N];

ll ans;

void init() {//预处理组合数组 
	c[0][0] = 1;
	for(int i = 1; i < N; i ++) {
		c[i][0] = 1;
		for(int j = 1; j <= i; j ++)
			c[i][j] = (c[i - 1][j] + c[i - 1][j - 1]) % mod;
	}
}

ll KSM(ll a, ll b) {
	ll ret = 1LL;
	while(b) {
		if(b & 1)
			ret = ret * a % mod;
		a = a * a % mod;
		b >>= 1;
	}
	return ret;
}

ll C(int n, int m) {//组合数，不存在的情况返回0 
	if(n < 0 || m < 0 || n < m)
		return 0;
	return c[n][m];
}


ll calc(ll s, ll p, ll m) {//计算f(s,p,m)
    if(p == 0 && s == 0)
	    return 1;
    ll ret = 0;
    for(int i = 0, sign = 1; i <= p; i ++) {
        ret += sign * (C(p, i) * C(s + p - 1 - i * (m + 1), p - 1) % mod) % mod;
        ret = (ret + mod) % mod;
        sign *= -1;
    }
    return ret;
}

int main() {
	init();
    scanf("%d%d%d", &p, &s, &r);
    for(int x = r; x <= s; x ++)//枚举第一个人的分数 
	    for(int k = 1; k <= p; k ++){//枚举并列人数 
	        (ans += ((C(p - 1, k - 1) * KSM(k, mod - 2) % mod) * calc(s - k * x, p - k, x - 1)) % mod) %= mod;
	    }
    ans *= KSM(C(s + p - 1 - r, p - 1), mod - 2);//用逆元代替除法 
    ans %=  mod;
    printf("%lld", ans);
    return 0;
}
```






