# Graph

# 基本算法

## 1⃣️. 存储方式

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



## 2⃣️. 遍历

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

## 3⃣️. 拓扑排序

Course II



## 4⃣️. 二分图

比如说我们需要一种数据结构来储存电影和演员之间的关系：某一部电影肯定是由多位演员出演的，且某一位演员可能会出演多部电影。你使用什么数据结构来存储这种关系呢？

既然是存储映射关系，最简单的不就是使用哈希表嘛，我们可以使用一个 `HashMap<String, List<String>>` 来存储电影到演员列表的映射，如果给一部电影的名字，就能快速得到出演该电影的演员。

但是如果给出一个演员的名字，我们想快速得到该演员演出的所有电影，怎么办呢？这就需要「反向索引」，对之前的哈希表进行一些操作，新建另一个哈希表，把演员作为键，把电影列表作为值。

显然，如果用哈希表存储，需要两个哈希表分别存储「每个演员到电影列表」的映射和「每部电影到演员列表」的映射。但如果用「图」结构存储，将电影和参演的演员连接，很自然地就成为了一幅二分图：

每个电影节点的相邻节点就是参演该电影的所有演员，每个演员的相邻节点就是该演员参演过的所有电影，非常方便直观。

### 1. 判定思路

遍历一遍图， 一边遍历一边着色，看看能不能用两种颜色给所有节点染色，且相邻节点的颜色都不相同。DFS， BFS都可以。

### 2. 图遍历框架

```java
void traverse(Graph graph, int v) {
  if (visited[v] == true) return;
  visited[v] = true;
  for (int neighbor : graph.neighbors[v]) {
    traverse(graph, neighbor);
  }
}
```

二分图需要修改一下

```java
void traverse(Graph graph, int v) {
  visited[v] = true;
  for (int neighbor : graph.neighbors[v]) {
    if (!visited[neighbor]) {
       // 相邻节点 neighbor 没有被访问过
            // 那么应该给节点 neighbor 涂上和节点 v 不同的颜色
            traverse(graph, visited, neighbor);
    } else {
      // 相邻节点 neighbor 已经被访问过
            // 那么应该比较节点 neighbor 和节点 v 的颜色
            // 若相同，则此图不是二分图
    }
  }
}
```

## 785. Is Graph Bipartite？

### 1. DFS

```java
class Solution {

    // 记录图是否符合二分图性质
    private boolean ok = true;
    // 记录图中节点的颜色，false 和 true 代表两种不同颜色
    private boolean[] color;
    // 记录图中节点是否被访问过
    private boolean[] visited;

    // 主函数，输入邻接表，判断是否是二分图
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        color = new boolean[n];
        visited = new boolean[n];
        // 因为图不一定是联通的，可能存在多个子图
        // 所以要把每个节点都作为起点进行一次遍历
        // 如果发现任何一个子图不是二分图，整幅图都不算二分图
        for (int v = 0; v < n; v++) {
            if (!visited[v]) {
                traverse(graph, v);
            }
        }
        return ok;
    }

    // DFS 遍历框架
    private void traverse(int[][] graph, int v) {
        // 如果已经确定不是二分图了，就不用浪费时间再递归遍历了
        if (!ok) return;

        visited[v] = true;
        for (int w : graph[v]) {
            if (!visited[w]) {
                // 相邻节点 w 没有被访问过
                // 那么应该给节点 w 涂上和节点 v 不同的颜色
                color[w] = !color[v];
                // 继续遍历 w
                traverse(graph, w);
            } else {
                // 相邻节点 w 已经被访问过
                // 根据 v 和 w 的颜色判断是否是二分图
                if (color[w] == color[v]) {
                    // 若相同，则此图不是二分图
                    ok = false;
                }
            }
        }
    }

}

```



### 2. BFS

```java
class Solution {
  private boolean[] visited;
  private boolean[] color;
  public boolean isBipartite(int[][] graph) {
    visited = new boolean[graph.length];
    color = new boolean[graph.length];
    
    for (int i = 0; i < graph.length; i++) {
      if (!BFS(graph, i)) {
        return false;
      }
    }
    return true;
  }
  
  public boolean BFS(int[][] graph, int c) {
    visited[c] = true;
    Queue<Integer> queue = new LinkedList<>();
    queue.add(c);
    while (!queue.isEmpty()) {
      int cur = queue.poll();
      for (int n : graph[cur]) {
        if (!visited[n]) {
          color[n] = !color[cur];
          visited[n] = true;
          queue.add(n);
        } else {
          if (color[n] == color[cur]){
            return false;
          }
        }
      }
    }
    return true;
  }
}
```

## 886. Possible Bipartition

### 1. DFS

```java
class Solution {
    private boolean[] visited;
    private boolean[] color;
    public boolean possibleBipartition(int n, int[][] dislikes) {
        visited = new boolean[n + 1];
        color = new boolean[n + 1];
        List<Integer>[] graph = new ArrayList[n + 1];
        for (int i = 0; i <= n; i++) {
            graph[i] = new ArrayList<>();
        }
        
        for (int j = 0; j < dislikes.length; j++) {
          //构建邻接表的时候要考虑这是undirected Graph,两边都要添加
            graph[dislikes[j][0]].add(dislikes[j][1]);
            graph[dislikes[j][1]].add(dislikes[j][0]);
        }
        
        for (int i = 0; i <= n; i++) {
            if (!DFS(graph, i)) {
                return false;
            }
        }
        return true;
    }
    
    public boolean DFS(List<Integer>[] graph, int cur) {
        visited[cur] = true;
        
        for (int n : graph[cur]) {
            if (!visited[n]) {
                color[n] = !color[cur];
                DFS(graph, n);
            } else {
                if (color[n] == color[cur]) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

### 2. BFS

```java
class Solution {
  private boolean[] visited;
  private boolean[] color;
  public boolean possibleBipartition(int n, int[][] dislikes) {
     visited = new boolean[n + 1];
        color = new boolean[n + 1];
        List<Integer>[] graph = new ArrayList[n + 1];
        for (int i = 0; i <= n; i++) {
            graph[i] = new ArrayList<>();
        }
        
        for (int j = 0; j < dislikes.length; j++) {
            graph[dislikes[j][0]].add(dislikes[j][1]);
            graph[dislikes[j][1]].add(dislikes[j][0]);
        }
        
        for (int i = 1; i <= n; i++) {
            if (!BFS(graph, i)) {
                return false;
            }
        }
        return true;
  }
  public boolean BFS(List<Integer>[] graph, int i) {
    Queue<Integer> queue = new LinkedList<>();
    visited[i] = true;
    queue.add(i);
    while (!queue.isEmpty()) {
      int cur = queue.poll();
      for (int n : graph[cur]) {
        if (!visited[n]) {
          color[n] = !color[cur];
          visited[n] = true;
          queue.add(n);
        } 
        if (color[n] == color[cur]) {
          return false;
        }
      }
    }
    return true;
  }
}
```





## 5⃣️. Union Find

### -- 简介

用来解决【动态连通性】

主要实现以下两个API

```java
class UF{
  /*将 p 和 q 连接起来*/
  public void union(int p, int q);
  /* 判断 p 和 q 是否连通 */
  public boolean connected(int p, int q);
    /* 返回图中有多少个连通分量 */
  public int count();
}
```

这里所说的「连通」是一种等价关系，也就是说具有如下三个性质：

#### 1. 自反性：节点 `p` 和 `p` 是连通的。

#### 2. 对称性：如果节点 `p` 和 `q` 连通，那么 `q` 和 `p` 也连通。

#### 3. 传递性：如果节点 `p` 和 `q` 连通，`q` 和 `r` 连通，那么 `p` 和 `r` 也连通。

比如 0～9互不连通， 调用connected都会判断为false， 连通分量为10

调用union(0, 1)， 0 和 1 会被连通， 连通分量为9

### -- 构造Union Find

#### (1) 设计构造函数

![image-20220826164646075](/Users/youhao/Library/Application Support/typora-user-images/image-20220826164646075.png)

```java
class UF{
  //记录连通分量
  private int count;
  //节点x的父节点是parent[x]
  private int[] parent;
  
  /*构造函数*， n为图的总结点数*/
  public UF(int n) {
    //连通分量一开始为n
    this.count = n;
    parent = new int[n];
    for (int i = 0; i < n; i++) {
      //一开始父节点都是自己
      parent[i] = i;
    }
  }
  
}

```

如果该节点与另一个节点连通， 那么连接他们的根节点

```java
class UF{
  //记录连通分量
  private int count;
  //节点x的父节点是parent[x]
  private int[] parent;
  
  /*构造函数*， n为图的总结点数*/
  public UF(int n) {
    //连通分量一开始为n
    this.count = n;
    parent = new int[n];
    for (int i = 0; i < n; i++) {
      //一开始父节点都是自己
      parent[i] = i;
    }
  }
  
  public void union(int p, int q) {
    int rootP = find(p);
    int rootQ = find(q);
    if (rootP == rootQ) {
      return;
    }
    //将两棵树合成一个
    parent[rootP] = rootQ;
    // parent[rootQ] = rootP 也一样
    count--; // 两个分量合二为一
  }
  
  public int find(int x) {
    //根节点的parent[x] = x
    while (parent[x] != x) {
      x = parent[x];
    }
    return parent[x];
  }
  /* 返回连通量*/
  public int count() {
    return count;
  }
  
}


```

#### (2) 平衡优化

![image-20220826170809718](/Users/youhao/Library/Application Support/typora-user-images/image-20220826170809718.png)

比如说 `size[3] = 5` 表示，以节点 `3` 为根的那棵树，总共有 `5` 个节点。这样我们可以修改一下 `union` 方法：

```java
class UF{
  //记录连通分量
  private int count;
  //节点x的父节点是parent[x]
  private int[] parent;
  // 设置重量
  private int[] size;
  
  /*构造函数*， n为图的总结点数*/
  public UF(int n) {
    //连通分量一开始为n
    this.count = n;
    parent = new int[n];
    size = new int[n];
    for (int i = 0; i < n; i++) {
      //一开始父节点都是自己
      parent[i] = i;
      size[i] = 1;
    }
  }
  
  public void union(int p, int q) {
    int rootP = find(p);
    int rootQ = find(q);
    if (rootP == rootQ) {
      return;
    }
      //小树枝接到大树下面， 平衡树
      if (size[rootP] > size[rootQ]) {
        parent[rootQ] = rootP;
        size[rootP] += size[rootQ];
      } else {
        parent[rootP] = rootQ;
        size[rootQ] += size[rootP];
   	  }
   	 	count--; // 两个分量合二为一
  }
  
  public int find(int x) {
    //根节点的parent[x] = x
    while (parent[x] != x) {
      x = parent[x];
    }
    return parent[x];
  }
  /* 返回连通量*/
  public int count() {
    return count;
  }
  
}
```

这样，通过比较树的重量，就可以保证树的生长相对平衡，树的高度大致在 `logN` 这个数量级，极大提升执行效率。

此时，`find` , `union` , `connected` 的时间复杂度都下降为 O(logN)，即便数据规模上亿，所需时间也非常少。

#### (3) 路经压缩

```java
class UF{
  //记录连通分量
  private int count;
  //节点x的父节点是parent[x]
  private int[] parent;
  // 设置重量
  private int[] size;
  
  /*构造函数*， n为图的总结点数*/
  public UF(int n) {
    //连通分量一开始为n
    this.count = n;
    parent = new int[n];
    size = new int[n];
    for (int i = 0; i < n; i++) {
      //一开始父节点都是自己
      parent[i] = i;
      size[i] = 1;
    }
  }
  
  public void union(int p, int q) {
    int rootP = find(p);
    int rootQ = find(q);
    if (rootP == rootQ) {
      return;
    }
    //小树枝接到大树下面， 平衡树
    if (size[rootP] > size[rootQ]) {
      parent[rootQ] = rootP;
      size[rootP] += size[rootQ];
    } else {
      parent[rootP] = rootQ;
      size[rootQ] += size[rootP];
    }
    count--; // 两个分量合二为一
  }
  
  public int find(int x) {
    //直接使当前节点的父节点指向根节点
    //这样看两个节点是否连通时，只需要看他们的父节点是不是一致的
    if (parent[x] != x) {
      parent[x] = find(parent[x]);
    }
    return parent[x];
  }
  /* 返回连通量*/
  public int count() {
    return count;
  }
  
}
```

## 323.  Number of Connected Components in an Undirected Graph

### 1. Union Find

```java
class Solution {
    class UF {
        private int[] parent;
        private int count;
        
        public UF(int n) {
            parent = new int[n];
            this.count = n;
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }
        
        public void union(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            if (rootP == rootQ) {return;}
            parent[rootP] = rootQ;
            count--;
        }
        
        public int find(int x) {
            if (x != parent[x]) {
                parent[x] = find(parent[x]);
            }
            return parent[x];
        }
        
        public int count() {
            return count;
        }
    }
    public int countComponents(int n, int[][] edges) {
        UF uf = new UF(n);
        for (int i = 0; i < edges.length; i++) {
            uf.union(edges[i][0], edges[i][1]);
        }
        return uf.count();
    }
}
```



## 990. Satisfiability of Equality Equations

### 1. Union Find

```java
class Solution {
    class UF {
        private int[] parent;
        private int count;
        
        public UF(int n) {
            parent = new int[n];
            
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
            
            count = n;
        }
        
        public void union(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            if (rootP == rootQ) {return;}
            
            parent[rootP] = rootQ;
            count--;
        }
        
        public int find(int x) {
            if (x != parent[x]) {
                parent[x] = find(parent[x]);
            }
            return parent[x];
        }
        public int count() {
            return count;
        }
    }
    public boolean equationsPossible(String[] equations) {
        UF uf = new UF(26);
        
        for (String cur : equations) {
            int f = cur.charAt(0) - 'a';
            int s = cur.charAt(3) - 'a';
            char r = cur.charAt(1);
            if (r == '=') {
                uf.union(f, s);
            }
        }
        
        for (String cur : equations) {
            int f = cur.charAt(0) - 'a';
            int s = cur.charAt(3) - 'a';
            char r = cur.charAt(1);
            if (r == '!') {
                if (uf.find(f) == uf.find(s)) {
                    return false;
                }
            }
        }
        return true;
    }
}
```



## 765. Couples Holding Hands

### 1. Union Find

```java
class Solution {
    
    class UF {
        private int[] parent;
        private int count;
        public UF(int n) {
            parent = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
            count = n;
        }
        
        public void union (int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            if (rootP == rootQ) {
                return;
            }
            
            parent[rootP] = rootQ;
            count--;
            
        }
        public int find(int x) {
            if (x != parent[x]) {
                parent[x] = find(parent[x]);
            }
            return parent[x];
        }
        
        public int count() {
            return count;
        }
    }
    public int minSwapsCouples(int[] row) {
        
        int n = row.length;
        UF uf = new UF(n);
        
        for (int i = 0; i < n; i+= 2) {
          
          //将couple_id 联合起来
            uf.union(row[i] / 2, row[i + 1] / 2);
        }
        return n - uf.count();
    }
    
}
```

## 6⃣️. Kruskal 最小生成树算法 (MST)

### -- 算法逻辑

**将所有边按照权重从小到大排序，从权重最小的边开始遍历，如果这条边和 `mst` 中的其它边不会形成环，则这条边是最小生成树的一部分，将它加入 `mst` 集合；否则，这条边不是最小生成树的一部分，不要把它加入 `mst` 集合**。



## 1135. Connecting Cities With Minimum Cost

### 1. Kruskal & Union Find

```java
class Solution {
    class UF {
        private int[] parent;
        private int[] size;
        private int count;
        
        public UF (int n) {
            parent = new int[n];
            size = new int[n];
            
            for (int i = 0; i < n; i++) {
                parent[i] = i;
                size[i] = 1;
            }
            count = n;
        }
        
        public void union(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            
            if (rootP == rootQ) {return;}
            if (size[rootP] > size[rootQ]) {
                parent[rootQ] = rootP;
            } else {
                parent[rootP] = rootQ;
            }
            count--;
        }
        
        public int find(int x) {
            if (parent[x] != x) {
                parent[x] = find(parent[x]);
            }
            
            return parent[x];
        }
        
        public boolean connected(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            
            if (rootP == rootQ) {return true;}
            else {
                return false;
            }
        }
        
        public int count() {
            return count;
        }
    }
    public int minimumCost(int n, int[][] connections) {
        Arrays.sort(connections, (a, b) -> (a[2] - b[2]));
        UF uf = new UF(n + 1);
        int res = 0;
        for (int i = 0; i < connections.length; i++) {
            int p = connections[i][0];
            int q = connections[i][1];
            int weight = connections[i][2];
            if (uf.connected(p, q)) {
                continue;
            } else {
                uf.union(p, q);
                res += weight;
            }
        }
        return (uf.count == 2) ? res : -1;
    }
}
```

### 2. Prim

```java
class Solution {
  class Prim{
        private PriorityQueue<int[]> pq;
        private boolean[] inMST;
        private int weightSum = 0;
        private List<int[]>[] graph;
        
        public Prim(List<int[]>[] graph) {
            this.graph = graph;
            this.pq = new PriorityQueue<>((a, b) -> (a[2] - b[2]));
            int n = graph.length;
            this.inMST = new boolean[n];
            
            //从0点开始切
            inMST[0] = true;
            cut(0);
            while (!pq.isEmpty()) {
                int[] edge = pq.poll();
                int to = edge[1];
                int weight = edge[2];
                if (inMST[to]) {
                    continue;
                }
                weightSum += weight;
                inMST[to] = true;
                cut(to);
            }
        }
        
        private void cut(int s) {
            for (int[] edge : graph[s]) {
                int to = edge[1];
                if (inMST[to]) {
                    continue;
                }
                pq.add(edge);
            }
        }
        
        private int weightSum() {
            return weightSum;
        }
        
        private boolean allConnected() {
            for (boolean cur : inMST) {
                if (!cur) {
                    return false;
                }
            }
            return true;
        }
    }
  
  public int minimumCost(int n, int[][] connections) {
        List<int[]>[] graph = buildGraph (n, connections);
        Prim pr = new Prim(graph);
        if (!pr.allConnected()) {
            return -1;
        }
        
        return pr.weightSum;
    }
    
    public List<int[]>[] buildGraph(int n, int[][] connections) {
        List<int[]>[] graph = new LinkedList[n];
        for (int i = 0; i < n; i++) {
        graph[i] = new LinkedList<>();
    }
        for (int[] connection : connections) {
            int f = connection[0] - 1;
            int t = connection[1] - 1;
            int weight = connection[2];
            graph[f].add(new int[]{f, t, weight});
            graph[t].add(new int[]{t, f, weight});
        }
        return graph;
    }
}
```







## 261. Graph Valid Tree

### 1. Union Find

```java
class Solution {
    class UF {
        private int[] parent;
        private int[] size;
        private int count;
        
        public UF(int n) {
            parent = new int[n];
            size = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
                size[i] = 1;
            }
            count = n;
        }
        
        public void union(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            
            if (rootP == rootQ) return;
            if (size[rootP] > size[rootQ]) {
                parent[rootQ] = rootP;
            } else {
                parent[rootP] = rootQ;
            }
            count--;
        }
        
        public boolean connected(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            
            if (rootP == rootQ) {
                return true;
            } else {
                return false;
            }
        }
        
        public int find(int x) {
            if (x != parent[x]) {
                parent[x] = find(parent[x]);
            } 
            return parent[x];
        }
        
        public int count() {
            return count;
        }
    }
    
    public boolean validTree(int n, int[][] edges) {
        UF uf = new UF(n);
        for (int i = 0; i < edges.length; i++) {
            int p = edges[i][0];
            int q = edges[i][1];
            if (uf.connected(p, q)) {
                return false;
            }
            uf.union(p, q);
        }
        return uf.count == 1 ? true : false;
    }
}
```


## 1584. Min Cost to Connect All Points

### 1. Union Find + Kruskal

```java
class Solution {
     class UF {
        private int[] parent;
        private int[] size;
        private int count;
        
        public UF(int n) {
            parent = new int[n];
            size = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
                size[i] = 1;
            }
            count = n;
        }
        
        public void union(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            
            if (rootP == rootQ) return;
            if (size[rootP] > size[rootQ]) {
                parent[rootQ] = rootP;
            } else {
                parent[rootP] = rootQ;
            }
            count--;
        }
        
        public boolean connected(int p, int q) {
            int rootP = find(p);
            int rootQ = find(q);
            
            if (rootP == rootQ) {
                return true;
            } else {
                return false;
            }
        }
        
        public int find(int x) {
            if (x != parent[x]) {
                parent[x] = find(parent[x]);
            } 
            return parent[x];
        }
        
        public int count() {
            return count;
        }
    }
    
    public int minCostConnectPoints(int[][] points) {
        
        List<int[]> edges = new ArrayList<>();
        //create Adjacency List
        for (int i = 0; i < points.length; i++) {
            for (int j = i + 1; j < points.length; j++) {
                int xi = points[i][0];
                int yi = points[i][1];
                int xj = points[j][0];
                int yj = points[j][1];
                edges.add(new int[]{i, j, Math.abs(xi - xj) + Math.abs(yi - yj)});
            }
        }
        
        Collections.sort(edges, (a, b) -> (a[2] - b[2]));
        
        UF uf = new UF(points.length);
        int res = 0;
        for (int[] edge : edges) {
            int p = edge[0];
            int q= edge[1];
            int weight = edge[2];
            if (uf.connected(p, q)) {
                continue;
            }
            uf.union(p, q);
            res += weight;
        }
        return res;
        
    }
}
```



### 2. Prim 

```java
class Solution {
  class Prim{
        private PriorityQueue<int[]> pq;
        private boolean[] inMST;
        private int weightSum = 0;
        private List<int[]>[] graph;
        
        public Prim(List<int[]>[] graph) {
            this.graph = graph;
            this.pq = new PriorityQueue<>((a, b) -> (a[2] - b[2]));
            int n = graph.length;
            this.inMST = new boolean[n];
            
            //从0点开始切
            inMST[0] = true;
            cut(0);
            while (!pq.isEmpty()) {
                int[] edge = pq.poll();
                int to = edge[1];
                int weight = edge[2];
                if (inMST[to]) {
                    continue;
                }
                weightSum += weight;
                inMST[to] = true;
                cut(to);
            }
        }
        
        private void cut(int s) {
            for (int[] edge : graph[s]) {
                int to = edge[1];
                if (inMST[to]) {
                    continue;
                }
                pq.add(edge);
            }
        }
        
        private int weightSum() {
            return weightSum;
        }
        
        private boolean allConnected() {
            for (boolean cur : inMST) {
                if (!cur) {
                    return false;
                }
            }
            return true;
        }
    }
  
  public int minCostConnectPoints(int[][] points) {
    List<int[]>[] graph = buildGraph(points);
    Prim pr = new Prim(graph);
    return pr.weightSum;
  }
  
  public List<int[]>[] buildGraph(int[][] points) {
    int n = points.length;
    List<int[]>[] graph = new LinkedList[n];
    for (int i = 0; i < n; i++) {
      graph[i] = new LinkedList<>();
    }
    for (int i = 0; i < n; i++) {
      for (int j = i + 1; j < n; j++) {
        int xi = points[i][0];
        int yi = points[i][1];
        int xj = points[j][0];
        int yj = points[j][1];
        graph[i].add(new int[]{i, j, Math.abs(xi - xj) + Math.abs(yi - yj)});
        graph[j].add(new int[]{j, i, Math.abs(xi - xj) + Math.abs(yi - yj)});
      }
    }
    return graph;
  }
}
```



## 7⃣️. PRIM 最小生成树算法（MST)

### -- 对比 Kruskal 算法

图论的最小生成树问题，就是让你从图中找若干边形成一个边的集合 `mst`，这些边有以下特性：

1、这些边组成的是一棵树（树和图的区别在于不能包含环）。

2、这些边形成的树要包含所有节点。

3、这些边的权重之和要尽可能小。

#### (1) Kruskal 用到了贪心算法，和Union-Find

#### (2) PRIM 使用了贪心算法， 切分定理。 用到了BFS 算法和visited布数组避免成环。

PRIM 不需要对边进行排序，利用优先级队列动态排序。

### -- 切分定理

当你对一个图进行横切时， 总有一个横切边是最小生成树的边，选权重最小的边。 

![image-20220831211953905](/Users/youhao/Library/Application Support/typora-user-images/image-20220831211953905.png)



### -- PRIM算法实现

```java
class Prim {
    // 核心数据结构，存储「横切边」的优先级队列
    private PriorityQueue<int[]> pq;
    // 类似 visited 数组的作用，记录哪些节点已经成为最小生成树的一部分
    private boolean[] inMST;
    // 记录最小生成树的权重和
    private int weightSum = 0;
    // graph 是用邻接表表示的一幅图，
    // graph[s] 记录节点 s 所有相邻的边，
    // 三元组 int[]{from, to, weight} 表示一条边
    private List<int[]>[] graph;

    public Prim(List<int[]>[] graph) {
        this.graph = graph;
        this.pq = new PriorityQueue<>((a, b) -> {
            // 按照边的权重从小到大排序
            return a[2] - b[2];
        });
        // 图中有 n 个节点
        int n = graph.length;
        this.inMST = new boolean[n];

        // 随便从一个点开始切分都可以，我们不妨从节点 0 开始
        inMST[0] = true;
        cut(0);
        // 不断进行切分，向最小生成树中添加边
        while (!pq.isEmpty()) {
            int[] edge = pq.poll();
            int to = edge[1];
            int weight = edge[2];
            if (inMST[to]) {
                // 节点 to 已经在最小生成树中，跳过
                // 否则这条边会产生环
                continue;
            }
            // 将边 edge 加入最小生成树
            weightSum += weight;
            inMST[to] = true;
            // 节点 to 加入后，进行新一轮切分，会产生更多横切边
            cut(to);
        }
    }

    // 将 s 的横切边加入优先队列
    private void cut(int s) {
        // 遍历 s 的邻边
        for (int[] edge : graph[s]) {
            int to = edge[1];
            if (inMST[to]) {
                // 相邻接点 to 已经在最小生成树中，跳过
                // 否则这条边会产生环
                continue;
            }
            // 加入横切边队列
            pq.offer(edge);
        }
    }

    // 最小生成树的权重和
    public int weightSum() {
        return weightSum;
    }

    // 判断最小生成树是否包含图中的所有节点
    public boolean allConnected() {
        for (int i = 0; i < inMST.length; i++) {
            if (!inMST[i]) {
                return false;
            }
        }
        return true;
    }
}
```



### -- 1135 & 1584



## 8⃣️. Dijkstra算法

### (1) 二叉树的层级遍历和BFS算法

```java
// 输入一棵二叉树的根节点，层序遍历这棵二叉树
void levelTraverse(TreeNode root) {
    if (root == null) return 0;
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    int depth = 1;
    // 从上到下遍历二叉树的每一层
    while (!q.isEmpty()) {
        int sz = q.size();
        // 从左到右遍历每一层的每个节点
        for (int i = 0; i < sz; i++) {
            TreeNode cur = q.poll();
            printf("节点 %s 在第 %s 层", cur, depth);

            // 将下一层节点放入队列
            if (cur.left != null) {
                q.offer(cur.left);
            }
            if (cur.right != null) {
                q.offer(cur.right);
            }
        }
        depth++;
    }
}
```

BFS的算法框架

```java
// 输入起点，进行 BFS 搜索
int BFS(Node start) {
    Queue<Node> q; // 核心数据结构
    Set<Node> visited; // 避免走回头路
    
    q.offer(start); // 将起点加入队列
    visited.add(start);

    int step = 0; // 记录搜索的步数
    while (q not empty) {
        int sz = q.size();
        /* 将当前队列中的所有节点向四周扩散一步 */
        for (int i = 0; i < sz; i++) {
            Node cur = q.poll();
            printf("从 %s 到 %s 的最短距离是 %s", start, cur, step);

            /* 将 cur 的相邻节点加入队列 */
            for (Node x : cur.adj()) {
                if (x not in visited) {
                    q.offer(x);
                    visited.add(x);
                }
            }
        }
        step++;
    }
}

```

### (2) Dijkstra 伪代码

```java
class State {
    // 图节点的 id
    int id;
    // 从 start 节点到当前节点的距离
    int distFromStart;

    State(int id, int distFromStart) {
        this.id = id;
        this.distFromStart = distFromStart;
    }
}
// 返回节点 from 到节点 to 之间的边的权重
int weight(int from, int to);

// 输入节点 s 返回 s 的相邻节点
List<Integer> adj(int s);

// 输入一幅图和一个起点 start，计算 start 到其他节点的最短距离
int[] dijkstra(int start, List<Integer>[] graph) {
    // 图中节点的个数
    int V = graph.length;
    // 记录最短路径的权重，你可以理解为 dp table
    // 定义：distTo[i] 的值就是节点 start 到达节点 i 的最短路径权重
    int[] distTo = new int[V];
    // 求最小值，所以 dp table 初始化为正无穷
    Arrays.fill(distTo, Integer.MAX_VALUE);
    // base case，start 到 start 的最短距离就是 0
    distTo[start] = 0;

    // 优先级队列，distFromStart 较小的排在前面
    Queue<State> pq = new PriorityQueue<>((a, b) -> {
        return a.distFromStart - b.distFromStart;
    });

    // 从起点 start 开始进行 BFS
    pq.offer(new State(start, 0));

    while (!pq.isEmpty()) {
        State curState = pq.poll();
        int curNodeID = curState.id;
        int curDistFromStart = curState.distFromStart;

        if (curDistFromStart > distTo[curNodeID]) {
            // 已经有一条更短的路径到达 curNode 节点了
            continue;
        }
        // 将 curNode 的相邻节点装入队列
        for (int nextNodeID : adj(curNodeID)) {
            // 看看从 curNode 达到 nextNode 的距离是否会更短
            int distToNextNode = distTo[curNodeID] + weight(curNodeID, nextNodeID);
            if (distTo[nextNodeID] > distToNextNode) {
                // 更新 dp table
                distTo[nextNodeID] = distToNextNode;
                // 将这个节点以及距离放入队列
                pq.offer(new State(nextNodeID, distToNextNode));
            }
        }
    }
    return distTo;
}

```

**什么时候会存在`curDistFromStart < distTo[curNodeID]`？为什么要用`distTo[curNodeID]`来计算`distToNextNode`？**

事实上`curDistFromStart`并不会小于`distTo[curNodeID]`。前面提到过，`distTo`内的值都是在节点入队之前更新的，也就是说是先更新的`distTo`再将节点入队。而`curDistFromStart`是在节点出队时得到的，也就是说`curDistFromStart`只会大于`distTo[curNodeID]`（`curDistFromStart`入队之后`distTo[curNodeID]`被更小的值更新了）或者等于`distTo[curNodeID]`（`curDistFromStart`入队之后`distTo[curNodeID]`没被更新）。

使用`distTo[curNodeID]`来计算`distToNextNode`的原因我认为是在定义上`distTo`中存储的是节点到起点的最短距离。虽然由于枝剪代码的存在，当计算`distToNextNode`时，`curDistFromStart`只可能等于`distTo[curNodeID]`（如之前所述，本来就不存在小于的情况，枝剪代码又去掉了大于的情况），但是根据算法最初的定义，`起点到邻居节点的最短距离=起点到当前节点的最短距离+当前节点到邻居节点的距离`，所以`distTo[curNodeID]`才是计算时应该使用的变量。而`distTo[curNodeID]`不可能大于`curDistFromStart`由于是一个隐藏条件，结合枝剪代码，可能造成了一些误解。

## 743. Network Delay Time

### 1. Dijkstra

```java
class Solution {
  public int networkDelayTime(int[][] times, int n, int k) {
    List<int[]>[] graph = new LinkedList[n + 1];
    for (int i = 1; i <= n; i++) {
      graph[i] = new LinkedList<>();
    }
    
    for (int[] time : times) {
      int from = time[0];
      int to = time[1];
      int dis = time[2];
      graph[from].add(new int[]{to, dis});
    }
    
    int[] dis = Dijkstra(graph, k);
    int res = 0;
    for (int i = 1; i < dis.length; i++) {
      if (dis[i] == Integer.MAX_VALUE) {
        return -1;
      }
      res = Math.max(res, dis[i]);
    }
    return res;
  }
  
  class State {
    int id;
    int distFromStart;
    public State(int id, int distFromStart) {
      this.id = id;
      this.distFromStart = distFromStart;
    }
  }
  
  public int[] Dijkstra(List<int[]>[] graph, int start) {
    int[] distTo = new int[graph.length];
    Arrays.fill(distTo, Integer.MAX_VALUE);
    Queue<State> pq = new PriorityQueue<>((a, b) -> (a.distFromStart - b.distFromStart));
    pq.add(new State(start, 0));
    distTo[start] = 0;
    while (!pq.isEmpty()) {
      State cur = pq.poll();
      int curID = cur.id;
      int curDis = cur.distFromStart;
      if (curDis <= distTo[curID]) {
        for (int[] neighbor : graph[curID]) {
          int nextID = neighbor[0];
          int nextDis = neighbor[1] + distTo[curID];
          if (nextDis < distTo[nextID]) {
            distTo[nextID] = nextDis;
            pq.add(new State(nextID, distTo[nextID]));
          }
        }
      }
    }
    return distTo;
  }
}
```



## 1631. Path With Minimum Effort

### 1. Dijkstra

```java
class Solution {
    // Dijkstra 算法，计算 (0, 0) 到 (m - 1, n - 1) 的最小体力消耗
    public int minimumEffortPath(int[][] heights) {
        int m = heights.length, n = heights[0].length;
        // 定义：从 (0, 0) 到 (i, j) 的最小体力消耗是 effortTo[i][j]
        int[][] effortTo = new int[m][n];
        // dp table 初始化为正无穷
        for (int i = 0; i < m; i++) {
            Arrays.fill(effortTo[i], Integer.MAX_VALUE);
        }
        // base case，起点到起点的最小消耗就是 0
        effortTo[0][0] = 0;

        // 优先级队列，effortFromStart 较小的排在前面
        Queue<State> pq = new PriorityQueue<>((a, b) -> {
            return a.effortFromStart - b.effortFromStart;
        });

        // 从起点 (0, 0) 开始进行 BFS
        pq.offer(new State(0, 0, 0));

        while (!pq.isEmpty()) {
            State curState = pq.poll();
            int curX = curState.x;
            int curY = curState.y;
            int curEffortFromStart = curState.effortFromStart;

            // 到达终点提前结束
            if (curX == m - 1 && curY == n - 1) {
                return curEffortFromStart;
            }

            if (curEffortFromStart > effortTo[curX][curY]) {
                continue;
            }
            // 将 (curX, curY) 的相邻坐标装入队列
            for (int[] neighbor : adj(heights, curX, curY)) {
                int nextX = neighbor[0];
                int nextY = neighbor[1];
                // 计算从 (curX, curY) 达到 (nextX, nextY) 的消耗
                int effortToNextNode = Math.max(
                        effortTo[curX][curY],
                        Math.abs(heights[curX][curY] - heights[nextX][nextY])
                );
                // 更新 dp table
                if (effortTo[nextX][nextY] > effortToNextNode) {
                    effortTo[nextX][nextY] = effortToNextNode;
                    pq.offer(new State(nextX, nextY, effortToNextNode));
                }
            }
        }
        // 正常情况不会达到这个 return
        return -1;
    }

    // 方向数组，上下左右的坐标偏移量
    int[][] dirs = new int[][]{{0, 1}, {1, 0}, {0, -1}, {-1, 0}};

    // 返回坐标 (x, y) 的上下左右相邻坐标
    List<int[]> adj(int[][] matrix, int x, int y) {
        int m = matrix.length, n = matrix[0].length;
        // 存储相邻节点
        List<int[]> neighbors = new ArrayList<>();
        for (int[] dir : dirs) {
            int nx = x + dir[0];
            int ny = y + dir[1];
            if (nx >= m || nx < 0 || ny >= n || ny < 0) {
                // 索引越界
                continue;
            }
            neighbors.add(new int[]{nx, ny});
        }
        return neighbors;
    }

    class State {
        // 矩阵中的一个位置
        int x, y;
        // 从起点 (0, 0) 到当前位置的最小体力消耗（距离）
        int effortFromStart;

        State(int x, int y, int effortFromStart) {
            this.x = x;
            this.y = y;
            this.effortFromStart = effortFromStart;
        }
    }
}


```



## 9⃣️. 名流问题

## 277. Find the Celebrity

```java
/* The knows API is defined in the parent class Relation.
      boolean knows(int a, int b); */

public class Solution extends Relation {
  public int findCelebrity(int n) {
    if (n == 1) return 0;
    // 将所有候选人装进队列
    LinkedList<Integer> q = new LinkedList<>();
    for (int i = 0; i < n; i++) {
        q.addLast(i);
    }
    // 一直排除，直到只剩下一个候选人停止循环
    while (q.size() >= 2) {
        // 每次取出两个候选人，排除一个
        int cand = q.removeFirst();
        int other = q.removeFirst();
        if (knows(cand, other) || !knows(other, cand)) {
            // cand 不可能是名人，排除，让 other 归队
            q.addFirst(other);
        } else {
            // other 不可能是名人，排除，让 cand 归队
            q.addFirst(cand);
        }
    }

    // 现在排除得只剩一个候选人，判断他是否真的是名人
    int cand = q.removeFirst();
    for (int other = 0; other < n; other++) {
        if (other == cand) {
            continue;
        }
        // 保证其他人都认识 cand，且 cand 不认识任何其他人
        if (!knows(other, cand) || knows(cand, other)) {
            return -1;
        }
    }
    // cand 是名人
    return cand;
  }
}
```

![image-20220901230302575](/Users/youhao/Library/Application Support/typora-user-images/image-20220901230302575.png)

#  -------------------------------------------------------

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



## 1490. Clone N-ary Tree

### 1. DFS

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    
    public Node() {
        children = new ArrayList<Node>();
    }
    
    public Node(int _val) {
        val = _val;
        children = new ArrayList<Node>();
    }
    
    public Node(int _val,ArrayList<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/
class Solution {
    // 定义：输入 N 叉树节点，返回以该节点为根的 N 叉树的深拷贝
    public Node cloneTree(Node root) {
        if (root == null) {
            return null;
        }
        // 先拷贝根节点
        Node cpRoot = new Node(root.val);
        // 根节点的孩子节点都是深拷贝
        cpRoot.children = new ArrayList<>();
        for (Node child : root.children) {
            cpRoot.children.add(cloneTree(child));
        }
        // 返回整棵树的深拷贝
        return cpRoot;
    }
}
```



## 133. Clone Graph (Same as 1490)

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



//使用邻接表来处理(Adjacency list)

### 2. Topological Sorting + DFS

```java
class Solution {
    // 记录一次 traverse 递归经过的节点
    boolean[] onPath;
    // 记录遍历过的节点，防止走回头路
    boolean[] visited;
    // 记录图中是否有环
    boolean hasCycle = false;

    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = buildGraph(numCourses, prerequisites);

        visited = new boolean[numCourses];
        onPath = new boolean[numCourses];

        for (int i = 0; i < numCourses; i++) {
            // 遍历图中的所有节点
            traverse(graph, i);
        }
        // 只要没有循环依赖可以完成所有课程
        return !hasCycle;
    }

    void traverse(List<Integer>[] graph, int s) {
        if (onPath[s]) {
            // 出现环
            hasCycle = true;
        }

        if (visited[s] || hasCycle) {
            // 如果已经找到了环，也不用再遍历了
            return;
        }
        // 前序遍历代码位置
        visited[s] = true;
        onPath[s] = true;
        for (int t : graph[s]) {
            traverse(graph, t);
        }
        // 后序遍历代码位置
        onPath[s] = false;
    }

    List<Integer>[] buildGraph(int numCourses, int[][] prerequisites) {
        // 图中共有 numCourses 个节点
        List<Integer>[] graph = new LinkedList[numCourses];
        for (int i = 0; i < numCourses; i++) {
            graph[i] = new LinkedList<>();
        }
        for (int[] edge : prerequisites) {
            int from = edge[1];
            int to = edge[0];
            // 修完课程 from 才能修课程 to
            // 在图中添加一条从 from 指向 to 的有向边
            graph[from].add(to);
        }
        return graph;
    }
}

```

在递归过程中 Onpath在递归结束后会删除当前的递归路线， visited不删除 是为了减少相同的节点重复的遍历， Onpath是记录一条路线中是否存在同样的数字

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
    private boolean[] onPath;
    private boolean[] visited;
    public int[] findOrder(int numCourses, int[][] prerequisites) {
      index = 0;
      onPath = new boolean[numCourses];
      visited = new boolean[numCourses];
      List<List<Integer>> graph = new ArrayList<>();
      int[] ans = new int[numCourses];
      for (int i = 0; i < numCourses; i++) {
        graph.add(new ArrayList<>());
      }
      
      for (int[] cur : prerequisites) {
        graph.get(cur[0]).add(cur[1]);
      }
      for (int i = 0; i < numCourses; i++) {
        if (DFS(i, graph, ans)) return new int[0];
      }
      return ans;
    }
    public Boolean DFS(int cur, List<List<Integer>> graph, int[] ans) {
      if (onPath[cur]) {return true;}
      if (visited[cur]) {return false;}
      
      visited[cur] = true;
      onPath[cur] = true;
      for (int next : graph.get(cur)) {
        if (DFS(next, graph, ans)) return true;
      }
      ans[index++] = cur;
      onPath[cur] = false;
      return false;
    }
}
```

