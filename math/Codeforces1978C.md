

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
#define endl '\n'
#define x first
#define y second
#define int long long 
typedef pair<int, int> PII;

const int N = 2e5 + 10, INF = 0x3f3f3f3f;

int n, k;
int a[N];

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;
    cin >> T;
    while(T--) {
        cin >> n >> k;
        for (int i = 1; i <= n; i++) a[i] = i;
        if (k % 2) {cout << "NO" << endl; continue;}
        int sm = 0;
        for (int i = 1; i <= n; i++) {
            sm += abs(a[i] - a[n - i + 1]);
        }
        if (sm < k) {cout << "NO" << endl; continue;}
        cout << "YES" << endl;
        
        k /= 2;
        for (int i = 1; ; i++) {
            if (k > n - 2 * i + 1) {
                swap(a[i], a[n - i + 1]);
                k -= n - 2* i + 1;
            }
            else {
                swap(a[i], a[i + k]);
                break;
            }
        }
        for (int i = 1; i <= n; i++) {
            cout << a[i] << " ";
        }
        cout << endl;
    }
    return 0;
}
```

