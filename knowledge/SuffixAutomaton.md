# 后置自动机性质
1. SAM是个状态机，一个起点，若干终点，原串的所有子串和从 SAM 起点开始的所有路径一一对应，不重不漏。所以终点就是包含后缀的点
2. 每个点包含若干子串，每个子串都一一对应一条从起点到改点的路径。且这些子串一定是里面最长子串的连续后缀
3. SAM问题中经常考虑两种边：
  （1）普通边，类似于trie。表示在某个状态所表示的所有子串的后面添加一个字符
  （2）Link，Father。表示将某个状态所表示的最短子串的首字母删除。这类边构成一棵树

# SAM的构造思路
1. endpos(s)：子串 s 所有出现的位置（尾字母下标）集合。SAM 中的每个状态都一一对应一个 endpos的等价类
2. endpos的性质：
   （1）令 s1， s2 为 S 的两个子串，不妨设 $|s1| < |s2|$ 则 s1 是 s2 的后缀当且仅当 $endpos(s1)\supseteq endpos(s2)$ s1 不是 s2 的后缀当且仅当 $endpos(s1)\cap endepos(s2)=\emptyset$
    (2)两个不同子串的endpos，要么有包含关系，要么没有交集
   （3）两个子串的endpos相同，那么短串为长串的后缀
   （4）对于一个状态 st，以及任意的 longest(st) 的后缀 s，如果 s 的长度满足 $|shortest(st)| \leq |s| \leq |longest(st)|$ ，那么 $s\in substrings(st)$

# 时间复杂度 
  O(n)
  空间要开 2 n

# 图

<img width="1088" height="700" alt="image" src="https://github.com/user-attachments/assets/153c89f4-d9bc-49b5-b127-d875bda45c76" />

# 模版题
(https://www.acwing.com/problem/content/2768/)
```cpp
#include<bits/stdc++.h>
using namespace std;
#define endl '\n'
#define x first
#define y second
typedef long long LL;
typedef pair<int, int> PII;

const int N = 2e6 + 10, M = 4e5 + 10, INF = 0x3f3f3f3f;
// 状态指endpos值
// 起点为1
int tot = 1 /*endpos*/, last = 1;
struct Node {
    int len/*状态最长串长度*/, fa/*状态父节点*/;
    int ch[26];
} node[N];
char str[N];
LL f[N]/*状态集合大小*/, ans;
int h[N], e[N], ne[N], idx; // fa树的反向边 (父 -> 子)

void extend(int c) {
    int p = last, np = last = ++tot;
    f[tot] = 1;
    node[np].len = node[p].len + 1;
    for (; p && !node[p].ch[c]; p = node[p].fa) node[p].ch[c] = np;
    if (!p) node[np].fa = 1;
    else {
        int q = node[p].ch[c];
        if (node[q].len == node[p].len + 1) node[np].fa = q;
        else {
            int nq = ++tot;
            node[nq] = node[q], node[nq].len = node[p].len + 1;
            node[q].fa = node[np].fa = nq;
            for (; p && node[p].ch[c] == q; p = node[p].fa) 
                node[p].ch[c] = nq;
        }
    }
} 

void add(int a, int b) {
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

void dfs(int u) {
    for (int i = h[u]; ~i; i = ne[i]) {
        dfs(e[i]);
        f[u] += f[e[i]];
        if (f[u] > 1) ans = max(ans, f[u] * node[u].len);
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin >> str;
    for (int i = 0; str[i]; i++) extend(str[i] - 'a');
    memset(h, -1, sizeof h);
    for (int i = 2; i <= tot; i++) add(node[i].fa, i);
    dfs(1);
    cout << ans << endl;

    return 0;
}

```

