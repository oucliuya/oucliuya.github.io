---
title: leetcode 做题笔记
layout: post
date:   2019-07-10 10:08:00 +0800
categories: document
tag: 笔记
---

## Array

In a 2 dimensional array grid, each value grid[i][j] represents 
the height of a building located there. We are allowed to increase 
the height of any number of buildings, by any amount (the amounts can 
be different for different buildings). Height 0 is considered to be a 
building as well. At the end, the "skyline" when viewed from all four 
directions of the grid, i.e. top, bottom, left, and right, must be the 
same as the skyline of the original grid. A city's skyline is the outer 
contour of the rectangles formed by all the buildings when viewed from a 
distance. See the following example.

What is the maximum total sum that the height of the buildings can be increased?


Example:
Input: grid = [[3,0,8,4],[2,4,5,7],[9,2,6,3],[0,3,1,0]]
Output: 35
Explanation: 
The grid is:
[ [3, 0, 8, 4], 
  [2, 4, 5, 7],
  [9, 2, 6, 3],
  [0, 3, 1, 0] ]

The skyline viewed from top or bottom is: [9, 4, 8, 7]
The skyline viewed from left or right is: [8, 7, 9, 3]

The grid after increasing the height of buildings without affecting skylines is:

gridNew = [ [8, 4, 8, 7],
        [7, 4, 7, 7],
        [9, 4, 8, 7],
        [3, 3, 3, 3] ]

Notes:

1 < grid.length = grid[0].length <= 50.

All heights grid[i][j] are in the range [0, 100].

All buildings in grid[i][j] occupy the entire grid cell: that is, they are a 1 x 1 x grid[i][j] rectangular prism.

解答：

思路是：一共进行3次遍历，
1. 第一次，获取竖直方向投影列表 v_max 
2. 第二次，获取水平方向的投影列表 h_max; 
3. 进行第三次遍历，矩阵中每一个元素的高度都可以增加到对应位置的竖直投影和水平投影中较小的一个。

{% highlight java %}
class Solution {
    public int maxIncreaseKeepingSkyline(int[][] grid) {
        int N = grid.length;
		int M = grid[0].length;
		int res = 0;
		int[] v_max = new int[M];
		int[] h_max = new int[N];
		for (int j = 0; j < M; j++)
			for (int i = 0; i < N; i++)
			{
				v_max[j] = (v_max[j] < grid[i][j]) ? grid[i][j] : v_max[j];
			}
		for (int i = 0; i < N; i++)
			for (int j = 0; j < M; j++)
			{
				h_max[i] = (h_max[i] < grid[i][j]) ? grid[i][j] : h_max[i];
			}
		for (int i = 0; i < N; i++)
			for (int j = 0; j < M; j++)
			{
				int max = (h_max[i] < v_max[j]) ? h_max[i] : v_max[j];
				res += max - grid[i][j];
			}
		return res;
    }
}
{% endhighlight %}

## Subsets
```
Given a set of distinct integers, S, return all possible subsets.
Note: Elements in a subset must be in non-descending order. The 
solution set must not contain duplicate subsets. For example, 
If S = [1,2,3], a solution is:

[
[3],
[1],
[2], [1,2,3], [1,3], [2,3], [1,2], []
]

```
### 解法一： 使用递归
