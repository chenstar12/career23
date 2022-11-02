网格问题 **DFS** 代码框架：

```python
def dfs(int[][] grid, int r, int c):
    # base case: grid[r][c]超出网格范围 (类比二叉树 if not root)
    if not inArea(grid, r, c): return
    # 访问上、下、左、右相邻结点(四叉树)
    dfs(grid, r - 1, c);
    dfs(grid, r + 1, c);
    dfs(grid, r, c - 1);
    dfs(grid, r, c + 1);

def inArea(grid, r, c) # 判断grid[r][c]是否在网格中
    return 0 <= r and r < len(grid) and 0 <= c and c < len(grid[0])
```
[1162. 地图分析](https://leetcode.cn/problems/as-far-from-land-as-possible/)

- 先用list存储所有陆地；再从各个陆地开始，一圈一圈的遍历海洋（BFS），最后遍历到的是离陆地最远的海洋

  ```python
          m, n = len(grid), len(grid[0])
          stack = []
          for i in range(m): 
              for j in range(n): 
                  if grid[i][j]: stack.append([i, j])
          if len(stack) == m * n or not stack: return -1
          step = 1
          while stack:
              nex = []
              for i, j in stack:
                  for x, y in [[i+1, j], [i-1, j], [i, j+1], [i, j-1]]:
                      if 0 <= x < m and 0 <= y < n and not grid[x][y]:
                          nex.append([x, y])
                          grid[x][y] = step
              stack = nex
              step += 1
          return step - 2
  ```

[1、颜色填充](https://leetcode.cn/problems/color-fill-lcci/)：BFS、DFS

```python
        rawColor = image[sr][sc]
        if rawColor == newColor: return image
        m, n = len(image), len(image[0])
        que = collections.deque([(sr, sc)]) # 队列
        image[sr][sc] = newColor # 着色
        while que:
            x, y = que.popleft() # 出队
            for i,j in [(x-1,y),(x+1,y),(x,y-1),(x,y+1)]: # BFS（广度优先搜索）
                if 0 <= i < m and 0 <= j < n and image[i][j] == rawColor:
                    que.append((i,j))
                    image[i][j] = newColor # 着色
        return image
```
```python
        m, n = len(image), len(image[0])
        rawColor = image[sr][sc]
        def dfs(x, y): # DFS（深度优先搜索）
            if image[x][y] == rawColor:
                image[x][y] = newColor
                for i, j in [(x - 1, y), (x + 1, y), (x, y - 1), (x, y + 1)]:
                    if 0 <= i < m and 0 <= j < n and image[i][j] == rawColor:
                        dfs(i, j)
        if rawColor != newColor: dfs(sr, sc) # DFS: 深度优先搜索
        return image
```
```python
        if newColor == image[sr][sc]: return image
        stack = [(sr, sc)] # DFS ---- 用栈模拟递归
        rawColor = image[sr][sc]
        while stack:
            r, c = stack.pop()
            image[r][c] = newColor
            for x, y in zip((r, r, r + 1, r - 1), (c + 1, c - 1, c, c)): 
                if 0 <= x < len(image) and 0 <= y < len(image[0]) and image[x][y] == rawColor:
                    stack.append((x, y))
        return image
```
[0、岛屿的最大面积](https://leetcode.cn/problems/max-area-of-island/)：DFS、“已遍历”标记为2、

```python
        def maxAreaOfIsland(grid,r,c): # DFS
            if not inArea(grid,r,c): return 0 # base case
            if grid[r][c] != 1: return 0 # 0或2，直接返回
            grid[r][c] = 2 # 标记为“已遍历”
            return 1 + dfs(grid,r-1,c)+dfs(grid,r+1,c)+dfs(grid,r,c-1)+dfs(grid,r,c+1) # 四叉树遍历
        
        res = 0
        for r in range(len(grid)):
            for c in range(len(grid[0])):
                if grid[r][c] == 1:
                    curRes = maxAreaOfIsland(grid,r,c)
                    res = max(res, curRes)
        return res
```

[2、岛屿周长](https://leetcode.cn/problems/island-perimeter/)：（1）相邻 -2；（2）DFS：每当岛屿方格走进非岛屿方格，周长加 1；

```python
        res = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])): # 法1：对于已遍历过的格子：相邻就-2
                if grid[i][j] == 1:
                    res += 4
                    if i>0 and grid[i-1][j] == 1: res -= 2
                    if j>0 and grid[i][j-1] == 1: res -= 2
        return res
```
```python
        def dfs(grid, r, c):
            if r < 0 or r >= len(grid) or c < 0 or c >= len(grid[0]): return 1 #从岛屿方格走到边界，周长+1
            if grid[r][c] == 0: return 1 #从岛屿方格走向水域，周长+1
            if grid[r][c] == 1:
                grid[r][c] = 2 # 标记为"已遍历"
                return dfs(grid,r-1,c) + dfs(grid,r+1,c) + dfs(grid,r,c-1) + dfs(grid,r,c+1)
            return 0 # grid[r][c] == 2

        for r in range(len(grid)):
            for c in range(len(grid[0])):
                if grid[r][c] == 1:
                    return dfs(grid, r, c)
        return 0
```
