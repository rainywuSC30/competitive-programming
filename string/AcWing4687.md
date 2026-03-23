---
tags: [KMP, 排序]

---

企业ID  https://www.acwing.com/problem/content/description/4690/


### 题目描述
给定 N 个由小写字母构成的字符串。
请你找到并输出它们的最长公共子串。

数据范围
2 ≤ N ≤ 4000,
输入字符串都由小写字母构成，长度范围 [1,200]。

#### 样例

```
输入：
3
aabbaabb
abbababb
bbbbbabb
2
xyz
abc
0

输出：
abb
IDENTITY LOST
```

----------

## kmp + 排序
找公共子串的时候 两个优化：
1. 使用 哈希表 避免重复遍历
2. 排序，将最短的串作为 模式串p， 其他作为 匹配串 s，这样可以减少 长度的遍历，（类似于dfs优先搜索分支少的情况）

#### C++ 代码

```cpp
#include<bits/stdc++.h>
using namespace std;

const int N = 4010;

int n;
int ne[500];
string a[N];

bool kmp(string p, string s) {
    memset(ne, 0, sizeof ne);
    int m = p.size(), n = s.size();
    p = '$' + p + '#' + s;
    for (int i = 2, j = 0; i < m + n + 2; i++) {
        while(j && p[j + 1] != p[i]) j = ne[j];
        if (p[j + 1] == p[i]) j ++;
        ne[i] = j;
        if (j == m) return true;
    }
    return false;
}

int main() {
    while(cin >> n, n) {
        for (int i = 1; i <= n; i++) cin >> a[i];
        // 类似于dfs遍历最少分支，这里将最短的字符串放开头作为模式串；
        sort(a + 1, a + n + 1, [](string a, string b) {
            return a.size() < b.size();
        });
        string res = "";
        unordered_set<string> mp;
        int m = a[1].size();
        for (int len = m; len; len--) {
            bool find = false;
            for (int l = 0; l + len - 1 < m; l++) {
                string p = a[1].substr(l, len);
                if (mp.count(p)) continue;
                mp.insert(p);
                bool valid = true;
                for (int k = 2; k <= n; k++) {
                    string s = a[k];
                    if (!kmp(p, s)) {valid = false; break;}
                }
                if (valid) {
                    if (p.size() > res.size()) res = p;
                    else if (p.size() == res.size()) res = min(res, p);
                    find = true;
                }
            }
            if (find) break;
        }
        if (res == "") cout << "IDENTITY LOST" << endl;
        else cout << res << endl;
    }
    return 0;
}
```

