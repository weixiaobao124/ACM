---
#### <center>[CodeForces 1102F Elongated Matrix](https://codeforces.com/problemset/problem/1102/F)</cener>

**题意：**
给一个$n$行$m$列的矩阵，可以改变行的顺序。
从第一列到最后一列，每一列从第一行到最后一行取数，生成一个一维数组$s_{nm}$，请找到一个最大值$k$，使得$|s_i-s_{i + 1}| \geq k$。

**题解:**
基本思路：状压DP + 记忆化搜索

先预处理两行相邻差的最小绝对值和两行分别为首行和末行差绝对值的最小值
```cpp
for(int i = 0; i < n; i ++) {
	for(int j = 0; j < n; j ++) {
		nxt[i][j] = ht[i][j] = inf;
        for(int k = 0; k < m; k ++) 
			nxt[i][j] = min(nxt[i][j], abs(a[i][k] - a[j][k]));//相邻行间距
        for(int k = 0; k < m - 1; k ++) 
			ht[i][j] = min(ht[i][j], abs(a[i][k] - a[j][k + 1]));// 首行末尾行间距 
        }
    }
```
$dp[i][s]$表示当前行为$i$，状态为$s$绝对值差的最大值。


状态转移方程为$$dp[i][s]\;=\;max\lbrace dp[i][s]\;,\;min\lbrace nxt[i][j],\;dp[j][s\bigoplus(1<<i)\rbrace\;\rbrace$$


枚举行首和行尾，初始化

```cpp
for(int i = 0; i < n; i ++) {//枚举首行
	memset(dp, -1, sizeof(dp)); 
    for(int j = 0; j < n; j ++)
    	dp[j][1 << j] = 0;  
    dp[i][1 << i] = inf;
    for(int j = 0; j < n; j ++) //枚举尾行
    	ans = max(ans, min(ht[i][j], DFS(j, (1 << n) - 1)));
    }
```

记忆化搜索
$dfs(i,s)$返回当前行为$i$，状态为$s$的最大值

```cpp
int DFS(int i, int s) {
    if(dp[i][s] != -1) 
		return dp[i][s];
    dp[i][s] = 0;
    for(int j = 0; j < n; j ++) {
        if(j == i) 
			continue;
        if((1 << j) & s)//第j行在状态中
            dp[i][s] = max(dp[i][s], min(nxt[i][j], DFS(j, s ^ (1 << i))));
    }
    return dp[i][s];
}
```

**时间复杂度：**
$$O()$$
完整代码

```cpp
#include<bits/stdc++.h>

using namespace std;

const int M = 1e4 + 5, N = 16;

const int inf = 0x3f3f3f3f;

int n, m, ans;

int a[N][M], nxt[N][N], ht[N][N], dp[N][1 << N];



int DFS(int i, int s) {
    if(dp[i][s] != -1) 
		return dp[i][s];
    dp[i][s] = 0;
    for(int j = 0; j < n; j ++) {
        if(j == i) 
			continue;
        if((1 << j) & s)
            dp[i][s] = max(dp[i][s], min(nxt[i][j], DFS(j, s ^ (1 << i))));
    }
    return dp[i][s];
}

int main() {
    cin >> n >> m;
    for(int i = 0; i < n; i ++)
        for(int j = 0; j < m; j ++) 
			cin >> a[i][j];
    for(int i = 0; i < n; i ++) {// 相邻行间距，首行末尾行间距 
        for(int j = 0; j < n; j ++) {
            nxt[i][j] = ht[i][j] = inf;
            for(int k = 0; k < m; k ++) 
				nxt[i][j] = min(nxt[i][j], abs(a[i][k] - a[j][k]));
            for(int k = 0; k < m - 1; k ++) 
				ht[i][j] = min(ht[i][j], abs(a[i][k] - a[j][k + 1]));
        }
    }
    for(int i = 0; i < n; i ++) {//枚举首
        memset(dp, -1, sizeof(dp)); 
        for(int j = 0; j < n; j ++)
            dp[j][1 << j] = 0;  
        dp[i][1 << i] = inf;
        for(int j = 0; j < n; j ++) //枚举尾行
            ans = max(ans, min(ht[i][j], DFS(j, (1 << n) - 1)));
    }
    cout << ans << endl;
    return 0;
}

```

