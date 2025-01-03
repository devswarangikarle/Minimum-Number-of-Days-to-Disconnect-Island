# Minimum-Number-of-Days-to-Disconnect-Island

You are given an m x n binary grid grid where 1 represents land and 0 represents water. An island is a maximal 4-directionally (horizontal or vertical) connected group of 1's.
The grid is said to be connected if we have exactly one island, otherwise is said disconnected.
In one day, we are allowed to change any single land cell (1) into a water cell (0).
Return the minimum number of days to disconnect the grid.

class Solution:
    def __init__(self):
        self.xDir = [0, 0, -1, 1]
        self.yDir = [-1, 1, 0, 0]

    def is_safe(self, grid, i, j, visited):
        return (0 <= i < len(grid) and 0 <= j < len(grid[0]) and not visited[i][j] and grid[i][j] == 1)

    def island_count(self, grid, i, j, visited):
        visited[i][j] = True
        
        for k in range(4):
            newRow = i + self.xDir[k]
            newCol = j + self.yDir[k]
            if self.is_safe(grid, newRow, newCol, visited):
                self.island_count(grid, newRow, newCol, visited)

    def count_land(self, grid, visited):
        count = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == 1 and not visited[i][j]:
                    self.island_count(grid, i, j, visited)
                    count += 1
        return count

    def minDays(self, grid):
        rows = len(grid)
        cols = len(grid[0])
        
        visited = [[False] * cols for _ in range(rows)]

        count = self.count_land(grid, visited)
        
        if count > 1 or count == 0:
            return 0

        for i in range(rows):
            for j in range(cols):
                if grid[i][j] == 1:
                    grid[i][j] = 0
                    
                    mat = [[False] * cols for _ in range(rows)]
                    count2 = self.count_land(grid, mat)
                    
                    grid[i][j] = 1
                    
                    if count2 > 1 or count2 == 0:
                        return 1   

        return 2
