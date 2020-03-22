<hr>


#### <center>[D题 God J and Decryption](https://www.luogu.com.cn/problem/T125994?contestId=27041)</center>

**题意：**

 给定 $N$ 个数构成一个环，每次可以使连续的 $K$ 个数增加 $1$ ，问是否能够吧所有的数变成 $0$

$1\leq K\leq N\leq 10^5$

**题解：**

进行环状差分，可以把区间修改操作变为环状差分数组上的两次单点修改：
$$
[L,(L+K)\%n]+1\iff Gap[L]+1,Gap[(L+K)\%n]-1
$$
通过不断增加 $K$ 可以得到若干个子环，不难发现问题转化成：

给定一个环，每次可以使一个数增加 $1$ ，使其顺时针的下一个数减少 $1$， 是否能够把环上所有的数变成 $0$。

环上的改变量是 $0$ 这意味着如果原先环上的值的和如果不能被 $10$ 整除，那么一定不能全部变成 $0$。

反之则一定可以。

暴力跑一遍所有环，如果所有环都能清零那么答案为`Yes`

否则答案为`No`

**时间复杂度：** $O(N)$

**AC代码：**

```c++
#include<bits/stdc++.h>
using namespace std;   
const int N = 1e5 + 5;
int n, k;
char a[N];
int b[N];
int r[N];
int dt;
bool vis[N];
bool solve(int x)
{
	int sum = 0;
	while(vis[x] == 0)
	{
		vis[x] = 1;
		sum += b[x];
		x = (x + k) % n;
	}
	return sum % 10 == 0;
}
int main()
{
	scanf("%d%d%s", &n, &k, a);
	if(k == 1)
	{
		puts("Yes");
		return 0;
	}
	b[0] = a[0] - a[n - 1];
	for(int i = 1; i < n; ++i) b[i] = a[i] - a[i - 1];
	bool f = 1;
	for(int i = 0; i < n; ++i)
	{
		if(vis[i]) continue;
		if(solve(i) == 0) f = 0;
	}
	if(f == 0) puts("No");
	else puts("Yes");
    return 0;
}
```



<hr>
