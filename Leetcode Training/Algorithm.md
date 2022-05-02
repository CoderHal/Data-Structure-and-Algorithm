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



## 160. Intersection of Two Linked List

### Solution

```java
public class Solution {
  public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    if (headA == null || headB == null) {
      return null;
    }
    ListNode currentA = headA;
    ListNode currentB = headB;
    while (currentA != currentB) {
      currentA = (currentA == null ? headB : currentA.next);
      currentB = (currentB == null ? headA : currentB.next);
    }
    return currentA;
    // Time O(N)
    // Space O(1)
  }
}
```

currentA and currentB are all loop for A + B times. If they have the intersection, they will meet at the process. If they have not the intersection, they all point to the null, currentA  == currentB. The result will return the null.

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

## 682. Baseball Game

### 1. Stack 

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

### 2. Deque Solution

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

### 3. Deque VS Stack

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

### 1. Sliding Window with Map

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

## 209. Minimum Size Subarray Sum

### 1. Two Pointer (Sliding Window) 

```java
class Solution {
  public int minSubArrayLen(int target, int[] nums) {
    int left = 0;
    int right = 0;
    int sum = 0;
    int res = Integer.MAX_VALUE;
    for(right = 0; right < nums.length; right++) {
      sum += nums[right];
      while (sum >= target && left <= right) {
        res = Math.min(res, right - left + 1);
        sum -= nums[left++];
      }
    }
    return res == Integer.MAX_VALUE ? 0 : res;
    //Time O(n)
    //Space O(1)
  }
}
```

## 633. Sum of Square Numbers

### 1. Two Pointers

```java
class Solution {
  public boolean judgeSquareSum(int c) {
    long i = 0;
    long j = (long) Math.sqrt(c);
    while (i <= j) {
      long sum = i * i + j * j;
      if (sum > c) {
        j--;
      }
      else if (sum < c) {
        i++;
      } else {
        return true;
      }
    }
    return false;
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



# Backtracking 

## Template of Backtracking

Backtrack( )

1. Base Case

2. For each possibility p

   ​	a. Memorize current state

   ​	b. Backtrack(next_state)

   ​	c. Restore current state

## 78. Subsets

### 1. Cascading

```java
class Solution {
  public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    res.add(new ArrayList<Integer>());
    for (int num : nums) {
      List<List<Integer>> newSubsets = new ArrayList<>();
      for (List<Integer> cur : res) {
        newSubsets.add(new ArrayList<Integer>(cur){{add(num)}}));
      }
      for (List<Integer> curr : newSubsets) {
        res.add(new ArrayList<>(curr));	
      }
    }
    return res;
    // Time O(n * 2^n)
    // Space O(n * 2^n)
  }
}
```

![image-20220411181205504](/Users/youhao/Library/Application Support/typora-user-images/image-20220411181205504.png)

### 2. Backtracking

```java
class Solution {
  public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();
    backtrack(nums, res, temp, 0);
    return res;
  }
  public void backtrack(int[] nums, List<List<Integer>> res, List<Integer> temp, int index) {
    res.add(new ArrayList<>(temp));
    for (int i = index; i < nums.length; i++) {
      temp.add(nums[i]);
      backtrack(nums, res, temp, i + 1);
      temp.remove(temp.size() - 1);
    }
    //Time O(n * 2^n);
    //Space O(n) We are using O(N) space to maintain curr, and are modifying curr in-place with backtracking. Note that for space complexity analysis, we do not count space that is only used for the purpose of returning output, so the output array is ignored.
  }
}
```

![image-20220411181911963](/Users/youhao/Library/Application Support/typora-user-images/image-20220411181911963.png)

![image-20220411181925632](/Users/youhao/Library/Application Support/typora-user-images/image-20220411181925632.png)



## 90. Subsets II

### 1. Cascading

```java
class Solution {
  public List<List<Integer>> subsetsWithDup(int[] nums) {
    Arrays.sort(nums); 
    List<List<Integer>> res = new ArrayList<>();
    res.add(new ArrayList<Integer>());
    int subsetsSize = 0;
    for (int i = 0; i < nums.length; i++) {
      int startIndex = (i > 0 && nums[i] == nums[i - 1]) ? subsetsSize : 0; // record the pre size of res
      subsetSize = res.size();
      for (int j = startIndex; j < subsetSize; j++) {
        List<Integer> path = new ArrayList<>(res.get(j));
        path.add(nums[i]);
        res.add(path);
      }  
    }
    return res;
    // Time O(n * 2^n) // n * 2^n + nlogn
    // Space O(logn) // Sort logN
  }
}
```

The space complexity of the sorting algorithm depends on the implementation of each programming language. For instance, in Java, the Arrays.sort() for primitives is implemented as a variant of quicksort algorithm whose space complexity is O*(log*n*). 

### 2. Backtracking

```java
class Solution {
  public List<List<Integer>> subsetsWithDup(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> res = new ArrayList<>();
    backtracking(nums, res, new ArrayList<Integer>(), 0);
    return res;
  }
  public void backtracking(int[] nums, List<List<Integer>> res, List<Integer> path, int index) {
    res.add(new ArrayList<Integer>(path));
    for (int i = index; i < nums.length; i++) {
      if (i != index && nums[i] == nums[i - 1]) {
        continue;
      }
      path.add(nums[i]);
      backtracking(nums, res, path, i + 1);
      path.remove(path.size() - 1);
    }
    //Time O(n * 2^n)
    //Space O(n)
  }
}
```

The space complexity of the sorting algorithm depends on the implementation of each programming language. For instance, in Java, the Arrays.sort() for primitives is implemented as a variant of quicksort algorithm whose space complexity is O*(log*n*). 



## 46. Permutations

### 1. Backtracking

```java
class Solution {
  public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    Arrays.sort(nums);
    backtracking(nums, res, 0);
    return res;
  }
  public void backtracking(int[] nums, List<List<Integer>> res, int index) {
    if (index == nums.length) {
      List<Integer> permutation = new ArrayList<>();
      for (int c : nums) {
        permutation.add(c);
      }
      res.add(new ArrayList<>(permutation));
      return;
    }
    for (int i = index; i < nums.length; i++) {
      swap(nums, index, i);
      backtracking(nums, res, index + 1);
      swap(nums, index, i);
    }
  }
  public void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
  }
  // Time O(n * n!)
  // Space O(n * n!)
}
```

index = 0， it has n possibilities. And then, based on the specific permutation, index move to next index.

index = index + 1, the previous index has been fixed, so you need to focus on the follow number and find all possibilities at this index.

The recursion will stop until the index beyond the length

In each recursion, after swap the index, we need to swap again in order to keep the order original.

![image-20220413210829540](/Users/youhao/Library/Application Support/typora-user-images/image-20220413210829540.png)



## 47. Permutations II

### 1. Backtracking

```java
class Solution {
  public List<List<Integer>> permuteUnique(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(nums, res, 0);
    return res;
  }
  public void backtrack(int[] nums, List<List<Integer>> res, int index) {
    if (index == nums.length) {
      List<Integer> permutation = new ArrayList<>();
      for (int c : nums) {
        permutation.add(c);
      }
      res.add(new ArrayList<>(permutation));
    }
    HashSet<Integer> set = new HashSet<>();
    for (int i = index; i < nums.length; i++) {
      swap(nums, index, i);
      if (set.add(nums[index])) {
        backtrack(nums, res, index + 1);
      }
      swap(nums, index, i);
    }
  }
  public void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
    //Time O(n * n!)
    // Space O(n * n!)
  }
}
```

When  we find all possiblities in  i = [(index), n),  we can use hashset to check out the same element whether if it has been used for many times. If we find the same element, we neet to skip current process.

![image-20220413213242044](/Users/youhao/Library/Application Support/typora-user-images/image-20220413213242044.png)

## 39. Combination Sum

### 1. Backtraking

```java
class Solution {
  public List<List<Integer>> combinationSum(int[] candidates, int target) {
    Arrays.sort(candidates);
    List<List<Integer>> res = new ArrayList<>();
    backtracking(candidates, res, new ArrayList<>(), 0, target);
    return res;
  }
  public void backtracking(int[] candidates, List<List<Integer>> res, List<Integer> path, int index, int remain) {
    if (remain == 0) {
      res.add(new ArrayList<>(path));
      return;
    }
    else if (index == candidates.length || remain < 0) {
      return;
    } else {
      for (int i = index; i < candidates.length; i++) {
        path.add(candidates[i]);
        backtracking(candidates, res, path, i, remain - candidates[i]);
        path.remove(path.size() - 1);
      }
    }
  }
}
```

![image-20220414125526451](/Users/youhao/Library/Application Support/typora-user-images/image-20220414125526451.png)

Time Complexity :

![image-20220414130615042](/Users/youhao/Library/Application Support/typora-user-images/image-20220414130615042.png)

Space Complexity : 

![image-20220414130644749](/Users/youhao/Library/Application Support/typora-user-images/image-20220414130644749.png)

## 40. Combination Sum II

### 1. Backtracking

```java
class Solution {
  public List<List<Integer>> combinationSum2(int[] candidates, int target) {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    Arrays.sort(candidates);
    backtracking(candidates, res, path, 0, target);
    return res;
  }
  public void backtracking(int[] candidates, List<List<Integer>> res, List<Integer> path, int index, int remain) {
    if (remain == 0) {
      res.add(new ArrayList<Integer>(path));
      return;
    }
    else if (remain < 0 || index == candidates.length) {
      return;
    }
    else {
      for (int i = index; i < candidates.length; i++) {
        if (i != index && candidates[i] == candidates[i - 1]) {
          continue;
        }
        path.add(candidates[i]);
        backtracking(candidates, res, path, i + 1, remain - candidates[i]);
        path.remove(path.size() - 1);
      }
    }
		// Time O(2^n) == 2^n + nlogn
    // Space O(N) 
  }
}
```

![image-20220415150635290](/Users/youhao/Library/Application Support/typora-user-images/image-20220415150635290.png)

![image-20220415150652691](/Users/youhao/Library/Application Support/typora-user-images/image-20220415150652691.png)

## 17. Letter Combinations of a Phone Number

### 1. Backtracking 

```java 
class Solution {
  public List<String> letterCombinations(String digits) {
    Map<Character, String> map = Map.of('2', "abc", '3', "def", '4', "ghi",
                                        '5', "jkl", '6', "mno", '7', "pqrs",
                                        '8', "tuv", '9', "wxyz");
    List<String> res = new ArrayList<>();
    if (digits.length() == 0) {
      return res;	
    }
    StringBuilder sb = new StringBuilder();
    backtrack(digits, res, sb, 0, map);
    return res;
  }
  public void backtrack(String digits, List<String> res, StringBuilder sb, int index, Map<Character, String> map) {
    if (sb.length() == digits.length()) {
      res.add(sb.toString());
      return;
    }
    String letter = map.get(digits.charAt(index));
    for (char c : letter.toCharArray()) {
      sb.append(c);
      backtrack(digits, res, sb, index + 1, map);
      sb.deleteCharAt(sb.length() - 1);
    }
    // Time O(4^N*N) 
    // Space O(n)
  }
}
```

![image-20220419135925097](/Users/youhao/Library/Application Support/typora-user-images/image-20220419135925097.png)

## 22. Generate Parentheses

### 1. Backtracking

```java
class Solution {
  public List<String> generateParenthesis(int n){
    List<String> res = new ArrayList<>();
    if (n == 0) {
      return res;
    }
    StringBuilder sb = new StringBuilder();
    backtrack(res, sb, n, n);
    return res;
  }
  public void backtrack(List<String> res, StringBuilder sb, int left, int right) {
    if (left == 0 && right == 0) {
      res.add(sb);
      return;
    }
    if (left > 0) {
      sb.append('(');
      backtrack(res, sb, left - 1, right);
      sb.deleteCharAt(sb.length() - 1);
    }
    if (right > left && right > 0) {
      sb.append(')');
      backtrack(res, sb, left, right - 1);
      sb.deleteCharAt(sb.length() - 1);
    }
  }
}
```

In the recursion solution, the first thing is add a '(' into sb, and the number of '(' will minus 1. Based on this, we can backtrack this method, only if the number of '(' is bigger than the number of ')', we can add the ')' in to sb. When the number of '(' and ')' are all equal to 0, we can add the result into res.

![image-20220419153558277](/Users/youhao/Library/Application Support/typora-user-images/image-20220419153558277.png)

## 79. Word Search 

### 1. Backtracking

```java
class Solution {
  boolean[][] visited;
  public boolean exist(char[][] board, String word) {
    int n = board.length;
    int m = board[0].length;
    visited = new boolean[n][m];
    for (int i = 0; i < n; i++) {
      for(int j = 0; j < m; j++) {
        if (board[i][j] == word.charAt(0) && searchWord(i, j, board, word, 0)) {
          return true;
        }
      }
    }
    return false;
  }
  public boolean searchWord(int i, int j, char[][] board, String word, int index) {
    if (index == word.length()) {
      return true;
    }
    if (i < 0 || i >= board.length || j < 0 || j >= board[0].length || visited[i][j] == true || word.charAt(index) != board[i][j]) {
      return false;
    }
    visited[i][j] = true;
    if (searchWord(i + 1, j, board, word, index + 1) ||
       	searchWord(i - 1, j, board, word, index + 1) ||
       	searchWord(i, j + 1, board, word, index + 1) ||
       	searchWord(i, j - 1, board, word, index + 1)) {
      return true;
    }
    visited[i][j] = false;
    return false;
  }
  //Time O(N * 3 ^ l) l : the length of word
  // Space O(L)
}
```



## 77. Combinations

### 1. Backtracking

```java
class Solution {
  public List<List<Integer>> combine(int n, int k) {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    combination(res, path, n, k, 1);
    return res;
  }
  public void combination(List<List<Integer>> res, List<Integer> path, int n, int k, int index) {
    if (path.size() == k) {
      res.add(new ArrayList<>(path));
      return;
    }
    for (int i = index; i <= n; i++) {
      path.add(i);
      combination(res, path, n, k, i + 1);
      path.remove(path.size() - 1);
    }
  }
}
```

![image-20220420151107219](/Users/youhao/Library/Application Support/typora-user-images/image-20220420151107219.png)

![image-20220420151919307](/Users/youhao/Library/Application Support/typora-user-images/image-20220420151919307.png)



## 93. Restore IP Address

### 1. Backtracking

```java
class Solution {
  public List<String> restoreIpAddresses(String s) {
    List<String> res = new ArrayList<>();
    storeIp(s, res, 4, 0, "");
    return res;
  }
  public void storeIp(String s, List<String> res, int remain, int index, String sb) {
    if (remain == 0) {
      if (sb.size() == s.length()) {
        res.add(sb);
      }
    }
    for (int i = 1; i <= 3; i++) {
      if (i + index > s.length()) break;
      if (i != 1 && sb.charAt(index) == '0') break;
      String cur = s.subString(index, index + i);// starting index is inclusive, ending index is exclusive
      int curN = Integer.valueOf(cur);
      if (curN <= 255) {
        storeIp(s, res, remain - 1, index + i, sb + cur + (remain == 1 ? "" : "."));
      }
    }
  }
}
```

Time Complexity :  27

![image-20220421135350778](/Users/youhao/Library/Application Support/typora-user-images/image-20220421135350778.png)

Space: 19

![image-20220421135656446](/Users/youhao/Library/Application Support/typora-user-images/image-20220421135656446.png)

# Matrix

## 48. Rotate Image

### 1. Rotate only 1/4

```java
class Solution {
  public void rotate(int[][] matrix) {
    int n = matrix.length;
    for (int i = 0; i < (n + 1) / 2; i++) {
      for (int j = 0; j < n / 2; j++) {
        int temp = matrix[j][n - i - 1];
        matrix[j][n - i - 1] = matrix[i][j];
        matrix[i][j] = matrix[n - 1 -j][i];
        matrix[n - 1 - j][i] = matrix[n - i - 1][n - 1 -j];
        matrix[n - i - 1][n - 1 - j] = temp;
      }
    }
    // Time O(M) n * n
    // Space O(1)
  }
}
```

![image-20220421144647277](/Users/youhao/Library/Application Support/typora-user-images/image-20220421144647277.png)

### 2. Reverse on Diagonal and then Reverse Left to Right

Rotate = Transposed(Reverse on Diagonal) + Reversed

```java
class Solution {
  public void rotate(int[][] matrix) {
    int n = matrix.length;
    // Transposed (Rverse on Diagonal)
    for (int i = 0; i < n; i++) {
      for (int j = i; j < n; j++) {
        int temp = matrix[i][j];
        matrix[i][j] = matrix[j][i];
        matrix[j][i] = temp;
      }
    }
    // Reversed 
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < n / 2; j++) {
        int temp = matrix[i][j];
        matrix[i][j] = matrix[i][n - 1 - j];
        matrix[i][n - 1 - j] = temp;
      }
    }
    // Time O(M); 
    // Space O(1)
  }
}
```

![image-20220421145052810](/Users/youhao/Library/Application Support/typora-user-images/image-20220421145052810.png)



## 54. Spiral Matrix

### 1. Using direction flag

```java
class Solution {
  public List<Integer> spiralOrder(int[][] matrix) {
    int n = matrix.length;
    int m = matrix[0].length;
    List<Integer> res = new ArrayList<>();
    int row = 0, col = 0, up = 0, down = 0, left = 0. right = 1;
    while (res.size() != n * m) {
      if (matrix[row][col] != -101){
        res.add(matrix[row][col]);
        matrix[row][col] = -101;
      }
      if (right == 1) {
        if(col < m && col + 1 < m && matrix[row][col + 1] != -101){
          col++;
        }else {
          down = 1;
          right = 0;
        }
      }
      if (down == 1) {
        if(row < n && row + 1 < n && matrix[row + 1][col] != -101){
          row++;
        }else {
          left = 1;
          down = 0;
        }
      }
      if (left == 1) {
        if(col >= 0 && col - 1 >= 0 && matrix[row][col - 1] != -101){
          col--;
        }else {
          up = 1;
          left = 0;
        }
      }
      if (up == 1) {
        if(row >= 0 && row - 1 >= 0 && matrix[row - 1][col] != -101){
          row--;
        }else {
          right = 1;
          up = 0;
        }
      }
    }
    return res;
    // Time O(M) m * n
    // Space O(1)
  }
}
```

![image-20220421160609071](/Users/youhao/Library/Application Support/typora-user-images/image-20220421160609071.png)

## 36. Valid Sudoku

### 1. HashSet

```java
class Solution {
  public boolean isValidSudoku(char[][] board) {
    int n = 9;
    Set<Character>[] row = new HashSet[n];
    Set<Character>[] col = new HashSet[n];
    Set<Character>[] box = new HashSet[n];
    for (int i = 0; i < n; i++) {
      row[i] = new HashSet<Character>();
      col[i] = new HashSet<Character>();
      box[i] = new HashSet<Character>();
    }
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < n; j++) {
        if (board[i][j] == '.') {
          continue;
        }
    		if (!row[i].add(board[i][j])) {
          return false;
        }
        if (!col[j].add(board[i][j])) {
          return false;
        }
        if (!box[(i / 3) * 3 + j / 3].add(board[i][j])){
          return false;
        }
      }
    }
    return true;
  }
}
```

![image-20220421172929429](/Users/youhao/Library/Application Support/typora-user-images/image-20220421172929429.png)

###  2. Array of Fixed Length

```java
class Solution {
  public boolean isValidSudoku(char[][] board) {
    int n = 9;
    int[][] row = new int[n][n];
    int[][] col = new int[n][n];
    int[][] box = new int[n][n];
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < n; j++) {
        if (board[i][j] == '.') {
          continue;
        }
        int cur = board[i][j] - '1';
        if (row[i][cur] == 1) {
          return false;
        }
        row[i][cur] = 1;
        if (col[j][cur] == 1) {
          return false;
        }
        col[j][cur] = 1;
        if (box[(i / 3) * 3 + (j / 3)][cur] == 1) {
          return false;
        }
        box[(i / 3) * 3 + (j / 3)][cur] = 1;
      }
    }
    return true;
  }
}
```

![image-20220421190517796](/Users/youhao/Library/Application Support/typora-user-images/image-20220421190517796.png)

### 3. BitMasking 

```java
class Solution {
  public boolean isValidSudoku(char[][] board) {
    int n = board.length;
    int[] row = new int[n];
    int[] col = new int[n];
    int[] box = new int[n];
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < n; j++) {
        if (board[i][j] == '.') {
          continue;
        }
        int cur = Integer.valueOf(board[i][j]);
        int cel = 1 << (cur - 1);
        if ((row[i] & cel) > 0) {
          return false;
        }
        row[i] |= cel;
        if ((col[j] & cel) > 0) {
          return false;
        }
        col[j] |= cel;
        if ((box[(i / 3) * 3 + (j / 3)] & cel) > 0) {
          return false;
        } 
        box[(i / 3) * 3 + (j / 3)] |= cel;
      }
    }
    return true;
  }
}
```

![image-20220421192136536](/Users/youhao/Library/Application Support/typora-user-images/image-20220421192136536.png)

## 59. Spiral MatrixII

```java
class Solution {
  public int[][] generateMatrix(int n) {
    int[][] res = new int[n][n];
    int step = 1;
    int right = 1, left = 0, up = 0, down = 0;
    int col = 0, row = 0;
    int change = 0;
    while(step <= n * n) {
      res[row][col] = step;
      if (step == n * n) return res;
      if (right == 1) {
        if (col < n && col + 1 < n && res[row][col + 1] == 0) {
          col++;
          step++;
        }
        else {
          down = 1;
          right = 0;
        }
      }
      if (down == 1) {
        if (row < n && row + 1 < n && res[row + 1][col] == 0) {
          row++;
          step++;
        }
        else {
          down = 0;
          left = 1;
        }
      }
      if (left == 1) {
        if (col >= 0 && col - 1 >= 0 && res[row][col - 1] == 0) {
          col--;
          step++;
        }
        else {
          left = 0;
          up = 1;
        }
      }
      if (up == 1) {
        if (row >= 0 && row - 1 >= 0 && res[row - 1][col] == 0) {
          row--;
          step++;
        }
        else {
          up = 0;
          right = 1;
        }
      }
    }
      return res;
  }
}
```



# Arrays

## 56. Merge Intervals

```java
class Solution {
  public int[][] merge(int[][] intervals) {
    if (intervals.length <= 1) {
      return intervals;
    }
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    List<int[]> output = new ArrayList<>();
    int[] cur = intervals[0];
    output.add(cur);
    for (int[] inter : intervals) {
      int curF = cur[0];
      int curE = cur[1];
      int nextF = inter[0];
      int nextE = inter[1];
      if (curE >= nextF) {
        cur[1] = Math.max(curE, nextE);//while cur changed, the List will changed
      } else {
        cur = inter;
        output.add(cur);
      }
    }
    return output.toArray(new int[output.size()][]);
    // Time O(nlog + n) --> n: the size of intervals
    // Space O(logn)
  }
}
```

## 560. Subarray Sum Equals K

### 1. HashMap

```java
class Solution {
  public int subarraySum(int[] nums, int k) {
    int sum = 0;
    int count = 0;
    Map<Integer, Integer> map = new HashMap<>();
    map.put(0, 1);//we need to record the first value as 0
    for (int i = 0; i < nums.length; i++) {
      sum += nums[i];
      if (map.containsKey(sum - k)) {
        count += map.get(sum - k);
      }
      map.put(sum, map.getOrDefault(sum, 0) + 1);
    }
    return count;
  }
  // Time O(n)
  // Space O(n)
}
```

![image-20220423164459752](/Users/youhao/Library/Application Support/typora-user-images/image-20220423164459752.png)

## 121. Best Time to Buy and Sell Stock

### 1. Find the largest Peak and smallest valley

```java
class Solution {
  public int maxProfit(int prices[]) {
    int maxProfit = 0;
    int minPrice = Integer.MAX_VALUE;
    for (int i = 0; i < prices.length; i++) {
      if (prices[i] < minPrice) {// if this index of prices is the minPrice, we just make this index as the smallest value, and then jump to next index.
        // we don't need to figure out the maxProfits
        minPrice = prices[i];
      }
      else if(maxProfit < prices[i] - minPrice) {// if this index is not the smallest price, we need to figure out the current profit
        maxProfit = prices[i] - minPrice;
      }
    }
    return maxProfit;
  }
}
```

![image-20220423171400798](/Users/youhao/Library/Application Support/typora-user-images/image-20220423171400798.png)

## 215. Kth Largest Element in an Array

### 1. Heap

```java
class Solution {
  public int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> heap = new PriorityQueue<Integer> ((a, b) -> a - b);
    for (int num : nums) {
      heap.add(num);
      if (heap.size() > k) {
        heap.poll();
      }
    }
    return heap.peek();
    // Time O(Nlogk) logk is the time of adding an element in a heap
    // Space O(k)
  }
}
```

![image-20220423193900143](/Users/youhao/Library/Application Support/typora-user-images/image-20220423193900143.png)

### 2. Quickselect

```java
public class Solution {
    public int findKthLargest(int[] nums, int k) {
        int start = 0, end = nums.length - 1, index = nums.length - k;
        while (true) {
            int pivot = partion(nums, start, end);
            if (pivot < index) start = pivot + 1; 
            else if (pivot > index) end = pivot - 1;
            else return nums[pivot];
        }
        return nums[start];
    }
    
    private int partion(int[] nums, int start, int end) {
        int pivot = start, temp;
        while (start <= end) {
            while (start <= end && nums[start] <= nums[pivot]) start++;
            while (start <= end && nums[end] > nums[pivot]) end--;
            if (start > end) break;
            temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
        }
        temp = nums[end];
        nums[end] = nums[pivot];
        nums[pivot] = temp;
        return end;
    }
  //Time O(n)
  //Space O(1)
}
```

<img src="/Users/youhao/Library/Application Support/typora-user-images/image-20220423211434231.png" alt="image-20220423211434231" style="zoom:50%;" />

## 811. Subdomain Visit Count

### 1. HashMap

```java
class Solution {
  public List<String> subdomainVisits(String[] cpdomains) {
    List<String> res = new ArrayList<>();
    if (cpdomains.length == 0) {
      return res;
    }
    Map<String, Integer> map = new HashMap<>();
    for (String cur : cpdomains) {
      String[] splited = cur.split("\\s+");
      String[] level = splited[1].split("\\.");
      String currentDom = "";
      int count = Integer.valueOf(splited[0]);
      for (int i = level.length - 1; i >= 0; i--) {
        currentDom = (i < level.length - 1 ? level[i] + "." + currentDom : level[i] + currentDom);
        map.put(currentDom, map.getOrDefault(currentDom, 0) + count);
      }
    }
    for (String dom : map.keySet()) {
      res.add(map.get(dom) + " " + dom);
    }
    return res;
    // Time O(N) N is cpdomains.length()
    // SPace O(N)
  }
}
```

### Java String 的空格分割方法 str.split("\\\s+")  "."的分割方法 str.split("\\\\.")

# OODS

## 1396. Design Underground System (unsolved)

### 1. HashTable

```java
class UndergroundSystem {
  private Map<Integer, Event> arrivals;
  private Map<String, Average> averages;
  public UndergroundSystem() {
    arrivals = new HashMap<>();
    averages = new HashMap<>();
  }
    
  public void checkIn(int id, String stationName, int t) {
    arrivals.add(id, new Event(id, stationName, t));
  }
    
  public void checkOut(int id, String stationName, int t) {
    Event arrivalEvent = arrivals.get(id);
    arrivals.remove(id);
    int diff = t - arrivalsEvent.time;
    String key = arrivalsEvent.stationName + stationName;
    Average average = average.containsKey(key) ? averages.get(key) : new Average();
    average.updateAverage(diff);
    averages.put(key, average);
  }
    
  public double getAverageTime(String startStation, String endStation) {
    String key = startStation + endStation;
    return average.get(key).caculateAverage();
 	}
  class Event {
    public int id;
    public int time;
    public String stationName;
    public void Event(int id, int time, String stationName) {
      id = this.id;
      t = this.time;
      stationName = this.stationName;
    }
  }
  class Average {
    public double total = 0;
    public int count = 0;
    public void updateAverage(int diff) {
      count++;
      total += diff;
    }
    public double caculateAverage() {
      return total / count;
    }
  }
}


```



# Dynamic Programming

## 509. Fibonacci Number

### 1. Recursion 

```java
class Solution {
  public int fib(int n) {
    if (n <= 1) {
      return n;
    }
    return fib(n - 1) + fib(n - 2);
  }
  // O(2 ^ n) 
  // Space O(N)
}
```



### 2. DP: Bottom-Up Approach using Tabulation(制表)

```java
class Solution {
  public int fib(int n) {
    if (n <= 1) {
      return n;
    }
    int[] cache = new int[n + 1];
    cache[1] = 1;
    for (int i = 2; i < cache.length; i++) {
      cache[i] = cache[i - 1] + cache[i - 2];
    }
    return cache[n];
    // O(N)
    // O(N)
  }
}
```



### 3. DP: Top - down Appraoch using Memoization(记忆)

```java
class Solution {
  private Map<Integer, Integer> map = new HashMap<>(Map.of(0, 0, 1, 1));
  public int fib(int n) {
    if (map.containsKey(n)) {
      return map.get(n);
    }
    int result = fib(n - 1) + fib(n - 2);
    map.put(n, result);
    return map.get(n);
  }
}
```

- Time complexity: O(N). Each number, starting at 2 up to and including `N`, is visited, computed and then stored for *O*(1) access later on.

Space complexity: O(n)



### 4. Iterative Bottom - up Approach

```java
class Solution {
  public int fib(int n) {
    if (n <= 1) {
      return n;
    }
    int pre1 = 1;
    int pre2 = 0;
    int fib = 0;
    for (int i = 2; i <= n; i++) {
      fib = pre1 + pre2;
      pre2 = pre1;
      pre1 = fib;
    }
    return fib;
  }
  //Time O(n)
  // Space O(1) 
}
```



## 1137. N - th Tribonacci Number

### 1. Recursion (Time Limit Exceeded)

```java
class Solution {
  public int tribonacci(int n) {
    if (n <= 1) {
      return n;
    }
    else if (n == 2) {
      return 1;
    }
    return tribonacci(n - 3) + tribonacci(n - 2) + tribonacci(n - 1);
  }
}
```

### 2. DP: Bottom - up Approach using Tabulation

```java
class Solution {
  public int tribonacci(int n) {
    if (n <= 1) {
      return n;
    }
    else if (n == 2) {
      return 1;
    }
    int[] cache = new int[n + 1];
    cache[1] = 1;
    cache[2] = 1;
    for (int i = 3; i < cache.length; i++) {
      cache[i] = cache[i - 3] + cache[i - 2] + cache[i - 1];
    }
    return cache[n];
    //Time O(n)
    //Space O(n)
  }
}
```

### 3. DP: Top - down Appraoch 

```java
class Solution {
  private Map<Integer, Integer> map = new HashMap<>(Map.of(0, 0, 1, 1, 2, 1));
  public int tribonacci(int n) {
    if (map.containsKey(n)) {
      return map.get(n);
    }
    int result = tribonacci(n - 3) + tribonacci(n - 2) + tribonacci(n - 1);
    map.put(n, result);
    return map.get(n);
  }
  // Time O(n)
  // Space O(n)
}
```

### 4. DP: Bottom - up Approach (Space Optimization)

```java
class Solution {
  public int tribonacci(int n) {
    if (n <= 1) {
      return n;
    }
    else if (n == 2) {
      return 1;
    }
    int pre1 = 1;
    int pre2 = 1;
    int pre3 = 0;
    int trib = 0;
    for (int i = 3; i <= n; i++) {
      trib = pre1 + pre2 + pre3;
      pre3 = pre2;
      pre2 = pre1;
      pre1 = trib;
    }
    return trib;
    //Time O(N)
    //Space O(1)
  }
}
```



## 70. Climbing Stairs

### 1. Dynamic Programming Bottom - Up with Tabulation

```java
class Solution {
  public int climbStairs(int n) {
    if (n <= 0) {
      return 0;
    }
    if (n == 1) {
      return 1;
    }
    if (n == 2) {
      return 2;
    }
    int[] dp = new int[n + 1];
    dp[1] = 1;
    dp[2] = 2;
    for (int i = 3; i <= n; i++) {
      dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
    //Time O(n)
    //SPace O(n)
  }
}
```

 ### 2. Fibonacci

```java
class Solution {
  public int climbStairs(int n) {
    if (n <= 0) {
      return 0;
    }
    if (n == 1) {
      return 1;
    }
    if (n == 2) {
      return 2;
    }
    int numStep = 0;
    int firstStairs = 1;
    int secondStairs = 2;
    for (int i = 3; i <= n; i++) {
      numStep = firstStairs + secondStairs;
      firstStairs = secondStairs;
      secondStairs = numStep;
    }
    return numStep;
    //Time O(n)
    //Space O(1)
  }
}
```

![image-20220428161935620](/Users/youhao/Library/Application Support/typora-user-images/image-20220428161935620.png)

### 3. Recursion with Memoization

```java
class Solution { 
  public int climbStairs(int n) {
    return helper(0, n , new int[n + 1]);
  }
  public int helper(int i, int n, int[] memo) {
    if (i > n) {
      return 0;
    }
    if (i == n) {
      return 1;
    }
    if (memo[i] > 0) {
      return memo[i];
    }
    memo[i] = helper(i + 1, n, memo) + helper(i + 2, n, memo);
    return memo[i];
    // Time O(n)
    // Space O(n)
  }
}
```

## 746. Min Cost Climbing Stairs

### 1. Recursion with Memoization

```java
class Solution {
  private HashMap<Integer, Integer> map = new HashMap<>();
  public int minCostClimbingStairs(int[] cost) {
    int n = cost.length;
    return helper(n, cost);
  }
  public int helper(int n, int[] cost) {
    if(n <= 1) {
      return 0;
    }
    if(map.containsKey(n)) {
      return map.get(n);
    }
    // helper(n - 1, cost) return the downOne stair's cost
    // helper(n - 2, cost) return the downTwo stair's cost
    // each of them add their own cost
    // Compare their cost
    map.put(n, Math.min(helper(n - 1, cost) + cost[n - 1], helper(n - 2, cost) + cost[n - 2]));
    return map.get(n);
  }
}
```

### 2. DP: Bottom- up with Tabulation

```java
class Solution {
  public int minCostClimbingStairs(int[] cost) {
    int n = cost.length;
    if (n <= 0) {
      return 0;
    }
    if (n == 1) {
      return cost[0];
    }
    if (n == 2) {
      return Math.min(cost[0], cost[1]);
    }
    int[] dp = new int[n + 1];
    for (int i = 2; i <= n; i++) {
      int oneStep = dp[i - 1] + cost[i - 1];
      int twoStep = dp[i - 2] + cost[i - 2];
      dp[i] = Math.min(oneStep, twoStep);
    }
    return dp[n];
    //Time O(n)
    //Space O(n)
  }
}
```

### 3. DP: Bottom - up with Constant Space

```java
class Solution {
  public int minCostClimbingStairs(int[] cost) {
    int n = cost.length;
    if (n <= 0) {
      return 0;
    }
    if (n == 1) {
      return cost[0];
    }
    if (n == 2) {
      return Math.min(cost[0], cost[1]);
    }
    int downOne = 0;
    int downTwo = 0;
    int res = 0;
    for (int i = 2; i <= n; i++){
      res = Math.min(downOne + cost[i - 1], downTwo + cost[i - 2]);
      downTwo = downOne;
      downOne = res;
    }
    return res;
    //O(n)
    //O(1)
  }
}
```

![image-20220428200937640](/Users/youhao/Library/Application Support/typora-user-images/image-20220428200937640.png)

## 198. House Robber

### 1. DP with Memoization

```java
class Solution {
  public int rob(int[] nums) {
    int n = nums.length;
    if(n == 0){return 0;}
    int[] dp = new int[n + 1];
    dp[0] = 0;
    dp[1] = nums[0];
    for (int i = 2; i <= n; i++) {
      dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i - 1]);
    }
    return dp[n];
  }
}
```

![image-20220429195521897](/Users/youhao/Library/Application Support/typora-user-images/image-20220429195521897.png)

### 2. Optimized DP

```java
class Solution {
  public int rob(int[] nums) {
    int n = nums.length;
    if (n == 0) {
      return 0;
    }
    int first = 0;
    int second = nums[0];
    int robMax = nums[0];
    for(int i = 1; i < n; i++) {
      robMax = Math.max(second, first + nums[i]);
      first = second;
      second = robMax;
    }
    return robMax;
  }
}
// Time O(n)
// Space O(1)
```



## 740. Delete and Earn

### 1. DP with Memoization 

```java
class Solution {
  public int deleteAndEarn(int[] nums) {
    if (nums.length == 0) {
      return 0;
    }
    Map<Integer, Integer> map = new HashMap<>();
    for (int num : nums) {
      map.put(num, map.getOrDefault(num, 0) + num);
    }
    int[] arr = new int[map.size()];
    int index = 0;
    for(int num : map.keySet()) {
        arr[index] = num;
        index++;
    }
    Arrays.sort(arr);
    int twoBack = 0;
    int oneBack = map.get(arr[0]);
    int current = oneBack;
    for (int i = 2; i <= arr.length; i++) {
      int element = arr[i - 1];
      int preElement = arr[i - 2];
      if (element == preElement + 1) {
        current = Math.max(oneBack, twoBack + map.get(element));
      } else {
        current = oneBack + map.get(element);
      }
      twoBack = oneBack;
      oneBack = current;
    }
    return current;
    // Time Complexity O(N*log(N))
    // Space Complexity O(N)
  }
}
```

###  2. DP with Bucket

```java
class Solution {
  public int deleteAndEarn(int[] nums) {
    if(nums.length == 0) {
      return 0;
    }
    int[] bucket = new int[10001];
    for (int num : nums) {
      bucket[num] += num;
    }
    int oneBack = bucket[1];
    int twoBack = bucket[0];
    int res = bucket[1];
    for (int i = 2; i < bucket.length; i++) {
      res = Math.max(twoBack + bucket[i - 1], oneBack);
      twoBack = oneBack;
      oneBack = res;
    }
    return res;
    // Time O(n);
    // Space O(n);
  }
}
```

![image-20220430145813062](/Users/youhao/Library/Application Support/typora-user-images/image-20220430145813062.png)

### 213. House Robber II

### 1. DP with Memoization

```java
class Solution {
  public int rob(int[] nums) {
    if (nums.length == 0) {
      return 0;
    }
    int n = nums.length;
    if (n == 1) {
        return nums[0];
    }
    int[] dp1 = new int[n];
    int[] dp2 = new int[n];
    dp1[0] = 0;
    dp1[1] = nums[0];
    dp2[0] = 0;
    dp2[1] = nums[1];
    for (int i = 2; i < n; i++) {
      dp1[i] = Math.max(dp1[i - 1], dp1[i - 2] + nums[i - 1]);
      dp2[i] = Math.max(dp2[i - 1], dp2[i - 2] + nums[i]);
    }
    return Math.max(dp1[n -1], dp2[n - 1]);
    //Time O(n)
    //Space O(n)
  }
}
```

### 2. DP with Optimized Space

```java
class Solution {
  public int rob(int[] nums) {
    int n = nums.length;
    if (n == 0) {
      return 0;
    }
    if (n == 1) {
      return 1;
    }
    int oneBack1 = nums[0];
    int twoBack1 = 0;
    int oneBack2 = nums[1];
    int twoBack2 = 0;
    int rob1 = nums[0];
    int rob2 = nums[1];
    for (int i = 2; i < nums.length; i++) {
      rob1 = Math.max(oneBack1, twoBack1 + nums[i - 1]);
      rob2 = Math.max(oneBack2, twoBack2 + nums[i]);
      twoBack1 = oneBack1;
      oneBack1 = rob1;
      twoBack2 = oneBack2;
      oneBack2 = rob2;
    }
    return Math.max(rob1, rob2);
  }
  //Time O(n)
  //Space O(1)
}
```

![image-20220430161436865](/Users/youhao/Library/Application Support/typora-user-images/image-20220430161436865.png)

## 55. Jump Game

### 1. Backtracking(Time Limit Exceeded)

```java
class Solution {
  public boolean canJump(int[] nums) {
    return backtrack(nums, 0);
  }
  public boolean backtrack(int[] nums,int index) {
    if(index + nums[index] >= nums.length - 1) {
      return true;
    }
    else if(nums[index] == 0) {
      return false;
    }
    int step = nums[index];
    for (int i = 1; i <= step; i++) {
      if(backtrack(nums, index + i)){
        return true;
      }
    }
      return false;
  }
}
```

### 2. Greedy

```java
class Solution {
  public boolean canJump(int[] nums) {
    int n = nums.length;
    if (n == 0) {
      return false;
    }
    if (n == 1) {
      return true;
    }
    int max = 0;
    for (int i = 0; i < n; i++) {
      if (i > max) {
        return false;
      }
      max = Math.max(max, nums[i] + i);
    }
    return true;
    // Time O(n)
    //Space O(1)
  }
}
```



## 45. Jump Game II

### 1. Greedy

```java
class Solution {
  public int jump(int[] nums) {
    int max = 0;
    int curMax = 0;
    int step = 0;
    int n = nums.length;
    if (n == 0) {return 0;}
    for(int i = 0; i < n - 1; i++) {
      max = Math.max(max, i + nums[i]);
      if (i == curMax){
        curMax = max;
        step++;
      }
    }
    return step;
    //Time O(n)
    //Space O(1)
  }
}
```

![image-20220430180854697](/Users/youhao/Library/Application Support/typora-user-images/image-20220430180854697.png)

### 2. BFS

```java
class Solution{
  public int jump(int[] nums) {
    int n = nums.length;
    if (n <= 1) {
      return 0;
    }
    int level = 0;
    int curLength = 0;
    int nextLength = 0;
    int index = 0;
    while (curLength - index >= 0) {
      level++;
      for(; index <= curLength; index++) {
        nextLength = Math.max(index + nums[index], nextLength);
        if (nextLength >= n - 1) {
          return level;
        }
      }
     index = curLength + 1;
     curLength = nextLength;
    }
    return 0;
    //Time O(n)
    //Space O(1)
  }
}
```

![image-20220430182858062](/Users/youhao/Library/Application Support/typora-user-images/image-20220430182858062.png)



## 53. Maximum  Subarray

### 1. DP 

```java
class Solution {
  public int maxSubArray(int[] nums) {
    int n = nums.length;
    if (n == 1) {
      return nums[0];
    }
    int[] dp = new int[n];
    dp[0] = nums[0];
    int res = 0;
    for (int i = 1; i < n; i++) {
      dp[i] = nums[i] + (dp[i - 1] < 0 ? 0 : dp[i - 1]);
      res = Math.max(res, dp[i]);
    }
    return res;
  }
}
```

### 2. DP with Constant Space

```java
class Solution {
  public int maxSubArray(int[] nums) {
    int n = nums.length;
    if (n == 1) {
      return nums[0];
    }
    int curMax = 0;
    int max = nums[0];
    int preMax = nums[0];
    for (int i = 1; i < n; i++) {
      curMax = nums[i] + (preMax < 0 ? 0 : preMax);
      max = Math.max(max, curMax);
      preMax = curMax;
    }
    return max;
  }
}
```

![image-20220501163459150](/Users/youhao/Library/Application Support/typora-user-images/image-20220501163459150.png)

Thinking Process

copy from https://leetcode.com/problems/maximum-subarray/discuss/20193/DP-solution-and-some-thoughts

```
My thought process:
Array: [-2,1,-3,4,-1,2,1,-5,4]

If all elements were positive, the answer would be the total sum.

But here we have negative numbers, so we should avoid them.
So, we could have sums of positive numbers on left and right of each negative number. And maximum of these sums is the answer.

But there is this hidden case when including a negative number helps: when the elements or sum of elements left or right to the negative element is greater than the negative element itself. **This is the key observation of this problem.**
eg: [2 -1 3]: here including -1 would only help me in getting better answer by adding 2 and 3 while subtracting just -1.

Now as I got an idea of the problem, i would start solving step by step considering 1 element at a time.

Considered if the array had only 1 element: [-2]
Since array now has only 1 element I could only choose -2.

Considered if the array had 2 elements: [-2, 1]
ans:

is the new element (1) greater than the previous ans: YES
is prev element (-2) + new element (1) greater than the new element (1): NO
So since new element (1) is greater than than both the cumulative sum(new element, previous element) and prev ans -2, I would choose the new element 1. (Also I would update cumulative sum to be this new element 1, for future incoming elements. **This is the key lesson of this pattern**)
[-2,1,-3]
ans: I would use the previous logic:
Is the new element (-3) greater than the previous ans (1): NO
Is the new cumulative sum(prev cumulative sum, new element) > new element: NO
So I choose prev ans: 1
..... so on
```

## 918. Maximum Sum Circular Subarray

### 1. DP

```java
class Solution {
  public int maxSubarraySumCircular(int[] nums) {
    int n = nums.length;
    if (n == 0) {return 0;}
    int curMax = 0;
    int preMax = 0;
    int curMin = 0;
    int preMin = 0;
    int maxSub = nums[0];
    int minSub = nums[0];
    int total = 0;
    for (int i = 0; i < n; i++) {
      curMax = nums[i] + (preMax > 0 ? preMax : 0);
      curMin = nums[i] + (preMin < 0 ? preMin : 0); 
      maxSub = Math.max(maxSub, curMax);
      minSub = Math.min(minSub, curMin);
      total += nums[i];
      preMax = curMax;
      preMin = curMin;
    }
    return maxSub > 0 ? Math.max(maxSub, total - minSub) : maxSub;
    //if all the numbers are negative, the maxSub = Math.max(nums), maxSub is the specific number which is negative,so the subArray cannot appear the case of "0".
    // But the minSub = total, if total - minSub, the result is 0, which means we returen a empty subarray.
    // Therefore, we need to split two case to discuss.
    //Time O(n)
    //Space O(1)
  }
}
```

![image-20220501170514279](/Users/youhao/Library/Application Support/typora-user-images/image-20220501170514279.png)



## 152. Maximum Product Subarray

### 1. DP 

```java
class Solution {
  public int maxProduct(int[] nums) {
    int n = nums.length;
    if (n == 0) {return 0;}
    if (n == 1) {return nums[0];}
    int curMax = nums[0];
    int max = nums[0];
    int curMin = nums[0];
    for (int i = 1; i < n; i++) {
      int temp = curMax;
      curMax = Math.max(nums[i] * curMax, Math.max(nums[i], curMin * nums[i]));
      curMin = Math.min(nums[i] * temp, Math.min(nums[i], curMin * nums[i]));
      max = Math.max(max, curMax);
    }
    return max;
  }
  // Time O(n)
  // SPace O(1)
}
```

![image-20220502154730231](/Users/youhao/Library/Application Support/typora-user-images/image-20220502154730231.png)



## 1567. Maximum Length of Subarray With Positive Product

### 1. DP

```java
class Solution {
  public int getMaxLen(int[] nums) {
    int n = nums.length;
    if (n == 0) {
      return 0;
    }
    int positive = 0;
    int negative = 0;
    int ans = 0;
    for (int i = 0; i < n; i++) {
      if (nums[i] == 0) {
        //restart the count
        positive = 0;
        negative = 0;
      }
      else if (nums[i] > 0) {
        positive++;
        negative = negative == 0 ? 0 : negative + 1;
      } else {
        int temp = positive;
        positive = negative == 0 ? 0 : negative + 1;
        negative = temp + 1;
      }
      ans = Math.max(positive, ans);
    }
    return ans;
  }
}
```

![image-20220502174612072](/Users/youhao/Library/Application Support/typora-user-images/image-20220502174612072.png)
