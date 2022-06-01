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

