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