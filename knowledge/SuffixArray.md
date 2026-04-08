

## 核心数组定义

* $s[i]$--- 排名为 i 的后缀在原字符串中的起始位置
* $rk[i]$ --- 起始位置为 i 的后缀在所有后缀中的排名
* $x[i]$ --- 第一关键字的排名(当前阶)， 本质上是rank数组，表示起始位置i的后缀的上一阶排名
* $y[i]$ --- 第二关键字的排名对应的后缀位置，存储的是**按第二关键字排序后**，后缀的起始位置
* $height[i]$ --- 表示$s[i]$ 和 $sa[i - 1]$ 这两个后缀的 **最长公共前缀(LCP)**




# 模版题
[https://www.acwing.com/problem/content/2717/]

get_sa() O(nlogn)
get_height() O(n)

```cpp
#include<bits/stdc++.h>
using namespace std;

const int N = 1e6 + 10;

int n, m;
char s[N];
int sa[N], x[N], y[N], c[N], rk[N], height[N];


void get_sa() {
    for (int i = 1; i <= n; i++) c[x[i] = s[i]]++;
    for (int i = 2; i <= m; i++) c[i] += c[i - 1];
    for (int i = n; i; i--) sa[c[x[i]]--] = i;
    for (int k = 1; k <= n; k <<= 1) {
        int num = 0;
        for (int i = n - k + 1; i <= n; i++) y[++num] = i;
        for (int i = 1; i <= n; i++) {
            if (sa[i] > k)
                y[++num] = sa[i] - k;
        }
        for (int i = 1; i <= m; i++) c[i] = 0;
        for (int i = 1; i <= n; i++) c[x[i]]++;
        for (int i = 2; i <= m; i++) c[i] += c[i - 1];
        for (int i = n; i; i--) sa[c[x[y[i]]]--] = y[i], y[i] = 0;
        swap(x, y);
        x[sa[1]] = 1, num = 1;
        for (int i = 2; i <= n; i++) 
            x[sa[i]] = (y[sa[i]] == y[sa[i - 1]] && y[sa[i] + k] == y[sa[i - 1] + k]) ? num : ++num;
        if (num == n) break;
        m = num;
    }
}

void get_height() {
    for (int i = 1; i <= n; i++) rk[sa[i]] = i;
    for (int i = 1, k = 0; i <= n; i++) {
        if (rk[i] == 1) continue;
        if (k) k--;
        int j = sa[rk[i] - 1];
        while (i + k <= n && j + k <= n && s[i + k] == s[j + k]) k++;
        height[rk[i]] = k;
    }
}

int main() {
    scanf("%s", s + 1);
    n = strlen(s + 1), m = 122;
    get_sa();
    get_height();
    
    for (int i = 1; i <= n; i++) printf("%d ", sa[i]);
    puts("");
    for (int i = 1; i <= n; i++) printf("%d ", height[i]);
    puts("");
    return 0;
}
```



