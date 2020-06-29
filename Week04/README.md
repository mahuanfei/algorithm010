[200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

```java
/*
给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。
岛屿总是被水包围，并且每座岛屿只能由水平方向或竖直方向上相邻的陆地连接形成。
此外，你可以假设该网格的四条边均被水包围。
示例 1: 输入:
11110
11010
11000
00000
输出: 1

示例 2: 输入:
11000
11000
00100
00011
输出: 3
解释: 每座岛屿只能由水平或竖直方向上相邻的陆地连接而成。

思想:首先用一个嵌套的循环,循环数组中的每一个元素,如果碰到是1,说明岛屿的数量至少找到一个,所以岛屿数量加一,然后把和1左右上下相邻的所有点且相邻的无限递归下去的1全部打为0,也就是说和所有1相连接的其他的1全部都打掉,打掉的意思就是夷为平地变为0,直到循环到末尾,整个地图被我们夷为平地打成0,说明我们把所有的岛屿数量统计完了,代码实现如下:
*/
```

```java
private int n, m;

public int numIslands(char[][] grid) {
	int count = 0;
	n = grid.length;
	if (n == 0) return 0;
	m = grid[0].length;
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < m; j++) {
			if (grid[i][j] == '1') {
				DFSMarking(grid, i, j);
				++count;
			}
		}
	}
	return count;
}

private void DFSMarking(char[][] grid, int i, int j) {
	if (i < 0 || j < 0 || i >= n || j >= m || grid[i][j] != '1') return;
	grid[i][j] = '0';
	DFSMarking(grid, i + 1, j);
	DFSMarking(grid, i - 1, j);
	DFSMarking(grid, i, j + 1);
	DFSMarking(grid, i, j - 1);
}
```