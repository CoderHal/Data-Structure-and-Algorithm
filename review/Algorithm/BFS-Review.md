## BFS的算法框架

一般用来

（一）遍历树结构（level order）

（二）遍历图结构 （BFS，Topological）

（三）遍历二维数组

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

## 111. Minimum Depth of Binary Tree

求最小深度

BFS Tips：

（1） 要考虑到在每层遍历时，如果当前节点没有左右子节点，它就是最小的深度，直接返回。因为我们是从Top到Bottom扫描的

（2） 如果只有单个节点，还是需要继续讨论

（3） 有两个子节点就需要全部加入

DFS Tips：

（1） 没有子节点时，返回1

（2）当前节点为空时，返回0

（3）和BFS一样，但是当两个子节点都需要讨论，但是在这里需要比较左右节点的深度，求出最小

时间复杂度分析：

需要遍历全部节点，O(n)

空间复杂度：

DFS：O(logN) ,worst case O(n)只有一边的不平衡树， best case只需要遍历O(logN) 层

BFS:  O(N)





## 102. Binary Tree Level Order Traversal 

BFS Tips:

正常的二叉树BFS遍历，在每一层设置一个ArrayList，将每层节点加入即可

Time ： O(N)

Space:  O(N)

DFS Tips:

如果想让DFS实现每层添加节点，需要记录每个节点的对应层数。如果res的size 小于等于level，说明res没有为当前level开辟空间，所以需要创造新的空间。

时间复杂度 O(N) 需要遍历所有节点

空间复杂度O(N) 要递归N次才能将所有节点添加进去



Follow up

## (102 Follow Up) 314. Binary Tree Vertical Order Traversal

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public List<List<Integer>> verticalOrder(TreeNode root) {
        List<List<Integer>> res = new LinkedList<>();
        if (root == null) return res;
        Map<Integer, ArrayList> map = new HashMap<>();
       
        Queue<TreeNode> queue = new LinkedList<>();
        Queue<Integer> queue2 = new LinkedList<>();
        queue.add(root);
        queue2.add(0);
        int minCol = Integer.MAX_VALUE;
        int maxCol = Integer.MIN_VALUE;
        while (!queue.isEmpty()) {
            int n = queue.size();
            for (int i = 0; i < n; i++) {
                TreeNode cur = queue.poll();
                int newC = queue2.poll();
                minCol = Math.min(minCol, newC);
                maxCol = Math.max(maxCol, newC);
                if (!map.containsKey(newC)) {
                    map.put(newC, new ArrayList<>());
                }
                map.get(newC).add(cur.val);
                if (cur.left != null) {
                    queue.add(cur.left);
                    queue2.add(newC - 1);
                }
                if (cur.right != null) {
                    queue.add(cur.right);
                    queue2.add(newC + 1);
                }
            }
        }
        for (int i = minCol; i <= maxCol; i++) {
            res.add(new ArrayList<>(map.get(i)));
        }
        return res;
    }
}
```

tips : 

（1）用两个Queue分别存储TreeNode 和对应的列数

（2）用Map记录对应列数的ArrayList，key为列数， value为对应的那一列的节点

（3）用minCol和maxCol记录最小列值，最大列值，方便后面res 添加每列节点时，从左到右依次加入



## 127. Word Ladder

### 1. BFS + HashMap

Tips ：

（1） 用标准的BFS进行遍历。 把整个过程当成树来看待。 我们需要将start看成root

（2） root的子节点就是变换一个字母将start的字符串进行替换，从'a' -> 'b'

（3）BFS遍历，去寻找子节点为end的情况，返回遍历的高度，这就是最小的路径

（4） 如果end不在wordList中，直接返回 0



Time 分析：

N为total number of words, M 为length of each word

可能需要遍历所有的words，所以利用Queue遍历，需要N

在遍历过程中，我们要对word的每一位进行变换， 需要M

变换的过程中，对特定位进行替换，需要M

所以时间复杂度 O(M * M * N)

### 2. Optimize

![image-20220915161115982](/Users/youhao/Library/Application Support/typora-user-images/image-20220915161115982.png)

Tips: 双向BFS

（1）要设置beginSet 和 endSet，还有visitedSet

（2）不同点是，我们在遍历过程中，需要不停转换起点位置，和终点位置。在过程中需要设置nextLine作为生成的新一行。添加nextLine的过程中，如果visitedSet中存在了，说明已经遍历过这个单词了，就不需要再添加了，减少计算量。

（3）相遇即为结束。 比如从beginSet中的值，在字符变换的过程中，发现和endSet中的单词一样，这说明beginSet和endSet已经连接成功。

（4）反复切换起点和终点的过程中，我们根据 新生成的nextLine和endSet去比较，哪边Size大，哪边作为EndSet，小的作为beginSet。这种优化能让beginSet的字母变换数量减少， endSet的搜索范围加大，找到相遇点的几率更大。



![image-20220915163240297](/Users/youhao/Library/Application Support/typora-user-images/image-20220915163240297.png)



## 490. The Maze

### 1. BFS

BFS讨论，在每次出队列时，将该节点一直往一个方向前进，如果碰到墙壁或者跑出迷宫就停止。将新的地点存入队列中，如果新地点是终点就返回。（加入一个visited数组，来标记已走过的点）



# Dijstra

## 505. The Maze II

### 1.Dijkstra (similar with BFS)

典型的SSSP单源最短路径算法，用于计算一个节点到其他所有节点的最短路径。主要特点是以起始点为中心向外层层扩展，直到扩展到终点为止。

BFS也是层层扩展，但是和Dijkstra不同的是，BFS不会计算下一层的遍历顺序。Dijkstra他将这层和起点的距离计算出来，利用堆排序存储最小的值，计算出下一层的preference，找路程较小的

```java
class Solution {
    private int[][] dir = new int[][]{{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        int[][] distance = new int[maze.length][maze[0].length];
        for (int i = 0; i < maze.length; i++) {
            Arrays.fill(distance[i], Integer.MAX_VALUE);
        }
        distance[start[0]][start[1]] = 0;
        Dijkstra(maze, start, distance);
        return distance[destination[0]][destination[1]] == Integer.MAX_VALUE ? -1 : distance[destination[0]][destination[1]];
    }
    
    public void Dijkstra(int[][] maze, int[] start,int[][] distance) {
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> (a[2] - b[2]));
        queue.add(new int[]{start[0], start[1], 0});
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            for (int[] d : dir) {
                int row = cur[0] + d[0];
                int col = cur[1] + d[1];
                int count = 0;
                while (row >= 0 && row < maze.length && col >= 0 && col < maze[0].length && maze[row][col] == 0) {
                    row += d[0];
                    col += d[1];
                    count++;
                }
                int newRow = row - d[0];
                int newCol = col - d[1];
                int newDistance = count + cur[2];
                
                if (newDistance < distance[newRow][newCol]) {
                    distance[newRow][newCol] = newDistance;
                    queue.add(new int[]{newRow, newCol, newDistance});
                }
            }
        }
    }
}
```



## Topological Sort

拓扑排序是一个有向无环图（DAG -- Directed Acyclic Graph）的所有顶点的排序。必须满足以下条件：

1. 每个顶点只出现一次
2. 若顶点A到顶点B的路径，那么在序列中顶点A出现在顶点B的前面。

**注：**有向无环图（DAG）才有拓扑排序， 非DAG没有拓扑排序



## 207. Course Schedule

Tips:

1. 首先将每个课程节点的入度计算出来
2. 将入度为0的设置为起始节点，放入Queue里面进行第一次遍历
3. 遍历Queue中的节点，其下一个节点的入度 - 1，如果下一个节点的入度为0，说明该节点变成了起始节点，放入Queue中
4. 在Queue为空之后，如果每个课程的入度为0，说明课程能够全部修完



