# KMP
KMP算法用于解决字符串的单模匹配问题，即在一个主串 S 中查找模式串 P 的所有出现位置

**时间复杂度O(|S| + |P|)**

KMP算法中利用 Next 数组加速 P 在S中的匹配过程

## Next 数组的性质

Next[i] 表示模式串 P 中以 i (下标从1开始) 结尾的真后缀能匹配 P 的前缀的最大长度

$Next[i]=\underset{1\leq j\leq i-1}{max}{j}\quad while\ P[1:j]==P[i-j+1:i]$

| P       | a    | b    | a    | b    | a    |
| ------- | ---- | ---- | ---- | ---- | ---- |
| i       | 1    | 2    | 3    | 4    | 5    |
| $Next[i]$ | 0    | 0    | 1    | 2    | 3    |

Next[]表示的是模式串 P 的信息

现在有一个字符串 P，并且已知了 Next[]数组，有 Next[i] =j, Next[j] = k, 那么有一个显然的性质：k < j < i, 然后可以画出下面的示意图
<img width="986" height="509" alt="image" src="https://github.com/user-attachments/assets/d5080b96-9a44-4cde-97d7-cdf94b7cdc71" />

即，若以 i 结尾的真后缀能匹配 P 的前缀的最大长度为 Next[i]， 那么它能匹配的次长前缀长度为 Next[Next[i]], 依次类推，Next[Next[Next[i]]]... 直至匹配长度为 0 为止。
```cpp
// s[] 是长文本，p[]是模式串，n是s的长度，m是p的长度
// 求模式串的 next数组：
for (int i = 2, j = 0; i <= m; i++) {
    while (j && p[i] != p[j + 1]) j = ne[j];
    if (p[i] == p[j + 1]) j++;
    ne[i] = j;
}

// 匹配
for (int i = 1, j = 0; i <= n; i++) {
    while (j && s[i] != p[j + 1]) j = ne[j];
    if (s[i] == p[j + 1]) j++;
    if (j == m) {
        j = ne[j];
        // 匹配成功后的逻辑
    }
}
```

## KMP匹配过程
若当前主串S匹配到下标 i-1 位置，模式串 P 匹配到下标 j 位置， 即满足 $S[i-j:i-1]==P[1:j]$, 如下图所示：
<img width="994" height="251" alt="image" src="https://github.com/user-attachments/assets/fc751005-f349-45de-b381-d6ff0ee79b2a" />

1. 若 $S[i]==P[j+1]$, 则 j 后移一位，表示这一位匹配成功，此时如果 j 到达了 P 串的末尾，说明 P 在 S 中出现了，在 S 中的起始下标为 i - |P| + 1
2. 若 $S[i]\neq P[j+1]$ ,发生失配，此时已经知道了 P 中以 j 结尾的后缀已经能与 S 中以 i-1 结尾的后缀匹配，希望 P 串向右移动最少，即希望 移动后 P 的前缀能与 S 中以 i-1 结尾的后缀匹配尽可能长，而 S中以 i-1 结尾的后缀等效于 P 以 j 结尾的后缀，这恰好就是 Next[j]的含义, $S[i - Next[j]:i-1]==P[1:Next[j]]$ ,也就是图中标记的紫色部分匹配。那么只要将 P 中第一段紫色移动到第二段位置即可，如下图所示：

<img width="1001" height="225" alt="image" src="https://github.com/user-attachments/assets/a3b87974-9a95-4633-acc1-f9abe3327d51" />

上述移动过程就是令 j = Next[j]。然后只需要再比较 S[i] 和 P[j+1] 是否匹配即可，如果不匹配，不断回退 j = Next[j], **其实就是不断用 P 中已经能与 S 中以 i-1结尾的后缀匹配的前缀，减少中间对于匹配过程**，所使用的 P 的前缀长度递减，直到为0.如果经过某次 j 的回退， 满足了 S[i] = P[j+1]，那么 j 就可以后移了。图中表现为经过依次回退，就能成功匹配的情况。

```cpp
/*n为模式串p的长度，m为主串s的长度*/
for (int i = 1, j = 0; i <= m; i++) {
    while (j && s[i] != p[j+1]) j = Next[j]; // 失配 j 就不断回退
    if (s[i] == p[j+1]) j++; // 匹配成功 j 后移
    if (j == n) { // p 在 s 中完整出现
        cout << i - n + 1 << " "; // 出现的起始位置
        j = Next[j]; 
    }
}
```

## Next数组求解

可以把 Next[]的求解过程看成是两个相同的串 P 匹配的过程，也可以看成 模式串 P 自身与自身匹配的过程

Next[1] = 0 (因为只考虑真 前/后缀[^1])， 

```cpp
Next[1] = 0;
for (int i = 2, j = 0; i <= n; i++) {
    while (j && p[i] != p[j+1]) j = Next[j];
    if (p[i] == p[j+1]) j++;
    Next[i] = j;
}
```

next[j] 增加的话一定是连续加1的
## 时间复杂度

Next[]求解与 KMP 匹配的时间复杂度类似，在每次 i++ 过程中，j最多增加一次，因此 j 最多增加 |S| 次，而 while 循环中 j 总共的归退次数不可能超过它增加的次数，因此 最多回退 |S| 次， 所以 KMP 匹配过程时间复杂度为 O(|S|)。 Next 时间复杂度为 O(|P|)

**时间复杂度P(|S| + |P|)**


