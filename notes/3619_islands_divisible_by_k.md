迭代版
from typing import List
import sys

class Solution:
    def countIslands(self, grid: List[List[int]], k: int) -> int:
        if not grid or not grid[0]:
            return 0

        # 递归可能很深（m*n 最多 1e5），防止 RecursionError
        sys.setrecursionlimit(300000)

        ans = 0
        m = len(grid)
        n = len(grid[0])

        def dfs(r, c):
            if r < 0 or r >= m or c < 0 or c >= n:
                return 0
            if grid[r][c] == 0:
                return 0

            val = grid[r][c]
            grid[r][c] = 0  # 标记访问过

            return val + dfs(r+1, c) + dfs(r-1, c) + dfs(r, c+1) + dfs(r, c-1)

        for i in range(m):
            for j in range(n):
                if grid[i][j] > 0:
                    total = dfs(i, j)
                    if total % k == 0:
                        ans += 1

        return ans

栈
from typing import List

class Solution:
    def countIslands(self, grid: List[List[int]], k: int) -> int:
        if not grid or not grid[0]:
            return 0

        m, n = len(grid), len(grid[0])
        ans = 0

        for i in range(m):
            for j in range(n):
                # 发现一座新岛屿的起点（正整数是陆地）
                if grid[i][j] > 0:
                    stack = [(i, j)]
                    total = 0

                    # 用栈做 DFS：把整座岛屿吃掉并累加价值
                    while stack:
                        r, c = stack.pop()

                        # 可能重复入栈；或者已经被吃掉了
                        if grid[r][c] == 0:
                            continue

                        total += grid[r][c]  # 累加当前格子的价值
                        grid[r][c] = 0       # 标记访问（淹没）

                        # 四个方向扩展（不使用 dirs 列表）
                        if r + 1 < m and grid[r + 1][c] > 0:
                            stack.append((r + 1, c))
                        if r - 1 >= 0 and grid[r - 1][c] > 0:
                            stack.append((r - 1, c))
                        if c + 1 < n and grid[r][c + 1] > 0:
                            stack.append((r, c + 1))
                        if c - 1 >= 0 and grid[r][c - 1] > 0:
                            stack.append((r, c - 1))

                    # 一座岛屿处理完，判断总价值是否可被 k 整除
                    if total % k == 0:
                        ans += 1

        return ans