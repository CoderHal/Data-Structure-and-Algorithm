# DFS & BFS

## 200. Number of Islands

### 1. DFS

``` java
class Solution {
  public int numIslands(char[][] grid) {
    int nr = grid.length;
    int nc = grid[0].length;
    if (grid == null || nr == 0) {
      return 0;
    }
    int numL = 0;
    for (int i = 0; i < nr; i++) {
      for (int j = 0; j < nc; j++) {
        if (grid[i][j] == '1') {
          numL++;
          dfs(grid, i, j);
        }
      }
    }
    return numL;
  }
  public void dfs (char[][] grid, int r, int c) {
    int nr = grid.length;
    int nc = grid[0].length;
    if (r < 0 || c < 0 || r >= nr || c >= nc || grid[r][c] == '0') {
      return;
    }
    grid[r][c] = '0'; // change it to '0' in order to mark this location has been traversed 
    dfs(grid, r - 1, c);
    dfs(grid, r + 1, c);
    dfs(grid, r, c - 1);
    dfs(grid, r, c + 1);
    // Time O(m * n)
    // Space O(m * n)
  }
}
```

### 2. BFS

```java
class Solution {
  public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) {
      return 0;
    }
    int nr = grid.length; 
    int nc = grid[0].length;
    int numL = 0;
    for (int i = 0; i < nr; i++) {
      for (int j = 0; j < nc; j++) {
        if (grid[i][j] == '1') {
          numL++;
          Queue<int[]> queue = new LinkedList<>();//store the adjacent nodes value of '1'
          grid[i][j] = 0;
          queue.add(new int[]{i, j});
          while (!queue.isEmpty()) {
            int[] cur = queue.remove();
            int r = cur[0];
            int c = cur[1];
            if (r - 1 >= 0 && grid[r - 1][c] == '1') {
                          queue.add(new int[]{r - 1, c});
                          grid[r - 1][c] = '0';
            }
            if (r + 1 < nr && grid[r + 1][c] == '1') {
              queue.add(new int[]{r + 1, c});
              grid[r + 1][c] = '0';
            }
            if (c - 1 >= 0 && grid[r][c - 1] == '1') {
              queue.add(new int[]{r, c - 1});
              grid[r][c - 1] = '0';
            }
            if (c + 1 < nc && grid[r][c + 1] == '1') {
              queue.add(new int[]{r, c + 1});
              grid[r][c + 1] = '0';
            }
          }
        }
      }
    }
    return numL;
    //Time O(m * n)
    //Space O(min (m, n))
  }
}
```



## 547. Number of Provinces

### 1. DFS

```java
class Solution {
  public int findCircleNum(int[][] M) {
    int n = M.length;
    int numP = 0;
    int[] visited = new int[n];
    for (int i = 0; i < n; i++) {
      if (visited[i] == 0) {
        dfs(M, visited, i);
        numP++;
      }
    }
    return numP;
  }
  public void dfs(int[][] M, int[] visited, int i) {
    for (int j = 0; j < M.length; j++) {
      if (M[i][j] == 1 && visited[j] == 0) {
        visited[j] = 1;
        dfs(M, visited, j);
      }
    }
    //Time O(n * n)
    // Space O(n)
  }
}
```

![image-20220404163341286](/Users/youhao/Library/Application Support/typora-user-images/image-20220404163341286.png)

### 2. BFS

```java
class Solution {
  public int findCircleNum(int[][] M) {
    int n = M.length;
    int[] visit = new int[n];
    int numP = 0;
    for (int i = 0; i < n; i++) {
      if (visit[i] == 0) {
        Queue<Integer> queue = new LinkedList<>();
        queue.add(i);
        while (!queue.isEmpty()) {
          int cur = queue.remove();
          visit[cur] = 1;
          for (int j = 0; j < n; j++) {
            if (M[cur][j] == 1 && visit[j] == 0) {
              queue.add(j);
            }
          }
        }
        numP++;
      }
    }
    return numP;
    //Time O(n * n)
    //Space O(n)
  }
}
```



## 116. Populating Next Right Pointers in Each Node

### BFS 

```java
class Solution {
  public Node connect(Node root) {
    Node cur = root;// record the current node
    while (cur != null) {
      Node head = cur; //Using head node to help cur node keep the origin state, when it finishes its traversal
      if (cur.left != null) {//Because it is a balanced tree, so if it has left node, it must have right node
        cur.left.next = cur.right;
      }
      if (cur.right != null && cur.next != null) { // we need to promise the cur.next is not null
        cur.right.next = cur.next.left;// populating next right pointer
      }
      cur = cur.next; // move to next node to continue connecting the link
    }
    cur = head.left; // Use head node to move to the next level
  }
  return root;
  // Time O(n) traversal each node
  // Space O(1)
}
```



## 117.  Populating Next Right Pointers in Each Node II

### BFS

```java
class Solution {
  public Node connect(Node root) {
    Node cur = root; 
    Node head = null;// record the first node of each link
    Node pre = null;// pre node is used to connect the link
    while (cur != null) {
      while (cur != null) {
        if (cur.left != null) {
          if (head == null) { // we haven't found the first node in this level yet
            head = cur.left;
            pre = cur.left;
          } else { // we have fount the first node in this level
            pre.next = cur.left;// just connect the pre and cur.left
            pre = pre.next;
          }
        }
        if (cur.right != null) {// same with cur.left
          if (head == null) {
            head = cur.right;
            pre = cur.right;
          } else {
            pre.next = cur.right;
            pre = pre.next; // keep the pre alway being the last node of this link
          }
        }
        cur = cur.next;
      }
      cur = head; // the next level's first node is head, so we just need to make the cur equal to the head
      head = null; // initial the head & pre node, in order to operate the next level node.
      pre = null;
      
    }
    return root;
    // Time O(n)
    // Space O(1)
  }
}
```

![image-20220407130700485](/Users/youhao/Library/Application Support/typora-user-images/image-20220407130700485.png)

## 572. Subtree of Another Tree

### 1. DFS

```java
class Solution {
  public boolean isSubTree(TreeNode root, TreeNode subRoot) {
    if (root == null) {
      return false; //the root has been traversed, and cannot find the subRoot
    }
    if (checkSub(root, subRoot)) {
      return true; // check the current root whether if it is same as the subRoot
    }
    return isSubTree(root.left, subRoot) || isSubTree(root.right, subRoot);// PreOrder Traversal to find the subTree
  }
  public boolean checkSub(TreeNode root, TreeNode subRoot) {
    if (root == null && subRoot == null) {
      return true;
    }
    if (root == null || subRoot == null) {
      return false;
    }
    if (root.val != subRoot.val) {
      return false;
    }
    return checkSub(root.left, subRoot.left) && checkSub(root.right, subRoot.right);
    //Time O(m * n)
    //Space O(1)
  }
}
```



## 1091. Shortest Path in Binary Matrix

Finding the shortest path between two nodes in a graph is almost always done using BFS, and all programmers should know this. BFS is one of the fundamental algorithms that you are *expected* to be confident coding before a tech interview. So, if you're finding this question challenging, then you're doing the right thing by working on it now.

### 1. BFS (overwrite the input)

```java
class Solution {
    
    private static final int[][] directions = 
        new int[][]{{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};
    
    public int shortestPathBinaryMatrix(int[][] grid) {
        
        // Firstly, we need to check that the start and target cells are open.
        if (grid[0][0] != 0 || grid[grid.length - 1][grid[0].length - 1] != 0) {
            return -1;
        }
        
        // Set up the BFS.
        Queue<int[]> queue = new ArrayDeque<>();
        grid[0][0] = 1;
        queue.add(new int[]{0, 0});
        
        // Carry out the BFS
        while (!queue.isEmpty()) {
            int[] cell = queue.remove();
            int row = cell[0];
            int col = cell[1];
            int distance = grid[row][col];
            if (row == grid.length - 1 && col == grid[0].length - 1) {
                return distance;
            }
            for (int[] neighbour : getNeighbours(row, col, grid)) {
                int neighbourRow = neighbour[0];
                int neighbourCol = neighbour[1];
                queue.add(new int[]{neighbourRow, neighbourCol});
                grid[neighbourRow][neighbourCol] = distance + 1;
            }
        }
        
        // The target was unreachable.
        return -1;
    }
    
    private List<int[]> getNeighbours(int row, int col, int[][] grid) {
        List<int[]> neighbours = new ArrayList<>();
        for (int i = 0; i < directions.length; i++) {
            int newRow = row + directions[i][0];
            int newCol = col + directions[i][1];
            if (newRow < 0 || newCol < 0 || newRow >= grid.length 
                    || newCol >= grid[0].length
                    || grid[newRow][newCol] != 0) {
                continue;    
            }
            neighbours.add(new int[]{newRow, newCol});
        }
        return neighbours; 
    }
    //Time O(n)
    //Space O(n)
}
```



### 2. BFS (Without Overwriting the Input)

```java
class Solution {
    
    private static final int[][] directions = 
        new int[][]{{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};
    
    
    public int shortestPathBinaryMatrix(int[][] grid) {
        
        // Firstly, we need to check that the start and target cells are open.
        if (grid[0][0] != 0 || grid[grid.length - 1][grid[0].length - 1] != 0) {
            return -1;
        }
        
        // Set up the BFS.
        Queue<int[]> queue = new ArrayDeque<>();
        queue.add(new int[]{0, 0, 1}); // Put distance on the queue
        boolean[][] visited = new boolean[grid.length][grid[0].length]; // Used as visited set.
        visited[0][0] = true;
        
        // Carry out the BFS
        while (!queue.isEmpty()) {
            int[] cell = queue.remove();
            int row = cell[0];
            int col = cell[1];
            int distance = cell[2];
            // Check if this is the target cell.
            if (row == grid.length - 1 && col == grid[0].length - 1) {
                return distance;
            }
            for (int[] neighbour : getNeighbours(row, col, grid)) {
                int neighbourRow = neighbour[0];
                int neighbourCol = neighbour[1];
                if (visited[neighbourRow][neighbourCol]) {
                    continue;
                }
                visited[neighbourRow][neighbourCol] = true;
                queue.add(new int[]{neighbourRow, neighbourCol, distance + 1});
            }
        }
        
        // The target was unreachable.
        return -1;  
    }
    
    private List<int[]> getNeighbours(int row, int col, int[][] grid) {
        List<int[]> neighbours = new ArrayList<>();
        for (int i = 0; i < directions.length; i++) {
            int newRow = row + directions[i][0];
            int newCol = col + directions[i][1];
            if (newRow < 0 || newCol < 0 || newRow >= grid.length 
                    || newCol >= grid[0].length
                    || grid[newRow][newCol] != 0) {
                continue;    
            }
            neighbours.add(new int[]{newRow, newCol});
        }
        return neighbours; 
    }
  //Time O(n)
  // Space O(n)
}
```

## 130. Surrounded Regions

### 1. DFS

```java
class Solution {
  public void solve(char[][] board) {
    int n = board.length;
    int m = board[0].length;
    /*---find out the border which have the 'O' and find its neighbor 'O'--*/
    for (int i = 0; i < n; i++) {
      if (board[i][0] == 'O') {
        dfs(board, i, 0);
      }
      if (board[i][m - 1] == 'O') {
        dfs(board, i, m - 1);
      }
    }
    for (int j = 0; j < m; j++) {
      if (board[0][j] == 'O'){
        dfs(board, 0, j);
      }
      if (board[n - 1][j] == 'O') {
        dfs(board, n - 1, j);
      }
    }
    /*---------------------------------------------------------------------*/
    /*---the rest of 'O' can be captured-----------------------------------*/
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < m; j++) {
        if (board[i][j] == 'O') {
          board[i][j] = 'X';
        }
        if (board[i][j] == 'V') {
          board[i][j] = 'O';
        }  
      }
    }
    return;
  }
  public void dfs(char[][] board, int row, int col) {
    int n = board.length;
    int m = board[0].length;
    if (row < 0 || col < 0 || row >= n || col >= m || board[row][col] == 'X' || board[row][col] == 'V') {
      return;
    }
    board[row][col] = 'V';
    dfs(board, row - 1, col);
    dfs(board, row + 1, col);
    dfs(board, row, col - 1);
    dfs(board, row, col + 1);
    return;
    // Time O(N) N = m * n
    // Space O(N) N = m * n
  }
}
```

### 2. BFS

```java
class Solution {
  public void solve(char[][] board) {
    int n = board.length;
    int m = board[0].length;
    for (int i = 0; i < n; i++) {
      if (board[i][0] == 'O') {
        bfs(board, i, 0);
      }
      if (board[i][m - 1] == 'O') {
        bfs(board, i, m - 1);
      }
    }
    for (int j = 0; j < m; j++) {
      if (board[0][j] == 'O') {
        bfs(board, 0, j);
      }
      if (board[n - 1][j] == 'O') {
        bfs(board, n - 1, j);
      }
    }
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < m; j++) {
        if (board[i][j] == 'O') {
          board[i][j] = 'X';
        }
        if (board[i][j] == 'V') {
          board[i][j] = 'O';
        }  
      }
    }
  }
  public void bfs(char[][] board, int row, int col) {
    int n = board.length;
    int m = board[0].length;
    Queue<int[]> res = new LinkedList<>();
    res.add(new int[]{row, col});
    board[row][col] = 'V';
    while (!res.isEmpty()) {
      int[] cell = res.remove();
      int newR = cell[0];
      int newC = cell[1];
      if (newR - 1 >= 0 && board[newR - 1][newC] == 'O') {
        res.add(new int[]{newR - 1, newC});
        board[newR - 1][newC] = 'V';
      }
      if (newR + 1 < n && board[newR + 1][newC] == 'O') {
        res.add(new int[]{newR + 1, newC});
        board[newR + 1][newC] = 'V';
      }
      if (newC - 1 >= 0 && board[newR][newC - 1] == 'O') {
        res.add(new int[]{newR, newC - 1});
        board[newR][newC - 1] = 'V';
      }
      if (newC + 1 < m && board[newR][newC + 1] == 'O') {
        res.add(new int[]{newR, newC + 1});
        board[newR][newC + 1] = 'V';
      }
    }
    return;
    }
  //Time O(N) N = m * n
  //Space O(N) N = m * n
  }
```

## 797. All Paths From Source to Target

### 1. DFS

```java
class Solution {
  public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    path.add(0);
    dfs(graph, res, path, 0);
    return res;
  }
  public void dfs(int[][] graph, List<List<Integer>> res, List<Integer> path, int node) {
    if (node == graph.length - 1){// arrive at the terminal node
      res.add(new ArrayList<>(path));
      return;
    }
    for(int cur : graph[node]) {
      path.add(cur); // increase the path
      dfs(graph, res, path, cur);
      path.remove(path.size() - 1);// remove all the node except the start node
    }
  }
}
```

