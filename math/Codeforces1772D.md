

1772D https://codeforces.com/problemset/problem/1772/D




### 题目描述

<img width="941" height="577" alt="image" src="https://github.com/user-attachments/assets/dad80b16-e54c-4966-8bff-8b0fafbcdee7" />


#### 样例

```
输入：
8
5
5 3 3 3 5
4
5 3 4 5
8
1 2 3 4 5 6 7 8
6
10 5 4 3 2 1
3
3 3 1
3
42 43 42
2
100000000 99999999
6
29613295 52036613 75100585 78027446 81409090 73215

输出：
4
-1
0
42
2
-1
100000000
40741153
```

----------

## 算法1
##### 推式子 数学结论题

<img width="685" height="310" alt="image" src="https://github.com/user-attachments/assets/629e4173-15a1-4b1b-a31f-58a9e748d680" />


#### 时间复杂度

#### 参考文献

#### C++ 代码
```cpp
#include<bits/stdc++.h>
using namespace std;
#define endl '\n'
#define x first
#define y second
typedef long long LL;
typedef pair<int, int> PII;

const int N = 2e5 + 10, INF = 0x3f3f3f3f;

int n;
int a[N];

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;
    cin >> T;
    while(T--) {
        cin >> n;
        for (int i = 1; i <= n; i++) cin >> a[i];
        
        int l = 0, r = 1e9 + 1;
        for (int i = 2; i <= n; i++) {
            if (a[i] > a[i - 1]) 
                r = min(r, (a[i] + a[i - 1]) / 2);
            if (a[i] < a[i - 1]) 
                l = max(l, (a[i] + a[i - 1] + 1) / 2);
        }
        if (l > r) cout << -1 << endl;
        else cout << l << endl;
    }
    return 0;
}
```

