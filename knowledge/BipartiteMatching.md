[TOC]

# 二分图

## 二分图定义

二分图又称为二部图 Bipartite graph

* 节点由两个集合组成，且两个集合内部没有边的图



## 二分图性质

* 如果两个集合中的点分别染成黑色和白色，可以发现二分图中的每一条边都一定是链接一个黑色点和白色点
* 定理：一张无向图是二分图，当且仅当图中**不存在长度为奇数的环**
  * 因为每一条边都是从一个集合走到另一个集合，只有走偶数次才可能回到同一个集合。

## 二分图判定

使用 dfs 或者 bfs 遍历图，如果发现了奇数环，那么就不是二分图，否则是。

>步骤：
>
>1. 开始对任意一未染色的顶点染色
>2. 判断其相邻的顶点中，若未染色则将其染上和相邻点不同的颜色
>3. 若已经染色且颜色和相邻点的颜色相同则说明不是二分图，如颜色不同则继续判断
>4. bfs或dfs遍历图
>5. 时间复杂度$O(n + m)$

```cpp
int n; 
int h[N], e[M], ne[M], idx;
int color[N]; // 表示每个点的颜色，0表示未染色，1表示白色，2表示黑色

// 参数：u表示当前节点，c表示当前点的颜色
bool dfs(int u, int c) {
    color[u] = c;
    for (int i = h[u]; i != -1; i = ne[i]) {
        int j = e[i];
        if (!color[j]) {
            if (!dfs(j, 3 - c)) return false;
        }
        else if (color[j] == c) return false;
    }
    return true;
}

bool check() {
    memset(color, -1, sizeof color);
    bool flag = true;
    for (int i = 1; i <= n; i++)
        if (color[i] == -1)
            if (!dfs(i, 0)) {
                flag = false;
                break;
            }
    return flag;
}
```

<img width="1100" height="440" alt="image" src="https://github.com/user-attachments/assets/cc4922c5-a034-4fb6-a895-12904d9c2a11" />


## 二分图的匹配

<img width="625" height="293" alt="image" src="https://github.com/user-attachments/assets/714394a7-5b4e-42d9-922b-01f68e922d0d" />


### 匈牙利算法

* 实际上时间复杂度远小于 $O(mn)$

```cpp
int n1, n2; // n1 表示第一个集合中的点数，n2表示第二个集合中的点数
int h[N], e[M], ne[M], idx; // 邻接表存储所有边，匈牙利算法中只会用到从第一个集合到第二个集合的边，所以这里只用存一个方向的边
int match[N]; // 存储第二个集合中的每个点当前匹配的第一个集合中的点是那个
bool st[N]; // 表示第二个集合中的每个点是否已经被遍历过

bool find(int x) {
    for (int i = h[x]; i != -1; i = ne[i]) {
        int j = e[i];
        if (!st[j]) {
            st[j] = true;
            if (match[j] == 0 || find(match[j])) {
                match[j] = x;
                return true;
            }
        }
    }
    return false;
}

// 求最大匹配数，一次枚举第一个集合中的每个点能否匹配第二个集合中的点
int res = 0;
for (int i = 1; i <= n1; i++) {
    memset(st, false, sizeof st);
    if (find(i)) res++;
}
```

## 二分图的多重匹配

<img width="563" height="414" alt="image" src="https://github.com/user-attachments/assets/b55f13de-6123-465f-9e78-f69ce193debb" />


## 二分图的带权匹配

​	给定一张二分图，二分图的每条边都带有一个权值，求出该二分图的一组最大匹配，使得匹配的权值总和最大。这个问题称为二分图的带权最大匹配，也称二分图最优匹配。注意，**二分图带权最大匹配的前提是匹配数最大，然后再最大化匹配边的权值总和**。

​	二分图带权最大匹配有两种解法：费用流和KM算法。本章内容介绍KM算法。

​	KM算法实现简单，再稠密图上的效率一般高于费用流。不过，KM算法有很大的局限性，**只能再满足“带权最大匹配一定是完备匹配”的图中正确求解**。 所以一般情况下，最好用费用流计算二分图带权最大匹配

（一般左部n不大，右部m很大，n都能匹配，KM有优势）

​	以 二分图左，右两部分节点数目为N为例



<img width="550" height="386" alt="image" src="https://github.com/user-attachments/assets/d56c3475-d1b7-4494-b978-7f0fdcb46372" />


### KM 算法

<img width="545" height="189" alt="image" src="https://github.com/user-attachments/assets/164df73d-5ba7-4251-864c-0dd189ef96df" />


<img width="549" height="564" alt="image" src="https://github.com/user-attachments/assets/03171e98-1caf-423c-ae68-a0859ef4445a" />


```cpp

const int N = 105;
int w[N][N]; // 边权
int la[N], lb[N]; // 左，右部点的顶标
bool va[N], vb[N]; // 访问标记：是否再交错树中
int match[N]; // 右部点匹配了哪一个左部点
int n, delta, upd[N];
bool dfs(int x, int fa) {
    va[x] = 1;
    for (int y = 1; y <= n; y++) {
        if (!vb[y]) {
            if (la[x] + lb[y] - w[x][y] == 0) {
                // 相等子图
                vb[y] = 1; last[y] = fa;
                if (!match[y] || dfs(match[y], y)) {
                    match[y] = x;
                    return true;
                }
                else if (upd[y] > la[x] + lb[y] - w[x][y]) {
                    upd[y] = la[x] + lb[y] - w[x][y];
                    last[y] = fa;
                }
            }
        }
    }
    return false;
}

long long KM() {
    for (int i = 1; i <= n; i++) {
        la[i] = -(1 << 30);
        lb[i] = 0;
        for (int j = 1; j <= n; j++) {
            la[i] = max(la[i], 1ll * w[i][j]);
        }
    }
    for (int i = 1; i <= n; i ++) {
        memset(va, 0, sizeof va);
        memset(vb, 0, sizeof vb);
        memset(last, 0, sizeof last);
        memset(upd, 0x7f, sizeof upd);
        // 从右部点st匹配的左部点开始dfs，一开始假设有一条0-i的匹配
        int st = 0; match[0] = i;
        while(match[st]) { // 当达到一个非匹配点st时停止
            long long delta = 1ll << 60;
            if (dfs(match[st]. st)) break;
            for (int j = 1; j <= n; j++) {
                if (!vb[j] && upd[j] < delta) {
                    delta = upd[j];
                    st = j; // 下一次直接从最小边权开始dfs
                }
            }
            for (int j = 1; j <= n; j++) { // 修改顶标
                if (va[j]) la[j] -= delta;
                if (vb[j]) lb[j] += delta;
                else upd[j] -= delta;
            }
            vb[st] = 1;
        }
        while(st) { // 倒推更新增广路
            match[st] = match[last[st]];
            st = last[st];
        }
    }
    long long ans = 0;
    for (int i = 1; i <= n; i++) ans += w[match[i]][i];
    return ans;
}
```





## 二分图的覆盖与独立集



<img width="581" height="160" alt="image" src="https://github.com/user-attachments/assets/7d7f321b-f4ae-4e57-8ebd-c2b9c76358e8" />


## 二分图的最大独立集

![image-20251227113117707](pics/image-20251227113117707.png)

![image-20251227114337778](pics/image-20251227114337778.png)

## 有向无环图的最小路径点覆盖

<img width="1305" height="345" alt="image" src="https://github.com/user-attachments/assets/b0a653a4-340d-4bf6-8961-261bfb35491f" />


<img width="572" height="223" alt="image" src="https://github.com/user-attachments/assets/e1b58f8e-21b1-43c6-bc92-e4fbcef84c5b" />


<img width="552" height="404" alt="image" src="https://github.com/user-attachments/assets/93baf6fa-d210-42fb-bc05-3865ec382a54" />


<img width="565" height="234" alt="image" src="https://github.com/user-attachments/assets/100994a8-b6b0-4476-8925-e26caa585141" />




# 参考题目

## 染色匹配

1. [关押罪犯](https://www.acwing.com/problem/content/description/259/)

## 匈牙利算法

1. [棋盘覆盖](https://www.acwing.com/problem/content/374/)
2. [机器任务](https://www.acwing.com/problem/content/378/)
3. [整除](https://www.acwing.com/problem/content/description/4240/)
   最大独立集
4. 





