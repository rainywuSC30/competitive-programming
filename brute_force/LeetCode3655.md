---
tags: [根号分治, 商分]

---

区间乘法查询后的异或 II https://leetcode.cn/problems/xor-after-range-multiplication-queries-ii/?envType=daily-question&envId=2026-04-09




### 题目描述

给你一个长度为 n 的整数数组 nums 和一个大小为 q 的二维整数数组 queries，其中 queries[i] = [li, ri, ki, vi]。
对于每个查询，需要按以下步骤依次执行操作：
设定 idx = li。
当 idx <= ri 时：
更新：nums[idx] = (nums[idx] * vi) % (109 + 7)。
将 idx += ki。
在处理完所有查询后，返回数组 nums 中所有元素的 按位异或 结果。

1 <= n == nums.length <= 1e5
1 <= nums[i] <= 1e9
1 <= q == queries.length <= 1e5
queries[i] = [li, ri, ki, vi]
0 <= li <= ri < n
1 <= ki <= n
1 <= vi <= 1e5

#### 样例

```
输入：
nums = [1,1,1], queries = [[0,2,1,4]]

输出：
4
```

----------

## 算法1
##### 商分 + 根号分治

1. 给定k，修改数组 只会影响 i， i+k, i+2k,。。。的值，使用商分数组 可以O(1）表示修改，然后恢复数组使用前缀乘法（类似前缀和）
2. 定义商分数组 d[k][j] 表示 跳跃距离 为 k 的商分数组，这样在k下，商分数组是连续的，对于v和修改区间原始数组区间[l, r] 只需要d[k][l] *= k, d[k][r’] *= （1/v） (mod是质数，v和mod互质，费马小定理求逆元);
3. 建立 B 个 商分数组，修改原始数组需要 $O(n * B)$
4. k大于B ，每次询问修改数组复杂度$O(\frac{n}/{B})$, 所有询问为 O(q * \frac{n}/{B});
5. 平衡两种算法 B = sqrt(q)


#### 时间复杂度

#### 参考文献

#### C++ 代码
```cpp
class Solution {
public:
    typedef long long LL;
    const int mod = 1e9 + 7;

    //求逆元
    LL qmi(int a, int k) {
        LL res = 1;
        while(k) {
            if (k & 1) res = (LL)res * a % mod;
            a = (LL)a * a % mod;
            k >>= 1;
        }
        return res;
    }

    int xorAfterQueries(vector<int>& a, vector<vector<int>>& q) {
        int n = a.size();
        int B = sqrt(q.size());
        vector<vector<int>> d(B + 1);
        // 时间复杂度为 O(qsqrt(q))
        for (int i = 0; i < q.size(); i++) {
            int l = q[i][0], r = q[i][1], k = q[i][2], v = q[i][3];
            if (k <= B) {
                if (d[k].empty()) {
                    d[k].resize(n + k, 1);
                }
                d[k][l] = (LL)d[k][l] * v % mod;
                r = r - (r - l) % k + k;
                d[k][r] = (LL)d[k][r] * qmi(v, mod - 2) % mod;
            }
            else {
                for (int j = l; j <= r; j += k) {
                    a[j] = ((LL)a[j] * v) % mod;
                }
            }
        }
        // 时间复杂度为 O(nsqrt(q))
        for (int i = 1; i <= B; i++) {
            if (d[i].empty()) continue;
            for (int j = 0; j < n; j++) {
                if (j >= i) {
                    d[i][j] = (LL)d[i][j] * d[i][j - i] % mod;
                }
                a[j] = (LL)a[j] * d[i][j] % mod;
            }
        }
        int res = 0;
        for (int i = 0; i < n; i++) {
            res ^= a[i];
        }
        return res;
    }
};
```



