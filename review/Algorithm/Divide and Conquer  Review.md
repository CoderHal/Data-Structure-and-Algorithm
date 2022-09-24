# Divide and Conquer Concept

## Divide

Divide the problem into a number of subproblems that are smaller instances of the same problem.

## Conquer

Conquer the subproblems by solving them recursively. If the subproblem sizes are small enough, however, just solve the subproblems in a straightforward manner.

## Combine

Combine the solutions to the subproblems into the solution for the original problem.

![image-20220922215229860](/Users/youhao/Library/Application Support/typora-user-images/image-20220922215229860.png)





![image-20220922221308136](/Users/youhao/Library/Application Support/typora-user-images/image-20220922221308136.png)



## 169. Majority Element

### 1. Divide and Conquer

Tips:

（1）将数组从left -> right， 每次对半分，分成left = right时结束，并返回当前数。因为在当前状态下[left, right]有且只有一个数，所以该数肯定是最大的。

（2） 在返回的过程中，由于divide时，我们分成了两组，我们需要分别比较两部分哪个数出现次数多。如果左右两部分的结果是一样的，说明该数是[left, right]中最多的数直接返回。其他的就conquer处理，分别统计在【left，right】范围内，哪个数出现次数最多。

（3）随着不断的返回，最终就能求出整个数组出现次数最多的数。



### 2. Candidate Vote

Tips:

（1）我们使用候选人投票的方式。 先将第一个人设置为候选人。如果在后面选择中，投票者都不是该候选人，就 - 1， 是候选人就 + 1。

（2）当该候选人得票为0票时，就更改候选人，换成当前的人。继续统计投票。

（3）由于真正的候选人得票数是大于n/2的，所以候选人的票肯定会大于0



## 50. Pow(x, n)

### 1. Divide and Conquer

Tips:

（1）首先讨论n为负数的情况，我们需要把n变成正数， n = -n，这种情况下 x的值变成了 1/x。还有一个情况是n = Integer.MIN_VALUE的情况， n = -n会越界。 所以我们需要让n = - (n + 1)， 如果是这种情况，结果需要多乘一个 1/x.

（2）n都为正数的情况下，我们就可以使用Divide and Conquer了。 在n = 0时结束，返回1。 n每次分成一半， 如果n为偶数，就是y = pow(x, n / 2).  return y * y。 如果是奇数 return x * y * y.



## 215. Kth Largest Element in an Array

### 1. Heap

Tips:

（1） 创建一个小根堆，使heap一直保持size为k的情况，如果在遍历过程中，heap的size 已经到了k，我们直接poll出heap，将当亲的num加入进去，这样的话会poll出length - k个最小数。 遍历结束后，heap的root就是第k大的数

### 2. Quick Select

Tips:

（1） Quick Sort的变种。

（2） 我们选择数组最后一个element成为pivot，将数组中小于pivot的放到pivot左边， 大于pivot的放在右边。

（3）在跟pivot比较的时候，用wall = left来帮助遍历，如果有小于pivot的就和wall交换，交换之后，wall到下一个index。在交换过整个数组之后，wall和right交换，pivot就换到了wall的位置，实现了pivot前面都是小的，后面都是大的。返回pivot所在的index

（4）index和 （length - k）比较，看是否是在第k大的位置，如果是直接返回。如果不是，如果在length - k的左边，那就让left = partition + 1， 反之 right = partition - 1

时间复杂度是 O(n)  因为我们不需要处理右边部分，只讨论左边的部分。 所以时间就变成了 T(n) = O(n  + n /2 + n/ 4 + ..... + 1) = O(n), 

worst 是O(n ^ 2), 是选择pivot错误导致的。如果pivot选的不好， 在处理时发现，pivot每次都是在范围内最大的，这就造成了每次都是操作left - > right步，这种方式就变成了O(N^2)





## 23. Merge k Sorted Lists

### 1. Heap

Tips: 

（1）创建小根堆，按照每个list的第一个节点值大小排序

（2）在heap不为空的情况下，每次poll出节点最小的list，将该节点加入到结果中，如果改list.next不为空，将list的下一个节点继续加入heap中



### 2. Divide and Conquer

Tips:

（1） Divide，将list数组从left - > right中分成一半讨论，直到分成了1个list （left == right)返回当前list。后面就是分开的两部分list进行合并，后面就是[left, mid]  和 [mid + 1, right]进行合并，成为最终结果。

（2） Conquer按照两个list的merge sort进行处理就可以。

