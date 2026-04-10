


# 网络流初步

<img width="712" height="634" alt="image" src="https://github.com/user-attachments/assets/987ca88e-e484-4d86-b789-e8dbf505110e" />
<img width="712" height="634" alt="image" src="https://github.com/user-attachments/assets/2a2f83c0-c66e-4a75-b414-b6c7acf2a821" />


## 最大流

<img width="732" height="713" alt="image" src="https://github.com/user-attachments/assets/0e0c9eed-ad4a-4787-9a30-b7128e92e4e2" />



### Edmonds-Karp增广路算法

<img width="713" height="609" alt="image" src="https://github.com/user-attachments/assets/b4fb1278-c638-4a8b-9955-dfec724331eb" />


<img width="665" height="64" alt="image" src="https://github.com/user-attachments/assets/ae64da76-ae6d-4a2a-9411-6ff9ed0ce062" />

[模版题目](https://www.acwing.com/problem/content/description/2173/)
```cpp
#include<bits/stdc++.h>
using namespace std;

const int N = 1010, M = 2e4 + 10, INF = 0x3f3f3f3f;

int n, m, S, T;
int h[N], ne[M], e[M], f[M], idx;
int q[N], d[N], pre[N];
bool st[N];

void add(int a, int b, int c) {
    e[idx] = b, f[idx] = c, ne[idx] = h[a], h[a] = idx++;
    e[idx] = a, f[idx] = 0, ne[idx] = h[b], h[b] = idx++;
}

bool bfs() {
    int hh = 0, tt = 0;
    memset(st, false, sizeof st);
    q[hh] = S;
    st[S] = true;
    d[S] = INF;
    while(hh <= tt) {
        int u = q[hh++];
        for (int i = h[u]; ~i; i = ne[i]) {
            int v = e[i];
            if (!st[v] && f[i]) {
                st[v] = true;
                d[v] = min(d[u], f[i]);
                pre[v] = i;
                if (v == T) return true;
                q[++tt] = v;
            }
        }
    }
    return false;
}

int EK() {
    int r = 0;
    while(bfs()) {
        r += d[T];
        for (int i = T; i != S; i = e[pre[i] ^ 1]) {
            f[pre[i]] -= d[T], f[pre[i] ^ 1] += d[T];
        }
    }
    return r;
}

int main() {
    cin >> n >> m >> S >> T;
    memset(h, -1, sizeof h);
    for (int i = 1; i <= m; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }
    cout << EK() << endl;
    return 0;
}
```

### Dinic 算法

<img width="657" height="620" alt="image" src="https://github.com/user-attachments/assets/a01900cc-c58d-48bb-9e33-e78715b9862f" />


* [模板题](https://www.acwing.com/problem/content/2174/)

```cpp
#include<bits/stdc++.h>
using namespace std;

const int N = 1e4 + 10, M = 2e5 + 10, INF = 1e8;

int n, m, S, T;
int h[N], e[M], ne[M], f[M], idx;
int q[N], d[N], cur[N];

void add(int a, int b, int c) {
    e[idx] = b, f[idx] = c, ne[idx] = h[a], h[a] = idx++;
    e[idx] = a, f[idx] = 0, ne[idx] = h[b], h[b] = idx++;
}

bool bfs() {
    memset(d, -1, sizeof d);
    int hh = 0, tt = 0;
    q[hh] = S, cur[S] = h[S], d[S] = 0;
    while(hh <= tt) {
        int u = q[hh++];
        for (int i = h[u]; ~i; i = ne[i]) {
            int v = e[i];
            if (d[v] == -1 && f[i]) {
                d[v] = d[u] + 1;
                cur[v] = h[v];
                q[++tt] = v;
                if (v == T) return true;
            }
        }
    }
    return false;
}

int find(int u, int limit) {
    if (u == T) return limit;
    int flow = 0;
    for (int i = cur[u]; ~i && flow < limit; i = ne[i]) {
        cur[u] = i; // 当前弧优化
        int v = e[i];
        if (d[v] == d[u] + 1 && f[i]) {
            int t = find(v, min(f[i], limit - flow));
            if (!t) d[v] = -1;
            f[i] -= t, f[i ^ 1] += t, flow += t;
        }
    }
    return flow;
}

int dinic() {
    int r = 0, flow;
    while(bfs()) while(flow = find(S, INF)) r += flow;
    return r;
}

int main() {
    cin >> n >> m >> S >> T;
    memset(h, -1, sizeof h);
    for (int i = 1; i <= m; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }
    
    cout << dinic() << endl;
    return 0;
}
```

## 题型
[参考](https://www.acwing.com/solution/content/24047/)

### 最大流之上下界可行流

1. 无源汇上下界可行流
[模版题目](https://www.acwing.com/problem/content/2190/)

2. 


## 二分图最大匹配的必须边和可行边

<img width="1" height="1" alt="image" src="https://github.com/user-attachments/assets/1e32e5bf-dec5-42ce-8a43-6d9e7ac7600a" />


**当最大匹配无需是完备时必须使用网络流求** 

<img width="564" height="546" alt="image" src="https://github.com/user-attachments/assets/e86c986e-b2a7-42cc-a4ee-3961d01d96b7" />


# 2.最小割

<img width="572" height="316" alt="image" src="https://github.com/user-attachments/assets/fef60b95-ac0c-44f0-8e46-0d03327af695" />


# 3. 费用流

<img width="581" height="554" alt="image" src="https://github.com/user-attachments/assets/27188b84-45ab-4738-b66e-c112c49b3d6b" />


# 补充内容
## 概念与性质

[网络流基本概念](https://www.acwing.com/file_system/file/content/whole/index/content/10718684/)







## acwing

```
1. 基本概念 
    1.1 流网络，不考虑反向边 
   
    1.2 可行流，不考虑反向边
       1.2.1 两个条件：容量限制、流量守恒
       1.2.2 可行流的流量指从源点流出的流量 - 流入源点的流量
       1.2.3 最大流是指最大可行流
   1.3 残留网络，考虑反向边，残留网络的可行流f' + 原图的可行流f = 原题的另一个可行流
       (1) |f' + f| = |f'| + |f|
       (2) |f'| 可能是负数
   1.4 增广路径
   1.5 割
       1.5.1 割的定义
       1.5.2 割的容量，不考虑反向边，“最小割”是指容量最小的割。
       1.5.3 割的流量，考虑反向边，f(S, T) <= c(S, T)
       1.5.4 对于任意可行流f，任意割[S, T]，|f| = f(S, T)
       1.5.5 对于任意可行流f，任意割[S, T]，|f| <= c(S, T)
       1.5.6 最大流最小割定理
           (1) 可行流f是最大流
           (2) 可行流f的残留网络中不存在增广路
           (3) 存在某个割[S, T]，|f| = c(S, T)
   1.6. 算法
       1.6.1 EK O(nm^2)
       1.6.2 Dinic O(n^2m)
   1.7 应用
       1.7.1 二分图
           (1) 二分图匹配
           (2) 二分图多重匹配
       1.7.2 上下界网络流
           (1) 无源汇上下界可行流
           (2) 有源汇上下界最大流
           (3) 有源汇上下界最小流
       1.7.3 多源汇最大流
```
