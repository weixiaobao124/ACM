#### [洛谷T125991 树上启发式合并+trie+链表](https://www.luogu.com.cn/problem/T125991)

题意：

给定一棵 $n(1≤n≤10^5)$ 个点的树，每个点有一个权值。找到一对点$(u,v)$，使得 $u$ 到根节点和 $v$ 到根节点的路径上的所有点的并集的异或和最大。求这个最大的异或和值。

![img](https://cdn.luogu.com.cn/upload/image_hosting/v2ze33q4.png)



题解：

预处理每个点到达根节点的路径上的点权异或和 $val[i]$ 。

先固定分叉点 $t$ 作为某条路径的中间点，上图中所示为2号点。分叉点有若干棵子树，假设在某棵子树上选择一个点 $a$ ,另一棵子树上选择一个点 $b$，那么这条路径的异或和是 $val[a] \bigoplus val[b] \bigoplus val[t]$ 。

问题是如何选择 $(a,b)$ 。对于树上的每个点 $i$ 建一棵 $trie$ 树，每个 $trie$ 只存一个初始值为 $val[i]$ 。在以 $t$ 为根的树中，从某棵子树中选择一个 $a$ 后，再从 $t$ 的其它子树中再选一个 $b$ ，使得路径异或和最大。这个就可以用 $trie$ 解决，对于某棵子树中的任意点 $a$ ，将其它所有子树的点的 $val$ 合并成一个 $trie$ ，然后在这个 $trie$ 上面找和 $val[a] \bigoplus val[t]$ 异或起来最大的某个 $val$ 。

这个过程相当于两棵 $trie$ ，枚举其中一棵 $trie$ 中存的的 $val$ ，通过这个 $val$ 确定另一棵 $trie$ 中应该选择的 $val$ 。但是为了保证复杂度，选择包含点数较小的 $trie$ 进行枚举。这种操作类似于将两个集合合并，暴力将小的集合逐个插入至大的集合中，该过程复杂度为启发式合并的复杂度。

由于我们需要知道树上每个点的 $trie$ 中存了哪些值，而每次遍历 $trie$ 取值的复杂度是很高的，所以对于树上每个点额外开一个链表保存每个点的子树的所有 $val$ 。链表是 $O(n)$ 遍历， $O(1)$ 合并，所这里很适合用链表。

处理完以 $t$ 为根的树后，将 $t$ 的所有子树的 $trie$ 合并到一起，然后回溯到上一个根继续求解。



$trie$ 合并复杂度类似于线段树合并，总复杂度为 $O(32*n)$ 。

启发式合并（求解）过程总复杂度为 $O(nlog(n))$ 。（如果使用 $set$ 而不是链表合并两个集合，复杂度为 $O(nlog^2(n))$ ）

总复杂度为  $O(nlog(n))$ 。





```c++


#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 2e5 + 5;
const int M = 31;
int a[maxn];
struct node{
    int val;
    node *next;
    node(){next = NULL;}
};
vector<int> G[maxn];
void insert(int u, int v){
    G[u].push_back(v);
    G[v].push_back(u);
}
int n;
int ch[maxn * M][2], sz, siz[maxn];
int val[maxn];
void getval(int u, int fa){
    for(int v : G[u]){
        if(v == fa) continue;
        val[v] = val[u] ^ a[v];
        getval(v, u);
    }
}
void init(){
    memset(ch, 0, sizeof ch);
    sz = n + 1;
}
void ins(int rt, ll x){
    int u = rt;
    for(int i = M; i > -1; i--){
        bool b = x & (1ll << i);
        if(!ch[u][b]) ch[u][b] = sz++;
        u = ch[u][b];
    }
}
int merge(int p, int q){
    if(!p) return q;
    if(!q) return p;
    ch[p][0] = merge(ch[p][0], ch[q][0]);
    ch[p][1] = merge(ch[p][1], ch[q][1]);
    return p;
}
int findmx(int rt, int x){
    int u = rt;
    int ret = 0;
    for(int i = M; i > -1; i--){
        bool b = x & (1ll << i);
        if(ch[u][!b]) {
            u = ch[u][!b];
            if(!b) ret |= (1ll << i);
        }
        else {
            u = ch[u][b];
            if(b) ret |= (1ll << i);
        }
    }
    return ret;
}
int mx = 0;
node *head[maxn];
void dfs(int u, int fa){
    int pre = -1;
    for(int v : G[u]){
        if(v == fa) continue;
        dfs(v, u);
        if(pre == -1) {
            pre = v;
        }
    }
    if(pre == -1) return;
    for(int v : G[u]){
        if(v == fa || v == pre) continue;
        if(siz[pre] < siz[v]) swap(pre, v);
        node *p = head[v];
        mx = max(mx, findmx(pre, (p -> val) ^ val[u]) ^ (p -> val) ^ val[u]);
        while((p -> next) != NULL){
            p = p -> next;
            mx = max(mx, findmx(pre, (p -> val) ^ val[u]) ^ (p -> val) ^ val[u]);
        }
        p -> next = head[pre];

        siz[v] += siz[pre];

        pre = merge(v, pre);
    }
    head[u] -> next = head[pre];
    siz[u] += siz[pre];
    merge(u, pre);
}


int main(){
    scanf("%d", &n);
    for(int i = 1; i <= n; i++){
        scanf("%d", a + i);
    }
    for(int i = 1, u, v; i < n; i++){
        scanf("%d%d", &u, &v);
        insert(u, v);
    }
    val[1] = a[1];
    getval(1, 0);
    for(int i = 1; i <= n; i++) mx = max(mx, val[i]);
    init();
    for(int i = 1; i <= n; i++){
        ins(i, val[i]);
        siz[i]++;
        head[i] = new node();
        head[i] -> val = val[i];
    }
    dfs(1, 0);
    printf("%d\n", mx);

    return 0;
}

```





