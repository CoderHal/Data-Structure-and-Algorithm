# Tree Traversal

# 算法思想

## 1. 前序遍历和后序遍历

快速排序是二叉树的前序遍历

归并排序是二叉树的后序遍历

### 1. 快速排序模版

```java
void sort(int[] nums, int lo, int hi) {
    /****** 前序遍历位置 ******/
    // 通过交换元素构建分界点 p
    int p = partition(nums, lo, hi);
    /************************/

    sort(nums, lo, p - 1);
    sort(nums, p + 1, hi);
}
```

### 2. 归并排序框架

```java
// 定义：排序 nums[lo..hi]
void sort(int[] nums, int lo, int hi) {
    int mid = (lo + hi) / 2;
    // 排序 nums[lo..mid]
    sort(nums, lo, mid);
    // 排序 nums[mid+1..hi]
    sort(nums, mid + 1, hi);

    /****** 后序位置 ******/
    // 合并 nums[lo..mid] 和 nums[mid+1..hi]
    merge(nums, lo, mid, hi);
    /*********************/
}
```

## 2. 二叉树遍历框架

```java
void traverse(TreeNode root) {
    if (root == null) {
        return;
    }
    // 前序位置
    traverse(root.left);
    // 中序位置
    traverse(root.right);
    // 后序位置
}
```

前序位置的代码在刚刚进入一个二叉树节点的时候执行；

后序位置的代码在将要离开一个二叉树节点的时候执行；

中序位置的代码在一个二叉树节点左子树都遍历完，即将开始遍历右子树的时候执行。



## 3. 后序位置的特殊之处

1. 中序遍历一般都出现在BST中
2. 前序遍历都是自顶向下的
3. 后序遍历中都是自底向上
4. 后序遍历一般都是和子树有关

比如如何打印出每个节点的左右子树各有多少节点？

```java
// 定义：输入一棵二叉树，返回这棵二叉树的节点总数
int count(TreeNode root) {
    if (root == null) {
        return 0;
    }
    int leftCount = count(root.left);
    int rightCount = count(root.right);
    // 后序位置
    printf("节点 %s 的左子树有 %d 个节点，右子树有 %d 个节点",
            root, leftCount, rightCount);

    return leftCount + rightCount + 1;
}

```

## 4. 层序遍历

### 1.通过迭代

```java
// 输入一棵二叉树的根节点，层序遍历这棵二叉树
void levelTraverse(TreeNode root) {
    if (root == null) return;
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    // 从上到下遍历二叉树的每一层
    while (!q.isEmpty()) {
        int sz = q.size();
        // 从左到右遍历每一层的每个节点
        for (int i = 0; i < sz; i++) {
            TreeNode cur = q.poll();
            // 将下一层节点放入队列
            if (cur.left != null) {
                q.offer(cur.left);
            }
            if (cur.right != null) {
                q.offer(cur.right);
            }
        }
    }
}

```





## 145. Binary Tree Postorder Traversal

### 1.1 Iteration Solution

![image-20220107211225181](/Users/youhao/Library/Application Support/typora-user-images/image-20220107211225181.png)

``` java
class Solution{
  public List<Integer> postorderTraversal(TreeNode root){
    List<Integer> res = new ArrayList();
    Stack<TreeNode> stack= new Stack<TreeNode>();
    if(root==null){
      return res;
    }
    stack.push(root);
    while(!stack.isEmpty()){
      TreeNode cur=stack.pop();
      res.add(0,cur.val);
      if(cur.left!=null){
        stack.push(cur.left);
      }
      if(cur.right!=null){
        stack.push(cur.right);
      }
    }
    return res;
    //Time Complexity O(n)
    //Space Complexity O(n)
  }
}
```

### 1.2 Iterative Solution

```java
class Solution{
    
  public List<Integer> postorderTraversal(TreeNode root){
      List<Integer> res= new ArrayList<Integer>();
      Stack<TreeNode> stack= new Stack<TreeNode>();
      TreeNode cur=root;
      while(!stack.isEmpty()|| cur!=null){
          while(cur!=null){
              res.add(0,cur.val);
              stack.push(cur);
              cur=cur.right;
          }
          cur=stack.pop();
          cur=cur.left;
      }
      return res;
  }
}
```

#### Idea

Pre: root-> left-> right 

Post: left-> right->root

reversal Post: root->right->left

So we just enumerate the right subTree Node at first, and the rest to solution is similar with Pre.

### 2. Recursion

#### Idea

​	Rules: left subtree-> right subtree-> root

``` java
class Solution{
  List<Integer> res = new ArrayList<Integer>();
  public List<Integer> postorderTraversal(TreeNode root){
    if(root==null){
      return res;
    }
    postorderTraversal(root.left);
    postorderTraversal(root.right);
    res.add(root.val);
    return res;
    //Time Complexity O(n)
    //Space Complexity worst case:O(n)  average case: O(logn)
  }
}
```

## 144. Binary Tree Preorder Traversal

### Solution

#### Idea

Rules: root-> left subtree -> right subtree

#### 1. Recursion

``` java
class Solution{
  List<Integer> res= new ArrayList<Integer>();
  pubic List<Integer> preorderTraversal(TreeNode root){
    if(root==null){
      return res;
    }
    res.add(root.val);
    preorderTraversal(root.left);
    preorderTraversal(root.right);
    return res;
    //Time Complexity O(n)
    //Time Complexity O(n)
  }
}
```

Initial situation:  The enumeration(枚举) starts at the root node, the function execution passes the root node as the argument.

End situation: if the left( or right) subnode equals null, recursion excution of this corresponding node is stopped.

Normal logic: The current point( store or output) is processed first, recursive call enumerates the left subtree(if not null) and the recursive call enumerates the right subtree(if not null).

#### 2. Iteration

![image-20220117153625512](/Users/youhao/Library/Application Support/typora-user-images/image-20220117153625512.png)

``` java
class Solution{
  public List<Integer> preorderTraversal(TreeNode root){
    List<Integer> res= new ArrayList<Integer>();
    TreeNode cur=root;
    Stack<TreeNode> stack= new Stack<TreeNode>();
    while(!stack.isEmpty()||cur!=null){
      while(cur!=null){
        res.add(cur.val);
        stack.push(cur);
        cur=cur.left;
      }
      cur=stack.pop();
      cur=cur.right;
    }
    return res;
    //Time Complexity O(n)
    //Space Complexity O(n)
  }
}
```



##  94. Binary Tree Inorder Traversal

### 1. Recursion

### Idea

Rules: left subtree-> root-> right subtree

``` java
class Solution{
  List<Integer> res= new ArrayList<Integer>();
	public List<Integer> inorderTraversal(TreeNode root){
    if(root==null){
      return res;
    }
    InorderTraversal(root.left);
    res.add(root.val);
    InorderTraversal(root.right);
    return res;
    //Time Complexity O(n)
    //Space Complexity O(n)
	}
}
```



### 2. Iterative 

``` java
class Solution{
  public List<Integer> inorderTraversal(TreeNode root){
    Stack<TreeNode> stack=new Stack<TreeNode>();
    List<Integer> res=new ArrayList<Integer>();
    TreeNode cur=root;
    while(!stack.isEmpty()||cur!=null){
      while(cur!=null){
        stack.push(cur);
        cur=cur.left;
      }
      cur=stack.pop();
      res.add(cur.val);
      cur=cur.right;
    }
    return res;
    //Time Complexity O(n)
    //Space Complexity O(n)
  }
}
```

### 3. Morris Traversal

```java
class Solution{
  public List<Integer> inorderTraversal(TreeNode root){
      List<Integer> res= new ArrayList<>();
      TreeNode cur= root;
      TreeNode pre;
      while(cur!=null){
          if(cur.left==null){
              res.add(cur.val);
              cur=cur.right;//move to next right node
          }else{//has a left subtree
              pre=cur.left;
              while(pre.right!=null){
                  pre=pre.right;
              }
              pre.right=cur;
              TreeNode temp=cur;//store cur node
              cur=cur.left;//move cur to the top of the new tree
              temp.left=null;// original cur left be null, avoid infinite loops,cut the old tree
          }
      }
      return res;
    //Time complexity O(n)
    //Space complexity O(1)
  }
}
```

Idea

Step 1: Initial current as root

Step 2: while current != null

​		if current.left!=null

​			a.  add -> ArrayList

​			b. Current->current.right

​		else

​			a. In current's subleft tree, make current  the right child of the rightmost node

​			b.  Go to this left child

## 116. Populating Next Right Pointers in Each Node

### 1. Recursion

```java
class Solution {
  public Node connect(Node root) {
    if (root == null) {return root;}
    //连接根结点的左右子节点
    traverse(root.left, root.right);
    return root;
  }
  public void traverse(Node n1, Node n2) {
    if (n1 == null || n2 == null) {
      return;
    }
    //连接当前两个节点
    n1.next = n2;
    //先连接左子树的
    traverse(n1.left, n1.right);
    traverse(n2.left, n2.right);
    traverse(n1.right, n2.left);
  }
}
```



## 114. Flatten Binary Tree to Linked List

### 1. Post Traversal

```java
class Solution {
    // 定义：将以 root 为根的树拉平为链表
    public void flatten(TreeNode root) {
        // base case
        if (root == null) return;
        // 先递归拉平左右子树
        flatten(root.left);
        flatten(root.right);

        /****后序遍历位置****/
        // 1、左右子树已经被拉平成一条链表
        TreeNode left = root.left;
        TreeNode right = root.right;

        // 2、将左子树作为右子树
        root.left = null;
        root.right = left;

        // 3、将原先的右子树接到当前右子树的末端
        TreeNode p = root;
        while (p.right != null) {
            p = p.right;
        }
        p.right = right;
    }
}


```

![image-20220812230719103](/Users/youhao/Library/Application Support/typora-user-images/image-20220812230719103.png)



## Summary Binary Tree Traveral

![image-20220117170214966](/Users/youhao/Library/Application Support/typora-user-images/image-20220117170214966.png)

## 589. N-ary Tree Preorder Traveral

### 1. Iterative Solution

``` java
class Solution{
  public List<Integer> preorder(Node root){
    List<Integer> res= new ArrayList<Integer>();
    Stack<Node> stack= new Stack<Node>();
    if(root==null){
      return res;
    }
    stack.push(root);
    while(!stack.isEmpty()){
      Node cur= stack.pop();
      res.add(cur.val);
      for(int i=cur.children.size()-1;i>=0;i--){
        stack.push(cur.children.get(i));//put children node into stack, and make the leftmost child node to the top
      }
    }
    return res;
    //Time Complexity O(n)
    //Space Complexity O(n)
  }
}
```

Idea

1. print the root.val 
2. push root's children node into stack and make the left children node become the top of stack.
3. Pop stack -> print node.val
4. loop



### 2. Recursive Solution

Root->left child node->.....right child node

``` java
class Solution{
  List<Integer> res=new ArrayList<Integer>();
  public List<Integer> preorder(Node root){
    if(root==null){
      return res;
    }
    res.add(root.val);
    for(int i=0;i<root.children.size()-1;i++){
      preorder(root.children.get(i));
    }
    return res;
  }
}
```

## 590. N-ary Tree Postorder Traveral

### 1. Recursive Solution

left->....right->root : Preorder(reverse)

```java
class Solution{
  List<Integer> res=new ArrayList<Integer>();
  public List<Integer> postorder(Node root){
    if(root==null){
      return res;
    }
    for(int i=0;i<root.children.size();i++){
      postorder(root.children.get(i));
    }
    res.add(root.val);
    return res;
  }
}
```

### 2. Iterative Solution

``` java
class Solution{
  public List<Integer> postorder(Node root){
    List<Integer> res= new ArrayList<Integer>();
    if(root==null){
      return res;
    }
    Stack<Node> stack=new Stack<Node>();
    stack.push(root);
    Node cur=root;
    while(!stack.isEmpty()){
      cur=stack.pop();
      res.add(0,cur.val);
      for(int i=0;i<cur.children.size();i++){
        stack.push(cur.children.get(i));
      }
    }
    return res;
    //Time O(n)
    //Space O(n)
  }
}
```

## 102. Binary Tree Level Order Traveral

### 1. Iterative Solution

``` java
	class Solution{
    public List<List<Integer>> levelOrder(TreeNode root){
      List<List<Integer>> res= new ArrayList<>();
      Queue<TreeNode> queue= new LinkedList<>();
      if(root==null){
        return res;
      }
      TreeNode cur=root;
      queue.add(root);
      while(!queue.isEmpty()){
        int size= queue.size();
        List<Integer> level= new ArrayList<>();
        for(int i=0;i<size;i++){
          cur= queue.poll();
          level.add(cur.val);
          if(cur.left!=null){
            queue.add(cur.left);
          }if(cur.right!=null){
            queue.add(cur.right);
          }
        }
        res.add(level);
      }
      return res;
      //Time O(n)
      //Space O(n)
    }
  }
```

#### Idea

It is different from DFS. The method it will use is using Queue to solve the problem. LinkedList implements the interface of Queue.

Initial the Queue:

Queue<TreeNode> queue= new LinkedList<TreeNode>();

Queue respect the rules of **FIFO**, Stack respect the FILO

### 2. Recursive Solution

``` java
class Solution {
    List<List<Integer>> levels = new ArrayList<List<Integer>>();

    public void helper(TreeNode node, int level) {
        // start the current level
        if (levels.size() == level)
            levels.add(new ArrayList<Integer>());

         // fulfil the current level
         levels.get(level).add(node.val);

         // process child nodes for the next level
         if (node.left != null)
            helper(node.left, level + 1);
         if (node.right != null)
            helper(node.right, level + 1);
    }
    
    public List<List<Integer>> levelOrder(TreeNode root) {
        if (root == null) return levels;
        helper(root, 0);
        return levels;
    }
}
```

#### Idea

List<List<Integer>> res = new ArrayList<>();

res.get(level) means:  all the data in the level+1 of the List

res.get(level).add(cur.val): add the node.val -> this level+1 of the List

![image-20220120211335214](/Users/youhao/Library/Application Support/typora-user-images/image-20220120211335214.png)

## 107. Binary Tree Level Order Traveral II

### 1. Iterative Solution 

```java
class Solution{
  public List<List<Integer>> levelOrderBottom(TreeNode root){
    Queue<TreeNode> queue=new LinkedList<>();
    List<List<Integer>> res=new LinkedList<>();
    if(root==null){
      return res;
    }
    queue.add(root);
    TreeNode cur=root;
    while(!queue.isEmpty()){
      int size=queue.size();
      List<Integer> level=new ArrayList<Integer>();
      for(int i=0;i<size;i++){
        cur=queue.poll();
        level.add(cur.val);
        if(cur.left!=null){
          queue.add(cur.left);
        }
        if(cur.right!=null){
          queue.add(cur.right);
        }
      }
      res.add(0,level);
    }
    return res;
  }
}
```

#### Idea

Same as 102, use the reverse order to store the node

### 2.1 Recursive Solution(use Collections.reverse())

``` java
class Solution{
  List<List<Integer>> res= new LinkedList<>();
  public List<List<Integer>> levelOrderBottom(TreeNode root){
    if(root==null){
      return 
    }
    helper.(root,0);
    Collections.reverse(res);
    return res;
  }
  public void helper(TreeNode root, int level){
    if(res.size()==level){
      res.add(new ArrayList<Integer>());
    }
    res.get(level).add(root.val);
    if(root.left!=null){
      helper(root.left,level+1);
    }
    if(root.right!=null){
      helper(root.right,level+1);
    }
  }
}
```

### 2.2 Recursive Solution(❌ Collections.reverse())

``` java
class Solution{
  List<List<Integer>> res= new LinkedList<>();
  public List<List<Integer>> levelOrderBottom(TreeNode root){
    if(root==null){
      return res;
    }
    helper(root,0);
    return res;
  }
    
  public void helper(TreeNode root, int level){
      if(root==null){
          return;
      }
    if(res.size()==level){
      res.add(0, new ArrayList<Integer>());
    }
      helper(root.left,level+1);
      helper(root.right,level+1);    
    res.get(res.size()-level-1).add(root.val);
  }
}
```

### ArrayList && LinkedList

|              | ArrayList                                                    | LinkedList                                                   |
| :----------- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| 表尾插入     | 时间复杂度O(1)                                               | 时间复杂度O(1)                                               |
| 表中插入     | 平均时间复杂度为O(n/2)。插入位置越靠近表头时间复杂度越大,最大能达到O(n)。插入位置越靠近表尾时间复杂度越小,最小能达到O(1)。但是因为使用了arraycopy赋值数组时间复杂度会小于理论值。 | 平均时间复杂度为O(n/4)插入位置越接近中间时间复杂度越大但是最大也只需O(n/2)最小为O(1) |
| 表头插入     | 时间复杂度为O(n)                                             | 时间复杂度为O(1)                                             |
| 删除         | 平均时间复杂度为O(n/2)。删除目标越靠近表头时间复杂度越大,最大能达到O(n)。删除目标越靠近表尾时间复杂度越小,最小能达到O(1)。但是因为使用了arraycopy赋值数组时间复杂度会小于理论值。 | 平均时间复杂度为O(n/4)删除越接近中间时间复杂度越大但是最大也只需O(n/2)最小为O(1) |
| 修改         | 时间复杂度O(1)                                               | 平均时间复杂度为O(n/4)                                       |
| 根据索引查找 | 时间复杂度O(1)                                               | 平均时间复杂度为O(n/4)                                       |

## 103. Binary Tree Zigzag Level Order Traversal

### 1. Iterative Solution (BFS)

``` java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res= new LinkedList<>();
        Queue<TreeNode> queue=new LinkedList<>();
        TreeNode cur= root;
        if(root==null){
            return res;
        }
        int height=0;
        queue.add(root);
        while(!queue.isEmpty()){
            List<Integer> level= new LinkedList<>();
            int size= queue.size();
                for(int i=0;i<size;i++){
                    cur=queue.poll();
                    if(height%2==0){
                        level.add(cur.val);
                    }else{
                        level.add(0,cur.val);
                    }
                    if(cur.left!=null){
                        queue.add(cur.left);
                    }
                    if(cur.right!=null){
                        queue.add(cur.right);
                    }
                }
            res.add(level);
            height++;
        }
        return res;
    }
}
```

Based on 102, add a height to record the level. IF the height is even, the order of output is left---->right, if the height is odd, the order of output is right------->left.

### 2. Recursive Solution(DFS)

``` java
class Solution{
  	List<List<Integer>> res= new LinkedList<>();
  	public List<List<Integer>> zigzagLevelOrder(TreeNode root){
      if(root==null){
        return res;
      }
      helper(root, 0);
      return res;
      //Time O(n)
      //Space O(n)
    }
  	public void helper(TreeNode root, int level){
      if(res.size()==level){
        res.add(new LinkedList<>());
      }
      if(level%2==0){
        res.get(level).add(root.val);
      }else{
        res.get(level).add(0,root.val);
      }
      if(root.left!=null){
        helper(root.left,level+1);
      }
      if(root.right!=null){
        helper(root.right,level+1);
      }
    }
}
```

## 24. Swap Nodes in Pairs

```java
class Solution {
  public ListNode swapPairs(ListNode head) {
    if (head == null || head.next == null) {
      return head;
    }
    ListNode slow = head;
    ListNode fast = head.next;
    ListNode pre = new ListNode(0);
    ListNode res = pre;
    pre.next = head;
    while (fast != null) {
      pre.next = fast;
      slow.next = fast.next;
      fast.next = slow;
      ListNode temp = fast;
      fast = slow;
      slow = temp;
      pre = fast;
      if (fast.next != null) {
        fast = fast.next.next;
      } 
      else {
        fast = fast.next;
      }
      slow = slow.next.next;
    }
    return res.next;
  }
}
```



## 652. Find Duplicate SubTrees

### 1. SubTrees Using PostTree

```java
class Solution {
    // 记录所有子树以及出现的次数
    HashMap<String, Integer> memo = new HashMap<>();
    // 记录重复的子树根节点
    LinkedList<TreeNode> res = new LinkedList<>();

    /* 主函数 */
    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
        traverse(root);
        return res;
    }

    String traverse(TreeNode root) {
        if (root == null) {
            return "#";
        }

        String left = traverse(root.left);
        String right = traverse(root.right);

        String subTree = left + "," + right + "," + root.val;

        int freq = memo.getOrDefault(subTree, 0);
        // 多次重复也只会被加入结果集一次
        if (freq == 1) {
            res.add(root);
        }
        // 给子树对应的出现次数加一
        memo.put(subTree, freq + 1);
        return subTree;
    }
}


```

自底向上进行查询， 零用String来存储到map中方便查找一样的元素

# Tree Construct

## 297. Serialize and Deserialize Binary Tree

### 1. Recursion (Preorder)

```java
public class Codec {
    // Encodes a tree to a single string.
  private StringBuilder preOrder;
  public String serialize(TreeNode root) {
    if (root == null) {
      return "#";
    }
    preOrder = new StringBuilder();
    serializeTree(root);
    return preOrder.toString();
  }
  
  public void serializeTree(TreeNode root) {
    if (root == null) {
      preOrder.append("#" + ",");
      return;
    }
    preOrder.append(root.val + ",");
    serializeTree(root.left);
    serializeTree(root.right);
    return;
  }
  
    // Decodes your encoded data to tree.
  public TreeNode deserialize(String data) {
    String[] newDatas = data.split(",");
    List<String> list = new LinkedList<>();
    for (String newData : newDatas) {
      list.add(newData);
    }
    return deserializeTree(list);
  }
  
  public TreeNode deserializeTree(List<String> list) {
    if (list.isEmpty()) {
      return null;
    }
    
    String cur = list.remove(0);
    if (cur.equals("#")) {
      return null;
    }
    
    TreeNode root = new TreeNode(Integer.parseInt(cur));
    root.left = deserializeTree(list);
    root.right = deserializeTree(list);
    return root;
  }
}
```

将Tree的node利用前序遍历记录在String中， null用“#”来表示， 每一个节点使用","来分隔

再将整个String根据“，”分割成数组

再根据前序遍历的数组中第一个都是根节点特点进行遍历构造

## 654. Maximum Binary Tree

### 1. Recursion

```java
class Solution {
    /* 主函数 */
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return build(nums, 0, nums.length - 1);
    }

    /* 定义：将 nums[lo..hi] 构造成符合条件的树，返回根节点 */
    TreeNode build(int[] nums, int lo, int hi) {
        // base case
        if (lo > hi) {
            return null;
        }

        // 找到数组中的最大值和对应的索引
        int index = -1, maxVal = Integer.MIN_VALUE;
        for (int i = lo; i <= hi; i++) {
            if (maxVal < nums[i]) {
                index = i;
                maxVal = nums[i];
            }
        }

        TreeNode root = new TreeNode(maxVal);
        // 递归调用构造左右子树
        root.left = build(nums, lo, index - 1);
        root.right = build(nums, index + 1, hi);

        return root;
    }
}

```

## 105. Construct Binary Tree from Preorder and Inorder Traversal

### 1. Recursion

```java
class Solution {
    // 存储 inorder 中值到索引的映射
    HashMap<Integer, Integer> valToIndex = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i = 0; i < inorder.length; i++) {
            valToIndex.put(inorder[i], i);
        }
        return build(preorder, 0, preorder.length - 1,
                    inorder, 0, inorder.length - 1);
    }

    /*
       定义：前序遍历数组为 preorder[preStart..preEnd]，
       中序遍历数组为 inorder[inStart..inEnd]，
       构造这个二叉树并返回该二叉树的根节点
    */
    TreeNode build(int[] preorder, int preStart, int preEnd,
                   int[] inorder, int inStart, int inEnd) {
        if (preStart > preEnd) {
            return null;
        }

        // root 节点对应的值就是前序遍历数组的第一个元素
        int rootVal = preorder[preStart];
        // rootVal 在中序遍历数组中的索引
        int index = valToIndex.get(rootVal);

        int leftSize = index - inStart;

        // 先构造出当前根节点
        TreeNode root = new TreeNode(rootVal);
        // 递归构造左右子树
        root.left = build(preorder, preStart + 1, preStart + leftSize,
                inorder, inStart, index - 1);

        root.right = build(preorder, preStart + leftSize + 1, preEnd,
                inorder, index + 1, inEnd);
        return root;
    }
}
// 详细解析参见：
// https://labuladong.github.io/article/?qno=105

```

![image-20220813225625268](/Users/youhao/Library/Application Support/typora-user-images/image-20220813225625268.png)

![image-20220813225701507](/Users/youhao/Library/Application Support/typora-user-images/image-20220813225701507.png)



## 106. Construct Binary Tree from Inorder and Postorder Traversal

### 1. Recursion

```java
class Solution {
    // 存储 inorder 中值到索引的映射
    HashMap<Integer, Integer> valToIndex = new HashMap<>();

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        for (int i = 0; i < inorder.length; i++) {
            valToIndex.put(inorder[i], i);
        }
        return build(inorder, 0, inorder.length - 1,
                    postorder, 0, postorder.length - 1);
    }

    /*
       定义：
       中序遍历数组为 inorder[inStart..inEnd]，
       后序遍历数组为 postorder[postStart..postEnd]，
       构造这个二叉树并返回该二叉树的根节点
    */
    TreeNode build(int[] inorder, int inStart, int inEnd,
                int[] postorder, int postStart, int postEnd) {

        if (inStart > inEnd) {
            return null;
        }
        // root 节点对应的值就是后序遍历数组的最后一个元素
        int rootVal = postorder[postEnd];
        // rootVal 在中序遍历数组中的索引
        int index = valToIndex.get(rootVal);
        // 左子树的节点个数
        int leftSize = index - inStart;
        TreeNode root = new TreeNode(rootVal);
        // 递归构造左右子树
        root.left = build(inorder, inStart, index - 1,
                         postorder, postStart, postStart + leftSize - 1);
        
        root.right = build(inorder, index + 1, inEnd,
                          postorder, postStart + leftSize, postEnd - 1);
        return root;
    }
}


```

![image-20220813231049012](/Users/youhao/Library/Application Support/typora-user-images/image-20220813231049012.png)

![image-20220813231111820](/Users/youhao/Library/Application Support/typora-user-images/image-20220813231111820.png)



## 889. Construct Binary Tree from PreOrder and Postorder Traversal

### 1. Recursion

```java
class Solution {
    // 存储 postorder 中值到索引的映射
    HashMap<Integer, Integer> valToIndex = new HashMap<>();

    public TreeNode constructFromPrePost(int[] preorder, int[] postorder) {
        for (int i = 0; i < postorder.length; i++) {
            valToIndex.put(postorder[i], i);
        }
        return build(preorder, 0, preorder.length - 1,
                    postorder, 0, postorder.length - 1);
    }

    // 定义：根据 preorder[preStart..preEnd] 和 postorder[postStart..postEnd]
    // 构建二叉树，并返回根节点。
    TreeNode build(int[] preorder, int preStart, int preEnd,
                   int[] postorder, int postStart, int postEnd) {
        if (preStart > preEnd) {
            return null;
        }
        if (preStart == preEnd) {
            return new TreeNode(preorder[preStart]);
        }

        // root 节点对应的值就是前序遍历数组的第一个元素
        int rootVal = preorder[preStart];
        // root.left 的值是前序遍历第二个元素
        // 通过前序和后序遍历构造二叉树的关键在于通过左子树的根节点
        // 确定 preorder 和 postorder 中左右子树的元素区间
        int leftRootVal = preorder[preStart + 1];
        // leftRootVal 在后序遍历数组中的索引
        int index = valToIndex.get(leftRootVal);
        // 左子树的元素个数
        int leftSize = index - postStart + 1;

        // 先构造出当前根节点
        TreeNode root = new TreeNode(rootVal);
        // 递归构造左右子树
        // 根据左子树的根节点索引和元素个数推导左右子树的索引边界
        root.left = build(preorder, preStart + 1, preStart + leftSize,
                postorder, postStart, index);
        root.right = build(preorder, preStart + leftSize + 1, preEnd,
                postorder, index + 1, postEnd - 1);

        return root;
    }
}

```

**1、首先把前序遍历结果的第一个元素或者后序遍历结果的最后一个元素确定为根节点的值**。

**2、然后把前序遍历结果的第二个元素作为左子树的根节点的值**。

**3、在后序遍历结果中寻找左子树根节点的值，从而确定了左子树的索引边界，进而确定右子树的索引边界，递归构造左右子树即可**。

![image-20220813232800179](/Users/youhao/Library/Application Support/typora-user-images/image-20220813232800179.png)



## 108. Convert Sorted Array to Binary Search Tree

### 1. Preorder Traversal: Always Choose Left Middle Node as a Root

```java
class Solution {
  
    int[] nums;

    public TreeNode helper(int left, int right) {
        if (left > right) return null;

        // always choose left middle node as a root
        int p = (left + right) / 2;

        // preorder traversal: node -> left -> right
        TreeNode root = new TreeNode(nums[p]);
        root.left = helper(left, p - 1);
        root.right = helper(p + 1, right);
        return root;
    }

    public TreeNode sortedArrayToBST(int[] nums) {
        this.nums = nums;
        return helper(0, nums.length - 1);
    }
  //Time O(n)
  //Space O(logN)
}
```

#### Idea

![image-20220126222947627](/Users/youhao/Library/Application Support/typora-user-images/image-20220126222947627.png)

### 2. Preorder Traversal: Right Middle Node as Root

``` java
class Solution {
    int[] nums;

    public TreeNode helper(int left, int right) {
        if (left > right) return null;

        // always choose right middle node as a root
        int p = (left + right) / 2;
        if ((left + right) % 2 == 1) ++p;

        // preorder traversal: node -> left -> right
        TreeNode root = new TreeNode(nums[p]);
        root.left = helper(left, p - 1);
        root.right = helper(p + 1, right);
        return root;
    }

    public TreeNode sortedArrayToBST(int[] nums) {
        this.nums = nums;
        return helper(0, nums.length - 1);
    }
}
```

#### Idea

![image-20220126224601607](/Users/youhao/Library/Application Support/typora-user-images/image-20220126224601607.png)

### 3. Preoderder Traversal: Random Choose Middle Node as Root

```java
class Solution {
    int[] nums;
    Random rand = new Random();
    
    public TreeNode helper(int left, int right) {
        if (left > right) return null;
        
        // choose random middle node as a root
        int p = (left + right) / 2; 
        if ((left + right) % 2 == 1) p += rand.nextInt(2);

        // preorder traversal: node -> left -> right
        TreeNode root = new TreeNode(nums[p]);
        root.left = helper(left, p - 1);
        root.right = helper(p + 1, right);
        return root;
    }
    
    public TreeNode sortedArrayToBST(int[] nums) {
        this.nums = nums;
        return helper(0, nums.length - 1);    
    }
}
```

#### Idea

Randomly choose the right or left middle node as root

# Tree Path/Depth/Reverse

## 104. Maximum Depth of Binary Tree

### 1. Recursive Solution

``` java
class Solution {

    public int maxDepth(TreeNode root) {
        if(root==null){
            return 0;
        }
        int left_height=maxDepth(root.left);
        int right_height=maxDepth(root.right);
        return java.lang.Math.max(left_height,right_height)+1;
        //Time O(N)
        //Space Best O(logN) Worst O(N)
    }
}
```

![image-20220131152649235](/Users/youhao/Library/Application Support/typora-user-images/image-20220131152649235.png)

### 2. Iterative Solution

``` java
class Solution{
  public int maxDepth(TreeNode root){
    Stack<TreeNode> stack=new Stack<>();
    Stack<Integer> depths=new Stack<>();
    stack.push(root);
    depths.push(1);
    int cur=0,depth=0;
    while(!stack.isEmpty()){
      root=stack.pop();
      cur=depths.pop();
      if(root!=null){
        depth=Math.max(cur,depth);
        stack.push(root.left);
        stack.push(root.right);
        depths.push(cur+1);
        depths.push(cur+1);
      }
    }
    return depth;
  }
}
```



### 3. BFS

``` java
class Solution{
	public int maxDepth(TreeNode root){
    if(root==null){
      return 0;
    }
    Queue<TreeNode> queue=new LinkedList<>();
    int depth=0;
    queue.add(root);
    while(!queue.isEmpty()){
      int level= queue.size();
      for(int i=0;i<level;i++){
        TreeNode cur=queue.poll();
        if(cur.left!=null){
          queue.add(cur.left);
        }
        if(cur.right!=null){
          queue.add(cur.right);
        }
      }
      depth++;
    }
    return depth;
    //Time O(n)
    //Space O(n)
  }
}
```

## 101. Symmetric Tree

### Recursion

``` java
class Solution{
	public boolean isSymmetric(TreeNode root){
    return mirror(root,root);
  }
  public boolean mirror(TreeNode t1,TreeNode t2){
    if(t1==null&&t2==null){return true;}
    if(t1==null||t2==null){return false;}
    return (t1.val==t2.val)
      		&&mirror(t1.left,t2.right)
      		&&mirror(t1.right,t2.left);
    //Time O(n)
    //Space O(n)
  }
}
```

![image-20220204174833084](/Users/youhao/Library/Application Support/typora-user-images/image-20220204174833084.png)

### Iterative Solution

``` java
class Solution{
  public boolean isSymmetric(TreeNode root){
    TreeNode t1=root;
    TreeNode t2=root;
    if(root==null){
      return true;
    }
    Queue<TreeNode> queue=new LinkedList<>();
    queue.add(t1);
    queue.add(t2);
    while(!queue.isEmpty()){
      t1=queue.poll();
      t2=queue.poll();
      if(t1==null&&t2==null){continue;}
      if(t1==null||t2==null){return false;}
      if(t1.val!=t2.val){return false;}
      queue.add(t1.left);
      queue.add(t2.right);
      queue.add(t1.right);
      queue.add(t2.left);
    }
    return true;
    //Time O(n)
    //Space O(n)
  }
}
```

## 226. Invert Binary Tree

### 1. Recursive Solution

``` java
class Solution{
  public TreeNode invertTree(TreeNode root){
    if(root==null){
      return null;
    }
    TreeNode left=invertTree(root.left);
    TreeNode right=invertTree(root.right);
    root.left=right;
    root.right=left;
    return root;
    //Time O(n)
    //Space O(n)
  }
}
```

```java
class Solution{
  TreeNode tamp;
  public TreeNode invertTree(TreeNode root){
    if(root==null){return null;}
    tamp=root.left;
    root.left=root.right;
    root.right=tamp;
    invertTree(root.left);
    invertTree(root.right);
    return root;
  }
}
```



![image-20220205171123028](/Users/youhao/Library/Application Support/typora-user-images/image-20220205171123028.png)

### 2. Iterative Solution

``` java
class Solution{
  public TreeNode invertTree(TreeNode root){
    if(root==null){return null;}
    TreeNode cur=root;
    Queue<TreeNode> queue=new LinkedList<>();
    queue.add(cur);
    while(!queue.isEmpty()){
      cur=queue.poll();
      TreeNode left=cur.left;
      TreeNode right=cur.right;
      cur.left= right;
      cur.right=left;
      if(cur.left!=null){queue.add(cur.left);}
      if(cur.right!=null){queue.add(cur.right);}
    }
    return root;
    //Time O(n)
    //Space O(n)
  }
}
```

## 543. Diameter of Binary Tree

### Recursive Solution

```java
class Solution{
  int max=0;
  public int diameterOfBinaryTree(TreeNode root){
    maxDiameter(root);
    return max;
  }
  public int maxDiameter(TreeNode root){
    if(root==null){return 0;}
    int left=maxDiameter(root.left);
    int right=maxDiameter(root.right);
    max=Math.max(max,left+right);
    return Math.max(left,right)+1;
		//Time O(n)
    //Space worst tree O(n) balanced tree O(logn)
  }
}
```

The general solution is same as **104** problem, and when we traveral a specific node, we need to find out the max diameter about this node. So we need to definite a Integer max to record. Using Math.max to calculate the longest diameter for traversaling each of node.

## 257. Binary  Tree Paths

### 1. Iterative Solution

``` java
class Solution{
  public List<String> binaryTreePaths(TreeNode root){
    Queue<TreeNode> queue= new LinkedList<>();
    LinkedList<String> res= new LinkedList<>();
    LinkedList<String> cur_path=new LinkedList<>();
    String path;
    TreeNode cur= root;
    queue.add(root);
    cur_path.add(Integer.toString(root.val));
    while(!queue.isEmpty()){
      cur=queue.poll();
      path=cur_path.poll();
      if(cur.left!=null){
        queue.add(cur.left);
        cur_path.add(path+"->"+Integer.toString(cur.left.val));
      }
      if(cur.right!=null){
        queue.add(cur.right);
        cur_path.add(path+"->"+Integer.toString(cur.right.val));
      }
      if(cur.left==null&&cur.right==null){
        res.add(path);
      }
    }
    return res;
    //Time O(n)
    //Space O(n)
  }
}
```

Idea 

Using LinkedList Path to store the paths. if the node doesn't have child node, we can put the Path into res.

### 2. Recursive Solution (Using StringBuilder)

``` java
class Solution{
  public List<String> binaryTreePaths(TreeNode root){
    LinkedList<String> res=new LinkedList<>();
    StringBuilder path=new StringBuilder();// Dynamic array can effectively reduce the loss of string concatenation.
    helper(root,path,res);
    return res;
  }
  public void helper(TreeNode root, StringBuilder path,LinkedList res){
    if(root==null){return;}
    path.append(root.val);
    if(root.left==null&&root.right==null){
      res.add(path.toString());
    }
    //if node has child, we need to add "->" to connect child and root
    path.append("->");
    String cur=path.toString();
    helper(root.left,path,res);
    helper(root.right,new StringBuilder(cur),res);
    return;
  }
}
```

Idea 

StringBuilder path=new StringBuilder(); This way makes dynamic array can effectively reduce the loss of string concatenation

In Recursion, we need to make sure the boundary "**root==null**"

Then, make a judgement: if root.left && root.right ==null, we can put the string "path" into res

Next, path.append("->") and go to the left child node.

Because after enter the left node, the path has been changed. We need to definite a new varible "cur" that equals the path, in order to record the state that before enter the left child node. In this case, we can go to the right child node.

Finally, return.

## 110. Balanced Binary Tree

### 1. Recursive Solution（Top-down)

```java
class Solution{
  
  public boolean isBalanced(TreeNode root){
    int left=helper(root.left);
    int right=helper(root.right);
    //jude the |left-right| whether if less than 1, and recurse to root.left & root.right
    return Math.abs(left-right)<=1&&isBalanced(root.left)&&isBalanced(root.right);
  }
  //Calculate each node's height
  public int helper(TreeNode root){
    if(root==null){
      return 0;
    }
    int left=helper(root.left);
    int right=helper(root.right);
    return Math.max(left,right)+1;
  }
  //Time O(n^2)
  //Space O(n)
}
```

### 2.Recursive Solution (Bottom-up)

```java
class Solution{
  public boolean isBalanced(TreeNode root){
    return helper(root)!=-1;
  }
  public int helper(TreeNode root){
    if(root==null){return 0;}
    int left=helper(root.left);
    if(left==-1){return -1;}
    int right=helper(root.right);
    if(right==-1){return -1;}
    if(Math.abs(left-right)>1){return -1;}
    return Math.max(left,right)+1;
  }
	//Time O(n)
  //Space O(n)
}
```



## 617. Merge Two Binary Trees

### 1. Recursion

```java
public class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
       if(t1==null){
           return t2;
       }
       if(t2==null){
           return t1;
       }
        t1.val+=t2.val;
        t1.left=mergeTrees(t1.left,t2.left);
        t1.right=mergeTrees(t1.right,t2.right);
        return t1;
      //m is total of m nodes need to be traversed
      //Time O(m)
      //Space O(m)  Average O(logm)
    }
}

```

### 2. Iteration

```java
class Solution{
  public TreeNode mergeTrees(TreeNode t1, TreeNode t2){
    if(t1==null)return t2;
    Queue<TreeNode[]> queue=new LinkedList<>();
    queue.add(new TreeNode[] {t1,t2});
    while(!queue.isEmpty()){
      TreeNode[] t=queue.poll();
      //if t1= null, which means last loop, the t1&t2's subnode are all null, so we don't need to operate, if  t2 =null, which means last loop the t2's subnode=null, so we jusk keep the t1.
      if(t[0]==null||t[1]==null){
        continue;
      }
      t[0].val+=t[1].val;
      if(t[0].left==null){
        t[0].left=t[1].left;
      }else{
        queue.add(new TreeNode[] {t[0].left,t[1].left});
      }
      if(t[0].right==null){
        t[0].right=t[1].right;
      }else{
        queue.add(new TreeNode[] {t[0].right,t[1].right});
      }
    }
    return t1;
    //Time O(n)
    //Space O(n)
  }
}
```



## 100. Same Tree

### Iterative Solution

```java
class Solution{
  public boolean isSameTree(TreeNode p, TreeNode q){
    Queue<TreeNode[]> queue=new LinkedList<>();
    queue.add(new TreeNode[] {p,q});
    while(!queue.isEmpty()){
      TreeNode[] t=queue.poll();
      if(t[0]==null&&t[1]==null){continue;}
      if(t[0]==null||t[1]==null){return false;}
      if(t[0].val!=t[1].val){return false;}
      queue.add(new TreeNode {t[0].left,t[1].left});
      queue.add(new TreeNode {t[0].right,t[1].right});
    }
    return true;
    //Time O(n)
    //Space best O(logN) worst O(N)
  }
}
```

put p & q into TreeNode array storage in queue. And then, make p&q traversal in the same time and compare the value of p&q.

### Recursive Solution 

```java
class Solution{
  public boolean isSameTree(TreeNode p,TreeNode q){
    //p and q are both null
    if(p==null&&q==null){return true;}
    //one of p and q is null
    if(p==null||q==null){return false;}
    if(p.val!=q.val){return false;}
    return isSameTree(p.left,q.left)&& isSameTree(p.right,q.right);
    //Time O(n)
    //Space best O(logN) worst O(n)
  }
}
```

## 112. Path Sum

### 1. Recursive Solution

``` java
class Solution{
  public boolean hasPathSum(TreeNode root,int targetSum){
    return helper(root,0,targetSum);
  }
  public boolean helper(TreeNode root, int path, int targetSum){
    if(root==null)return false;
    path+=root.val;
    if(path==targetSum&&root.left==null&&root.right==null){
      return true;
    }
    return helper(root.left,path,targetSum)||helper(root.right,path,targetSum);//if exist the path that equal targetSum, the result is true
    //Time O(n)
    //Space Best O(logn) Worst O(n)
  }
}
```

### 2. Iterative Solution

``` java
class Solution{
  public boolean hasPathSum(TreeNode root,int targetSum){
    if(root==null){return false;}
    Queue<TreeNode> queue=new LinkedList<>();
    Queue<Integer> path=new LinkedList<>();//Use Queue to store the current path of current Node
    queue.add(root);
    path.add(0);
    while(!queue.isEmpty()){
      TreeNode cur=queue.poll();
      int curPath=path.poll();
      curPath+=cur.val;
      if(curPath==targetSum&&cur.left==null&&cur.right==null){
        return true;
      }
      if(cur.left!=null){
        queue.add(cur.left);
        path.add(curPath);
      }
      if(cur.right!=null){
        queue.add(cur.right);
        path.add(curPath);
      }
    }
    return false;
    //Time O(n)
    //Space Best O(logn) Worst O(n)
  }
}
```

## 111. Minimum Depth of Binary Tree

### 1. Recursive Solution (DFS)

```java
class Solution{
  public int minDepth(TreeNode root){
    if(root==null){return 0;}
    int left=0;
    int right=0;
    if(root.left!=null&&root.right!=null){
      left=minDepth(root.left);
      right=minDepth(root.right);
    }
    if(root.left!=null&&root.right==null){
      left=minDepth(root.left);
      right=left;
    }
    if(root.right!=null&&root.left==null){
      right=minDepth(root.right);
      left=right;
    }
    return Math.min(right,left)+1;
  }
  //Time O(n)
  //Space best O(logn) worst O(n)
}
```

![image-20220222160746056](/Users/youhao/Library/Application Support/typora-user-images/image-20220222160746056.png)

### 2. Iterative Solution（DFS)

```java
class Solution{
  public int minDepth(TreeNode root){
    if(root==null)return 0;
    Stack<TreeNode> stack=new Stack<>();
    Stack<Integer> path=new Stack<>();
    stack.add(root);
    path.add(1);
    int minPath=Integer.MAX_VALUE;
    while(!stack.isEmpty()){
      TreeNode cur=stack.pop();
      int curPath=path.pop();
      if(cur.left==null&&cur.right==null){
        minPath= Math.min(minPath,curPath);
      }
      if(cur.left!=null){
        stack.add(cur.left);
        path.add(curPath+1);
      }
      if(cur.right!=null){
        stack.add(cur.right);
        path.add(curPath+1);
      }
    }
    return minPath;
  }
  //Time O(n)
  //Space O(n)
}
```

![image-20220222161621842](/Users/youhao/Library/Application Support/typora-user-images/image-20220222161621842.png)

### 3. Iterative Solution(BFS)

```java
class Solution{
  public int minDepth(TreeNode root){
    if(root==null){return 0;}
    Queue<TreeNode> queue=new LinkedList<>();
    Queue<Integer> path=new LinkedList<>();
    int minPath=0;
    queue.add(root);
    path.add(1);
    while(!queue.isEmpty()){
      TreeNode cur=queue.poll();
      minPath= path.poll();
      if(cur.left==null&&cur.right==null){
        break;
      }
      if(cur.left!=null){
        queue.add(cur.left);
        path.add(minPath+1);
      }
      if(cur.right!=null){
        queue.add(cur.right);
        path.add(minPath+1);
      }
    }
    return minPath;
  }
  //Time O(n)
  //Space O(n)
}
```

![image-20220222162521473](/Users/youhao/Library/Application Support/typora-user-images/image-20220222162521473.png)

# Binary Search Tree

**二叉查找树**（英语：Binary Search Tree），也称为**二叉搜索树**、**有序二叉树**（ordered binary tree）或**排序二叉树**（sorted binary tree），是指一棵空树或者具有下列性质的[二叉树](https://zh.wikipedia.org/wiki/二叉树)：

1. 若任意节点的左子树不空，则左子树上所有节点的值均小于它的根节点的值；
2. 若任意节点的右子树不空，则右子树上所有节点的值均大于它的根节点的值；
3. 任意节点的左、右子树也分别为二叉查找树；

## 1. 中序遍历的BST是有序的(升序)

## 2. 利用中序遍历的BST实现累加树

## 538. Convert BST to Greater Tree

### 1. Inorder Tree Traversal

```java
class Solution {
    public TreeNode convertBST(TreeNode root) {
        traverse(root);
        return root;
    }

    // 记录累加和
    int sum = 0;
    void traverse(TreeNode root) {
        if (root == null) {
            return;
        }
        traverse(root.right);
        // 维护累加和
        sum += root.val;
        // 将 BST 转化成累加树
        root.val = sum;
        traverse(root.left);
    }
}

```

由于中序遍历可以得到升序的序列，如果先遍历root.right，能够得到降序的序列，就可以从最右边开始遍历， 我们需要定义一个累加计数器，将每次遍历的节点值与累加器相加。

## 3. BST的基础操作： 判断BST的合法性， 增，删，改，查。 【删除】 和 【判断合法性比较复杂】

### 1. BST 基础操作框架 

```java
void BST(TreeNode root, int target) {
    if (root.val == target)
        // 找到目标，做点什么
    if (root.val < target) 
        BST(root.right, target);
    if (root.val > target)
        BST(root.left, target);
}

```



## 98. Validate Binary Search Tree (判断合法性)

### 1. Recursion

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, null, null);
    }

    /* 限定以 root 为根的子树节点必须满足 max.val > root.val > min.val */
    boolean isValidBST(TreeNode root, TreeNode min, TreeNode max) {
        // base case
        if (root == null) return true;
        // 若 root.val 不符合 max 和 min 的限制，说明不是合法 BST
        if (min != null && root.val <= min.val) return false;
        if (max != null && root.val >= max.val) return false;
        // 限定左子树的最大值是 root.val，右子树的最小值是 root.val
        return isValidBST(root.left, min, root)
                && isValidBST(root.right, root, max);
    }
}

```

根据BST 左子树的值小于当前根节点值， 右子树的值大于根节点值来讨论。 但是如果我们只讨论root.left.val 是否小于root.val，或者root.right.val是否大于root.val，我们只能判断两层之间是否是合理的BST，而不能讨论整个左子树，右子树的值和当前根节点的值的大小关系。 

所以在递归的时候，我们需要向根节点的左子树传递当前节点值作为左子树中的最大值， 以当前节点值作为右子树的最小值，让子树的值和这些比较从而能够整体验证是否属于BST



## 700. Search in a Binary Search Tree  (搜索)

### 1. Recursion

```java
 class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null) {
            return root;
        }
        
        if (root.val == val) {
            return root;
        }
        else if (root.val < val) {
            return searchBST(root.right, val);
        }
        else {
            return searchBST(root.left, val);
        }
        
    }
}
```

这种方式属于BST的特性，可以避免将所有的节点都进行遍历

## 701. Insert into a Binary Search Tree(插入)

### 1. Recursion

```java
class Solution {
  public TreeNode insertIntoBST(TreeNode root, int val) {
      // 找到空位置插入新节点
    if (root == null) return new TreeNode(val);
    // if (root.val == val)
    //     BST 中一般不会插入已存在元素
    if (root.val < val) 
        root.right = insertIntoBST(root.right, val);
    if (root.val > val) 
        root.left = insertIntoBST(root.left, val);
    return root;
  }
}
```



## 450. Delete Node in a BST

### 1. Recursion

```java
class Solution {
  public TreeNode deleteNode(TreeNode root, int key) {
    if (root == null) {
      return root;
    }
    // find the key node
    if (root.val == key) {
      //case 1 & case 2
      if (root.left == null) {return root.right;}
      if (root.right == null) {return root.left;}
      //case 3
      // find the min of the right subTreeNode
      TreeNode minRightNode = minRight(root.right);
      // delete the min of the right subTreeNode
      root.right = deleteNode(root.right, minRightNode.val);
      // let the current root's value equals to minRightNode's value
      root.val = minRightNode.val;
    }
    // current val < key, to find the right subTreeNode
    else if (root.val < key) {
      root.right = deleteNode(root.right, key);
    }
    //current val > key, to find the left subTreeNode
    else {
      root.left = deleteNode(root.left, key);
    }
    return root;
  }
  
  public TreeNode minRight(TreeNode root) {
    if (root == null) {return root;}
    //if root.left != null, the min of subTreeNode is in the left subTree
    if (root.left != null) {return minRight(root.left);}
    // if root.left == null, the current root is the min
    return root;
  }
}
```

case 1 & 2: target的子节点最多有一个，我们只需要删除当前节点，返回它的子节点就可

case 3:  target的子节点有两个， 我们需要找到右子树中最小的节点值， 让最小节点值来替代当前节点，使其满足BST结构。



## 4. 构造搜索树

## 96.  Unique Binary Search Trees

### 1. DP ： Bottom Up

```java
class Solution {
  public int numTrees(int n) {
    int[] dp = new int[n + 1];
    //n = 0时，节点为null也算是BST，所以个数为1
    dp[0] = 1;
    dp[1] = 1;
    // dp存储的是 当前index的一共的BST可能性
    for (int i = 2; i <= n; i++) {
      // root的可能性是从 1 -> i， 列举所有可能相加起来
      // 如果root 为 j， 我们需要计算 1 -> j - 1的组合也就是dp[j - 1]；和 j + 1 -> i的组合，该组合和（1 -> i - j)的情况一样，也就是dp[i - j]; 组合和组合的情况相乘
      for (int j = 1; j <= i; j++) {
        dp[i] += dp[j - 1] * dp[i - j];
      }
    }
    return dp[n];
  }
}
```



### 2. Recursion: Top Down

```java
class Solution {
  private int[][] memo;
  public int numTrees(int n) {
    memo = new int[n + 1][n + 1];
    if (n == 0) {return 1;}
    return build(1, n);
  }
  
  public int build(int l, int h) {
    //因为不存在的话说明为空节点，也算是BST的一种情况
    if (l > h) {return 1;}
    //如果以前讨论过该范围的数，我们直接返回 就不需要计算了，减少时间损耗
    if (memo[l][h] != 0) {
      return memo[l][h];
    }
    int res = 0;
    //将l->h范围内的每一个数字作为一次根节点进行讨论
    for (int i = l; i <= h; i++) {
      //左边是 l -> i - 1
      int left = build(l, i - 1);
      //右边是 i + 1 -> h
      int right = build(i + 1, h);
      res += left * right;
    }
    //记录memo
    memo[l][h] = res;
    return res;
  }
}
```

## 95. Unique Binary Search Trees II （Follow up )

### 1. Recursion

```java
class Solution {
  public List<TreeNode> generateTrees(int n) {
    if (n == 0) {return new ArrayList<>();}
    return build(1, n);
  }
  
  public List<TreeNode> build(int l, int h) {
    List<TreeNode> res = new ArrayList<>();
    if (l > h) {
      res.add(null);
      return res;
    }
    //将每个 l->h范围内的值当作root
    for (int i = l; i <= h; i++) {
      //记录[l, i - 1] 和 [i + 1, h] 的BST的所有可能性， 然后再和root进行组合
      List<TreeNode> leftTree = build(l, i - 1);
      List<TreeNode> rightTree = build(i + 1, h);
      for (TreeNode left : leftTree) {
        for (TreeNode right : rightTree) {
          TreeNode root = new TreeNode(i);
          root.left = left;
          root.right = right;
          res.add(root);
        }
      }
    }
    return res;
  }
}
```





## 230. Kth Smallest Element in a BST

### 1. Recursion

```java
class Solution {
  public int kthSmallest(TreeNode root, int k) {
    List<Integer> res = new LinkedList<>();
    inOrder(root, res);
    return res.get(k - 1);
  }
  public void inOrder(TreeNode root, List<Integer> res) {
    if (root == null) return;
    inOrder(root.left, res);
    res.add(root.val);
    inOrder(root.right, res);
    //O(n)
    //O(n)
  }
}
```

### 2. Iterative 

```java
class Solution {
  public int kthSmallest(TreeNode root, int k) {
    if (root == null) {
      return 0;
    }
    Stack<TreeNode> stack = new Stack<>();
    while(true) {
      while(root != null) {
        stack.push(root);
        root = root.left;
      }
      TreeNode cur = stack.pop();
      if (--k == 0) {
        return cur.val;
      }
      root = root.right;
    }
    //Time O(H + k)
    //Space O(H)
  }
}
```



# Lowest Common Ancestor Series

## 235. Lowest Common Ancestor of a Binary Search Tree

### 1. Recursive Solution

```java
class Solution {
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root.val > p.val && root.val > q.val) {
      return lowestCommonAncestor(root.left, p, q);
    }
    else if (root.val < p.val && root.val < q.val) {
      return lowestCommonAncestor(root.right, p, q);
    } else {
      return root;
    }
  }
}
```



### 2. Iterative Solution 

```java
class Solution {
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    int pV = p.val;
    int qV = q.val;
    while(root != null) {
      int curV = root.val;
      if (curV > pV && curV > qV) {
        root = root.left;
      }
      else if (curV < pV && curV < qV) {
        root = root.right;
      } else {
        return root;
      }
    }
    return root;
  }
}
```

![image-20220703205117873](/Users/youhao/Library/Application Support/typora-user-images/image-20220703205117873.png)

## 236. Lowest Common Ancestor of a Binary Tree

### 1. Recursive Solution

```java
class Solution {
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == q || root == p) {
      return root; 
    }
    // search its left subTreeNode
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    // search its right subTreeNode
    TreeNode right = lowestCommonAncestor(root.right, p, q);
    // Based on the boundry condition to discuss
    if (left != null && right != null) {
      return root;// its left and right can find p and q
    }
    if (left != null) {
      return left;
    }
    return right;
    
  }
}
```

![image-20220703224243670](/Users/youhao/Library/Application Support/typora-user-images/image-20220703224243670.png)

## 1644.  Lowest Common Ancestor of a Binary Tree II

### 1. Recursive Solution 

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
  private boolean pFound,qFound;
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) { 
    TreeNode res = helper(root, p, q);
    if (qFound && pFound) {return res;} 
    else {return null;}
  }
  public TreeNode helper(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null) {
      return root;
    }
    TreeNode left = helper(root.left, p, q);
    TreeNode right = helper(root.right, p, q);
    if (root == p || root == q) {
        if (root == p) {
          pFound = true;
      }
        if (root == q) {
          qFound = true;
      }
        return root;
    }
    if (left != null && right != null){
      return root;
    }
    if (left != null) {
      return left;
    }
    return right;
  }
}
```



和236题不同的是，root == p|| root == q的判断语句需要放在 递归函数的后面。必须要先去判断其左右子树，再去判断当前节点。因为我们需要完全的找到p和q。  236题可以只找到1个就判断，因为p, q都存在于Tree中





# TreeMap

## 729. My Calendar I

### 1. Brute force

```java
class MyCalendar {
    private List<Pair> res; 
    public MyCalendar() {
        res = new ArrayList<>();
    }
    
    public boolean book(int start, int end) {
        if (res.isEmpty()) {
            res.add(new Pair(start,end));
            return true;
        } else {
            int s = res.size();
            for (int i = 0; i < s; i++) {
                Pair cur = res.get(i);
                if (Math.max(cur.start, start) < Math.min(cur.end, end)){
                    return false;
                }
            }
        }
        res.add(new Pair(start, end));
        return true;
    }
    class Pair {
        private int end;
        private int start;
        public Pair(int start, int end) {
            this.start = start;
            this.end = end;
        }
    }
}

/**
 * Your MyCalendar object will be instantiated and called as such:
 * MyCalendar obj = new MyCalendar();
 * boolean param_1 = obj.book(start,end);
 */
```



### 2. Balanced Tree (TreeMap)

```java
class MyCalendar {
    TreeMap<Integer, Integer> tree;
    public MyCalendar() {
        tree = new TreeMap<>();
    }
    
    public boolean book(int start, int end) {
        Integer prev = tree.floorKey(start);
        Integer next = tree.ceilingKey(start);
        if ((prev == null || tree.get(prev) <= start) && (next == null || next >= end)) {
            tree.put(start, end);
            return true;
        }
        return false;
    }
}

/**
 * Your MyCalendar object will be instantiated and called as such:
 * MyCalendar obj = new MyCalendar();
 * boolean param_1 = obj.book(start,end);
 */
```

 TreeMap的接口是SortedMap， CeilKey 就是比当前start大的集合里最小的key， floorKey是比当前start小的集合中最大的key， 如果没有返回null， 原理是红黑树