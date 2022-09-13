# 思路

不需要检测每一个时刻，只需要检测起点或者终点的位置！

![image-20220913163540737](/Users/youhao/Library/Application Support/typora-user-images/image-20220913163540737.png)



## 253. Meeting Rooms II

### 1. Heap + Sort

```java
class Solution {
  public int minMeetingRooms(int[][] intervals) {
      Arrays.sort(intervals, (a, b) -> (a[0] - b[0]));
      PriorityQueue<int[]> heap = new PriorityQueue<>((a, b) -> (a[1] - b[1]));
      if (intervals.length != 0) {
          heap.add(intervals[0]);
      }
      for (int i = 1; i < intervals.length; i++) {
          int[] cur = heap.poll();
          if (cur[1] <= intervals[i][0]) {
              cur[1] = intervals[i][1];
          } else {
              heap.add(intervals[i]);
          }
          heap.add(cur);
      }
      return heap.size();
  }
}
```

heap按照结束时间进行排序，先将整个数组进行排序。每次for loop的时候，只需要找heap的root就行。因为heap堆顶的结束时间是最早的，所以最有可能和当前会议进行合并。如果堆顶元素不符合那就说明其他的也不符合。

时间复杂度 

- There are two major portions that take up time here. One is `sorting` of the array that takes O(N\log N)*O*(*N*log*N*) considering that the array consists of N*N* elements.
- In the worst case, extract-min operation on a heap takes O*(log*N).
- Overall, the Time complexity is O(NlogN)

### 2. Scan Line

```java
class Solution {
  public int minMeetingRooms(int[][] intervals) {
      List<int[]> list = new ArrayList<>();
      for (int i = 0; i < intervals.length; i++) {
          list.add(new int[]{intervals[i][0], 1});
          list.add(new int[]{intervals[i][1], -1});
      }
      Collections.sort(list, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
      int res = 0;
      int count = 0;
      for (int[] cur : list) {
          count += cur[1];
          res = Math.max(res, count);       
      }
      return res;
  }
}
```

- Time Complexity: O(Nlog N) because all we are doing is sorting the two arrays for `start timings` and `end timings` individually and each of them would contain N elements considering there are N intervals.
- Space Complexity: O(N because we create two separate arrays of size N*N*, one for recording the start times and one for the end times.





## 56. Merge Intervals

### 1. Scan Line

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        List<int[]> list = new ArrayList<>();
        for (int i = 0; i < intervals.length; i++) {
            list.add(new int[]{intervals[i][0], 1});
            list.add(new int[]{intervals[i][1], -1});
        }
        Collections.sort(list, (a, b) -> (a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]));
        List<int[]> res = new ArrayList<>();
        int count = 0;
        int start = 0;
        int end = 0;
        for (int[] cur : list) {
            if (count == 0) {
                start = cur[0];
            }
            count += cur[1];
            if (count == 0) {
                end = cur[0];
                res.add(new int[]{start, end});
            }
        }
        return res.toArray(new int[res.size()][]);
    }
}
```

和前面一样进行扫描，将开始和结束分别放入不同的数组， 给开始设置 + 1， 结束设置为 -1。 当遇到结束和开始在同一个节点时， 先执行开始节点。 这样,如果一开始count为0, 说明是一段新的数组，此为起点。 如果在操作完当前位置的加减， count 为0，说明该点是结束。

Time O(nlogn)



### 2. Sorting

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        List<int[]> res = new ArrayList<>();
        Arrays.sort(intervals, (a, b) -> (a[0] - b[0]));
        if (intervals.length != 0) {
            res.add(intervals[0]);
        }
        for (int[] inter : intervals) {
            int[] cur = res.get(res.size() - 1);
            if (cur[1] >= inter[0]) {
                cur[1] = Math.max(cur[1], inter[1]);
                res.remove(res.size() - 1);
                res.add(cur);
            } else {
                res.add(inter);
            }
        }
        return res.toArray(new int[res.size()][]);
    }
}
```

每一个interval 都会和 res的最后一个进行比较，如果interval的开始小于等于 res最后的结束，说明他们可以合并成一个interval。反之，创建一个新的interval



## 57. Insert Interval

## 1. Normal Insert

```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> res = new ArrayList<>();
        
        for (int[] cur : intervals) {
            if (newInterval == null || cur[1] < newInterval[0]) {
                res.add(cur);
            }
            else if (cur[0] > newInterval[1]) {
                res.add(newInterval);
                res.add(cur);
                newInterval = null;
            } else {
                newInterval[0] = Math.min(cur[0], newInterval[0]);
                newInterval[1] = Math.max(cur[1], newInterval[1]);
            }
        }
        if (newInterval != null) {
            res.add(newInterval);
        }
        return res.toArray(new int[res.size()][]);
    }
}
```

newInterval会有三种情况

1. 在当前cur的左边，这种情况下，newInterval不需要合并，直接加入到list中，结束讨论。
2. 在cur右边，我们不确定后面数组的情况，所以list中加入cur，继续讨论。
3. 需要merge，这种情况下cur, 和newinterval都不能加入到list中去，将它俩合并之后成为新的newInterval，继续向后寻找。

在for loop之后，如果newInterval还没有加入到list中，说明newInterval在list最后一个的右边，所以直接加入。