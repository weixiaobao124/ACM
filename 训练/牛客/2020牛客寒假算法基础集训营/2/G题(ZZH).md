#### <center>G题 判正误 </center>

**题意：** 给定 $a, b, c, d, e, f, g$,问方程$a^d+b^e+c^f=g$ 是否成立。

最多1000组数据

$-10^9\leq a, b, c, g\leq10^9,0\leq d,e,f\leq 10^9$

**题解：**

两边同时对一个数取模，最好是素数，如果等式两边的正余数不相等那么一定无解，否则可能有解。

由于方程值的分布非常离散，没有任何规律，只有当它有解的时候对任意取模都成立，否则只要多取几个不同的模就一定能判定它无解。

**时间复杂度：**  $O(C*logN)$ $C$ 为取不同模的数量。

**AC代码：**

```c++
#include<bits/stdc++.h>
using namespace std;   
typedef long long ll;
const int N = 2e5 + 5;
const int M = 1e6 + 5;
const ll Mod = 1e9 + 7;
typedef pair<ll, ll> pll;
ll base[] = {998244353,44701,44711,44729,44741,44753,44771,44773};
ll a, b, c, d, e, f, g;
ll ksm(ll a, ll k, ll mod)
{
	ll res = 1;
	while(k)
	{
		if(k & 1) (res *= a) %= mod;
		(a *= a) %= mod;
		k >>= 1;
	}
	return res;
}
ll ans[4];
int main()
{
	ll T;
	cin >> T;
 
	while(T--)
	{
		cin >> a >> b >> c >> d >> e >> f >> g;
		bool flag = 1;
		for(int i = 0; i < 8; ++i)
		{
			ans[0] = ksm(a, d, base[i]);
			ans[1] = ksm(b, e, base[i]);
			ans[2] = ksm(c, f, base[i]);
			ans[3] = -g % base[i];
			ll cur = 0;
			for(int i = 0; i < 4; ++i) cur += ans[i];
			if(cur % base[i]) flag = 0;
		}
		if(flag) puts("Yes");
		else puts("No");
	}
    return 0;
}

```



