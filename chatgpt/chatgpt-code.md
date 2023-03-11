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

#### ChatGPT 回答

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

### 

