---
tags: [排序, 中位数贪心]

---

七夕祭 https://www.acwing.com/problem/content/107/




### 题目描述
行和列不会相互干扰， 各自看成一个环
给你一个环，环上有 n 个位置，各个位置上有物品，求最少的移动物品次数使得每个位置的物品个数相等。

#### 样例

```
输入：
2 3 4
1 3
2 1
2 2
2 3

输出：
row 1
```

----------

## 算法1
##### 贪心
<img width="995" height="557" alt="image" src="https://github.com/user-attachments/assets/4efc4b1d-4e86-4e08-8df1-801317ce54ed" />
<img width="967" height="323" alt="image" src="https://github.com/user-attachments/assets/cf008cc3-dc2e-4d47-9925-e334f9be6487" />

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

