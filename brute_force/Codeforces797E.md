[根号分治]: # (推荐题解模板，请替换blablabla等内容 ^^)

### 难度分：2000



### 题目描述

*a* is an array of *n* positive integers, all of which are not greater than *n* .

You have to process *q* queries to this array. Each query is represented by two numbers *p* and *k* . Several operations are performed in each query; each operation changes *p* to *p*+*$a_p$*+*k* . There operations are applied until *p* becomes greater than *n* . The answer to the query is the number of performed operations.


Input
The first line contains one integer n (1 ≤ n ≤ 100000).
The second line contains n integers — elements of a (1 ≤ ai ≤ n for each i from 1 to n).
The third line containts one integer q (1 ≤ q ≤ 100000).
Then q lines follow. Each line contains the values of p and k for corresponding query (1 ≤ p, k ≤ n).

#### 样例

```
输入：
3
1 1 1
3
1 1
2 1
3 1

输出：
2
1
1

```

### 根号分治

1. 预处理出 $k < \sqrt{n}$ 的情况，时间复杂度 O(n$\sqrt{n}$)
2. 对于  $k > \sqrt{n}$ 的情况暴力模拟，O(n$\sqrt{n}$)

#### 时间复杂度

O(n$\sqrt{n}$)

#### C++ 代码

```cpp
#include<bits/stdc++.h>
using namespace std;

const int N = 100010;

int n, m;
int a[N];
int f[N][400];

int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) scanf("%d", &a[i]);
    
    for (int i = 1; i < 400; i++)  
        for (int j = n; j >= 1; j--) {
            int v = a[j];
            if (j + v + i > n) 
                f[j][i] = 1;
            else {
                f[j][i] = f[j + v + i][i] + 1;
            }
        }
    scanf("%d", &m);
    while (m--) {
        int p, k;
        scanf("%d%d", &p, &k);
        if (k < 400) 
            printf("%d\n", f[p][k]);
        else {
            int res = 0;
            while (p <= n) {
                p += (a[p] + k);
                res++;
            }
            printf("%d\n", res);
        }
    }
}
```
