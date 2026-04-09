### 难度分：2100

### 题目描述

给你一个长度为 500000 的序列，初值为 0 ，你要完成 *q* 次操作，操作有如下两种：

1. `1 x y` : 将下标为 *x* 的位置的值加上 *y*
2. `2 x y` : 询问所有下标模 *x* 的结果为 *y* 的位置的值之和

q≤500000

接下来 q 行，每行三个整数 t,x,y 表示一次操作。(t∈{1,2})
若 t=1 则为第一种操作，保证：
1≤x≤500000,−1000≤y≤1000
若 t=2 则为第二种操作，保证：
1≤x≤500000,0≤y<x
数据保证至少有一个操作 2 

#### 样例

```
输入：
5
1 3 4
2 3 0
2 4 3
1 4 -4
2 1 0

输出：
4
4
0
```


----------

### 根号分治

暴力枚举 $\sqrt{n}$ 区间，

建立数组 $sm[x][y]$ 表示 模 x 余 y 的所有元素之和
枚举 $\sqrt_{n}$ 范围内的x，其他暴力

#### 时间复杂度

O(q$\sqrt{n}$) 

#### C++ 代码

```cpp
#include<bits/stdc++.h>
using namespace std;

typedef long long LL;

const int N = 5000010;

int n;
int a[N];
LL sm[710][710];

int main() {
    scanf("%d", &n);
    while(n--) {
        int op, x, y;
        scanf("%d%d%d", &op, &x, &y);
        if (op == 1) {
            a[x] += y;
            for (int i = 1; i < 700; i++) sm[i][x % i] += y;
        }
        else {
            if (x < 700) 
                printf("%d\n", sm[x][y]);
            else {
                LL res = 0;
                for (int i = y; i <= 500000; i += x)
                    res += a[i];
                printf("%d\n", res);
            }
        }
    }
}

```
