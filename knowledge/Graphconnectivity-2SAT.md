# 1. 双连通分量


## 概念和定义

* 无向图的双连通：
  * 图中图论删除那个点和边，任意两点仍然联通，则该图为双连通图
  * 不存在**割点**，则称为“点双连通图”
  * 不存在**桥**，则称为“边双连通图”
* 对于不是双连通的图，它的极大的双连通子图称为**双连通分量**
  * 极大 指的是该双连通子图不能再扩展了，否则变为非双连通
  * **v-DCC**： 极大点的双连通分量
  * **e-DCC**：极大边的双连通分量

> 定理：
>
> 1. 一张无向连通图是“点双连通图”，当且仅当满足下列两个条件之一：
>    1. 图的顶点数不超过2
>    2. 图中**任意两点**同时包含在至少一个简单环中，其中“简单环”指的是不自交的环，也就是我们通常画出的环
> 2. 一张无向联通图是“边双连通图”，当且仅当**任意一条边**都包含在至少一个简单环中

## 点双
<img width="762" height="348" alt="image" src="https://github.com/user-attachments/assets/f1aaf66f-d4f2-4741-9c13-5e63577eaf2c" />

### 题目链接

https://www.acwing.com/problem/content/367/

https://www.acwing.com/problem/content/398/

```cpp
void tarjan(int x) {
    dfn[x] = low[x] = ++num;
    stk[++top] = u;
    if (root == x && h[x] == -1) { // 孤立点
        ++dcc_cnt;
        dcc[dcc_cnt].push_back(x);
        return;
    }
    int flag = 0; // 判断根是否是割点
    for (int i = head[x]; i; i = nxt[i]) {
        int v = ver[i];
        if (!dfn[v]) { // 树边
            tarjan(v);
            low[x] = min(low[x], low[v]);
            if (dfn[x] <= low[v]) {
                flag++;
                if (x != root || flag > 1) cut[x] = true;
                dcc_cnt++;
                int y;
                do {
                    y = stk[top--];
                    dcc[dcc_cnt].push_back(y);
                } while(y != v);
                dcc[dcc_cnt].push_back(x);
            }
        }
        else low[x] = min(low[x], dfn[v]);
    }
}
```

注意：孤立点不是割点

## 边双
<img width="732" height="503" alt="image" src="https://github.com/user-attachments/assets/6a3e53c1-b775-4a25-9e56-6d370fd03bc4" />


### 题目链接
https://www.acwing.com/problem/content/366/
```cpp
void tarjan(int u, int from) {
    dfn[u] = low[u] = ++timestamp;
    stk[++top] = u;
    for (int i = h[u]; ~i; i = ne[i]) {
        int v = e[i];
        if (!dfn[v]) {
            tarjan(v, i);
            low[u] = min(low[u], low[v]);
            if (dfn[u] < low[v]) {
                is_bridge[i] = is_bridge[i ^ 1] = true;
            }
        }
        else if (i != (from ^ 1)) {
            low[u] = min(low[u], dfn[v]);
        }
    }
    // 连通块
    if (dfn[u] == low[u]) {
        int y;
        ++dcc;
        do {
            y = stk[top--];
            id[y] = dcc;
        } while(y != u);
    }
}
```
# 2. 强联通分量
## 有向图的边
![DFS 生成树](https://oi-wiki.org/graph/images/dfs-tree.svg)

有向图的DFS生成树主要有4种边：

1. 树边：图中黑色，
2. 后向边：图中红色，指向祖先节点的边
3. 横插边：图中蓝色，搜索过程中遇到的访问过的边，但是这个节点并不是当前节点的祖先
4. 前向边：图中绿色，搜索过程种遇到子树中的节点

## 有向图的SCC

* 给定一张有向图，若对于图中任意两个节点 x，y，既存在从x到y的路径，也存在从y到x的路径，则称该有向图是“强连通图”
* 有向图的**极大**强连通子图被称为“强连通分量”，简记为SCC。
  * 极大含义和双连通分量中极大类似
* 一个环一定是强连通图。
  * 前向边没什么用处，
  * 后向边非常有用，应为可以构成环
  * 横插边视情况而定：
    * 如果从 y 出发能够找到一条路径回到x 的祖先节点，那么该边有用


## 强连通分量的判定法则

* 在追溯计算过程中，若从x回溯前，有$low[x] = dfn[x]$ 成立，则栈中从 x 到栈顶的所有节点构成一个SCC

### tarjan

```cpp
int dfn[N], low[N], timestamp;
int stk[N], top;
bool in_stk[N];
int id[N], scc_cnt; // 结点 i 所在SCC的编号
int sz[N]; // 强连通 i 的大小
void tarjan(int u) {
    dfn[u] = low[u] = ++timestamp;
    stk[++tp] = u, in_stk[u] = 1;
    for (int i = h[u]; ~i; i = ne[i]) {
        int v = e[i];
        if (!dfn[v]) {
            tarjan(v);
            low[u] = min(low[u], low[v]);
        }
        else if (in_stk[v]) {
            low[u] = min(low[u], dfn[v]); // 注意， low[u] 表示 u 能回溯到的最早祖先
        }
    }
    if (dfn[u] == low[u]) {
        ++scc_cnt;
        int y
        do {
            y = stk[top--];
            id[y] = scc_cnt;
            sz[scc_cnt]++;
            in_stk[y] = 0;
        } while (y != u);
    }
}

```
## 有向图的必经点和必经边

<img width="581" height="581" alt="image" src="https://github.com/user-attachments/assets/64040f19-5cd2-4ff0-a1b4-d1167c5486ea" />


# 3. 2-SAT

<img width="627" height="639" alt="image" src="https://github.com/user-attachments/assets/5a29d1e7-b5b5-410f-a9f1-48b4c8780246" />


<img width="779" height="556" alt="image" src="https://github.com/user-attachments/assets/bb98f42a-dc16-4b39-a4b1-055db6531838" />


<img width="756" height="464" alt="image" src="https://github.com/user-attachments/assets/25310ff5-aab4-433a-9765-8d43bf5f8450" />



# 参考题目 

1. [受欢迎的牛](https://www.acwing.com/problem/content/description/1176/)
2. [学校网络](https://www.acwing.com/problem/content/description/369/)
3. [最大半连通子图](https://www.acwing.com/problem/content/description/1177/)
4. [银河](https://www.acwing.com/problem/content/description/370/)
5. [强联通](https://www.acwing.com/problem/content/description/4451/)

## 桥

1. [冗余路径](https://www.acwing.com/problem/content/description/397/)

## 割点

1. [电力](https://www.acwing.com/problem/content/1185/)
2. [矿场搭建](https://www.acwing.com/problem/content/description/398/)
3. 











