#### <center>[洛谷T125995 God J and Eel](https://www.luogu.com.cn/problem/T125995)</center>

<hr>


**题意：**

&nbsp;有$n$个人和$q$次询问，每秒进行一次询问。

&nbsp;$q(1,a,s,b)$表示第$a$个人在第$b$秒的状态为$s$，过了第$b$秒该状态过期。

&nbsp;$q(2,a)$表示询问在当前时间第$a$个人最近一次的状态$s$，如果第$a$个人状态全部过期或者没有状态，则输出$＂happy＂$。

&nbsp;$q(3,a,s)$表示询问在当前时间第$a$个人的状态s是否过期，过期输出$＂GG＂$，否则输出$＂OK＂$。

**数据范围：**

&nbsp;$1 \leq   n,m \leq   1000 $

**题解：**

&nbsp;直接模拟。

**代码**：

```cpp
#include <bits/stdc++.h>
#define mp make_pair
using namespace std;

const int N = 1e3 + 10;

int n, m, op, a, b;

string s;

set <int> st[N];//存放每个人存在状态的时间
set <int> :: iterator it;

string p[N][N];//p[i][j]表示第i个人第j秒的状态

map < pair<int, string>, int > tim;//表示第i个人状态s的时间

int main() {
	cin >> n >> m;
	for(int i = 1; i <= m; i ++) {
		cin >> op;
		if(op == 1) {
			cin >> a >> s >> b;
			st[a].insert(b);//第a个人在第b秒有状态
			p[a][b] = s;//第a个人在第b秒的状态s
			tim[mp(a, s)] = b;//第a个人状态为s的时间
		}
		else if(op == 2) {
			cin >> a;
			it = st[a].lower_bound(i);//二分查找时间

			if(it == st[a].end())
				puts("Happy");
			else
				cout << p[a][*it] << endl;
				
		}
		else if(op == 3) {
			cin >> a >> s;
			if(tim[mp(a, s)] < i)
				puts("GG");
			else
				puts("OK");
		}
	}
	return 0;
}
```
<hr>



**复杂度：**

#### $$O(m\cdot    log_{2}m)$$
