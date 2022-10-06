# 1. 使用

```java
Deque<Integer> q = new ArrayDeque();
```

对于java, 我们可以使用LinkedList或者ArrayDeque来实现双端队列Deque Interface

方法：

peekFirst()

pollFirst()

offerFirst()

peekLast()

pollLast()

offerLast()

ArrayDeque的空间消耗比linkedList小，建议用ArrayDeque



# 2. 模版

一般情况为pq实现的优化解，因为pq为O(nlogn), Deque 为O(n)

一般分为4部分：

1. 去尾操作： 队尾元素出列。当前队列有新元素进入时，需要从队尾开始，删除影响队列单调性的元素，维护队列的单调性。（删除一个队尾元素后，就需要重新判断新的队尾元素）
2. 去尾操作后，将该新元素入队列
3. 删头操作：对头元素出队列。判断对头元素是否在待求解的区间内，如果不在，就将其删除。
4. 取解操作：经过上面两个操作，取出队列的头元素，就是当前区间的极值。

**这里可以有不同的顺序，但需要保证**

**1. 插入必须在去尾之后来保持单调性**

**2. 根据不同题目来改变取解的具体位置**

```java
public int[] MonotonicQueue(int[] nums, int k) {
  int N = nums.length;
  Deque<Integer> q = new ArrayQueue<>();
  int[] res = new int[N - k + 1];
  for (int i = 0; i < N; i++) {
    while (!q.isEmpty() && i - q.peekFirst() >= k) q.pollFirst(); // 左出q，保证窗口大小k - 1
    while (!q.isEmpty() && nums[q.peekLast()] <= nums[i]) q.pollLast(); // 右出q，保证递减队列
    q.offerLast(i);//进q，此时q.size == k
    q.peekFirst(); // use in the res;             //使用q左边最大值处理结果，可以在不同的位置
  }                             
  return res;
}
```



## 239. Sliding Window Maximum

### 1. Deque to implement Monotonic Queue

Tips:

（1）利用Deque来实现队头和队尾的操作

（2）每次遍历中，我们需要实现队列中，呈单调递减的形式

（3）保证队列中的头元素离当前元素的距离小于k - 1, 实现客观意义上的sliding window

（4）保证当前元素插入队列中时，仍能保持单调递减的趋势