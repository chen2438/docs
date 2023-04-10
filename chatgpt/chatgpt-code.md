---
description: 发布于 2023-03-11
categories:
- chatgpt
date: 2023-03-11
slug: chatgpt-code
title: ChatGPT 编程
---

# ChatGPT 编程

### 深入浅出学算法010-车厢重组

#### 题目描述

在一个旧式的火车站旁边有一座桥，其桥面可以绕河中心的桥墩水平旋转。一个车站的职工发现桥的长度最多能容纳两节车厢，如果将桥旋转180度，则可以把相邻两节车厢的位置交换，用这种方法可以重新排列车厢的顺序。于是他就负责用这座桥将进站的车厢按车厢号从小到大排列。他退休后，火车站决定将这一工作自动化，其中一项重要的工作是编一个程序，输入初始的车厢顺序，计算最少用多少步就能将车厢排序。

#### 输入

共两行。
第一行是车厢总数N(≤10000)。
第二行是N个不同的数表示初始的车厢顺序。

#### 输出

一个整数，最少的旋转次数。

#### 样例输入

```
4
4 3 2 1
```

#### 样例输出

```
6
```

#### ChatGPT

把车厢按顺序排列的问题可以转化为求初始状态与最终状态之间的最小旋转次数。因为每次旋转可以交换相邻两个车厢，所以这个问题可以看作是一个排序问题。我们可以使用冒泡排序或者快速排序等算法实现，**但是其时间复杂度都比较高**。

更好的方法是使用逆序对的概念来解决这个问题。逆序对指的是在一个序列中，如果前面的数字比后面的数字大，则这两个数字组成一个逆序对。例如，在序列 2, 4, 1, 3 中，(2, 1)、(4, 1) 和 (4, 3) 是逆序对。显然，要将一个序列从小到大排序，需要交换的次数就是该序列的逆序对个数。

因此，我们可以先计算出给定序列的逆序对个数，然后用归并排序或者其他排序算法将其排序，得到有序的序列，这个有序序列就是目标状态，其逆序对个数为0。最少旋转次数即为初始状态的逆序对个数。

以下是C++程序实现：

```cpp
#include <iostream>
using namespace std;

const int MAXN = 1e4 + 5;
int a[MAXN], tmp[MAXN];
long long ans; // 记录逆序对个数

void merge_sort(int l, int r) {
    if (l >= r) return;
    int mid = l + (r - l) / 2;
    merge_sort(l, mid);
    merge_sort(mid+1, r);
    int i = l, j = mid+1, k = l;
    while (i <= mid && j <= r) {
        if (a[i] <= a[j]) tmp[k++] = a[i++];
        else {
            tmp[k++] = a[j++];
            ans += (mid-i+1); // 统计逆序对
        }
    }
    while (i <= mid) tmp[k++] = a[i++];
    while (j <= r) tmp[k++] = a[j++];
    for (int i = l; i <= r; ++i) a[i] = tmp[i];
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    int n;
    cin >> n;
    for (int i = 0; i < n; ++i)
        cin >> a[i];
    merge_sort(0, n-1);
    cout << ans << '\n';
    return 0;
}
```

其中merge_sort函数是归并排序的实现，ans变量用来记录逆序对的个数。

### 深入浅出学算法032-骨牌铺法

#### 题目描述

有1×n的一个长方形，用一个1×1、1×2和1×3的骨牌铺满方格。例如当n=3时为1×3的方格。此时用1×1、1×2和1×3的骨牌铺满方格，共有四种铺法。如下图： 
 ![img](https://media.opennet.top/i/2023/03/13/640eb813c97ed.png)

#### 输入

输入整数n

#### 输出

输出铺法总数

#### 样例输入 [Copy](javascript:CopyToClipboard($('#sampleinput').text()))

```
3
```

#### 样例输出 [Copy](javascript:CopyToClipboard($('#sampleoutput').text()))

```
4
```

#### 提示

n <= 40

#### ChatGPT

以下是C++代码实现：

```cpp
#include <iostream>
using namespace std;

const int MAXN = 50;
long long dp[MAXN]; //使用long long类型，避免整型溢出

int main() {
    int n;
    cin >> n;
    dp[0] = 1; //初始化dp[0]
    for (int i = 1; i <= n; i++) {
        dp[i] = dp[i - 1];
        if (i >= 2) dp[i] += dp[i - 2];
        if (i >= 3) dp[i] += dp[i - 3];
    }
    cout << dp[n] << endl;
    return 0;
}
```

解释：

使用动态规划的思想：设$dp[i]$表示用1×1、1×2和1×3的骨牌铺满一个1×i的长方形的铺法总数。

则当i≥3时，可以有三种情况：

- 最后一块骨牌是1×1，则前面部分需要用1×(i-1)的骨牌进行铺设，铺法为dp[i-1]种；
- 最后一块骨牌是1×2，则前面部分需要用1×(i-2)的骨牌进行铺设，铺法为dp[i-2]种；
- 最后一块骨牌是1×3，则前面部分需要用1×(i-3)的骨牌进行铺设，铺法为dp[i-3]种。

所以，状态转移方程为：

$$ dp[i]=dp[i-1]+dp[i-2]+dp[i-3] $$

当i=1时，只能使用1×1的骨牌进行铺设，铺法为1种。

因此，需要先将dp[0]初始化为1。最终答案为dp[n]。

### 深入浅出学算法034-数塔问题

#### 题目描述

设有一个三角形的数塔，顶点为根结点，每个结点有一个整数值。从顶点出发，可以向左走或向右走，如图所示：
![img](https://media.opennet.top/i/2023/03/13/640eba5ee85dd.png)

​    若要求从根结点开始，请找出一条路径，使路径之和最大，只要输出路径的和。



#### 输入

第一行为n(n<10)，表示数塔的层数

从第2行至n+1行，每行有若干个数据，表示数塔中的数值。

#### 输出

输出路径和最大的路径值。

#### 样例输入 [Copy](javascript:CopyToClipboard($('#sampleinput').text()))

```
5
13
11  8
12  7  26
6  14  15  8
12  7  13  24  11
```

#### 样例输出 [Copy](javascript:CopyToClipboard($('#sampleoutput').text()))

```
86
```

#### ChatGPT

以下是对应的C++代码实现：

```cpp
#include <iostream>
using namespace std;

const int MAXN = 10; // 数塔最大层数
int n; // 数塔层数
int triangle[MAXN][MAXN]; // 存储数塔中每个结点的值
int f[MAXN][MAXN]; // 存储从根节点到当前节点的最大路径和

int main() {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= i; j++) {
            cin >> triangle[i][j];
        }
    }

    // 初始化第一层的最大路径和
    f[1][1] = triangle[1][1];

    // 动态规划求解最大路径和
    for (int i = 2; i <= n; i++) {
        for (int j = 1; j <= i; j++) {
            if (j == 1) { // 左侧边界
                f[i][j] = f[i-1][j] + triangle[i][j];
            } else if (j == i) { // 右侧边界
                f[i][j] = f[i-1][j-1] + triangle[i][j];
            } else { // 中间位置
                f[i][j] = max(f[i-1][j], f[i-1][j-1]) + triangle[i][j];
            }
        }
    }

    // 输出最大路径和
    int ans = 0;
    for (int i = 1; i <= n; i++) {
        ans = max(ans, f[n][i]);
    }
    cout << ans << endl;

    return 0;
}
```

算法思路：使用动态规划求解，设 `f[i][j]` 表示从根节点到第 `i` 行第 `j` 列结点的最大路径和，那么状态转移方程为：
$$
f[i][j] = \begin{cases}
f[i-1][j] + triangle[i][j] & j=1 \\
f[i-1][j-1] + triangle[i][j] & j=i \\
\max(f[i-1][j], f[i-1][j-1]) + triangle[i][j] & \text{otherwise}
\end{cases}
$$
其中 `triangle` 数组存储了数塔中每个结点的值。最终的答案即为 `f[n][1...n]` 中的最大值。

### 深入浅出学算法030-兔子繁殖

#### 题目描述

有一种兔子，出生后一个月就可以长大，然后再过一个月一对长大的兔子就可以生育一对小兔子且以后每个月都能生育一对。现在，我们有一对刚出生的这种兔子，那么，n个月过后，我们会有多少对兔子呢？假设所有的兔子都不会死亡。

#### 输入

输入文件仅一行，包含一个自然数n (1 <= n <= 50)。

#### 输出

输出文件仅一行，包含一个自然数，即n个月后兔子的对数。

#### 样例输入 [Copy](javascript:CopyToClipboard($('#sampleinput').text()))

```
5
```

#### 样例输出 [Copy](javascript:CopyToClipboard($('#sampleoutput').text()))

```
5
```

#### ChatGPT

以下是C++代码：

```c++
#include <iostream>
using namespace std;
int main() {
    int n, a = 1, b = 0, c;
    cin >> n;
    n--;
    for (int i = 1; i <= n; i++) {
        c = b;
        b = a + b;
        a = c;
    }
    cout << a + b << endl;
    return 0;
}
```

这段代码使用了斐波那契数列的计算方法，其中 `a` 表示前两个月的兔子对数，`b` 表示上一个月的兔子对数，`c` 用于交换 `a` 和 `b` 的值。

在循环中，根据斐波那契数列的规律，每个月的兔子对数等于上一个月兔子对数加上前两个月兔子对数。每次循环结束后更新 `a` 和 `b` 的值即可。

最后输出 `a+b` 即为 n 个月后兔子的对数。

### 深入浅出学算法035-过河卒

#### 题目描述

棋盘上A点有一个过河卒，需要走到目标B点。卒行走的规则：可以向下、或者向右。同时在棋盘上的任一点有一个对方的马（如C点），该马所在的点和所有跳跃一步可达的点称为对方马的控制点（如图中的C点和P1，P2，……，P8）。卒不能通过对方马的控制点。棋盘用坐标表示，A点(0,0)、B点(n, m) (n,m为不超过20的整数),同样马的位置坐标是需要给出的，C≠A且C≠B。现在输入B点坐标和C点的坐标，要你计算出卒从A点能够到达B点的路径的条数。
![img](https://media.opennet.top/i/2023/03/13/640ec38e63e0b.png)

#### 输入

输入B点坐标和C点的坐标

#### 输出

卒从A点能够到达B点的路径的条数。

#### 样例输入 [Copy](javascript:CopyToClipboard($('#sampleinput').text()))

```
4  8  2  4
```

#### 样例输出 [Copy](javascript:CopyToClipboard($('#sampleoutput').text()))

```
0
```

#### ChatGPT

以下是题目所需的C++代码实现：

```cpp
#include <iostream>
using namespace std;

const int MAXN = 21;
long long f[MAXN][MAXN];

int main() {
    int n, m, x, y;
    cin >> n >> m >> x >> y;

    // 初始化棋盘和马控制点
    bool horse[MAXN][MAXN] = { false };
    if (x > 1 && y > 0) horse[x - 2][y - 1] = true;
    if (x > 0 && y > 1) horse[x - 1][y - 2] = true;
    if (x < n - 1 && y > 0) horse[x + 2][y - 1] = true;
    if (x < n && y > 1) horse[x + 1][y - 2] = true;
    if (x < n && y < m - 1) horse[x + 1][y + 2] = true;
    if (x < n - 1 && y < m) horse[x + 2][y + 1] = true;
    if (x > 0 && y < m - 1) horse[x - 1][y + 2] = true;
    if (x > 1 && y < m) horse[x - 2][y + 1] = true;

    // 动态规划计算路径条数
    f[0][0] = 1;
    for (int i = 0; i <= n; i++) {
        for (int j = 0; j <= m; j++) {
            if (i == x && j == y) continue; // 对方马的控制点不能到达
            if (i > 0 && !horse[i - 1][j]) f[i][j] += f[i - 1][j];
            if (j > 0 && !horse[i][j - 1]) f[i][j] += f[i][j - 1];
        }
    }

    cout << f[n][m] << endl;
    return 0;
}
```

其中，`f[i][j]` 表示从起点 `(0, 0)` 到点 `(i, j)` 的路径条数。我们使用动态规划来计算这个值，转移方程为：

$$f[i][j] = \begin{cases} f[i-1][j] + f[i][j-1], & \text{如果点 }(i,j)\text{ 不在对方马的控制点上} \\ 0, & \text{如果点 }(i,j)\text{ 在对方马的控制点上} \end{cases}$$

初始化 $f[0][0] = 1$，最终答案为 $f[n][m]$。
