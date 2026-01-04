+++
title = "Leetcode 2026"
date = "2026-01-04T21:19:14+08:00"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["leetcode"]
keywords = []
description = ""
showFullContent = false
readingTime = false
hideComments = false
+++

- [1411. 给 N x 3 网格图涂色的方案数](#1411-给-n-x-3-网格图涂色的方案数)

## 1411. 给 N x 3 网格图涂色的方案数

> 你有一个 n x 3 的网格图 grid ，你需要用 红，黄，绿 三种颜色之一给每一个格子上色，且确保相邻格子颜色不同（也就是有相同水平边或者垂直边的格子颜色不同）。
> 给你网格图的行数 n 。
> 请你返回给 grid 涂色的方案数。由于答案可能会非常大，请你返回答案对 10^9 + 7 取余的结果。

总共三种颜色且网格宽度只有 3，那么不难列出所有排列排列。
假设三种颜色分别为 `0`、`1`、`2`，想要相邻的颜色不同，有两种方式：
- 左右中间的颜色都不同：`012` `021` `102` `120` `201` `210` 六种可能性；
- 左右两边颜色相同，中间不同：`010` `020` `101` `121` `202` `212` 同样也有六种可能性；

使用 `f[i][0]` 和 `f[i][1]` 分别表示以上两类的方案数，现在开始考虑纵向如何排列才能使得相邻的格子颜色不同：

1. 上一行是颜色不同的，假设上一行是 `012`。如果当前行也是颜色不同的，则当前行可以是 `120` 和 `201`。如果当前行是左右颜色相同的，则当前行可以是 `101` 和 `121`，剩下五组以此类推，分别也是各有两种可能；
2. 上一行是左右颜色相同的，假设上一行是 `010`。如果当前行是颜色不同的，则当前行可以是 `102` 和 `201`。果当前行是左右颜色相同的，则当前行可以是 `101`、`121`、`202` 三种，剩下五组以此类推，分别也是有两种和三种可能；

也就是说递推式为：
$$
\left\{
  \begin{aligned}
    f[i][0] = f[i - 1][0] * 2 + f[i - 1][1] * 2 \\
    f[i][1] = f[i - 1][0] * 2 + f[i - 1][1] * 3
  \end{aligned}
\right.
$$

```cpp
class Solution {
private:
    static constexpr int mod = 1000000007;

public:
    int numOfWays(int n) {
        int fi0 = 6, fi1 = 6;
        for (int i = 2; i <= n; ++i) {
            int new_fi0 = (2LL * fi0 + 2LL * fi1) % mod;
            int new_fi1 = (2LL * fi0 + 3LL * fi1) % mod;
            fi0 = new_fi0;
            fi1 = new_fi1;
        }
        return (fi0 + fi1) % mod;
    }
};

```