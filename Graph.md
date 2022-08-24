# Graph

# 基本算法

## 1. 存储方式

邻接表

邻接矩阵

```java
// 邻接表
// graph[x] 存储 x 的所有邻居节点
List<Integer>[] graph;

// 邻接矩阵
// matrix[x][y] 记录 x 是否有一条指向 y 的边
boolean[][] matrix;

```

有向加权图

```java
// 邻接表
// graph[x] 存储 x 的所有邻居节点以及对应的权重
List<int[]>[] graph;

// 邻接矩阵
// matrix[x][y] 记录 x 指向 y 的边的权重，0 表示不相邻
int[][] matrix;
```



## 2. 遍历

### (1) 多叉树的遍历

```java
//多叉树的遍历

void traverse(TreeNode root) {
  if (root == null) {return;}
  //前序位置
  for (TreeNode child : root.children) {
    traverse(child);
  }
  //后序位置
}
```

### (2) 图的遍历和多叉树遍历的比较

图很可能有环， 如果从图的某一点开始遍历，有可能走了一圈回到原点。所以需要visited数组来辅助

```java
// 记录被遍历过的节点
boolean[] visited;
// 记录从起点到当前节点的路径
boolean[] onPath;

/* 图遍历框架 */
void traverse(Graph graph, int s) {
    if (visited[s]) return;
    // 经过节点 s，标记为已遍历
    visited[s] = true;
    // 做选择：标记节点 s 在路径上
    onPath[s] = true;
    for (int neighbor : graph.neighbors(s)) {
        traverse(graph, neighbor);
    }
    // 撤销选择：节点 s 离开路径
    onPath[s] = false;
}

```

### (3) DFS 和 回溯算法比较

```java
class Solution {
  // DFS 算法，关注点在节点
  void traverse(TreeNode root) {
    if (root == null) {
      return null;
    }
    printf("进入节点 %s", root);
    for (TreeNode child : root.children) {
        traverse(child);
    }
    printf("离开节点 %s", root);
  }
  
  //回溯算法，关注点在树枝上
  void backtrack(TreeNode root) {
    if (root == null) {
      return null;
    }
    for (TreeNode child : root.children) {
        // 做选择
        printf("从 %s 到 %s", root, child);
        backtrack(child);
        // 撤销选择
        printf("从 %s 到 %s", child, root);
    }
  }
}
```

## 797. All Paths From Source to Target

### 1. DFS

```java
class Solution {
    // 记录所有路径
    List<List<Integer>> res = new LinkedList<>();

    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        LinkedList<Integer> path = new LinkedList<>();
        traverse(graph, 0, path);
        return res;
    }

    /* 图的遍历框架 */
    void traverse(int[][] graph, int s, LinkedList<Integer> path) {

        // 添加节点 s 到路径
        path.addLast(s);

        int n = graph.length;
        if (s == n - 1) {
            // 到达终点
            res.add(new LinkedList<>(path));
            path.removeLast();
            return;
        }

        // 递归每个相邻节点
        for (int v : graph[s]) {
            traverse(graph, v, path);
        }

        // 从路径移出节点 s
        path.removeLast();
    }
}

```



## 133. Clone Graph

### 1. DFS

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/
class Solution {
  private HashMap<Node, Node> visited= new HashMap<>();
  public Node cloneGraph(Node node) {
    if (node == null) {
      return node;
    }
    if (visited.containsKey(node)) {
      //如果存在，就说明回到了原点，要避免无限循环
      //需要return visited.get(node)， 返回克隆的节点
      return visited.get(node);
    }
    Node cloneNode = new Node(node.val, new ArrayList<>());
    visited.put(node, cloneNode);
    //添加他的邻居
    for (Node n : node.neighbors) {
      cloneNode.neightbors.add(cloneGraph(n));
    }
    return cloneNode;
  }
}
```



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



## 417. Pacific Atlantic Water Flow

### 1. BFS

```java
class Solution {
  private int[][] dir = new int[][]{{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
  public List<List<Integer>> pacificAtlantic(int[][] heights) {
    int n = heights.length;
    int m = heights[0].length;
    List<List<Integer>> res = new ArrayList<>();
    if (n == 0 || m == 0) {
      return res;
    }
    Queue<int[]> q1 = new LinkedList<>();
    Queue<int[]> q2 = new LinkedList<>();
    for (int i = 0; i < n; i++){
      q1.add(new int[]{i, 0});
      q2.add(new int[]{i, m - 1});
    }
    for (int j = 0; j < m; j++) {
      q1.add(new int[]{0, j});
      q2.add(new int[]{n - 1, j});
    }
    boolean[][] r1 = BFS(heights, q1);
    boolean[][] r2 = BFS(heights, q2);
    for (int i = 0; i < n; i++){
      for (int j = 0; j < m; j++){
        if (r1[i][j] && r2[i][j]) {
          res.add(List.of(i,j));
        }
      }
    }
    return res;
  }
  public boolean[][] BFS(int[][] heights, Queue<int[]> queue) {
    int n = heights.length;
    int m = heights[0].length;
    boolean[][] reach = new boolean[n][m];
    while (!queue.isEmpty()) {
      int[] cur = queue.poll();
      int row = cur[0];
      int col = cur[1];
      reach[row][col] = true;
      for (int i = 0; i < 4; i++) {
        int newR = row + dir[i][0];
        int newC = col + dir[i][1];
        if (newR < 0 || newR == n || newC < 0|| newC == m){
          continue;
        }
        if (reach[newR][newC]){
          continue;
        }
        if (heights[newR][newC] < heights[row][col]) {
          continue;
        }
        queue.add(new int[]{newR, newC});
      }
    }
    return reach;
  }
}
```

This problem want to solve which land water can both flow to the Pacific Ocean and Atlantic Ocean.

Method : Using two boolean matrix to traversal, find their common “true”



## 841. Keys and Rooms

### 1. BFS

```java
class Solution{
  public boolean canVisitAllRooms(List<List<Integer>> rooms){
    if(rooms.isEmpty()) {
      return false;
    }
    boolean[] visited = new boolean[rooms.size()];
    Queue<Integer> queue = new LinkedList<>();
    List<Integer> zero = rooms.get(0);
    for (int i = 0; i < zero.size(); i++) {
      queue.add(zero.get(i));
    }
    visited[0] = true;
    while (!queue.isEmpty()) {
      int key = queue.poll();
      visited[key] = true;
      List<Integer> cur = rooms.get(key);
      for (int i = 0; i < cur.size(); i++) {
        int index = cur.get(i);
        if (!visited[index])
          queue.add(cur.get(i));
      }
    }
    boolean res = true;
    for (int i = 0; i < visited.length; i++) {
      res &= visited[i];
    }
    return res;
  }
}
```



## 207. Course Schedule

### 1. Using Queue and Array 

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        int[] requisteNum = new int[numCourses];
        for (int[] cur : prerequisites) {
            requisteNum[cur[0]]++;
        }
        
        // add all the courses that dont need the prerequistes
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < requisteNum.length; i++) {
            if (requisteNum[i] == 0) {
                queue.add(i);
            }
        }
        // make all the courses in the queue to match the prerequistes[][]
        while (!queue.isEmpty()) {
            int curCourse = queue.poll();
            for (int[] cur : prerequisites) {
                //this case means the current course can be taken now and don't need the prerequisites
                //so we can step to the next
                if (requisteNum[cur[0]] == 0) {
                    continue;
                }
                if (curCourse == cur[1]) {
                    requisteNum[cur[0]]--;
                }
                if (requisteNum[cur[0]] == 0) {
                    queue.add(cur[0]);
                }
            }
        }
        
        for (int num : requisteNum) {
            if (num != 0){
                return false;
            }
        }
        return true;
    }
}
```



### 2. Topological Sorting + DFS

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
      List<List<Integer>> graph = new ArrayList<>();
      for (int i = 0; i < numCourses; i++) {
        graph.add(new ArrayList<>());
      }
      
      for (int[] cur : prerequisites) {
        graph.get(cur[0]).add(cur[1]);
      }
      int visited[] = new int[numCourses];
      for (int i = 0; i < numCourses; i++) {
        if (DFS(i, graph, visited)) return false;
      }
      return true;
    }
    public Boolean DFS(int cur, List<List<Integer>> graph, int[] visited) {
      if (visited[cur] == 2) {return false;}
      if (visited[cur] == 1) {return true;}
      
      visited[cur] = 1;
      for (int next : graph.get(cur)) {
        if (DFS(next, graph, visited)) return true;
      }
      visited[cur] = 2;
      return false;
    }
}
```

找环：

![image-20220716220258640](/Users/youhao/Library/Application Support/typora-user-images/image-20220716220258640.png)

为每个节点创造一个ArrayList

对于该节点的先修课程进行遍历，如果遇到了原来走过的点，就说明有环存在。 在遍历过程中visited[cur] = 2说明后面没有环就可以直接结束讨论





## 210. Course Schedule II

### 1. Use Queue

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        int[] need = new int[numCourses];
        for (int[] cur : prerequisites) {
           need[cur[0]]++; 
        }
        Queue<Integer> queue = new LinkedList<>();
        int[] res = new int[numCourses];
        int index = 0;
        for (int i = 0; i < need.length; i++) {
            if (need[i] == 0) {
                queue.add(i);
                res[index++] = i;
            }
        }
        
        while(!queue.isEmpty()) {
            int curCourse = queue.poll();
            for (int[] cur : prerequisites) {
                if (need[cur[0]] == 0) {
                    continue;
                }
                
                if (cur[1] == curCourse) {
                    need[cur[0]]--;
                }
                
                if (need[cur[0]] == 0) {
                    queue.add(cur[0]);
                    res[index++] = cur[0];
                }
            }
        }
        for (int i = 0; i < need.length; i++) {
            if (need[i] != 0) {
                return new int[0];
            }
        }
       return res;
    }
}
```

Same as 207



### 2. Topological Sorting + DFS

```java
class Solution {
    private int index;
    public int[] findOrder(int numCourses, int[][] prerequisites) {
      index = 0;
      List<List<Integer>> graph = new ArrayList<>();
      int[] ans = new int[numCourses];
      for (int i = 0; i < numCourses; i++) {
        graph.add(new ArrayList<>());
      }
      
      for (int[] cur : prerequisites) {
        graph.get(cur[0]).add(cur[1]);
      }
      int visited[] = new int[numCourses];
      for (int i = 0; i < numCourses; i++) {
        if (DFS(i, graph, visited, ans)) return new int[0];
      }
      return ans;
    }
    public Boolean DFS(int cur, List<List<Integer>> graph, int[] visited, int[] ans) {
      if (visited[cur] == 2) {return false;}
      if (visited[cur] == 1) {return true;}
      
      visited[cur] = 1;
      for (int next : graph.get(cur)) {
        if (DFS(next, graph, visited, ans)) return true;
      }
      ans[index++] = cur;
      visited[cur] = 2;
      return false;
    }
}
```

