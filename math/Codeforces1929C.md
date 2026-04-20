
Codeforces1929C https://codeforces.com/problemset/problem/1929/C




### 题目描述

<img width="953" height="676" alt="image" src="https://github.com/user-attachments/assets/297a9e2c-8457-4eec-aee5-5b7156d8a607" />


#### 样例

```
输入：
9
2 1 7
2 1 1
2 3 15
3 3 6
4 4 5
5 4 7
4 88 1000000000
25 69 231
13 97 18806

输出：
YES
NO
YES
NO
NO
YES
NO
NO
NO
```

----------

## 算法1
##### 博弈 + 推式子

保底是下注 x+1 次赢一次，所以现在假设我们次次都吃保底
设第 i 次下注的金额为 $c_i$







$\sum_{i = 1}^{x + 1}c_i < k * c_{x + 1}$

化简得到
$c_{x+1} > \frac{\sum_{i=1}^x c_i}{k - 1}$

因此，每一天的总金额为 $c_i=\lfloor\frac{\sum_{j=1}^{i-1} c_j}{k - 1}\rfloor + 1$

因此要成立则 $a \geq \sum_{i = 1}^{x + 1}c_i$ 



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

const int N = 110, INF = 0x3f3f3f3f;

int k, x, a;
int c[N];

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int T;
    cin >> T;
    while(T--) {
        cin >> k >> x >> a;
        c[1] = 1;
        int sm = 0;
        bool flag = true;
        for (int i = 1; i <= x + 1; i++) {
            sm = sm + c[i];
            c[i + 1] = sm / (k - 1) + 1;
            if (sm > a) {
                flag = false;
                break;
            }
        }
        if (sm <= a && flag) cout << "YES" << endl;
        else cout << "NO" << endl;
    }
    return 0;
}
```

