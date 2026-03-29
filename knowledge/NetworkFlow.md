


# 网络流初步

<img width="712" height="634" alt="image" src="https://github.com/user-attachments/assets/987ca88e-e484-4d86-b789-e8dbf505110e" />
<img width="712" height="634" alt="image" src="https://github.com/user-attachments/assets/2a2f83c0-c66e-4a75-b414-b6c7acf2a821" />


## 最大流

<img width="732" height="713" alt="image" src="https://github.com/user-attachments/assets/0e0c9eed-ad4a-4787-9a30-b7128e92e4e2" />



### Edmonds-Karp增广路算法

<img width="713" height="609" alt="image" src="https://github.com/user-attachments/assets/b4fb1278-c638-4a8b-9955-dfec724331eb" />


<img width="665" height="64" alt="image" src="https://github.com/user-attachments/assets/ae64da76-ae6d-4a2a-9411-6ff9ed0ce062" />


```cpp
const int INF = 0x3f3f3f3f
int n, m, s, t;
int h[N], e[M], ne[M], w[M], idx;
int v[N], incf[N], pre[N], maxflow;

void add(int a, int b, int c) {
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
    e[idx] = a, w[idx] = 0, ne[idx] = h[b], h[b] = idx++;
}

bool bfs() {
    memset(v, 0, sizeof v);
    queue<int> q;
    q.push(s); v[s] = 1;
    incf[s] = INF; // 增广路上各边的最小剩余容量
    while(!q.empty()) {
        int x = q.front(); q.pop();
        for (int i = h[x]; ~i; i = ne[i]) {
            if (w[i]) { // w[i] 表示 c - f的值
                int y = e[i];
                if (v[y]) continue;
                incf[y] = min(incf[x], w[i]);
                pre[y] = i; // 记录前驱，便于找到最长路的实际方案
                q.push(y);
                v[y] = 1;
                if (y == t) return 1;
            }
        }
    }
    return 0;
}

void update() { // 更新增广路及其反向边的剩余容量
    int x = t;
    while(x != s) {
        int i = pre[x];
        w[i] -= incf[t];
        w[i ^ 1] += incf[t]; // 利用“成对存储”的xor 1技巧
        x = e[i ^ 1];
    }
    maxflow += incf[t];
}
```

### Dinic 算法

<img width="657" height="620" alt="image" src="https://github.com/user-attachments/assets/a01900cc-c58d-48bb-9e33-e78715b9862f" />


* [模板题](https://www.acwing.com/problem/content/2174/)

```cpp
const int INF = 0x3f3f3f3f;
int n, m, s, t, maxflow;
int h[N], e[M], w[M], ne[M], idx; // w[i]: 容量-流量
int d[N], now[M]; // Now 当前弧优化
queue<int> q;
void add(int a, int b, int c) {
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx++;
    e[idx] = a, w[idx] = 0, ne[idx] = h[b],
    h[b] = idx++;
}

bool bfs() { // 在残量网络中构造分层图
    memset(d, 0, sizeof d);
    while(!q.empty()) q.pop();
    q.push(s);
    d[s] = 1; now[s] = h[s];
    while(!q.empty()) {
        int x = q.front(); q.pop();
        for (int i = h[x]; ~i; i = ne[i]){
            int v = e[i];
            if (w[i] && !d[v]) {
                q.push(v);
                now[v] = h[v];
                d[v] = d[x] + 1;
                if (v == t) return 1;
            }
        }
    }
    return 0;
}

int dinic(int x, int flow) { // 在当前分层图上增广
    if (x == t) return flow;
    int rest = flow, k, i;
    for (i = now[x]; ~i && rest; i = ne[i]) {
        now[x] = i; // 当前弧优化（避免重复遍历从x出发不可扩展的边）
        int v = e[i];
        if(w[i] && d[v] == d[x] + 1) {
            k = dinic(v, min(rest, w[i]));
            if (!k) d[v] = 0; // 剪枝，去掉增广完毕的点
            w[i] -= k;
            w[i ^ 1] += k;
            rest -= k;
        }
    }
    return flow - rest;
}

int main() {
    int flow = 0;
    while(bfs()) {
        while(flow = dinic(s, inf)) maxflow += flow;
    }
    cout << maxflow << endl;
}
```

## 二分图最大匹配的必须边和可行边

<img width="1" height="1" alt="image" src="https://github.com/user-attachments/assets/1e32e5bf-dec5-42ce-8a43-6d9e7ac7600a" />


**当最大匹配无需是完备时必须使用网络流求** 

<img width="564" height="546" alt="image" src="https://github.com/user-attachments/assets/e86c986e-b2a7-42cc-a4ee-3961d01d96b7" />


## 最小割

<img width="572" height="316" alt="image" src="https://github.com/user-attachments/assets/fef60b95-ac0c-44f0-8e46-0d03327af695" />


## 费用流

<img width="581" height="554" alt="image" src="https://github.com/user-attachments/assets/27188b84-45ab-4738-b66e-c112c49b3d6b" />


# 补充内容
## 概念与性质

[网络流基本概念](https://www.acwing.com/file_system/file/content/whole/index/content/10718684/)







## acwing

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
