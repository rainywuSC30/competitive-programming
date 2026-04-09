
| Source | Difficulty | Problem | Content | Hints | Solution |
| :----: | :----: | :----: | :----: | :----: | :----: |
| Kuangbin | $\color{green}{简单}$ | [循环项链](https://www.acwing.com/problem/content/4681/) | KMP + 循环节 | 先计算字符串的next数组，然后循环节计算 n - ne[n] | [answer](../string/AcWing4678.md) |
| Kuangbin | $\color{green}{简单}$ | [企业ID](https://www.acwing.com/problem/content/4681/) | 排序 + KMP + 哈希表 | 多个串找共有子串，哈希避免重复遍历，排序将最短串作为模式串， | [answer](../string/AcWing4687.md) |
| 洛谷 | $\color{green}{普及+/提高}$ | [Milk Patterns G](https://www.luogu.com.cn/problem/P2852) | 离散化 + 后缀数组 + 单调队列 | 离散化值域，后缀数组得到height数组，然后在height数组上滑动窗口k-1，使用单调队列维护区间最小值 | [answer](../string/P2852.md) |
| 洛谷 | $\color{green}{省选/NOI−}$ | [差异](https://www.luogu.com.cn/problem/P4248) | 后缀数组 + 单调栈 | 后缀数组得到height数组，后缀对集合求和对应到rank空间求和，单调栈维护height数组区间最小值，计算贡献 | [answer](../string/P2852.md) |
