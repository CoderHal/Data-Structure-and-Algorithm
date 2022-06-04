# Graph

## 733. Flood Fill

```java
class Solution {
  public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
    if (image[sr][sc] != newColor) {
      DFS(image, sr, sc, newColor, image[sr][sc]);
    }
    return image;
  }
  public void DFS(int[][] image, int row, int col, int newColor, int target) {
    if (row >= image.length || col >= image[0].length || row < 0 || col < 0 || image[row][col] != target) {
      return;
    }
    image[row][col] = newColor;
    DFS(image, row + 1, col, newColor, target);
    DFS(image, row, col + 1, newColor, target);
    DFS(image, row - 1, col, newColor, target);
    DFS(image, row, col - 1, newColor, target);
    return;
  }
}
```

Watch out the situation : image[sr] [sc] == newColor, in this case, you don't need to do anything, just return the original array. You must add this condition, cause it may lead to stackoverflow



## 200. Number of Island

### Union-Find (aka Disjoint Set) 

```java
class Solution {
  class UnionFind{
    int count; // connected components
    int[] rank;
    int[] parent;
      
    public UnionFind(char[][] grid) {
      count = 0;
      int n = grid.length;
      int m = grid[0].length;
      parent = new int[n * m];
      rank = new int[n * m];
      for(int i = 0; i < n; i++) {
        for(int j = 0; j < m; j++) {
          if (grid[i][j] == '1') {
            parent[i * m + j] = i * m + j;
            ++count;
          }
          rank[i * m + j] = 0;
        }
      }
    }
    
    public int find(int x) {
      if (x != parent[x]) {
        parent[x] = find(parent[x]);
      }
      return parent[x];
    }
    
    public void union(int x, int y) {
      int px = find(x), py = find(y);
      if (px != py) {
        if(rank[px] > rank[py]) {
        parent[py] = px;
        }
        else if (rank[px] < rank[py]) {
          parent[px] = py;
        } else {
          parent[py] = px;
          rank[px] += 1;
        }
        --count;
      }
    }
    
    public int getCount(){
      return count;
    }
  }
  
  public int numIslands(char[][] grid) {
    int n = grid.length;
    int m = grid[0].length;	
    if (n == 0 || m == 0) {
      return 0;
    }
    UnionFind uf = new UnionFind(grid);
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < m; j++) {
        if (grid[i][j] == '1') {
          grid[i][j] = '0';
          if (i - 1 >= 0 && grid[i - 1][j] == '1') {
            uf.union(i * m + j, (i - 1) * m + j);
          }
          if (i + 1 < n && grid[i + 1][j] == '1') {
            uf.union(i * m + j, (i + 1) * m + j);
          }
          if (j - 1 >= 0 && grid[i][j - 1] == '1') {
            uf.union(i * m + j, i * m + (j - 1));
          }
          if (j + 1 < m && grid[i][j + 1] == '1') {
            uf.union(i * m + j, i * m + (j + 1));
          }
        }
      }
    }
    return uf.getCount();
  }
}
```



## 695. Max Area of Island

### 1. DFS

```java
class Solution {
  public int c;
  public int maxAreaOfIsland(int[][] grid) {
    int n = grid.length;
    int m = grid[0].length;
    int res = 0;
    if (n == 0 || m == 0) {
      return 0;
    }
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < m; j++) {
        c = 0;
        DFS(grid, i, j);
        res = Math.max(res, c);
      }
    }
    return res;
  }
  public void DFS(int[][] grid, int row, int col) {
    int n = grid.length;
    int m = grid[0].length;
    if (row < 0 || col < 0 || row >= n|| col >= m || grid[row][col] != 1) {
      return;
    }
    grid[row][col] = 0;
    c++;
    DFS(grid, row - 1, col);
    DFS(grid, row + 1, col);
    DFS(grid, row, col - 1);
    DFS(grid, row, col + 1);

  }
}
```


## 1254. Number of Closed Islands

### 1. DFS

```java
class Solution {
  public int closedIsland(int[][] grid) {
    int n = grid.length;
    int m = grid[0].length;
    if (n == 0 || m == 0) {
      return 0;
    }
    int num = 0;
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < m; j++) {
        if (grid[i][j] == 0) {
          if (DFS(grid, i, j)) {
            res++;
          }
        }
      }
    }
    return res;
  }
  public void DFS(int[][] grid, int row, int col) {
    int n = grid.length;
    int m = grid[0].length;
    if (row < 0 || row >= n || col < 0 || col >= m) {
      return false;
    } 
    if (grid[row][col] == 1) {
      return true;
    }
    grid[row][col] == 1;
    boolean close = true;
    return close && DFS(grid, row - 1, col) && DFS(grid, row + 1, col) && DFS(grid, row, col - 1) && DFS(grid, row, col + 1);
  }
}
```

"&" will evaluate both side even the left part is false
"&&" will ignore the right part if the left part is false

res = res && dfs(grid, x + d[0], y + d[1]);
if res is false, the right dfs will not be evaluated.
**for this question, we need to enter dfs(grid, x + d[0], y + d[1]) no matter what.** （important！！！）

for example:
111
101
100
111
think about if we are in the position of 0 at (2,1) - if dfs goes to right 0 (2, 2) first, res will be false
in the meantime, we still want to dfs go to (1, 1)'s 0 to set it as 1 even res is false

```java
        for(int[] d : dir){
            if(!dfs(grid, x + d[0], y + d[1])){
                res = false;
            }
        }
```



## 1020. Number of Enclaves

### 1. DFS

```java
class Solution {
  int count = 0;
  public int numEnclaves(int[][] grid) {
    int n = grid.length;
    int m = grid[0].length;
    if (n == 0 || m == 0) {
      return 0;
    }
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < m; j++) {
        int pre = count;
        if (grid[i][j] == 1) {
          if (DFS(grid, i, j)) {
            count = pre;
          }
        }  
      }
    }
    return count;
  }
  public boolean DFS(int[][] grid, int row, int col) {
    int n = grid.length;
    int m = grid[0].length;
    if (row < 0 || row >= n || col < 0 || col >= m || grid[row][col] == 0) {
      return false;
    }
    boolean res = false;
    grid[row][col] = 0;
    count++;
    if (row == 0 || row == n - 1 || col == 0 || col == m - 1) {
      res = true;
    }
    return res | DFS(grid, row - 1, col) | DFS(grid, row + 1, col) | DFS(grid, row, col - 1) | DFS(grid, row, col + 1);
  }
}

```



## 1905. Count Sub Islands

### 1. DFS

```java
class Solution {
  public int countSubIslands(int[][] grid1, int[][] grid2) {
    int n = grid1.length;
    int m = grid1[0].length;
    int count = 0;
    for(int i = 0; i < n; i++) {
      for(int j = 0; j < m; j++) {
        if (grid2[i][j] == 1) {
          count += DFS(grid1, grid2, i, j);
        }
      }
    }
    return count;
  }
  public int DFS(int[][] grid1, int[][] grid2, int row, int col) {
    int n = grid1.length;
    int m = grid1[0].length;
    int res = 1;
    if (row < 0 || row >= n || col < 0 || col >= m || grid2[row][col] == 0){
      return 1;
    }
    grid2[row][col] = 0;
    
    res &= DFS(grid1, grid2, row - 1, col);
    res &= DFS(grid1, grid2, row + 1, col);
    res &= DFS(grid1, grid2, row, col - 1);
    res &= DFS(grid1, grid2, row, col + 1);
    return res & grid1[row][col];
  }
}
```

### 2. Using Boolean 

```java
class Solution {
  public int countSubIslands(int[][] grid1, int[][] grid2) {
    int n = grid1.length;
    int m = grid1[0].length;
    int count = 0;
    for(int i = 0; i < n; i++) {
      for(int j = 0; j < m; j++) {
        if (grid2[i][j] == 1) {
          if(DFS(grid1, grid2, i, j)) {
              count++;
          }
        }
      }
    }
    return count;
  }
  public boolean DFS(int[][] grid1, int[][] grid2, int row, int col) {
    int n = grid1.length;
    int m = grid1[0].length;
    boolean res = true;
    if (row < 0 || row >= n || col < 0 || col >= m || grid2[row][col] == 0){
      return true;
    }
    grid2[row][col] = 0;
    if (grid1[row][col] == 0){
        res = false;
    }
    return res & DFS(grid1, grid2, row - 1, col) & DFS(grid1, grid2, row + 1, col) & DFS(grid1, grid2, row, col - 1) & res & DFS(grid1, grid2, row, col + 1);
  }
}
```



## 1162. As Far from Land as Possible

### 1. DFS (low speed)

```java
class Solution{
  public int maxDistance(int[][] grid) {
    int res = 0;
    int n = grid.length;
    int m = grid[0].length;
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < m; j++) {
        if (grid[i][j] == 1) {
          grid[i][j] = 0;
          DFS(grid, i, j, 1);
        }
      }
    }
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < m; j++) {
        res = Math.max(res, grid[i][j] - 1);
      }
    }
    return res;
  }
  public void DFS(int[][] grid, int row, int col, int dis) {
    int n = grid.length;
    int m = grid[0].length;
    if (row < 0 || row = n || col < 0 || col = m || (grid[row][col] != 0 && grid[row][col] <= dis)) {
      return;
    }
    grid[row][col] = dis;
    DFS(grid, row - 1, col, dis + 1);
    DFS(grid, row + 1, col, dis + 1);
    DFS(grid, row, col - 1, dis + 1);
    DFS(grid, row, col + 1, dis + 1);
    return;
  }
}
```

### 2. BFS

```java
class Solution {
  public int maxDistance(int[][] grid) {
    int[][] dir = new int[][]{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    Queue<int[]> queue = new LinkedList<>();
    int n = grid.length;
    int m = grid[0].length;
    boolean[][] visit = new boolean[n][m];
    for(int i = 0; i < n; i++) {
      for(int j = 0; j < m; j++) {
        if(grid[i][j] == 1) {
          queue.add(new int[]{i, j});
          visit[i][j] = true;
        }
      }
    }
    if (queue.size() == m * n) {
      return -1;
    }
    int dis = 1;
    while (!queue.isEmpty()) {
      int s = queue.size();
      for (int i = 0; i < s; i++){ 
        int[] cur = queue.poll();
        int rol = cur[0];
        int col = cur[1];
        for (int j = 0; j < dir.length; j++){
          int newR = rol + dir[j][0];
          int newC = col + dir[j][1];
          if (newR >= 0 && newC >= 0 && newR < n && newC <m && !visit[newR][newC]) {
            queue.add(new int[]{newR, newC});
            visit[newR][newC] = true;
          }
        }
      }
      dis++;
    }
    return (dis == 1) ? -1 : dis - 2;
  }
}
```



Here, we find our land cells and put surrounding water cells in the queue. We mark water cells as visited and continue the expansion from land cells until there are no more water cells left. In the end, the number of steps in DFS is how far can we go from the land.



Using the same example as above, the picture below shows the state of the grid after each step of BFS
![image](https://assets.leetcode.com/users/votrubac/image_1566199213.png)
