---
tags: [KMP, 循环节]

---

循环项链 https://www.acwing.com/problem/content/4681/


### 题目描述
给定一个由小写字母构成的字符串，你可以对其进行补全操作，具体操作为：在其两端添加任意数量的字符。

要求，最终得到的字符串满足：其可以由某个字符串重复多次（至少两次）得到。

例如，aaa 由 a 重复 3 次得到，abcabc 由 abc 重复 2 次得到。

请问，为了使得最终字符串满足要求，最少需要添加多少个字符？

#### 样例

```
输入：
3
aaa
abca
abcde

输出：
0
2
5
```

----------

## kmp
##### 循环节

#### 时间复杂度
O(m + n)

#### 参考文献

#### C++ 代码
```cpp
#include<bits/stdc++.h>
using namespace std;

const int N = 1e5 + 10;

char a[N];
int ne[N];

int main() {
    int T;
    cin >> T;
    while(T--) {
        cin >> a + 1;
        int n = strlen(a + 1);
        memset(ne, 0, sizeof ne);
        for (int i = 2, j = 0; i <= n; i++) {
            while(j && a[j + 1] != a[i]) j = ne[j];
            if (a[i] == a[j + 1]) j++;
            ne[i] = j;
        }
        int len = n - ne[n]; // 最小循环节；
        if (n % len == 0 && n != len) cout << 0 << endl;
        else {
            cout << len - n % len << endl; // 将字符串长度补充到下一个被循环节整除的长度
        }
    }
    return 0;
}
```

