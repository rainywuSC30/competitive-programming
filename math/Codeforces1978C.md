

Codeforces1978C https://codeforces.com/problemset/problem/1978/C




### 题目描述

<img width="933" height="648" alt="image" src="https://github.com/user-attachments/assets/3942bbe3-179c-48a7-8b95-4d6b4fc6191c" />


#### 样例

```
输入：
8
3 4
4 5
7 0
1 1000000000000
8 14
112 777
5 12
5 2

输出：
Yes
3 1 2
No
Yes
1 2 3 4 5 6 7
No
Yes
8 2 3 4 5 6 1 7
No
Yes
5 4 3 1 2
Yes
2 1 3 4 5
```

----------

## 算法1
##### 数学 + 构造

1. 曼哈顿距离最大是 排列正序和倒序之间的 距离
2. 交换每对下标的数，对答案的贡献一定是偶数
3. 对于特定的 值域内的任意偶数 k 一定可以构造出答案

<img width="940" height="131" alt="image" src="https://github.com/user-attachments/assets/1e48d533-e829-4d70-8989-06a2d27938bd" />


#### 时间复杂度

#### 参考文献

#### C++ 代码
```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long LL;

const int N = 1e5 + 10;

int n, m, t;
int row[N], col[N], s[N];

LL work(int n, int a[]) {
    if (t % n) return - 1;
    s[1] = 0;
    int avg = t / n;
    for (int i = 2; i <= n; i++) s[i] = s[i - 1] + (avg - a[i]);
    sort(s + 1, s + n + 1);
    int d = s[n / 2 + 1];
    LL res = 0;
    for (int i = 1; i <= n; i++) res += abs(d - s[i]);
    return res;
}

int main() {
    cin >> n >> m >> t;
    for (int i = 1; i <= t; i++) {
        int x, y;
        cin >> x >> y;
        row[x]++, col[y]++;
    }
    
    LL r = work(n, row);
    LL c = work(m, col);
    if (r != -1 && c != -1) printf("both %lld\n", r + c);
    else if (r != -1) printf("row %lld\n", r);
    else if (c != -1) printf("column %lld\n", c);
    else cout << "impossible" << endl;
    return 0;
}
```

