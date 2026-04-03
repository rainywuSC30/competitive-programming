
---
tags: [DSU]

---

[Make Bipartite 3](https://atcoder.jp/contests/abc451/tasks/abc451_f)



### 题目描述
<img width="1151" height="608" alt="image" src="https://github.com/user-attachments/assets/917de46a-f8ed-433d-852e-50ae42c36726" />



#### 样例

```
输入：
4 4
1 2
2 3
3 4
1 3

输出：
1
1
2
-1
```

----------

## 算法1
##### 带扩展域并查集
看到黑白染色 + 动态加边容易想到扩展域并查集。
每个结点分成两个，一个黑结点，一个白结点。每个连通块维护有几个黑结点和有几个白结点。很明显，一个连通块的贡献就是这两个值的 min。
那么对所有的连通块求和就行了吗？其实会发现，每个连通块都会有一个对称的连通块（黑点与白点互换），并且它们的贡献显然是相等的，所以求和后要除以 2。
另外，显然不用担心对称连通块和自身是同一个，因为一个结点的黑部分和白部分合并起来意味着无法染色，直接无解就是了。

#### 时间复杂度
O(nlogn)

#### 参考文献

#### C++ 代码
```cpp
#include<bits/stdc++.h>
using namespace std;

const int N = 4e5 + 10;

int n, q;
int p[N];
int cnt[N][2];
int res;

int find(int x) {
    if (p[x] != x) p[x] = find(p[x]);
    return p[x];
}

void merge(int x, int y) {
    int px = find(x), py = find(y);
    if (px == py) return;
    // 合并前，减去这两个独立集合原本的贡献
    res -= min(cnt[px][0], cnt[px][1]);
    res -= min(cnt[py][0], cnt[py][1]);
    // 合并
    p[px] = py;
    cnt[py][0] += cnt[px][0];
    cnt[py][1] += cnt[px][1];

    // 3. 合并后，加上新集合的贡献
    res += min(cnt[py][0], cnt[py][1]);
}

int main() {
    cin >> n >> q;
    for (int i = 1; i <= n; i++) {
        p[i] = i, p[i + n] = i + n;
        cnt[i][0] = 1, cnt[i + n][1] = 1;
    }
    bool flag = true;
    while(q--) {
        int a, b;
        cin >> a >> b;
        if (!flag) {
            cout << -1 << endl;
            continue;
        }
        int pa = find(a), pb = find(b);
        if (pa == pb) {
            flag = false;
            cout << -1 << endl;
            continue;
        }
        merge(a, b + n);
        merge(a + n, b);
        cout << res / 2 << endl;
    }
    return 0;
}
```

## 算法2
##### 带权并查集

#### 时间复杂度
O(nlogn)

#### 参考文献

#### C++ 代码
```cpp
#include<bits/stdc++.h>
using namespace std;

const int N = 2e5 + 10;

int n, q;
int p[N];
int d[N]; // 相对于根节点的颜色
int cnt[N][2];
int res;

int find(int x){
    if (p[x] != x) {
        int u = find(p[x]);
        d[x] ^= d[p[x]];
        p[x] = u;
    }
    return p[x];
}

int main() {
    cin >> n >> q;
    // 初始每个旗子为白色0
    for (int i = 1; i <= n; i++) p[i] = i, cnt[i][0] = 1;
    bool flag = true;
    while(q--) {
        int a, b;
        cin >> a >> b;
        if (!flag) {
            cout << -1 << endl;
            continue;
        }
        int pa = find(a), pb = find(b);
        
        if (pa == pb) {
            if ((d[a] ^ d[b]) == 0) {
                flag = false;
                cout << -1 << endl;
                continue;
            }
            else cout << res << endl;
        }
        else {
            // 减去之前的贡献
            res -= min(cnt[pa][0], cnt[pa][1]);
            res -= min(cnt[pb][0], cnt[pb][1]);

            p[pa] = pb;
            d[pa] = d[a] ^ d[b] ^ 1;
            // 颜色不同，pa白变黑，黑变白
            if (d[pa]) {
                cnt[pb][0] += cnt[pa][1];
                cnt[pb][1] += cnt[pa][0]; 
            }
            else {
                cnt[pb][0] += cnt[pa][0];
                cnt[pb][1] += cnt[pa][1]; 
            }
            res += min(cnt[pb][0], cnt[pb][1]);
            cout << res << endl;
        }
    }
    return 0;
}
```
