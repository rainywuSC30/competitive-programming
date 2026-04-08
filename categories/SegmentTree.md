| Source | Difficulty | Problem | Content | Hints | Solution |
| :----: | :----: | :----: | :----: | :----: | :----: |
| kuangbin | $\color{yellow}{中等}$ | [市长海报](https://www.acwing.com/problem/content/description/4344/) | 离散化 + lazy线段树 | 离散化添加节点防止区间信息丢失，线段树维护区间最小值 | [answer](../data_structure/SegmentTree/POJ2528.md) |
| kuangbin | $\color{green}{简单}$ | [地道战](https://www.acwing.com/problem/content/4348/) | 线段树 维护最大子段和 | 节点存储区间最大子段和，左边最大，右边最大信息(tmax, lmax, rmax)，根据三者之间的关系更新信息 | [answer](../data_structure/SegmentTree/HDU1540.md) |
| kuangbin | $\color{green}{简单}$ | [分配任务](https://www.acwing.com/problem/content/4349/) | 线段树 + dfn序 |  对树上每个节点的时间戳建立线段树，节点及其子树的时间戳在连续区间内 | [answer](../data_structure/SegmentTree/HDU1540.md) |
