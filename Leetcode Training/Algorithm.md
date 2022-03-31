# Array

## 1.Two Sum

### Problem Description

Given an array of integers `nums` and an integer `target`, return *indices(index的复数，索引) of the two numbers such that they add up to `target`*.

You may assume that each input would have ***exactly\* one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

**Example 1:**

```java
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**Example 2:**

```java
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

**Example 3:**

```java
Input: nums = [3,3], target = 6
Output: [0,1]
```

### Solution

#### Brute Force Solution

``` pseudocode
i= 0-> N

j= i+1-> N

a[i]+a[j]=Exp Result
```

``` 
class Solution{
	public int[] twoSum(int[] nums,int target){
		for(int i=0;i<nums.length;i++){
			for(int j=i+1;j<nums.length;j++){
				if(nums[i]+nums[j]== target){
				return new int[]{nums[i],nums[j]};
				}
			}
		}
		return new int[0];
	}
}
```



#### Map Solution



``` pseudocode
numMap->empty Map

For i= 0 to N

Delta = target-arr[i]

​	if numMap contains delta

​	return [i, numMpa(delta).value]

numMap.put(arr[i],i)
//HashMap function is accessed by mapping(映射) the hash value of the key to a location in the array.
//By this case, we don't need to through the array, we can dirrectly find the data what we want.
```

``` java
class Solution{
  public int[] twoSum(int[] nums,int target){
    HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
    //Map<Integer,Integer> map= new HashMap<>();
    //Map<Integer,Integer> map= new HashMap<Integer,Integer>();
    for(int i=0;i<nums.length;i++){
      int data= target-nums[i];
      if(map.containsKey(data)){
        return new int[]{map.get(data),i};
      }
      map.put(nums[i],i);
    }
    return new int[0];
  }
}
```



# Linked List

## 206.Reverse Linked List

### Solution

``` Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
      ListNode pre=null;
      while(head!=null){
        ListNode next_code=head.next;
        head.next=pre;
        pre=head;
        head=next_code;
      }
      return pre;
    }
}
```

Idea:

![image-20211229223619245](/Users/youhao/Library/Application Support/typora-user-images/image-20211229223619245.png)

## 234. Palindrome Linked List (Related with Reverse List)

### Idea

Palindrome which means the list of the data is same as its reverse list.

### Solution

#### 1.Copy into Array List and then Use Two Pointer Technique

``` Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public boolean isPalindrome(ListNode head) {
        List<Integer> vals = new ArrayList<>();

        // Convert LinkedList into ArrayList.
        ListNode currentNode = head;
        while (currentNode != null) {
            vals.add(currentNode.val);
            currentNode = currentNode.next;
        }

        // Use two-pointer technique to check for palindrome.
        int front = 0;
        int back = vals.size() - 1;
        while (front < back) {
            // Note that we must use ! .equals instead of !=
            // because we are comparing Integer, not int.
            if (!vals.get(front).equals(vals.get(back))) {
                return false;
            }
            front++;
            back--;
        }
        return true;
    }
}
```

Put the each value of the ListNode into the List "vals" and use two-pointer technique to check for palindrome. One of pointer goes from front to back, the other goes from back to front. When two pointer have met, the loop would be done. Therefore, we can judge that whether the list is palindrome or not.

#### 2. Recursive (Advanced) 递归

``` java
class Solution {

    private ListNode frontPointer;

    private boolean recursivelyCheck(ListNode currentNode) {
        if (currentNode != null) {
            if (!recursivelyCheck(currentNode.next)) return false;//aim to point the last Node of List
            if (currentNode.val != frontPointer.val) return false;//if front!=current this list is not a palindrome
            frontPointer = frontPointer.next;
        }
        return true;
    }

    public boolean isPalindrome(ListNode head) {
        frontPointer = head;
        return recursivelyCheck(head);
    }
}
```

Use recursive to make the currentNode point the last Node of list.  And then pop the stack, the currentNode return to the front until currentNode reach the front of the list.

Flaw: stack needs too much space.

#### 3. Reverse

``` Java
class Solution {

    public boolean isPalindrome(ListNode head) {

        if (head == null) return true;

        // Find the end of first half and reverse second half.
        ListNode firstHalfEnd = endOfFirstHalf(head);
        ListNode secondHalfStart = reverseList(firstHalfEnd.next);

        // Check whether or not there is a palindrome.
        ListNode p1 = head;
        ListNode p2 = secondHalfStart;
        boolean result = true;
        while (result && p2 != null) {
            if (p1.val != p2.val) result = false;
            p1 = p1.next;
            p2 = p2.next;
        }        

        // Restore the list and return the result.
        firstHalfEnd.next = reverseList(secondHalfStart);
        return result;
    }

    // Taken from https://leetcode.com/problems/reverse-linked-list/solution/
    private ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode nextTemp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }

    private ListNode endOfFirstHalf(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;//In order to find the middle node which is the last node of first list.
            slow = slow.next;
        }
        return slow;
    }
}
```

Idea:

1. Find the end of the first half.
2. Reverse the second half.
3. Determine whether or not there is a palindrome.
4. Restore the list.
5. Return the result.

Divide the List into two parts. Firstly, we need to find the last node in the first part, so that we can find the second list. And then, we can compare the first with the second, in order to judge whether the list is palindrome or not.

Attention：when we find the end node of the first list, we need to consider of the situation of fast.next=null and fast.next.next=null.

Space Complexity: O(1)

Flaws: The downside(缺点) of this approach is that in a concurrent(并发) environment (multiple threads and processes accessing the same data), access to the Linked List by other threads or processes would have to be locked while this function is running, because the Linked List is temporarily broken. This is a limitation of many in-place algorithms though.



## 141. Linked List Cycle

### Idea

If you want to detect the list whether has a list cycle or not. We can use two pointers fast & slow to do this. If Linked List has a cycle, fast & slow would be encounter.  

Attention: fast.next & fast, we need to consider about the situation that fast.next=null or fast=null. Just like this Linked List: [1]  or [1,2], pos=-1. pos=-1 which means the Linked List is not a cycle.

### Solution

``` 
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head==null){return false;}
        ListNode slow=head;
        ListNode fast=head.next;
        while(slow!=fast){
            if(fast==null||fast.next==null){
                return false;
            }
            slow=slow.next;
            fast=fast.next.next;
        }
        return true;
    }
}
```



##  876.Middle of the Linked List

### Idea

We just need to figure out the middle Node of this list.

### Solution

#### 1. Output to Array

``` 
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode[] A = new ListNode[100];
        int t = 0;
        while (head != null) {
            A[t++] = head;
            head = head.next;
        }
        return A[t / 2];
    }
}
}
```

Using the Array to storage the List Node. And we just need to return A[t/2]. Let think about the situation that has odd Nodes. When the loop is done, t= ListNode.size. For example, Size = 3 , t= 3. t/2=1. Therefore, return the second Node. If it has even Nodes, Size =4, t=4, t/2=2. Therefore, return the third Node.



#### 2. Fast and Slow Pointer

``` 
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
}

```

Using slow and fast pointers to find out the mid node. (When u want to find the mid Node, You need to remember this method)

#### 🌟Fast & Slow Pointer to Find the Middle Node

##### 1.Find the Right Middle Node

``` java
 ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
```

##### 2.Find the Left Middle Node

``` 
 ListNode slow = head, fast = head;
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
```

## 83. Remove Duplicates from Sorted List

### Idea

Loop the Linked List. If we find that the next node equals the current node, we need to make current node.next points to the node.next.next. And then, we don't need to made the current node equals current.next, until we find the next node doesn't equal to current node.

### Solution

``` java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head==null){
            return null;
        }
        ListNode current=head;
        while(current!=null&&current.next!=null){
            if(current.val==(current.next).val){
                current.next=current.next.next;
            }else{
                current=current.next;
            }
        }
        return head;
    }
}
```

## 203. Remove Linked List Elements

### Idea

Sentinel Node

Using Sentinel Node, in order to delete the head node. And then I can use the current and pre pointers to delete the Elements.

### Algorithm:

1. Initialize the sentinel Node and point to the head. sentinel.next = head;
2. Definite the current and pre pointers, pre = sentinel, current = head;
3. loop the Linked List, current !=null
4. If current node equals to val, pre.next=current.next. Be careful, the pre pointer need to keep still, and current =current.next;  If current node doesn't equal to val, pre=pre.next, current=current.next;
5. return sentinel.next;

### Solution

``` java
/*
	public class ListNode{
		int val;
		ListNode next;
		ListNode(){}
		ListNode(int val){ this.val=val;}
		ListNode(int val, ListNode next){this.next=next; this.val=val;}
	}
*/
class Solution{
	public ListNode removeElements(ListNode head,int val){
		ListNode sentinel=new ListNode(0);
		ListNode pre=sentinel;
		ListNode current=head;
		while(current!=null){
			if(current.val==val){
				pre.next=current.next;
			}else{
				pre=pre.next;
			}
				current=current.next;
		}
		return sentinel.next;
	}
}
```



## 237 Delete Node in a Linked List

### Idea

Duplicate the next node, and delete the next node. Made the current node point to the next next node.

### Solution

``` java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public void deleteNode(ListNode node) {
        node.val=node.next.val;
        node.next=node.next.next;
    }
}
```

## 21. Merge Two Sorted Lists

### Idea 

Compare each node of list1 with list2. if list1.val < list2.val, add the list1 current node into the new Linked List and make the list1 equals to list1.next. And then, if list1 or list2 point to the null, we can returen the corresponding list.

### Solution

#### 1. Recursion

``` java
class Solution{
  public ListNode mergeTwoLists(ListNode list1, ListNode list2){
    if(list1==null){return list2;}
    if(list2==null){return list1;}
    ListNode head=null;
    if(list1.val<list2.val){
      head=list1;
      list1=list1.next;
    }else{
      head=list2;
      list2=list2.next;
    }
    head.next=mergeTwoLists(list1,list2);
    return head;
  }
}
//Time Complexity O(m+n)
//Space Complexity O(m+n)
```



#### 2. Iteration

``` java
class Solution{
  public ListNode mergeTwoLists(ListNode list1,ListNode list2){
    ListNode head=new ListNode(-1);
    ListNode tail= head;
    while(list1!=null&&list2!=null){
      if(list1.val<list2.val){
        tail.next=list1;
        list1=list1.next;
      }else{
        tail.next=list2;
        list2=list2.next;
      }
      	tail=tail.next;
    }
     //one of list1&list2=nullif list1==null return list2 list2==null return list1
    tail.next= list1==null?list2:list1;
    return head.next;
  }
}
//Time complexity = O(m+n)
//Space complexity = O(1)
//The iterative approach only allocates a few pointers, so it has a constant overall memory footprint.
```

## 92. Reverse Linked List II

### Solution

#### 1. My Iteration Solution

``` java
class Solution{
  public ListNode reverseBetween(ListNode head, int left, int right){
    ListNode preLeft=new ListNode(-1);
    preLeft.next=head;
    ListNode leftHead=head;
    ListNode nextRight=head;
    ListNode rightTail=head;
    for(int i=1;i<left;i++){
      preLeft=preLeft.next;
    }
    leftHead=preLeft.next;
    for(int i=1;i<right;i++){
      rightTail=rightTail.next;
    }
    nextRight=rightTail.next;
    ListNode pre=nextRight;
    while(leftHead!=nextRight){
      ListNode nextNode= leftHead.next;
      leftHead.next=pre;
      pre=leftHead;
      leftHead=nextNode;
    }
    preLeft.next=pre;
    if(head.next==nextRight){//left pointer point the head
      return pre;
    }else{
      return head;
    }
  }
}
```

![image-20220105200914414](/Users/youhao/Library/Application Support/typora-user-images/image-20220105200914414.png)

#### 2. Recursion

``` java
class Solution {

    // Object level variables since we need the changes
    // to persist across recursive calls and Java is pass by value.
    private boolean stop;
    private ListNode left;

    public void recurseAndReverse(ListNode right, int m, int n) {

        // base case. Don't proceed any further
        if (n == 1) {
            return;
        }

        // Keep moving the right pointer one step forward until (n == 1)
        right = right.next;

        // Keep moving left pointer to the right until we reach the proper node
        // from where the reversal is to start.
        if (m > 1) {
            this.left = this.left.next;
        }

        // Recurse with m and n reduced.
        this.recurseAndReverse(right, m - 1, n - 1);

        // In case both the pointers cross each other or become equal, we
        // stop i.e. don't swap data any further. We are done reversing at this
        // point.
        if (this.left == right || right.next == this.left) {
            this.stop = true;            
        }

        // Until the boolean stop is false, swap data between the two pointers
        if (!this.stop) {
            int t = this.left.val;
            this.left.val = right.val;
            right.val = t;

            // Move left one step to the right.
            // The right pointer moves one step back via backtracking.
            this.left = this.left.next;
        }
    }

    public ListNode reverseBetween(ListNode head, int m, int n) {
        this.left = head;
        this.stop = false;
        this.recurseAndReverse(head, m, n);
        return head;
    }
}
```

![image-20220105211042659](/Users/youhao/Library/Application Support/typora-user-images/image-20220105211042659.png)

## 143. Reorder List

### Solution

#### Idea

![image-20220106220301926](/Users/youhao/Library/Application Support/typora-user-images/image-20220106220301926.png)

#### 1. Middle of the Linked List->Reverse Linked List-> Merge Two Sorted Lists

``` java
class Solution{
	public void reorderList(ListNode head){
    ListNode secondHalfList = findMidNode(head).next;
    ListNode firstTail=findMidNode(head);
    firstTail.next=null;
    secondHalfList=reverseList(secondHalfList);
    ListNode current=head;
    while(secondHalfList!=null){
      ListNode firstNext= current.next;
      ListNode secondNext= secondHalfList.next;
      current.next=secondHalfList;
      secondHalfList.next=firstNext;
      current=firstNext;
      secondHalfList=secondNext;
    }
    return;
  }
  public ListNode findMidNode(ListNode head){
    ListNode slow = head;
    ListNode fast = head;
    while(fast.next!=null&&fast.next.next!=null){
      fast=fast.next.next;
      slow=slow.next;
    }
    return slow;
  }
  public ListNode reverseList(ListNode head){
    ListNode pre=null;
    while(head!=null){
      ListNode nextHead=head.next;
      head.next=pre;
      pre=head;
      head=nextHead;
    }
    return pre;
  }
}
```

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

## Iterative Solution（DFS）

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

### Iterative Solution(BFS)

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

# Stack

## 20. Valid Parentheses(括号)

### 1. Solution 1

```java
class Solution{
  public boolean isValid(String s){
    Map<Character,Character> map= Map.of('(',')',
                                         '[',']',
                                         '{','}');
    Stack<Character> stack=new Stack<>();
    for(char c : s.toCharArray()){
      if(map.containsKey(c)){
        stack.push(c);
      }else{
        if(stack.isEmpty()){return false;}
        char open = stack.pop();
        if(map.get(open)!=c){
          return false;
        }
      }
    }
    return stack.isEmpty();
    //Time O(n)
    //Space O(n)
  }
}
```

### 2. Solution 2

```java
class Solution{
  public boolean isValid(String s){
    Stack<Character> stack=new Stack();
    for(char c: s.toCharArray()){
      if(c=='('){
        stack.push(')');
      }
      else if(c=='['){
        stack.push(']');
      }
      else if(c=='{'){
        stack.push('}');
      }else{
        if(stack.isEmpty()||stack.pop()!=c){
          return false;
        }
      }
    }
    return stack.isEmpty();
    //Time O(n)
    //Space O(n)
  }
}
```

![image-20220223161343235](/Users/youhao/Library/Application Support/typora-user-images/image-20220223161343235.png)

## 1047. Replace All Adjacent Duplicates in String

### 1. Stack Solution

```java
class Solution{
  public String removeDuplicates(String S){
    StringBuilder sb=new StringBuilder();
    for(char c: S.toCharArray()){
      int size=sb.length();
      if(size>0&&sb.charAt(size-1)==c){
        sb.deleteCharAt(size-1);
      }else{
        sb.append(c);
      }
    }
    return sb.toString();
    //Time O(n)
    //Space O(n)
  }
}
```

![image-20220223200944313](/Users/youhao/Library/Application Support/typora-user-images/image-20220223200944313.png)

### 2. Two Pointers 

```java
class Solution{
  public String removeDuplicates(String s){
    int i=0;
    int j=0;
    char[] res=s.toCharArray();
    for(i=0;i<res.length();i++){
      res[j]=res[i];
      if(j>0&&res[j]==res[j-1]){
        j-=2;
      }
      j++;
    }
    return new String(res,0,j);
  }
}
```

![image-20220223203128958](/Users/youhao/Library/Application Support/typora-user-images/image-20220223203128958.png)

## 232. Implement Queue using Stacks

### 1. Solution 1

```java
class MyQueue{
  Stack<Integer> s1=new Stack<>();
  Stack<Integer> s2=new Stack<>();
  public MyQueue(){
  
  }
  void push(int x){
    s1.push(x);
    //Time O(1)
    //Space O(n)
  }
  int pop(){
    if(!s2.isEmpty()){
      return s2.pop();
    }else{
      while(!s1.isEmpty()){
        s2.push(s1.pop());
      }
      return s2.pop();
    }
    //Time O(1), worst-case O(n)
    //Space O(1)
  }
  int peek(){
    if(!s2.isEmpty()){
      return s2.peek();
    }else{
      while(!s1.isEmpty()){
        s2.push(s1.pop());
      }
      return s2.peek();
    }
    //Time O(1)
    //Space O(1)
  }
  boolean empty(){
    return s1.isEmpty()&&s2.isEmpty();
    //Time O(1)
    //Space O(1)
  }
}
```

![image-20220224154408401](/Users/youhao/Library/Application Support/typora-user-images/image-20220224154408401.png)

### PoP(Time Analysis)

![image-20220224160214829](/Users/youhao/Library/Application Support/typora-user-images/image-20220224160214829.png)

## 155. Min Stack

### 1. Stack of Value/Minimum Pairs

```java
class MinStack{
  Stack<int[]> stack=new Stack<>();
  public MinStack(){
    
  }
  public void push(int val){
    if(stack.isEmpty()){
      stack.push(new int[]{val,val});
    }else{
      int min=stack.peek()[1];
      stack.push(new int[]{val,Math.min(val,min)});
    }
  }
  public void pop(){
    stack.pop();
  }
  public int top(){
    return stack.peek()[0];
  }
  public int getMin(){
    return stack.peek()[1];
  }
  //All operation Time complexity O(1);
  //Space O(n);
}
```

Idea

Push{val, min} into stack. if execute pop()function, min will also pop.

### 2. Double Stack

```java
class MinStack{
  Stack<Integer> stack1=new Stack<>();
  Stack<Integer> stack2=new Stack<>();
  public MinStack(){
    
  }
  public void push(int val){
    stack1.push(val);
    if(stack2.isEmpty()||stack2.peek()>=val){
      stack2.push(val);
    }
  }
  public void pop(){
    int val=stack1.pop();
    if(val==stack2.peek()){
      stack2.pop();
    }
  }
  public int top(){
    return stack1.peek();
  }
  public int getMin(){
    return stack2.peek();
  }
  // Time: O(1) for all operation
  // Space: O(n)
}
```

Idea

Use double stack. Stack1 store the value. Stack2 store the min. The principle of Stack2 is when val <= peek, we need to push val into stack2.



## 225. Implement Stack using Queues.

### 1. Using One Queue to solve the problem

```java
class MyStack {
    Queue<Integer> queue1=new LinkedList<>();
    public MyStack() {
        
    }
    
    public void push(int x) {
        queue1.add(x);
        int size= queue1.size();
        for(int i=size;i>1;i--){
            queue1.add(queue1.poll());
        }
        //Time O(n)
        //Space O(1)
    }
    
    public int pop() {
       return queue1.poll();   
        //Time O(1)
        //Space O(1)
    }
    
    public int top(){
        return queue1.peek();
        //Time O(1)
        //Space O(1)
    }
    public boolean empty() {
        return (queue1.isEmpty());
    }
}
```

![image-20220227182542377](/Users/youhao/Library/Application Support/typora-user-images/image-20220227182542377.png)

### 2. Using Two Queue 

```java
class MyStack {
    Queue<Integer> queue1=new LinkedList<>();
  	Queue<Integer> queue2=new LinkedList<>();
    public MyStack() {
        
    }
    
    public void push(int x) {
        queue2.add(x);
        while(!queue1.isEmpty()){
          queue2.add(queue1.poll());
        }
        Queue<Integer> tamp=new LinkedList<>();
      	tamp=queue1;
      	queue1=queue2;
      	queue2=tamp;
        //Time O(n)
        //Space O(1)
    }
    
    public int pop() {
       return queue1.poll();   
        //Time O(1)
        //Space O(1)
    }
    
    public int top(){
        return queue1.peek();
        //Time O(1)
        //Space O(1)
    }
    public boolean empty() {
        return (queue1.isEmpty());
    }
}
```

![image-20220227200029378](/Users/youhao/Library/Application Support/typora-user-images/image-20220227200029378.png)

## 1021. Remove Outermost Parenthese

### 1. No Stack Solution

```java
class Solution{
  public String removeOuterParentheses(String s){
    int counter=0;
    StringBuilder sb=new StringBuilder();
    for(char c: s.toCharArray()){
      if(c=='('){
        if(counter++>0){
          sb.append(c);
        }
      }else{
        if(--counter>0){
          sb.append(c);
        }
      }
    }
    return sb.toString();
  }
  //Time O(n)
  //Space O(n)
}
```

![image-20220228123413336](/Users/youhao/Library/Application Support/typora-user-images/image-20220228123413336.png)

### 2. One Stack Solution

```java
class Solution{
  public String removeOuterParentheses(String s){
    Stack<Character> stack=new Stack<>();
    StringBuilder sb=new StringBuilder();
    for(char c: s.toCharArray()){
      if(c=='('){
        if(stack.size()==0){
          stack.push(c);
        }else{
          sb.append(c);
          stack.push(c);
        }
      }else{
        if(stack.size()==1){
          stack.pop();
        }
        else if(stack.size()>1){
          stack.pop();
          sb.append(c);
        }
      }
    }
    return sb.toString();
    //Time O(n)
    //Space O(n)
  }
}
```

![image-20220228124032479](/Users/youhao/Library/Application Support/typora-user-images/image-20220228124032479.png)

# 682. Baseball Game

## 1. Stack 

```java
class Solution{
  public int calPoints(String[] ops){
    Stack<Integer> stack=new Stack<>();
    for(String c: ops){
      if(c.equals("C")){
        stack.pop();
      }
      else if(c.equals("D")){
        stack.push(2*stack.peek());
      }
      else if(c.equals("+")){
        int top=stack.pop();
        int newTop=top+stack.peek();
        stack.push(top);
        stack.push(newTop);
      }else{
        stack.push(Integer.valueOf(c));
      }
    }
    int total=0;
    while(!stack.isEmpty()){
      total+=stack.pop();
    }
    return total;
  }
}
```







## 2. Deque Solution

```java
class Solution{
  public int calPoints(String[] ops){
    Deque<Integer> stack=new ArrayDeque<>();
    for(String c: ops){
      if(c.equals("C")){
        stack.pop();
      }
      else if(c.equals("D")){
        stack.push(2*stack.peek());
      }
      else if(c.equals("+")){
        int top=stack.pop();
        int newTop=top+stack.peek();
        stack.push(top);
        stack.push(newTop);
      }else{
        stack.push(Integer.valueOf(c));
      }
    }
    int total=0;
    while(!stack.isEmpty()){
      total+=stack.pop();
    }
    return total;
  }
}
```

## 3. Deque VS Stack

https://blog.csdn.net/qq_44013629/article/details/106461200

https://blog.csdn.net/linysuccess/article/details/109038453?spm=1001.2101.3001.6650.13&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-13.queryctrv4&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-13.queryctrv4&utm_relevant_index=22

Java中[Stack](https://so.csdn.net/so/search?q=Stack&spm=1001.2101.3001.7020)类从Vector类继承，底层是用数组实现的**线程安全**的栈。栈是一种后进先出(LIFO)的容器，常用的操作`push/pop/peek`

不过Java中用来表达栈的功能（`push/pop/peek`），更适用的是使用双端队列[接口](https://so.csdn.net/so/search?q=接口&spm=1001.2101.3001.7020)Deque，并用实现类`ArrayDeque/LinkedList`来进行[初始化](https://so.csdn.net/so/search?q=初始化&spm=1001.2101.3001.7020)。

#### 1. 不用Stack至少有以下两点原因

1、从性能上来说应该使用Deque代替Stack。

Stack和Vector都是线程安全的，其实多数情况下并不需要做到线程安全，因此没有必要使用Stack。毕竟保证线程安全需要上锁，有额外的系统开销。

2、Stack从Vector继承是个历史遗留问题，JDK官方已建议优先使用Deque的实现类来代替Stack。

Stack从Vector继承的一个副作用是，暴露了set/get方法，可以进行随机位置的访问，这与Stack只能从尾巴上进行增减的本意相悖。

此外，Deque在转成ArrayList或者stream的时候保持了“后进先出”的语义，而Stack因为是从Vector继承，没有这个语义。

#### 2. Deque: ArrayList or LinkedList？

ArrayList: Store the data by Array

LinkedList: Store the data by LinkedList

Recommendation: ArrayList (more fast)

## 844. Backspace String Compare

### 1. Use Stack (Space Complexity O(M+N))

```java
class Solution{
  public boolean backspaceCompare(String s, String t){
    Deque<Character> stack1=new ArrayDeque<>();
    Deque<Character> stack2=new ArrayDeque<>();
    for(char c: s.toCharArray()){
      if(c=='#'&&!stack1.isEmpty()){
        stack1.pop();
      }else if(c=='#'&&stack1.isEmpty()){
          continue;
      }else{
        stack1.push(c);
      }
    }
    for(char c: t.toCharArray()){
      if(c=='#'&&!stack2.isEmpty()){
        stack2.pop();
      }else if(c=='#'&&stack2.isEmpty()){
          continue;
      }else{
        stack2.push(c);
      }
    }
    while(!stack1.isEmpty()&&!stack2.isEmpty()){
      if(stack1.pop()!=stack2.pop()){
        return false;
      }
    }
    return true&&stack1.isEmpty()&&stack2.isEmpty();
    //Time O(m+n)
    //Space O(m+n)
  }
}
```

### 2. Use Two Pointer(O(1))

```java
class Solution {
    public boolean backspaceCompare(String S, String T) {
        int i = S.length() - 1, j = T.length() - 1;
        int skipS = 0, skipT = 0;

        while (i >= 0 || j >= 0) { // While there may be chars in build(S) or build (T)
            while (i >= 0) { // Find position of next possible char in build(S)
                if (S.charAt(i) == '#') {skipS++; i--;}
                else if (skipS > 0) {skipS--; i--;}
                else break;
            }
            while (j >= 0) { // Find position of next possible char in build(T)
                if (T.charAt(j) == '#') {skipT++; j--;}
                else if (skipT > 0) {skipT--; j--;}
                else break;
            }
            // If two actual characters are different
            if (i >= 0 && j >= 0 && S.charAt(i) != T.charAt(j))
                return false;
            // If expecting to compare char vs nothing
            if ((i >= 0) != (j >= 0))
                return false;
            i--; j--;
        }
        return true;
    }
  //Time O(M+N)
  //Space O(1)
}
```

![image-20220302151529448](/Users/youhao/Library/Application Support/typora-user-images/image-20220302151529448.png)

# Monotonous Stack

## 496. Next Greater Element I

### 1. Better Brute Force O(m*n)

```java
class Solution{
  public int[] nextGreaterElement(int[] nums1,int[] nums2){
    HashMap<Integer,Integer> map=new HashMap<>();
    int[] res=new int[nums1.length];
    int max=0;
    for(int i=0;i<nums2.length;i++){
      map.put(nums2[i],i);
    }
    for(int i=0;i<nums1.length;i++){
      int j=map.get(nums1[i]);
      for(j=map.get(nums1[i])+1;j<nums2.length;j++){
        if(nums1[i]<nums2[j]){
          max=nums2[j];
          break;
        }else{
          max=-1;
        }
      }
     res[i]=max;
     if(map.get(nums1[i])==nums2.length-1){
        res[i]=-1;
     }
    }
    return res;
  }
}
```

### 2. Monotonous Stack

```java
class Solution{
  public int[] nextGreaterElement(int[] n1,int[] n2){
    Deque<Integer> stack=new ArrayDeque<>();
    HashMap<Integer,Integer> map=new HashMap<>();
    int[] res=new int[n1.length];
    for(int cur: n2){
      while(!stack.isEmpty()&&cur>stack.peek()){
        map.put(stack.pop(),cur);
      }
      stack.push(cur);
    }
    for(int i=0;i<n1.length;i++){
      if(map.containsKey(n1[i])){
        res[i]=map.get(n1[i]);
      }else{
        res[i]=-1;
      }
    }
    return res;
    //Time O(m+n+n)---> O(m+n)
    //Space O(n)
  }
}
```

![image-20220303164637253](/Users/youhao/Library/Application Support/typora-user-images/image-20220303164637253.png)

## 739. Daily Temperatures 

### 1. Monotonous Stack

```java
class Solution{
  public int[] dailyTemperatures(int[] temperatures){
    Deque<Integer> stack=new ArrayDeque<>();
    int[] res=new int[temperatures.length];
    for(int i=0;i<temperatures.length;i++){
      while(!stack.isEmpty()&&temperatures[i]>temperatures[stack.peek()]){
        res[stack.peek()]=i-stack.pop();
      }
      stack.push(i);
    }
    return res;
    //Time O(2*n) -> O(n)
    //Space O(n)
  }
}
```

### 2. Array, Optimized(使最优化) Space 

```java
class Solution{
  public int[] dailyTemperatures(int[] temperatures){
      int hottest=0;//if temperatures>hottest, hottest=temperatures, which means this temperature dont have the hotter number in next, because we loop from end to front.
      int n=temperatures.length;
      int[] res=new int[n];
      for(int i=n-1;i>=0;i--){
          int curT=temperatures[i];
          if(curT>=hottest){
              hottest=curT;
              continue;
          }
          int days=1;
          while(temperatures[i+days]<=curT){
              days+=res[i+days];
          }
          res[i]=days;
      }
      return res;
    //Time O(n)
    //Space O(1)
  }
}
```

![image-20220303212959244](/Users/youhao/Library/Application Support/typora-user-images/image-20220303212959244.png)

## 402. Remove K Digits

### 1. Greedy with Stack

```java
class Solution{
  public String removeKdigits(String num,int k){
    Deque<Character> stack=new ArrayDeque<>();
    StringBuilder sb=new StringBuilder();
    for(char c: num.toCharArray()){
      while(!stack.isEmpty()&&k>0&&stack.peek()>c){
        stack.pop();
        k--;
      }
      stack.push(c);
    }
    /* remove the remaining digits from the tail. */
    while(k>0){
      stack.pop();
      k--;
    }
    while(!stack.isEmpty()){
      sb.append(stack.pop());
    }
    // build the final string, while removing the leading zeros.
    for(int i=sb.length()-1;i>=0;i--){
      if(sb.charAt(i)=='0'){
        sb.deleteCharAt(i);
      }else{
        break;
      }
    }
    sb.reverse();
    //return the final string
    if(sb.length()==0) return "0";
    return sb.toString();
  }
  //Time O(n)
  //Space O(n)
}
```

![image-20220306113022500](/Users/youhao/Library/Application Support/typora-user-images/image-20220306113022500.png)

## 1124. Longest Well-Performing Interval

### 1. Using HashMap

```java
class Solution{
  public int longestWPI(int[] hours){
    HashMap<Integer,Integer> map=new HashMap<>();
    int s=0; //calculate the corresponding sum of each number of array
    int res=0; // calculate the result
    for(int i=0;i<hours.length;i++){
      s+=hours[i]>8?1:-1;
      if(s>0){
        res=i+1;
      }else{
        if(!map.containsKey(s)){
          map.put(s,i);
        }
        if(map.containsKey(s-1)){
          res=Math.max(res, i-map.get(s-1));
        }
      }
    }
    return res;
    //Time O(n)
    //Space O(n)
  }
}
```

![image-20220306202010127](/Users/youhao/Library/Application Support/typora-user-images/image-20220306202010127.png)

# Heap

## 1046. Last Stone Weight

### 1. Heap Solution

```java
class Solution{
  public int lastStoneWeight(int[] stones){
    PriorityQueue<Integer> heap=new PriorityQueue<>(Comparator.reverseOrder());
    //Store the data into heap: nlogn
    for(int s:stones){
      heap.add(s);
    }
    //n*2*logn
    while(heap.size()>1){
      int s1=heap.remove();
      int s2=heap.remove();
      if(s1!=s2){
        s1=s1-s2;
        heap.add(s1);
      }
    }
    //Time 3nlogn -> O(nlogn)
    //Space O(N): Create the PriorityQueue
    return heap.size()==1?heap.peek():0;
  }
}
```

## 2. Sorted Array-Based Simulation (模拟)

```java
class Solution{
  public int lastStoneWeight(int[] stones){
    List<Integer> res=new ArrayList<>();
    for(int s:stones){
      res.add(s);
    }
    Collections.sort(res);
    while(res.size()>1){
      int s1=res.remove(res.size()-1);
      int s2=res.remove(res.size()-1);
      if(s1!=s2){
        int newS=s1-s2;
        int index=Collections.binarySearch(res,newS);
        if(index<0){
          res.add(-index-1,newS);
        }else{
          res.add(index,newS);
        }
      }
    }
    return res.size()==1?res.remove(0):0;
  }
  //Time O(n^2)
  //Space O(n)
}
```

#### Collections

https://blog.51cto.com/u_14954398/2559204

If Collections.binarySearch(list, num) is not exist, just like {1,2,3,4}-> Collections.binarySearch(list, 5) -> return -4-1=-5 

## 703. Kth Largest Element in a Stream

### 1. Heap

```java
class KthLargest {
    PriorityQueue<Integer> heap=new PriorityQueue<>();
    int index=0;
    public KthLargest(int k, int[] nums) {
         for(int c:nums){
             heap.add(c);
         }
        index=k;
        while(heap.size()>index){
            heap.remove();
        }
    }
    
    public int add(int val) {
        heap.add(val);
        while(heap.size()>index){
            heap.remove();
        }
        return heap.peek();
    }
  //Space O(n)
  //Time O(nlogn+mlogk) m=the number of calls to add()
}
```

![image-20220307215240763](/Users/youhao/Library/Application Support/typora-user-images/image-20220307215240763.png)

### 2. ArrayList

```java
class KthLargest{
  List<Integer> res=new ArrayList<>();
  int key=0;
  public KthLargest(int k, int[] nums){
    for(int c: nums){
      res.add(c);
    }
    Collections.sort(res);
    key=k;
  } 
  public int add(int val){
    int index=Collections.binarySearch(res,val);
    if(index<0){
      res.add(-index-1,val);
    }else{
      res.add(index,val);
    }
    return res.get(res.size()-key);
    //Time O(n^2)
    //Space O(n)
  }
}
```

# BinarySearch

## 69. Sqrt(x)

### 1. Binary Search

```java
class Solution {
  public int mySqrt(int x) {
    if (x < 2) return x;

    long num;
    int pivot, left = 2, right = x / 2;
    while (left <= right) {
      pivot = left + (right - left) / 2;
      num = (long)pivot * pivot;
      if (num > x) right = pivot - 1;
      else if (num < x) left = pivot + 1;
      else return pivot;
    }

    return right;
  }
  //Time O(logn)
  //Space O(1)
}
```

Let's go back to the interview context. For x \ge 2*x*≥2 the square root is always smaller than x / 2*x*/2 and larger than 0 : 0 < a < x / 20<*a*<*x*/2.
Since a is an integer, the problem goes down to the iteration over the sorted set of integer numbers. Here the binary search enters the scene.

When the loop end, the left should >right,  no matter in the last loop this right=mid-1 or left=mid+1.

1) if the right=mid-1, and the loop end. which means the mid*mid >x, so the (mid-1) is the answer.
2) if the left=mid+1, which means mid*mid<x, right=mid, so the mid is the answer

Therefore, we should return right;

**Algorithm**

- If x < 2, return x.
- Set the left boundary to 2, and the right boundary to x / 2.
- While left <= right:
  - Take num = (left + right) / 2 as a guess. Compute num * num and compare it with x:
    - If num * num > x, move the right boundary right = pivot -1
    - Else, if num * num < x, move the left boundary left = pivot + 1
    - Otherwise num * num == x, the integer square root is here, let's return it
- Return right

### 2. Newton's Method

```java
class Solution {
  public int mySqrt(int x) {
    if(x<2)return x;
    double x0=x;
    double x1=(x1+x0/x1)/2;
    while(Math.abs(x1-x0)>=1){
      x0=x1;
      x1=(x0+x/x0)/2.0;
    }
    return (int)x1;
  }
  //Time O(logn)
  //Space O(1)
}
```

![image-20220309155027781](/Users/youhao/Library/Application Support/typora-user-images/image-20220309155027781.png)

## 704. Binary Search

### Binary Search

```java
class Solution{
  public int search(int[] nums, int target){
    int left=0;
    int right=nums.length-1;
    int mid=0;
    while(left<=right){
      mid=left+(right-left)/2;
      if(nums[mid]>target){
        right=mid-1;
      }
      else if(nums[mid]<target){
        left=mid+1;
      }
      else{
        return mid;
      }
    }
    return -1;
    //Time O(logN)
    //Space O(1)
  }
}
```

## 349. Intersection of Two Arrays

### 1. Two Sets (HashSet)

```java
class Solution{
  public int[] intersection(int[] nums1,int[] nums2){
    HashSet<Integer> s1=new HashSet<>();
    HashSet<Integer> s2=new HashSet<>();
    for(int c: nums1){
      s1.add(c);
    }
    for(int c: nums2){
      s2.add(c);
    }
    if(s1.size()>s2.size()){
      return help(s1,s2);
    }else{
      return help(s2,s1);
    }
  }
  public int[] help(HashSet<Integer> s1, HashSet<Integer> s2){
    int[] res=new int[s2.size()];
    int index=0;
    for(int c: s2){
      if(s1.contains(c)){
        res[index++]=c;
      }
    }
    return Arrays.copyOf(res,index);
  }
  //Time O(m+n)
  //Space O(m+n)
}
```

### 2. Built-in Set Intersection

```java
class Solution{
  public int[] intersection(int[] n1, int[] n2){
    HashSet<Integer> s1=new HashSet<>();
    HashSet<Integer> s2=new HashSet<>();
    for(int c:n1){
      s1.add(c);
    }
    for(int c:n2){
      s2.add(c);
    }
    s1.retainAll(s2);
    int[] res=new int[s1.size()];
    int index=0;
    for(int c: s1){
      res[index++]=c;
    }
    return res;
  }
  //Time O(m+n) Worst O(m*n)
  //Space O(m+n)
}
```

### Time Analysis

![image-20220313184100416](/Users/youhao/Library/Application Support/typora-user-images/image-20220313184100416.png)

### HashSet

https://cloud.tencent.com/developer/article/1415388

## 167. Two Sum II - Input Array Is Sorted

### 1. Two Pointer

```java
class Solution{
  public int[] twoSum(int[] n, int target){
    int i=0;
    int j=n.length-1;
    while(i<j){
      int sum=n[i]+n[j];
      if(sum>target){
        j--;
      }
      else if(sum<target){
        i++;
      }else{
        return new int[]{i+1,j+1};
      }
    }
    return new int[]{-1,-1};
    //Time O(n)
    //Space O(1)
  }
}
```

### 2. Binary Search

```java
class Solution{
  public int[] twoSum(int[] n,int target){
    for(int i=0;i<n.length;i++){
      int left=i+1;
      int right=n.length-1;
      while(left<=right){
        int mid=left+(right-left)/2;
        if(n[mid]+n[i]>target){
          right=mid-1;
        }else if(n[mid]+n[i]<target){
          left=mid+1;
        }else{
          return new int[] {i+1,mid+1};
        }
      }
    }
    return new int[] {-1,-1};
    //Time O(nlogn)
    //Space O(1)
  }
}
```

## 278. First Bad Version

### 1. Binary Search

```java
public class Solution extends VersionControl{
  public int firstBadVersion(int n){
    int left=1;
    int right=n;
    while(left<=right){
      int mid=left+(right-left)/2;
      if(isBadVersion(mid)){
        right=mid-1;
      }else{
        left=mid+1;
      }
    }
    return left;
    //Time O(logn)
    //Space O(1)
  }
}
```

## 34. Find First and Last Position of Element in Sorted Array

### 1. Binary Search

```java
class Solution{
  public int[] searchRange(int[] nums, int target){
    int left = 0, right = nums.length - 1;
    int[] res = new int[]{-1, -1};
    // Binary Search
    while (left <= right){
      // Find the mid index
      int mid = left + (right - left) / 2;
      int midN = nums[mid];
      if (midN == target){
        int f = mid - 1;//find the first target
        int l = mid + 1;// find the last target;
        while (f >= 0 && nums[f] == target){//f must >= 0
          f--;
        }
        while (l <= nums.length - 1 && nums[l] ==target){//l must <= nums.length - 1
          l++;
        }
        res[0] = f + 1;
        res[1] = l - 1;
        return res;
      }else if (midN > target){
        right = mid - 1;
      }else {
        left = mid + 1;
      }
    }
    // If don't find the target, return -1
    return res;
    // Time O(logn)
    // Space O(1)
  }
}
```

## 33. Search in Rotated Sorted Array

### 1. Binary Search

```java
class Solution {
  public int search(int[] nums, int target) {
    int start = 0, end = nums.length - 1;
    while (start <= end) {
      int mid = start + (end - start) / 2;
      if (nums[mid] == target) return mid;
      else if (nums[mid] >= nums[start]) {
        if (target >= nums[start] && target < nums[mid]) end = mid - 1;
        else start = mid + 1;
      }
      else {
        if (target <= nums[end] && target > nums[mid]) start = mid + 1;
        else end = mid - 1;
      }
    }
    return -1;
    //Time O(logn)
    //Space O(1)
  }
}
```

![image-20220327194802720](/Users/youhao/Library/Application Support/typora-user-images/image-20220327194802720.png)

## 74. Search a 2D Matrix

###   1. Binary Search

```java
class Solution{
  public boolean searchMatrix(int[][] matrix, int target){
    int l1 = 0, r1 = matrix.length - 1; // set two pointers to determine the range in which target is located
    while (l1 <= r1){
      int mid1 = l1 + (r1 - l1) / 2;
      int n2 = matrix[mid1].length - 1;
      if (target < matrix[mid1][0]){
        r1 = mid1 - 1;
      }
      else if (target > matrix[mid1][n2]){
        l1 = mid1 + 1;
      }
      else{
        int l2 = 0, r2 = matrix[mid1].length - 1;// set two pointers to deteremine the accurate location of target
        while (l2 <= r2){
          int mid2 = l2 + (r2 - l2) / 2;
          if (target < matrix[mid1][mid2]){
            r2 = mid2 - 1;
          }
          else if (target > matrix[mid1][mid2]){
            l2 = mid2 + 1;
          }else{
            return true;
          }
        }
        return false;
      }
    }
    return false;
    //Time O(log(m*n) log(m*n) = log(m) + log(n)
    //Space O(1)
  }
}
```

## 153. Find Minimum in Rotated Sorted Array

### 1. Binary Search

```java
class Solution{
  public int findMin(int[] nums) {
    int l = 0, r = nums.length - 1; // set two pointer 
    if (nums[l] <= nums[r]) return nums[l];
    while (l <= r){
      int mid = l + (r - l) / 2;
      if (nums[mid] > nums[mid + 1]){
        return nums[mid + 1];
      }
      if (nums[mid - 1] > nums[mid]){
        return nums[mid];
      }
      // if midN > nums[0],which means they are in the same list, and the minimum is located in another list.
      // make the l = mid + 1 so as to close to the minimum
      if (nums[mid] >= nums[0]){
        l = mid + 1;
      } else { // midN, nums[0] are not in the same order，make r = mid - 1 to close to the minimum
        r = mid - 1;
      }
    }
    return -1;
    //Time O(logn)
    //Space O(1)
  }
}
```

## 162. Find Peak Element

### 1. Binary Search

```java
class Solution{
  public int findPeakElement(int[] nums) {
    int l = 0, r = nums.length - 1;
    while (l + 1 < r) { // Make the left and right nodes adjacent numbers
      int mid = l + (r - l) / 2;
      if (nums[mid] > nums[mid + 1]){//l < r - 1, so mid < nums.length - 1
        r = mid; //descend list, find the left part which may have the increasing list
      }else{
        l = mid; // increasing list, find the right part which may have the descend list
      }
    }
    if (nums[l] > nums[r]){
      return l;
    }else {
      return r;
    }
    // Time O(logN)
    // Space O(1)
  }
}
```

![image-20220328132248105](/Users/youhao/Library/Application Support/typora-user-images/image-20220328132248105.png)

# Bit Operation

## 136. Single Number

### 1. HashSet

```java
class Solution{
  public int singleNumber(int[] nums){
    int sum=0,sumSet=0;
    Set<Integer> set=new HashSet<>();
    for(int c:nums){
      if(!set.contains(c)){
        set.add(c);
        sum+=c;
      }
      sumSet+=c;
    }
    return sum*2-sumSet;
    //Time O(n)
    //Space O(n)
  }
}
```

![image-20220317162611367](/Users/youhao/Library/Application Support/typora-user-images/image-20220317162611367.png)

### 2. List

```java
class Solution{
  public int singleNumber(int[] nums){
    List<Integer> res=new ArrayList<Integer>();
    for(int c: nums){
      if(!res.contains(c)){
        res.add(c);
      }else{
        res.remove(new Integer(c));
      }
    }
    return res.get(0);
    //Time O(1)
    //Space O(n)
  }
}
```

Be careful! If you want to remove the Integer "c", you need to be on this way: res.remove(new Integer(c));



### 3. Bit Operation

```java
class Solution{
  public int singleNumber(int[] nums){
    int a=0;
    for(int b: nums){
      a^=b;
    }
    return a;
    //Time O(n)
    //Space O(1)
  }
}
```

![image-20220317163415468](/Users/youhao/Library/Application Support/typora-user-images/image-20220317163415468.png)

## 191. Number of 1 Bits

### 1. Bits Operating 

``` java
class Solution{
  public int hammingWeight(int n){
    int mask=1;
    int bits=0;
    for(int i=0;i<32;i++){
      if((mask & n)!=0){
        bits++;
      }
      mask<<=1;
    }
    return bits;
    //Time O(1)
    //Space O(1)
  }
}
```

![image-20220318134158716](/Users/youhao/Library/Application Support/typora-user-images/image-20220318134158716.png)

### 2. Bit Manipulation Trick 

```java
class Solution{
  public int hammingWeight(int n){
    int sum=0;
    while(n!=0){
      sum++;
      n = & (n-1);
    }
    return sum;
    //Time O(1)
    //Space O(1)
  }
}
```

![image-20220318135839692](/Users/youhao/Library/Application Support/typora-user-images/image-20220318135839692.png)

## 169. Majority Element

## 1. Array Sort

```java
class Solution{
  public int majorityElement(int[] nums){
    Arrays.sort(nums);
    return nums[nums.length/2];
    //Time O(nlogn);
    //Space O(1);
  }
}
```

Time complexity of Arrays.sort() is O(nlogn) in Java and Python.

### 2. HashMap

```java
class Solution{
  public int majorityElement(int[] nums){
    HashMap<Integer, Integer> map = new HashMap<>();
    HashSet<Integer> set=new HashSet<>();
    for(int c : nums){
      if(map.containsKey(c)){
        int i = map.get(c);
        map.put(c, ++i);
      }else{
        map.put(c, 1);
      }
      set.add(c);
    }
    for(int c : set){
      if(map.get(c) > nums.length/2){
        return c;
      }
    }
    return -1;
    //Time O(n)
    //Space O(n)
  }
}
```

### 3. Randomization

```java
class Solution{
  public int randomIndex(Random random, int min, int max){
    return random.nextInt(max-min)+min;
  }
  public int numCount(int index, int[] nums){
    int c=0;
    for(int i=0; i < nums.length; i++){
      if(nums[i] == nums[index]){
        c++;
      }
    }
    return c;
  }
  public int majorityElement(int[] nums){
    Random random = new Random();
    int midCount = nums.length/2;
    while(true){
      int index = randomIndex(random, 0, nums.length);
      if(numCount(index, nums) > midCount){
        return nums[index];
      }
    }
    // Time O(∞)
    // Space O(1)
  }
}
```

![image-20220320160257920](/Users/youhao/Library/Application Support/typora-user-images/image-20220320160257920.png)

### 4. Boyer- Moore Voting 

```java
class Solution{
  public int majorityElement(int[] nums){
    int count = 0;
    Integer candidateNum = null;
    for(int c : nums){
      if(count == 0){
        candidateNum = c;
      }
      count += (c == candidateNum) ? 1 : -1;
    }
    return candidateNum;
  }
  // Time O(n);
  // Space O(1);
}
```

![image-20220320160044856](/Users/youhao/Library/Application Support/typora-user-images/image-20220320160044856.png)

## 190. Reverse Bits

### 1. Bit Operation

```java
class Solution{
  public int reverseBits(int n){
    int ans = 0;
    for(int i=0; i < 32; i++){
      ans = (ans << 2) | (n & 1);
      n >>= 1;
    }
    return ans;
    //Time O(logn)
    //Space O(1)
  }
}
```

![image-20220320175639083](/Users/youhao/Library/Application Support/typora-user-images/image-20220320175639083.png)

![image-20220320175920239](/Users/youhao/Library/Application Support/typora-user-images/image-20220320175920239.png)

**Using fixed fixed bits 32 bits to process the "0" at front bits. Therefore, we just need to loop 32 times**

### Bit Operation ( Base-10, Base-2)

#### 1. Base-10 Time Case

ans = ans * 10 + n % 10;

n / = 10;

#### 2. Base-2 Time Case (Postive Integers) 

ans = ans * 2 + n % 2;

n /= 2;

#### 3. Base-2 Time Case (Postive & Negative Integers)

ans = (ans << 1) | (n & 1);

n >>= 1;

## 231. Power of Two

### 1. Using Loop

```java
class Solution{
  public boolean isPowerOfTwo(int n){
    if(n <= 0){return false;}
    int count=0;
    for(int i = 0; i < 32; i++){
      if((n & 1) == 1){
        count++;
      }
      n >>= 1;
    }
    return (count > 1) ? false : true;
    //Time O(logn)
    //Space O(1)
  }
}
```

### 2. Bitwise Operators : Get the Rightmost 1-bit

```java
class Solution{
  public boolean isPowerOfTwo(int n){
    return (n > 0) && ((n * (-n)) == n);
    //Time O(1)
    //Space O(1)
  }
}
```

![image-20220320185209718](/Users/youhao/Library/Application Support/typora-user-images/image-20220320185209718.png)

### 3. Bitwise operators : Turn off the Rightmost 1-bit

```java
class Solution{
  public boolean isPowerOfTwo(int n){
    return (n > 0) && ((n & (n-1)) == 0);
    //Time O(1)
    //Space O(1)
  }
}
```

![image-20220320190011750](/Users/youhao/Library/Application Support/typora-user-images/image-20220320190011750.png)

## 389. Find the Difference

### 1. Sorting

```java
class Solution{
  public char findTheDifference(String s, String t){
    char[] sortedS = s.toCharArray();
    char[] sortedT = t.toCharArray();
    Arrays.sort(sortedS);
    Arrays.sort(sortedT);
    int i=0;
    for(i=0; i < s.length(); i++){
      if(sortedS[i] != sortedT[i]){
        return sortedT[i];
      }
    }
    return sortedT[i];
    //Time O(nlogn)
    //Space O(n) sort function will occupy O(n)
  }
}
```

![image-20220320201610850](/Users/youhao/Library/Application Support/typora-user-images/image-20220320201610850.png)

### 2. HashMap

```java
class Solution{
  public char findTheDifference(String s, String t){
    HashMap<Character,Integer> map = new HashMap<>();
    for(char c : s.toCharArray()){
      if(!map.containsKey(c)){
        map.put(c, 1);
      }else{
        int i = map.get(c);
        map.put(c, ++i);
      }
    }
    for(char c : t.toCharArray()){
      if(map.containsKey(c)){
        if(map.get(c) == 0){
          return c;
        }else{
          int i = map.get(c);
          map.put(c, --i);
        }
      }else{
        return c;
      }
    }
    return 0;
    //Time O(n)
    //Space O(1) Because the map space is fixed. We only have 26 lowercase letter. Therefore the HashMap space isn't relate with String's length;
  }
}
```

![image-20220320201620293](/Users/youhao/Library/Application Support/typora-user-images/image-20220320201620293.png)

### 3. Bits Manipulation

```java
class Solution{
  public char findTheDifference(String s, String t){
    char c = 0;
    for(char cur : s.toCharArray()){
      c ^= cur;
    }
    for(char cur : t.toCharArray()){
      c ^= cur;
    }
    return c;
    //Time O(n)
    //Space O(1)
  }
}
```

![image-20220320201631621](/Users/youhao/Library/Application Support/typora-user-images/image-20220320201631621.png)

## 461. Hamming Distance

### 1. Bit Shift

```java
class Solution{
  public int hammingDistance(int x, int y){
    int s = x ^ y;
    int count = 0;
    for(int i = 0; i < 32; i++){
      if((s & 1) == 1){
        count++;
      }
      s >>= 1;
    }
    return count;
    //Time O(1)
    //Space O(1)
  }
}
```

### 2. Brian Kernighan's Algorithm

```java
class Solution{
  public int hammingDistance(int x, int y){
    int xor = x ^ y;
    int count = 0;
    while(xor != 0){
      xor &= (xor - 1);
      count++;
    }
    return count;
    // Time O(1)
    // Space O(1)
  }
}
```

![image-20220321123433179](/Users/youhao/Library/Application Support/typora-user-images/image-20220321123433179.png)

## 405. Convert a Number to Hexadecimal

```java
class Solution{
  public String toHex(int num){
    if(num == 0) return "0";
    char[] map={'0','1','2','3','4','5','6','7',
                '8','9','a','b','c','d','e','f'};
    StringBuilder sb = new StringBuilder();
    while(num != 0){
      int hex = 15;
      hex &= num;
      sb.append(map[hex]);
      num >>>= 4;
    }
    return sb.reverse().toString();
    //Time O(1);
    //Space O(1);
  }
}
```

In java, >>>  is operated to unsigned Integer, >> is operated to signed Integer

### 268. Missing Number

### 1. Sorting

```java
class Solution{
  public int missingNumber(int[] nums){
    int n = nums.length;
    int[] numSorted = nums;
    Arrays.sort(numSorted);
    for(int i = 0; i < n+1; i++){
      if(i == n){
        return i;
      }else{
        if(numSorted[i] != i){
          return i;
        }
      }
    }
    return -1;
    //Time O(nlogn)
    //Space O(n)
  }
}
```

### 2. Bit Manipulation

```java
class Solution{
  public int missingNumber(int[] nums){
    int n = nums.length;
    int res = 0;
    int i = 0;
    for(i = 0; i < n; i++){
      res = res ^ i ^ nums[i];
    }
    return res ^ i;
    //Time O(n)
    //Space O(1)
  }
}
```

### 3. Gauss' Formula 

```java
class Solution{
  public int missingNumber(int[] nums){
    int n = nums.length;
    int sumN = n * (n+1) / 2;
    int sumNum = 0;
    for(int i = 0; i < n; i++){
      sumNum += nums[i];
    }
    return sumN - sumNum;
    //Time O(n)
    //Space O(1)
  }
}
```

![image-20220321144110903](/Users/youhao/Library/Application Support/typora-user-images/image-20220321144110903.png)



# Two Pointers and Sliding Window

## 713. Subarray Product Less Than K

### 1. Sliding Window

```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        if (k <= 1) return 0;
        int prod = 1, ans = 0, left = 0;
        for (int right = 0; right < nums.length; right++) {
            prod *= nums[right];
            while (prod >= k) prod /= nums[left++];
            ans += right - left + 1;
        }
        return ans;
      //Time O(n)
      // Space O(1)
    }
}
```

**Intuition**

For each `right`, call `opt(right)` the smallest `left` so that the product of the subarray `nums[left] * nums[left + 1] * ... * nums[right]` is less than `k`. `opt` is a monotone increasing function, so we can use a sliding window.

**Algorithm**

Our loop invariant is that `left` is the smallest value so that the product in the window `prod = nums[left] * nums[left + 1] * ... * nums[right]` is less than `k`.

For every right, we update `left` and `prod` to maintain this invariant. Then, the number of intervals with subarray product less than `k` and with right-most coordinate `right`, is `right - left + 1`. We'll count all of these for each value of `right`.

## 387. First Unique Character in a String

```java
class Solution{
  public int firstUniqChar(String s){
    HashMap<Character, Integer> map = new HashMap<>();
    int n = s.length();
    for(char c : s.toCharArray()){
      map.put(c, map.getOrDefault(c, 0)+1);
    }
    for(int i = 0; i < n; i++){
      char cur = s.charAt(i);
      if(map.get(cur) == 1){
        return i;
      }
    }
    return -1;
    //Time O(n)
    //Space O(1)
  }
}
```

## 3. Longest Substring Without Repeating Characters

### 1. Sliding Window

```java
class Solution{
  public int lengthOfLongestSubstring(String s){
    int left = 0, right = 0;
    int lg = 0; // largest lenght
    HashMap<Character, Integer> map = new HashMap<>();
    while(right < s.length()){
      char cur = s.charAt(right);
      map.put(cur, map.getOrDefault(cur, 0)+1);
      while(map.get(cur) > 1){
        char pre = s.charAt(left);
        map.put(pre, map.get(pre)-1);
        left++;
      }
      lg = Math.max(lg, right - left + 1);
      right++;
    }
    return lg;
    //Time O(2n)=O(n)
    //Space O(min(m,n))
  }
}
```

```java
class Solution{
  public int lengthOfLongestSubstring(String s){
    int left = 0, right = 0;
    int lg = 0;
    char[] charSet = new char[128];
    while(right < s.length()){
      char cur = s.charAt(right);
      charSet[cur]++;
      while(charSet[cur] > 1){
        char pre = s.charAt(left);
        charSet[pre]--;
        left++;
      }
      lg = Math.max(lg, right - left + 1);
      right++;
    }
    return lg;
    //Time O(2n) = O(n)
    //Space O(min(m, n)) charSet: m   s: n
  }
}
```

![image-20220322150943795](/Users/youhao/Library/Application Support/typora-user-images/image-20220322150943795.png)

### 2. Sliding Window Optimized

```java
class Solution{
  public int lengthOfLongestSubstring(String s){
    int ans = 0;
    HashMap<Character, Integer> map = new HashMap<>();
    for(int i = 0, j = 0; j < s.length(); j++){
      char cur = s.charAt(j);
      if(map.containsKey(cur)){
        i = Math.max(i, map.get(cur));
      }
      ans = Math.max(ans, j - i + 1);
      map.put(cur, j+1);
    }
    return ans;
    //Time O(n)
    //Space O(min(m,n))
  }
}
```

![image-20220322151423110](/Users/youhao/Library/Application Support/typora-user-images/image-20220322151423110.png)

## 438. Find All Anagrams in a String

### 1. Sliding Window with Array

```java
class Solution {
  public List<Integer> findAnagrams(String s, String p) {
    int ns = s.length(), np = p.length();
    if (ns < np) return new ArrayList();
    Map<Character, Integer> pCount = new HashMap();
    Map<Character, Integer> sCount = new HashMap();
    // build reference hashmap using string p
    for (char ch : p.toCharArray()) {
      if (pCount.containsKey(ch)) {
        pCount.put(ch, pCount.get(ch) + 1);
      }
      else {
        pCount.put(ch, 1);
      }
    }

    List<Integer> output = new ArrayList();
    // sliding window on the string s
    for (int i = 0; i < ns; ++i) {
      // add one more letter 
      // on the right side of the window
      char ch = s.charAt(i);
      if (sCount.containsKey(ch)) {
        sCount.put(ch, sCount.get(ch) + 1);
      }
      else {
        sCount.put(ch, 1);
      }
      // remove one letter 
      // from the left side of the window
      if (i >= np) {
        ch = s.charAt(i - np);
        if (sCount.get(ch) == 1) {
          sCount.remove(ch);
        }
        else {
          sCount.put(ch, sCount.get(ch) - 1);
        }
      }
      // compare hashmap in the sliding window
      // with the reference hashmap
      if (pCount.equals(sCount)) {
        output.add(i - np + 1);
      }
    }
    return output;
    //Time O(Ns) 
    //Space O(K) pCount and sCount will contain at most KK elements each.  Since KK is fixed at 2626 for this problem, this can be considered as O(1)O(1) space.
  }
}
```

### 2. Sliding Window with Array

```java
class Solution {
  public List<Integer> findAnagrams(String s, String p) {
    int ns = s.length(), np = p.length();
    if (ns < np) return new ArrayList();

    int [] pCount = new int[26];
    int [] sCount = new int[26];
    // build reference array using string p
    for (char ch : p.toCharArray()) {
      pCount[(int)(ch - 'a')]++;
    }

    List<Integer> output = new ArrayList();
    // sliding window on the string s
    for (int i = 0; i < ns; ++i) {
      // add one more letter 
      // on the right side of the window
      sCount[(int)(s.charAt(i) - 'a')]++;
      // remove one letter 
      // from the left side of the window
      if (i >= np) {
        sCount[(int)(s.charAt(i - np) - 'a')]--;
      }
      // compare array in the sliding window
      // with the reference array
      if (Arrays.equals(pCount, sCount)) {
        output.add(i - np + 1);
      }
    }
    return output;
    //Time O(Ns);
    //Space O(K)
  }
}
```

![image-20220322193914762](/Users/youhao/Library/Application Support/typora-user-images/image-20220322193914762.png)

![image-20220322193931136](/Users/youhao/Library/Application Support/typora-user-images/image-20220322193931136.png)

## 1004. Max Consecutive Ones III

### 1. Sliding Window

```java
class Solution{
  public int longestOnes(int[] nums, int k){
    int left = 0, right = 0;
    while(right < nums.length){
      if (nums[right] == 0) k--;
      if (k < 0){
        if (nums[left] == 0) k++;
        left++;
      }
      right++;
    }
    return right - left;
    // Time O(n)
    // Space O(1)
  }
}
```

Creating a sliding window. The criteria are: 

1. if k>=0, left pointer keep still, move the right pointer to next number.
2. if k<0, keep the window's space still. You need to move the left pointer to next number, when you move the right pointer. If left pointer point to '0', k++. If k > 0 again, it means that you can continue increasing your space of window.

## 567. Permutation in String

### 1. Sliding Window with HashMap

```java
class Solution{
  public boolean checkInclusion(String s1, String s2){
    int n1 = s1.length();
    int n2 = s2.length();
    // If s1 is longer than s2, s2 cannot exist the permutation of s1
    if (n1 > n2) return false;
    HashMap<Character, Integer> map1 = new HashMap<>();
    HashMap<Character, Integer> map2 = new HashMap<>();
    for(char c : s1.toCharArray()){
      map1.put(c, map1.getOrDefault(c, 0) + 1);
    }
    for (int i = 0, j = 0; j < n2; j++){
      char cur = s2.charAt(j);
      map2.put(cur, map2.getOrDefault(cur, 0) + 1);
      if (j >= n1){
        char pre = s2.charAt(i);
        if (map2.get(pre) == 1){
          map2.remove(pre);
        }else{
          map2.put(pre, map2.get(pre) - 1);
        }
        i++;
      }
      if (map1.equals(map2)){
        return true;
      }
    }
    return false;
    //Time O(Ns2)
    //Space O(K) K means the word that exist in s2, s1. K<=26
  }
}
```

### 2. Sliding Window with Arrays

```java
class Solution{
  public boolean checkInclusion(String s1, String s2){
    int n1 = s1.length();
    int n2 = s2.length();
    if (n1 > n2) return false;
    int[] a1 = new int[26];
    int[] a2 = new int[26];
    for (char c : s1.toCharArray()){
      a1[c - 'a']++;
    }
    for(int i = 0; j = 0; j < n2; j++){
      char cur = s2.charAt(j);
      a2[cur - 'a']++;
      if (j >= n1){
        char pre = s2.charAt(i);
        a2[pre - 'a']--;
        i++;
      }
      if (Arrays.equals(a1, a2)){
        return true;
      }
    }
    return false;
  }
}
```

The solution is same with<< 438. Find All Anagrams in a String>>



## 11. Container With Most Water

### 1. Two Pointer

```java
class Solution{
  public int maxArea(int[] height){
    int maxA = 0; 
    int left = 0, right = height.length - 1;// set two pointers to traversal.
    // The loop doesn't end until left == right
    while (left < right){
      // height must be chosed the smaller one, otherwise the water will overflow.
      // compare each square, store the bigger one
      maxA = Math.max(maxA, Math.min(height[left], height[right]) * (right - left));
      // find the taller height
      if (height[left] > height[right]){
        right--;
      }else{
        left++;
      }
    }
    return maxA;
    // Time O(n)
    // Space O(1)
  }
}
```

## 82. Remove Duplicates from Sorted List II

### 1. Sentinel Head + Predecessor (Two Pointers)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
  public ListNode deleteDuplicates(ListNode head) {
    if (head == null) return null;
    // set a sentinel node, which can solve the prolem that the first node is duplicates
    ListNode slow = new ListNode(0, head);
    ListNode fast = head;
    ListNode res = slow;
    while (fast != null) {
      // compare the value of slow.next and fast.next
      if (fast.next != null && fast.next.val == slow.next.val) {
        // Until the slow.next != fast.next, slow.next can point to the fast.next so as to remove the duplicates
        while (fast.next !=null && fast.next.val == slow.next.val) {
          fast = fast.next;
        }
        fast = fast.next;
        slow.next = fast;
      }else {
        slow = slow.next;
        fast = fast.next;
      }
    }
    return res.next;
    // Time O(n)
    // Space O(1)
  }
}
```

## 15. 3Sum

### 1. Two Pointers

```java
class Solution {
  public List<List<Integer>> threeSum(int[] nums) {
    int n = nums.length;
    List<List<Integer>> res = new ArrayList<>();
    Arrays.sort(nums);
    //The result must have 3 num, so we just loop to the last third number
    // only find nums[i] <= 0
    for (int i = 0; i < n - 2 && nums[i] <= 0; i++){
      // Only if i== 0 or nums[i] != nums[i-1] to prevent the duplicate triplets
      if (i == 0 || nums[i] != nums[i-1]) {
        // set two pointers
        int f = i + 1;
        int e = n - 1;
        while (f < e){
          int sum = nums[i] + nums[f] + nums[e];
          if (sum == 0){
            res.add(Arrays.asList(nums[i], nums[f++], nums[e--]));// after add the res, we need to continue finding the possible answer
            // if first node is equals to the last one, it may have the same result, so we need to avoid the duplicate triplets
            while(f < e && nums[f] == nums[f - 1]){
              f++;
            }
          }
          else if (sum > 0) {
            e--; // sum > 0, means nums[e] is bigger than what we want to find
          }else {
            f++; // sum < 0, means nums[f] is smaller than what we want to find
          }
        }
      }
    }
    return res;
    // Time O(n^2) --> nlogn + n*n
    // Space O(logN) to O(n), it depends on the sort algorithm
  }
}
```

### 2. HashSet

```java
class Solution {
  public List<List<Integer>> threeSum (int[] nums) {
    Arrays.sort(nums);
    int n = nums.length;
    List<List<Integer>> res = new ArrayList<>();
    for (int i = 0; i < n - 2 && nums[i] <= 0; i++) {
      if (i == 0 || nums[i] != nums[i - 1]) {
        Set<Integer> set = new HashSet<>();
        for (int j = i + 1; j < n; j++){
          int sum = - nums[i] - nums[j];
          // if exist the sum, which means the result is nums[j], nums[i], sum
          if (set.contains(sum)){
            res.add(Arrays.asList(nums[i], sum, nums[j]));
            //In order to avoid finding the duplicate triplets, we need to find: nums[j] != nums[j+1]
            //In the Iteration, the current j will add 1 to the next number and next number is not equal to the current nums[j]
            while (j + 1 < n && nums[j] == nums[j + 1]){
              j++;
            }
          }
          //store the nums[j] in order to find the result, when continue looping
          set.add(nums[j]);
        }
      }
    }
    return res;
    // Time O(n * n) nlogn + n * n;
    // Space O(n) sort is logn and Hashset is n, N > logN so the space --> O(n)
  }
}
```

## 986. Interval List Intersections

## 1. Merge two Arrays ( Two Pointers)

```java
class Solution {
  public int[][] intervalIntersection(int[][] A, int[][] B) {
    int i = 0;
    int j = 0; //set two pinters to loop the A, B arrays
    List<int[]> res = new ArrayList<>();
    while (i < A.length && j < B.length) {
      int lf = Math.max(A[i][0], B[j][0]);
      int le = Math.min(A[i][1], B[j][1]);
      // Find the intersection of A and B. The way is to find the max of first number, min of end number
      // lf <= le, which means they have intersections, else they don't have intersections
      if (lf <= le) {
        res.add(new int[]{lf, le});
      }
      // make the pointers move to next range
      if (le < B[j][1]){
        i++;
      }else {
        j++;
      }
    }
    return res.toArray(new int[res.size()][]);
    //Time O(M + N)
    //Space O(M + N)
  }
}
```



# Math

## 9. Palindrome Number

```java
public class Solution {
    public bool IsPalindrome(int x) {
        // Special cases:
        // As discussed above, when x < 0, x is not a palindrome.
        // Also if the last digit of the number is 0, in order to be a palindrome,
        // the first digit of the number also needs to be 0.
        // Only 0 satisfy this property.
        if(x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }

        int revertedNumber = 0;
        while(x > revertedNumber) {
            revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }

        // When the length is an odd number, we can get rid of the middle digit by revertedNumber/10
        // For example when the input is 12321, at the end of the while loop we get x = 12, revertedNumber = 123,
        // since the middle digit doesn't matter in palidrome(it will always equal to itself), we can simply get rid of it.
        return x == revertedNumber || x == revertedNumber/10;
    }
}
```

## 125. Valid Palindrome

### 1. Two Pointer

```java
class Solution{
  public boolean isPalindrome(String s){
    s = s.toLowerCase();
    int left = 0, right = s.length() - 1; // set two pointer
    // Iterate until left meet right
    while (left < right){
      //Make the left pointer point the lowercase letter or numbers
      while ((left < right) && (!Character.isLetterOrDigit(s.charAt(left)))){
        left++;
      }
      //Make the right pointer point the lowercase letter or numbers
      while ((left < right) && (!Character.isLetterOrDigit(s.charAt(right)))){
        right--;
      }
      //Judge the left pointer whether if is equal to right
      if (s.charAt(left) != s.charAt(right)){
        return false;
      }
      left++;
      right--;
    }
    return true;
    // Time O(n)
    // SPace O(1)
  }
}
```

