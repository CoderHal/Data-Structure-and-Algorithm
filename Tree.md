# Tree Traversal

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

# Tree Construct

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