#### <center>F题 拿物品</center>

**题意：**

 有 $N$ 件物品，每件物品有两个属性值：$a_i,b_i$ 。两个人轮流拿物品，彼此希望自己的分数减去对方的分数的值最大化，问最后两个人取物品方案的可能集合。

$2\leq N\leq 2*10^5,0\leq a_i \le b_i\leq10^9$

**题解：**

设 $X$ 是第一个人选用的物品集合中的**任意**一件物品的编号。

设 $Y$ 是第二个人选用的物品集合中的**任意**一件物品的编号。

由于选法最优：$X_a-Y_b\geq Y_a-X_b$ 任意交换两个物品第一个人都没有办法优化答案。

通过移项：$X_a+X_b\geq Y_a+Y_b$

会发现对于第一个人来说 $X_a+X_b$ 越大是越优秀的，同样对第二个人也是。

定义新的关键字排序以后直接按顺序轮流取即可。

**时间复杂度：** $O(NlogN)$

**AC代码：**

```c++
#include<bits/stdc++.h>
using namespace std;   
typedef long long ll;
const int N = 2e5 + 5;
const int M = 1e6 + 5;
const ll Mod = 1e9 + 7;
typedef pair<ll, ll> pll;
#define xx first
#define yy second
pll x[N];
ll a[N], b[N];
vector<int> ans[2];
int n;
int main()
{
	cin >> n;
	for(int i = 1; i <= n; ++i) cin >> a[i];
	for(int i = 1; i <= n; ++i) cin >> b[i];
	for(int i = 1; i <= n; ++i)
	{
		x[i].xx = a[i] + b[i];
		x[i].yy = i;
	}
	sort(x + 1, x + n + 1);
	int cnt = 0;
	for(int i = n; i >= 1; --i)
	{
		ans[cnt % 2].push_back(x[i].yy);
		cnt++;
	}
	for(auto i : ans[0]) printf("%d ", i);
	puts("");
	for(auto i : ans[1]) printf("%d ", i);
    return 0;
}
```

