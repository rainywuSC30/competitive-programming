
CF55D [https://www.luogu.com.cn/problem/CF55D]




### 题目描述

<img width="938" height="359" alt="image" src="https://github.com/user-attachments/assets/71d99fb1-92f0-4e64-a885-3fd39ae7773e" />


#### 样例

```
输入：
1
1 9

1
12 15

输出：
9

2
```

----------

## 算法1
##### 数论 + 状态压缩 + 数位dp


1. 模运算的性质：
   x对a的模，等于x对a的倍数 (ka)的模，余数对 a 的模

   $x(mod \ a)=x(mod\ (ka))(mod\ a)$

2. 1 - 9 的lcm 等于 2520， 对于一个数x的所有位数的lcm必然是2520的约数，题目要求每个位数可以整除 x,等价于x可以整除 所有位数的lcm，等价于x 模 2520 然后再 模 lcm

3. 2520 的约数大约为 50个，因此状态压缩后为 $f[20][2521][50]$
4. 注意，记忆化搜索要所有数字的最低位对齐
    从最高位遍历到最低位，对应 u 从 0 到 n - 1， 则 记忆化搜索的 第一个维度 为 n - u.


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
#define int long long
typedef pair<int, int> PII;

const int N = 2e5 + 10, M = 4e5 + 10, INF = 0x3f3f3f3f;

int l, r;
int memo[20][3010][55];
int val[55];
int mp[3010], cnt, n; // mp[i] 表示 i 是第几个约数，val[i] 表示第i个约数的值
vector<int> num;

void init() {
    for (int i = 1; i < 3000; i++) {
        if (2520 % i == 0) {
            mp[i] = ++cnt;
            val[cnt] = i;
        }
    }
}

int gcd(int a, int b) {
    return b ? gcd(b, a % b) : a;
}

int lcm(int a, int b) {
    return a / gcd(a, b) * b;
}

int dfs(int u, int mod, int div, bool limit_high) {
    if (u == n) return mod % val[div] == 0;
    if (!limit_high && memo[n - u][mod][div] != -1) return memo[n - u][mod][div]; 
    int hi = limit_high ? num[u] : 9;
    int res = 0;
    for (int d = 0; d <= hi; d++) {
        res += dfs(u + 1, (mod * 10 + d) % 2520, d ? mp[lcm(val[div], d)] : div, limit_high && hi == d);
    }
    if (!limit_high) memo[n - u][mod][div] = res;
    return res;
}

int get_num(int x) {
    num.clear();
    while(x) num.emplace_back(x % 10), x /= 10;
    n = num.size();
    reverse(num.begin(), num.end());
    return dfs(0, 0, 1, true);
}

signed main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    init();
    memset(memo, -1, sizeof memo);
    int T;
    cin >> T;
    while(T--) {
        cin >> l >> r;
        if (l > r) swap(l, r);
        cout << get_num(r) - get_num(l - 1) << endl;
    
    }
    return 0;
}

```

