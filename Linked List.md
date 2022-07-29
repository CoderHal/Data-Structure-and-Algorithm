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

```java
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



## 876.Middle of the Linked List

### Idea

We just need to figure out the middle Node of this list.

### Solution

#### 1. Output to Array

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

```
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

```
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

```
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

```
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

```
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



## 23. Merge k Sorted Lists

### 1. Compared One by one

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
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) {
            return null;
        }
        if (lists.length == 1) {
            return lists[0];
        }
        ListNode res = new ListNode(0);
        ListNode cur = res;
        int n = lists.length;
        ListNode list1 = lists[0];
        for (int i = 1; i < n; i++) {
            ListNode list2 = lists[i];
            cur = res;
            while (list1 != null && list2 != null) {
                if (list1.val <= list2.val) {
                    cur.next = list1;
                    list1 = list1.next;
                    cur = cur.next;
                } else {
                    cur.next = list2;
                    list2 = list2.next;
                    cur = cur.next;
                }
            } 
            if (list1 == null) {
                cur.next = list2;
            } else {
                cur.next = list1;
            }
            list1 = res.next;
        }
        return res.next;
    }
}
```



### 2. Using Priority Queue

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) {
           return null;
        }
        if (lists.length == 1) {
           return lists[0];
        }
        //最小堆
        PriorityQueue<ListNode> pq = new PriorityQueue<>(lists.length, (a, b) -> (a.val - b.val));
        for (ListNode list : lists) {
            if (list != null) {
                pq.add(list);
            }
        }
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        while (pq.size() > 0) {
            ListNode node = pq.poll();
            cur.next = node;
            if (node.next != null){
                pq.add(node.next);
            }
            cur = cur.next;
        }
        return dummy.next;
    }
//优先队列 pq 中的元素个数最多是 k，所以一次 poll 或者 add 方法的时间复杂度是 O(logk)；所有的链表节点都会被加入和弹出 pq，所以算法整体的时间复杂度是 O(Nlogk)，其中 k 是链表的条数，N 是这些链表的节点总数
}

```

创建一个二叉堆，二叉堆的规则是： 创建一个lists.length空间的二叉堆， 根据list头节点的值进行排序

利用dummy的新节点去遍历整个二叉堆，将最小的头节点拿出来，再把头节点后面的节点加入堆中重新排序

## 92. Reverse Linked List II

### Solution

#### 1. My Iteration Solution

```
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

![image-20220105200914414](file:///Users/youhao/Library/Application%20Support/typora-user-images/image-20220105200914414.png?lastModify=1653950456)

#### 2. Recursion

```
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

![image-20220105211042659](file:///Users/youhao/Library/Application%20Support/typora-user-images/image-20220105211042659.png?lastModify=1653950456)

## 143. Reorder List

### Solution

#### Idea

![image-20220106220301926](file:///Users/youhao/Library/Application%20Support/typora-user-images/image-20220106220301926.png?lastModify=1653950456)

#### 1. Middle of the Linked List->Reverse Linked List-> Merge Two Sorted Lists

```
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

### 2. Add Two Numbers

### 1. LinkedList

```java
class Solution {
  public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode p = l1;
    ListNode q = l2;
    ListNode cur = new ListNode(0);
    ListNode res = cur;
    int add = 0;
    while (p != null || q != null) {
      int x = (p != null) ? p.val : 0;
      int y = (q != null) ? q.val : 0;
      int sum = x + y + add;
      add = sum / 10;
      cur.next = new ListNode(sum % 10);
      p = (p != null) ? p.next : p;
      q = (q != null) ? q.next : q;
      cur = cur.next;
    }
    if (add > 0) {
      cur.next = new ListNode(add);
    }
    return res.next;
  }
}
```



## 19. Remove Nth Node From End of List

### 1. LinkedList

```java
class Solution {
  public ListNode removeNthFromEnd(ListNode head, int n) {
    int length = 0;
    ListNode temp = head;
    ListNode res = head;
    while(temp != null) {
      length++;
      temp = temp.next;
    }
    if (n == length) {
      res = res.next;
      return res;
    }
    int count = 1;
    while (count < length - n) {
      res = res.next;
      count++;
    }
    res.next = res.next.next;
    return head;
  }
}
```



## 142. Linked List Cycle II

### 1. Floyd‘s Tortoise  and Hare (Cycle Detection) ( P287)

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        do {
            if(fast == null || fast.next == null) {
                return null;
            }
            fast = fast.next.next;
            slow = slow.next;
        } while (fast != slow);
        
        fast = head;
        while (fast != slow) {
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }
}
```

#  Sort List Problem 

## 148. Sort List (Divided and Conquor)



![image-20220708202824696](/Users/youhao/Library/Application Support/typora-user-images/image-20220708202824696.png)

### 1. Merge Sort (top down) 

```java
class Solution {
  public ListNode sortList(ListNode head) {
    if (head == null || head.next == null) {return head;}
    // Using fast and slow to find the mid 
    ListNode fast = head.next;
    ListNode slow = head;
    while (fast != null && fast.next != null) {
      fast = fast.next.next;
      slow = slow.next;
    }
    // split to two Lists
    ListNode mid = slow.next;
    slow.next = null;
    return mergeSort(sortList(head), sortList(mid));
  }
  public ListNode mergeSort(ListNode l1, ListNode l2) {
    ListNode pre = new ListNode(0);
    ListNode tail = pre;
    while (l1 != null && l2 != null) {
      if (l1.val < l2.val) {
        tail.next = l1;
        l1 = l1.next;
        tail = tail.next;
      } else {
        tail.next = l2;
        l2 = l2.next;
        tail = tail.next;
      }
    }
    tail.next = (l1 != null) ? l1 : l2;
    return pre.next;
    //Time O(nlogn)
    //Space O(logn)
  }
}
```

![image-20220708211344441](/Users/youhao/Library/Application Support/typora-user-images/image-20220708211344441.png)





## 147. Insertion Sort List

### 1. Insertion Sort

```java
class Solution {
  public ListNode insertionSortList(ListNode head) {
    if (head == null || head.next == null) {
      return head;
    }
    ListNode dummy = new ListNode(); // Create a dependent node 
    ListNode cur = head; // Use cur as the traversal node
    while (cur != null) {
      ListNode pre = dummy; // make pre always equal the result List's pre node
      // in order to start at the first node
      ListNode temp = cur.next;
      while (pre.next != null && pre.next.val < cur.val) {
        // 1. for the started loop, pre.next == null, so we can just add the cur
        // 2. pre.next.val > cur
        // 3. all the pre < cur
        pre = pre.next;
      }
      cur.next = pre.next;
      pre.next = cur;
      cur = temp;
    }
    return dummy.next;
    //O(n * n)
    //O(1)
  }
}
```





## 138. Copy List with Random Pointer

### 1. HashMap

```java
class Solution {
    public Node copyRandomList(Node head) {
        Node cur = head;
        Map<Node, Node> map = new HashMap<>();
        while (cur != null) {
            map.put(cur, new Node(cur.val));
            cur = cur.next;
        }
        cur = head;
        while (cur != null) {
            map.get(cur).next = map.get(cur.next);
            map.get(cur).random = map.get(cur.random);
            cur = cur.next;
        }
        return map.get(head);
    }
}
```

![image-20220722222737600](/Users/youhao/Library/Application Support/typora-user-images/image-20220722222737600.png)


## 2130. Maximum Twin Sum of a Linked List

### 1. Two Pointers + Reverse LinkList

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
    public int pairSum(ListNode head) {
       ListNode fast = head;
       ListNode slow = head;
       if (fast.next.next == null) {
           return head.val + head.next.val;
       }
       while (fast.next.next != null) {
           fast = fast.next.next;
           slow = slow.next;
       }
       fast = slow.next;
       slow.next = null;
       ListNode pre = null;
       ListNode cur = head;
       while (cur != null) {
           ListNode temp = cur;
           temp = temp.next;
           cur.next = pre;
           pre = cur;
           cur = temp;
       }
       int res = 0;
       while (slow != null) {
           res = Math.max(res, slow.val + fast.val);
           slow = slow.next;
           fast = fast.next;
       }
        return res;
    }
}


```



## 86. Partition List

### 1. Two pointers 

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
    public ListNode partition(ListNode head, int x) {
        ListNode before_head = new ListNode(0);
        ListNode before = before_head;
        ListNode after_head = new ListNode(0);
        ListNode after = after_head;
        ListNode cur = head;
        while (cur != null) {
            if (cur.val < x) {
                before.next = cur;
                before = before.next;
            } else {
                after.next = cur;
                after = after.next;
            }
            cur = cur.next;
        }
        after.next = null;
        before.next = after_head.next;
        return before_head.next;
    }
}
```

Before- head ->>>>>>>> 小于x

After- head ->>>>>>>>大于等于x -> null

Made before-head -> after-head.next
