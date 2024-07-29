# W13D4-Assignment


counting islands.

given a grid, array[[row 1],[row 2],...[row n]] with a row length of len(row1)

any 1 connected in the grid either to the right or bottom is in the same group, so track visited elements in set, use bfs to capture all connected 1s and update the visited map to all the connected 1,s by col,row, returning to search from the beginning, skipping values [col,row] in the visited set. when new 1 found start bfs to map range of connected 1,s updating island count to two, ans adding bfs results back to visited set. returning to beginning, skipping over visited locations until all locations found returning number of islands.




        # use bfs when 1 or island found:
        #     add all adjoining 1, s to visited grid:
        #     island count += 1
        #     return to last 1, search again not visited for 1:
class Solution(object):
    def numIslands(self, grid):
        if not grid:
            return 0

        rows, cols = len(grid), len(grid[0])
        visit = set()
        islands = 0

        def bfs(r,c):
            q = collections.deque()
            visit.add((r,c))
            q.append((r,c))
            while q:
                row,col = q.popleft()
                directions = [[1,0],[-1,0],[0,1],[0,-1]]
                for dr,dc in directions:
                    r,c = row + dr, col + dc
                    if (r in range(rows) and
                        c in range(cols) and
                        grid[r][c] == "1" and
                        (r,c) not in visit):

                        q.append((r,c))
                        visit.add((r,c))

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == "1" and (r,c) not in visit:
                    bfs(r,c)
                    islands += 1
        return islands



seeking to optimize using chat gpt


from collections import deque

class Solution:
    def numIslands(self, grid):
        if not grid:
            return 0

        rows, cols = len(grid), len(grid[0])
        islands = 0

        def bfs(r, c):
            q = deque([(r, c)])
            grid[r][c] = '0'  # Mark as visited by setting to '0'
            while q:
                row, col = q.popleft()
                for dr, dc in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
                    rr, cc = row + dr, col + dc
                    if 0 <= rr < rows and 0 <= cc < cols and grid[rr][cc] == '1':
                        q.append((rr, cc))
                        grid[rr][cc] = '0'  # Mark as visited

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == '1':
                    bfs(r, c)
                    islands += 1
        
        return islands

# Example usage:
grid = [
    ["1","1","1","1","0"],
    ["1","1","0","1","0"],
    ["1","1","0","0","0"],
    ["0","0","0","0","0"]
]

solution = Solution()
print(solution.numIslands(grid))  # Output: 1

#### Conclusion

The optimized version of the code produces runtime of 228 ms(beats 84.86%) and 18.85mb(beats 71.46%) ram space

non optimized is :

308ms (beats 16.10%)
24.08 mb (beats 36.45%)

This problem is hard to optimize, the ideal solution changes values in place, so no additional set of visited is needed sharing memory. this is a dangerous data practice and maybe suitable for recreational or scientific approach, it is not ideal to manipulate the original data.    The time and space is based on O(m+n) as the grid size is m * n so it expected to sweep one way left to right for every row. Space with this approach is nominally above n+m as we are doing operation of O(1) per node and only handling the integer num for island count.





















ADDON:
this is one of the python 3 ranked answers and the results.

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        n, m=len(grid), len(grid[0])
        def inside(i, j):
            return 0<=i and i<n and 0<=j and j<m
        def dfs(i, j):
            if not inside(i, j): return
            if grid[i][j]=='1':
                grid[i][j]='2'
                dfs(i+1,j)
                dfs(i,j+1)
                dfs(i-1,j)
                dfs(i,j-1)
        num=0
        for i, row in enumerate(grid):
            for j, x in enumerate(row):
                if x=='1':
                    num+=1
                    dfs(i, j)
        return num

        
returns :

247 ms beats 53.99%
18.81 mb beats 71.46%