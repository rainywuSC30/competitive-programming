# 1. AC 自动机
##  AC自动机原理
<img width="979" height="576" alt="image" src="https://github.com/user-attachments/assets/82db935e-a76f-4928-b27c-28b15f05ce86" />
这里分析一下一下next数组的求解代码
```cpp
// 求next数组，ne[1] = 0表示如果 p[1]没有匹配上，从头开始匹配
for (int i = 2, j = 0; i <= n; i++) {
  // 这里的j起始就是ne[i-1]
  // 因为当i=2时，ne[2-1]=0，符合要求；之后每次循环最后ne[i]被赋值为j，然后i++, 因此ne[i - 1]就是j
  while(j && p[i] != p[j + 1]) j = ne[j];
  if (p[i] == p[j + 1]) j++;
  ne[i] = j;
}
```

根据上面代码注释中的解释，上述代码等价于：

```cpp
// 求next数组, ne[1]=0表示如果p[1]没有匹配上，从头开始匹配
for (int i = 2; i <= n; i++) {
  int j = ne[i - 1];
  while(j && p[i] != p[j + 1]) j = ne[j];
  if (p[i] == p[j + 1]) j++;
  ne[i] = j;
}
```
因此上述代码就可以理解为，根据$ne[0]~ne[i-1]$求解$ne[i]$。
<img width="985" height="259" alt="image" src="https://github.com/user-attachments/assets/92f693b4-1f99-47cb-8766-84505ae9cab1" />

<img width="954" height="661" alt="image" src="https://github.com/user-attachments/assets/f1b8b066-90c3-4280-bec9-a2c96ca966ac" />
<img width="985" height="539" alt="image" src="https://github.com/user-attachments/assets/fcdcbe47-e866-4dff-bd17-a3903481bd66" />
<img width="1002" height="742" alt="image" src="https://github.com/user-attachments/assets/5c9cafdd-6ad8-4b75-864e-2063e3be81fa" />
<img width="972" height="372" alt="image" src="https://github.com/user-attachments/assets/456ec4e9-0071-4d83-8796-237445f323a8" />

```cpp
while(hh <= tt) {
  int t = q[hh++];
  for (int i = 0; i < 26; i++) { // 这里的i枚举的是字母
    int c = tr[t][i];  // 字母i(0代表'a')对应的节点编号为c
    if (!c) continue;  // 说明从节点t不能走到字母i
    
    int j = next[t];
    while(j && !tr[j][i]) j = next[j]; // !tr[j][i]代表不能从节点j到达字母i
    if (tr[j][i]) j = tr[j][i]; // 如果能达到，更新节点j对应的编号
    next[c] = j;
    q[++tt] = c;
	}
}
```
<img width="1005" height="786" alt="image" src="https://github.com/user-attachments/assets/b36b6d4d-948a-4e01-89d8-2c1fcfe37c35" />
<img width="984" height="476" alt="image" src="https://github.com/user-attachments/assets/abb583b9-6f08-4f98-b489-4089c742a8e0" />

```cpp
// 此时，tr的定义被更新了，如果有i这个儿子，向下跳；否则不存在这个孩子，直接跳到next指针应该走到的位置
while(hh <= tt) {
	int t = q[hh++];
  for (int i = 0; i < 26; i++) { // 这里的i枚举的是字母
    int &p = tr[t][i]; // 字母i(0代表'a')对应的节点编号为c
    // 如果不存在到i的边，让节点p指向其父节点t的next指向的位置的第i个儿子
    // 即此时tr[t][i]存储了next数组应该存储的内容
    if (!p) p = tr[next[t]][i];
    else {
      next[p] = tr[next[t]][i];
      q[++tt] = p;
    }
  }
}
```

<img width="981" height="384" alt="image" src="https://github.com/user-attachments/assets/a03f2b96-2822-435b-9114-30ea2a5a3fcd" />

## 模版题
[搜索关键字](https://www.acwing.com/problem/content/1284/)

```cpp
#include <iostream>
#include <cstring>

using namespace std;

const int N = 10010, S = 55, M = 1000010;

int n;  // 单词数量
int tr[N * S][26];
int cnt[N * S];  // 以每个节点结尾的单词的数量
int idx;
char str[M];  // 读取输入字符串
int q[N * S];  // BFS求ne数组时的队列
int ne[N * S];

// trie中的插入函数
void insert() {

    int p = 0;  // 0既代表根节点，也代表空节点
    for (int i = 0; str[i]; i++) {
        int t = str[i] - 'a';
        if (!tr[p][t]) tr[p][t] = ++idx;
        p = tr[p][t];
    }
    cnt[p]++;
}

void build() {

    int hh = 0, tt = -1;
    // 第一层、第二层对应的ne值都为0，直接将第二层入队即可
    for (int i = 0; i < 26; i++)
        if (tr[0][i])  // 根节点0存在孩子i
            q[++tt] = tr[0][i];

    while (hh <= tt) {
        int t = q[hh++];
        for (int i = 0; i < 26; i++) {
            int &p = tr[t][i];

            if (!p) p = tr[ne[t]][i];  // 不存在到i的边
            else {
                ne[p] = tr[ne[t]][i];
                q[++tt] = p;
            }
        }
    }
}

int main() {

    int T;
    scanf("%d", &T);
    while (T--) {
        memset(tr, 0, sizeof tr);
        memset(cnt, 0, sizeof cnt);
        memset(ne, 0, sizeof ne);
        idx = 0;

        // (1) 建立trie数
        scanf("%d", &n);
        for (int i = 0; i < n; i++) {
            scanf("%s", str);
            insert();
        }

        // (2) 在trie树上求解next数组
        build();

        // (3) 匹配过程：trie树中的单词匹配文章
        scanf("%s", str);
        int res = 0;  // 表示匹配的单词的数量
        for (int i = 0, j = 0; str[i]; i++) {  // 遍历文章中的每个字符
            int t = str[i] - 'a';
            j = tr[j][t];

            int p = j;
            while (p) {
                res += cnt[p];
                cnt[p] = 0;  // 该单词如果出现过，统计一遍即可
                p = ne[p];
            }
        }

        printf("%d\n", res);
    }

    return 0;
}
```















