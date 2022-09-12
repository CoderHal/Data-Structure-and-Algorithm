# Array

# 常见算法

## 1. 前缀和， 数组

### (1) 数组

计算索引区间的和，可以使用前缀和来求，减少累加次数

![image-20220806201537357](/Users/youhao/Library/Application Support/typora-user-images/image-20220806201537357.png)

如果计算[1, 4]内的元素之和， 只需要 preSum[5] - preSum[1]就可以求出来

### (2). 矩阵

![image-20220806202515590](/Users/youhao/Library/Application Support/typora-user-images/image-20220806202515590.png)

创造一个比当前矩阵多一行一列的新矩阵， 从新矩阵的[1, 1]来存储，开始计算 从(0, 0) - > (i, j)的前缀和。根据上述图来计算矩阵某一区域的和

## 2. 差分数组

**差分数组的主要适用场景是频繁对原始数组的某个区间的元素进行增减**

差分就是nums[i] - nums[i - 1];

![image-20220806203234438](/Users/youhao/Library/Application Support/typora-user-images/image-20220806203234438.png)

如果对区间[i...j]中元素全部+3， 那么只需要让 `diff[i] += 3`，然后再让 `diff[j+1] -= 3` 即可

### 差分数组工具类模版

```java
// 差分数组工具类
class Difference {
    // 差分数组
    private int[] diff;
    
    /* 输入一个初始数组，区间操作将在这个数组上进行 */
    public Difference(int[] nums) {
        assert nums.length > 0;
        diff = new int[nums.length];
        // 根据初始数组构造差分数组
        diff[0] = nums[0];
        for (int i = 1; i < nums.length; i++) {
            diff[i] = nums[i] - nums[i - 1];
        }
    }

    /* 给闭区间 [i, j] 增加 val（可以是负数）*/
    public void increment(int i, int j, int val) {
        diff[i] += val;
        if (j + 1 < diff.length) {
            diff[j + 1] -= val;
        }
    }

    /* 返回结果数组 */
    public int[] result() {
        int[] res = new int[diff.length];
        // 根据差分数组构造结果数组
        res[0] = diff[0];
        for (int i = 1; i < diff.length; i++) {
            res[i] = res[i - 1] + diff[i];
        }
        return res;
    }
}
```

比如 预定问题， 拼车

## 3. 原地反转

#### (1) 反转字符串

先反转整个字符串， 再对每个单词进行反转

s = "labuladong world hello"

s = "gnodalubal dlrow olleh"

s = "labuladong world hello"

#### (2) 反转matrix

##### ---顺时针旋转90度

先镜像对称

![image-20220806210738331](/Users/youhao/Library/Application Support/typora-user-images/image-20220806210738331.png)

再进行反转

![image-20220806210814782](/Users/youhao/Library/Application Support/typora-user-images/image-20220806210814782.png)

这就是顺时针翻转90度

![image-20220806210854474](/Users/youhao/Library/Application Support/typora-user-images/image-20220806210854474.png)



##### ---逆时针旋转90度

![image-20220806211156220](/Users/youhao/Library/Application Support/typora-user-images/image-20220806211156220.png)



## 4. Sliding Window模版

```java
public void slidingWindow(String s) {
  HashMap<Character, Integer> map = new HashMap<>();
  int left = 0;
  int right = 0;
  while (right < s.length()) {
    //c是将要移入窗口的的字符
    c = s.charAt(right);
    //增大窗口
    right++;
    //进行窗口的一系列更新
    ...
    
    //判断左侧窗口是否要收缩
    while (window need to shrink) {
      //d 是将要移出的字符
      d = s.charAt(left);
      //缩小窗口
      left++;
      // 将窗口进行一系列更新
      。。。
    }
  }
}
```





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

```

## 16. 3 Sum Closet

### 1. Two Pointer

```java
class Solution {
  public int threeSumClosest(int[] nums, int target) {
    if (nums.length < 3) {
      return - 1;
    }
    Arrays.sort(nums);
    int diff = Integer.MAX_VALUE;
    int res = 0;
    for (int i = 0; i < nums.length - 2; i++) {
      int left = i + 1;
      int right = nums.length - 1;
      while (left < right) {
        int sum = nums[i] + nums[left] + nums[right];
        if (sum == target) {
          return target;
        }
        else if (sum > target) {
          right--;
        }
        else {
          left++;
        }
        int interval = Math.abs(target - sum);
        if (diff > interval) {
          diff = interval;
          res = sum;
        }
      }
    }
    return res;
  }
}
```

## 18. 4 Sum

### 1. Two Pointers

```java
class Solution{
  public List<List<Integer>> fourSum(int[] nums, int target) {
    List<List<Integer>> res = new ArrayList<>();
    int n = nums.length;
    Arrays.sort(nums);
    if (n <= 3 && nums[0] > target) {
      return res;
    }
    for(int i = 0; i < n - 3; i++) {
      if (i == 0 || nums[i] != nums[i - 1]) {
        for(int j = i + 1; j < n -2; j++) {
          if (j == i + 1 || nums[j] != nums[j - 1]) {
            int left = j + 1;
            int right = n - 1;
            long subTarget = (long)(target - (long)nums[i] - (long)nums[j]);// in case of exceed the boundary
            while (left < right) {
              if (nums[left] + nums[right] == subTarget){
                res.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                while (left < right && nums[left] == nums[left + 1]) left++;
                while (left < right && nums[right] == nums[right - 1]) right--;
                left++;
                right--;
              } else if (nums[left] + nums[right] < subTarget) {
                left++;
              } else {
                right--;
              }
            }
          }
        }
      }
    }
    return res;
  }
}
```

## 454. 4Sum II

### 1. HashTable

```java
class Solution {
  public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
    HashMap<Integer, Integer> map = new HashMap<>();
    int res = 0;
    for (int a : nums1) {
      for (int b : nums2) {
        map.put(a + b, map.getOrDefault(a + b, 0) + 1);
      }
    }
    for (int c : nums3) {
      for (int d : nums4) {
        int sum = 0 - (c + d);
        if (map.containsKey(sum)) {
          res += map.get(sum);
        }
      }
    }
    return res;
  }
}
```

![image-20220701171629217](/Users/youhao/Library/Application Support/typora-user-images/image-20220701171629217.png)

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

### 2. DP

```java
class Solution {
  public int maxProfit(int prices[]) {
    int n = prices.length;
    if (n == 0) {
      return 0;
    }
    int preMin = prices[0];
    int res = 0;
    for (int i = 1; i < n; i++) {
      res = Math.max(res, prices[i] - preMin);
      preMin = Math.min(preMin, prices[i]);
    }
    return res;
  }
}
```



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

## 347. Top K Frequent Elements

### 1. Heap

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int n : nums) {
            map.put(n, map.getOrDefault(n, 0) + 1);
        }
        PriorityQueue<Integer> heap = new PriorityQueue<>((n1, n2) -> map.get(n1) - map.get(n2));
        for (int c : map.keySet()) {
            heap.add(c);
            if (heap.size() > k) {heap.poll();}
        }
        int[] res = new int[k];
        for (int i = 0; i < k; i++) {
            res[i] = heap.poll();
        }
        return res;
    }
  //Time  O(nlogk)
  //Space O(n + k)
}
```

### 2. Bucket Sort

```java
class Solution {
  public int[] topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int n : nums) {
      map.put(n, map.getOrDefault(n, 0) + 1);
    }
    // create a bucket, the bucket's indexes are according to the frequent of the number, max is n
    List<Integer>[] bucket = new List[nums.length + 1];
    for (int num : map.keySet()) {
      int count = map.get(num);
      if (bucket[count] == null) {
        bucket[count] = new ArrayList();
      }
      bucket[count].add(num);
    }
    int[] res = new int[k];
    for (int i = nums.length; i >= 0; i--) {
      if (bucket[i] == null) {continue;}
      while (!bucket[i].isEmpty() && --k >= 0) {//It is guaranteed that the answer is unique.
        res[k] = bucket[i].remove(0);
      }
    }
    return res;
  }
}
```

![image-20220705225840828](/Users/youhao/Library/Application Support/typora-user-images/image-20220705225840828.png)



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

## 1304. Find N Unique Integers Sum up to Zero

```java
class Solution {
    public int[] sumZero(int n) {
        int[] res = new int[n];
        if (n == 1) {return res;}
        if (n % 2 == 0) {
            for (int i = 1; i <= n/2; i++) {
                res[n/2 + i - 1] = i;
                res[n/2 - i] = - i;
            }
        }
        if (n % 2 != 0) {
            for (int i = 0; i <= (n - 1) / 2; i++) {
                res[n / 2 + i] = i;
                res[n / 2 - i] = -i;
            }
        }
        return res;
    }
}
```



# Traverse the 2D array

## 151. Reverse Words in a String

### 1. Reverse the original Array

```java
class Solution {
  public String reverseWords(String s) {
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < s.length(); i++) {
      char cur = s.charAt(i);
      if (cur != ' ') {
        sb.append(cur);
      } else {
        if (sb.length() != 0 && sb.charAt(sb.length() - 1) != ' ') {
          sb.append(cur);
        }
      }
    }
    if (sb.charAt(sb.length() - 1) == ' ') {
      sb.deleteCharAt(sb.length() - 1);
    }
    char[] res = sb.toString().toCharArray();
    reverse(res, 0, sb.length() - 1);
    int start = 0;
    for (int i = 0; i < res.length; i++) {
      char c = res[i];
      if (i == res.length - 1) {
        reverse(res, start, i);
      } else if (c == ' ') {
        reverse(res, start, i - 1);
        start = i + 1;
      }
    }
    return new String(res);
  }
  
  public void reverse(char[] ch, int start, int end) {
    while (start < end) {
      char temp = ch[start];
      ch[start] = ch[end];
      ch[end] = temp;
      start++;
      end--;
    }
  }
}
```

![image-20220731200253861](/Users/youhao/Library/Application Support/typora-user-images/image-20220731200253861.png)



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



# Matrix

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



## 73. Set Metrix Zeroes

### 1. HashTable

```java
class Solution {
  public void setZeroes(int[][] matrix) {
    int n = matrix.length;
    int m = matrix[0].length;
    Set<Integer> row = new HashSet<>();
    Set<Integer> col = new HashSet<>();
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < m; j++) {
        if (matrix[i][j] == 0) {
          row.add(i);
          col.add(j);
        }
      }
    }
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < m; j++) {
        if (row.contains(i) || col.contains(j)) {
          matrix[i][j] = 0;
        }
      }
    }
    return;
  }
}
```

### 2. Constant Space 

```java
class Solution {
  public void setZeroes(int[][] matrix) {
    int n = matrix.length;
    int m = matrix[0].length;
    boolean col = false;
    boolean row = false;
    for (int i = 0; i < n; i++) {
      if (matrix[i][0] == 0) {
        col = true;
      }
    }
    for (int j = 0; j < m; j++) {
      if (matrix[0][j] == 0) {
        row = true;
      }
    }
    for (int i = 1; i < n; i++ ){
      for (int j = 1; j < m; j++) {
        if (matrix[i][j] == 0) {
          matrix[i][0] = 0;
          matrix[0][j] = 0;
        }
      }
    }
    for (int i = 1; i < n; i++ ){
      for (int j = 1; j < m; j++) {
        if (matrix[i][0] == 0 || matrix[0][j] == 0) {
          matrix[i][j] = 0;
        }
      }
    }
    if (col == true) {
      for (int i = 0; i < n; i++) {
        matrix[i][0] = 0;
      }
    }
    if (row == true) {
      for (int j = 0; j < m; j++) {
        matrix[0][j] = 0;
      }
    }
  }
}
```

![image-20220530184206011](/Users/youhao/Library/Application Support/typora-user-images/image-20220530184206011.png)

## 1861. Rotating the Box

###  1. Simple rotating 

```java
class Solution{
  public char[][] rotateTheBox(char[][] box) {
    int r = box.length;
    int col = box[0].length;
    char[][] newBox = new char[c][r];
    for (int i = 0; i < col; i++) {
      for(int j = r - 1; j >= 0; j--) {
        newBox[i][r - 1 - j] = box[j][i];
      }
    }
    return newBox;
  }
}
```



## 289. Game of Life

### 1. Create a new Array

```java
class Solution {
    public void gameOfLife(int[][] board) {
        int n = board.length;
        int m = board[0].length;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                int count = helper(i, j, board);
                if (board[i][j] == 1) {// the cell live
                    if (count == 2 || count == 3) {
                        board[i][j] += 2;
                    }
                } else if (board[i][j] == 0) { // the cell die
                    if (count == 3) {
                        board[i][j] += 2;
                    }
                }
            }
        }
        for (int i = 0; i < n; i++) { 
            for (int j = 0; j < m; j++) {
                board[i][j] >>= 1;
            }
        }
        return;
    }
    public int helper(int row, int col, int[][] board) {
        int n = board.length;
        int m = board[0].length;
        int c = 0;
        for (int i = Math.max(0, row - 1); i <= Math.min(n - 1, row + 1); i++) {
            for (int j = Math.max(0, col - 1); j <= Math.min(m - 1, col + 1); j++) {
                if (i == row && j == col) {
                    continue;
                } else {
                    int cur = board[i][j] & 1;//figure out the current state without the future state
                    if (cur == 1) {
                        c++;
                    }
                }
            }
        }
        return c;
    }
}

```

![image-20220705201224917](/Users/youhao/Library/Application Support/typora-user-images/image-20220705201224917.png)

## 238. Product to Array Except Self

### 1. Create Left and Right Array

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] l = new int[n];
        int[] r = new int[n];
        int[] ans = new int[n];
        l[0] = 1;
        r[n - 1] = 1;
        for (int i = 1; i < n; i++) {
            l[i] = l[i - 1] * nums[i - 1];
        }
        for (int j = n - 2; j >= 0; j--) {
            r[j] = r[j + 1] * nums[j + 1];
        }
        for (int a = 0; a < n; a++) {
            ans[a] = l[a] * r[a];
        }
        return ans;
    }
}
```

![image-20220705204420842](/Users/youhao/Library/Application Support/typora-user-images/image-20220705204420842.png)

### 2. Not using extra space

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int l = 1;
        int r = 1;
        int[] ans = new int[n];
        for (int i = 0; i < n; i++) {
            ans[i] = l;
            l *= nums[i];
        }
        for (int j = n - 1; j >= 0; j--) {
            ans[j] *= r;
            r *= nums[j];
        }
        return ans;
    }
}
```



## 287. Find the Duplicated Number

### 1. Negative Marking ( not satisfy the problem constrains) (P41)

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int n = nums.length;
        int duplicate = -1;
        for (int i = 0; i < n; i++) {
            int cur = Math.abs(nums[i]);
            if (nums[cur] < 0) {
                duplicate = cur;
                break;
            }
            nums[cur] *= -1;
        }
        
        for (int i = 0; i < nums.length; i++) {
            nums[i] = Math.abs(nums[i]);
        }
        return duplicate;
    }
}
```

![image-20220707175819457](/Users/youhao/Library/Application Support/typora-user-images/image-20220707175819457.png)

### 2. Binary Search

```java
class Solution {
  public int findDuplicate(int[] nums) {
    int n = nums.length - 1;
    int r = n; 
    int l = 1;
    int duplicate = -1;
    while (l <= r) {
      int mid = l + (r - l) / 2;
      int count = 0;
      for (int num : nums) {
        if (num <= mid) {
          count++;
        }
      }
      if (count > mid) {
        duplicate = mid;
        r = mid - 1;
      } else {
        l = mid + 1;
      }
    }
    return duplicate;
  }
}
```

![image-20220707214240157](/Users/youhao/Library/Application Support/typora-user-images/image-20220707214240157.png)

### 3. Floyd‘s Tortoise  and Hare (Cycle Detection) ( P142)

```java
class Solution {
  public int findDuplicate(int[] nums) {
    int slow = 0;
    int fast = 0;
    do { // find the meet point 
        slow = nums[slow];
        fast = nums[nums[fast]];
    } while (slow != fast);
    
    fast = 0; // find the cycle point
    while (fast != slow) {
      fast = nums[fast];
      slow = nums[slow];
    }
    return slow;
  }
}
```



![image-20220707211557022](/Users/youhao/Library/Application Support/typora-user-images/image-20220707211557022.png)

## 1207. Unique Number of Occurrences

### 1. HashTable

```java
class Solution {
    public boolean uniqueOccurrences(int[] arr) {
        HashMap<Integer, Integer> map = new HashMap<>();
        HashSet<Integer> set = new HashSet<>();
        for (int num : arr) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        for (int c : map.keySet()) {
            if (set.contains(map.get(c))) {
                return false;
            }
            set.add(map.get(c));
        }
        return true;
    }
}
```



## 128. Longest Consecutive Sequence

### 1. HashSet

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int num : nums) {
            set.add(num);
        }
        int longest = 0;
        for (int num : set) {
            if (!set.contains(num - 1)) {
                int length = 1;
                int cur = num;
                while (set.contains(cur + 1)) {
                    length++;
                    cur++;
                }
                longest = Math.max(longest, length);
            }
        }
        return longest;
    }
}
```

第二次用set遍历防止数组中的重复数字，为什么时间复杂度是O(n)的原因： 我们会去讨论该子序列中最小的数字，如果不是最小的那就不讨论，从最小的开始讨论避免了重复讨论，所以一个子序列只可能遍历一次。



## 163. Missing Number

### 1. Linear Scan

```java
class Solution {
    public List<String> findMissingRanges(int[] nums, int lower, int upper) {
        List<String> res = new ArrayList<>();
        int left = lower - 1;
        int right = lower - 1;
        for (int i = 0; i < nums.length; i++) {
            right = nums[i];
            String str = "";
            if (right - left == 2) {
                    str += (left + 1);
                    res.add(str);
                }
            else if (right - left > 2) {
                    str = (left + 1) + "->" + (right - 1);
                res.add(str);
            }
            left = right;
        }
        if (upper - right == 1) {
            res.add("" + upper);
        }
        if (upper - right > 1) {
            res.add((right + 1) + "->" + upper);
        }
        return res;
    }
}
```



# Arrays-----(Prefix )

## 303. Range Sum Query -Immutable

### 1. Prefix Sum (Cache)

```java
class NumArray {
    private int[] sum;
    public NumArray(int[] nums) {
        int n = nums.length;
        sum = new int[n + 1];
        for (int i = 0; i < n; i++) {
            sum[i + 1] = sum[i] + nums[i];
        }
    }
    
    public int sumRange(int left, int right) {
        return sum[right + 1] - sum[left];
    }
}

```

### 2. Prefix Sum (Using Map)

```java
class NumArray{
  private Map<Integer, Integer> map;
  public NumArray(int[] nums) {
    map = new HashMap<>();
    int sum = 0;
    map.put(-1, sum);
    for (int i = 0; i < nums.length; i++){
      sum += nums[i];
      map.put(i, sum);
    }
  }
  public int sumRange(int left, int right) {
    return map.get(right) - map.get(left - 1);
  }
}
```

![image-20220730211208340](/Users/youhao/Library/Application Support/typora-user-images/image-20220730211208340.png)



## 304. Range Sum Query 2D - immutable

### 1. Using Cashing

```java
class NumMatrix {
    private int[][] presum;
    public NumMatrix(int[][] matrix) {
        int n = matrix.length;
        int m = matrix[0].length;
        presum = new int[n + 1][m + 1];
        for (int i = 1; i <= n; i++){
            for (int j = 1; j <= m; j++) {
                presum[i][j] = presum[i][j - 1] - presum[i - 1][j - 1] + matrix[i - 1][j - 1] + presum[i - 1][j];
            }
        }
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        return presum[row2 + 1][col2 + 1] - presum[row2  + 1][col1] - presum[row1][col2 + 1] + presum[row1][col1];
    }
}

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * int param_1 = obj.sumRegion(row1,col1,row2,col2);
 */
```

![image-20220730213643817](/Users/youhao/Library/Application Support/typora-user-images/image-20220730213643817.png)





# Arrays----Differences

## 1094. Car Pooling

```java
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        // 最多有 1000 个车站
        int[] nums = new int[1001];
        // 构造差分解法
        Difference df = new Difference(nums);

        for (int[] trip : trips) {
            // 乘客数量
            int val = trip[0];
            // 第 trip[1] 站乘客上车
            int i = trip[1];
            // 第 trip[2] 站乘客已经下车，
            // 即乘客在车上的区间是 [trip[1], trip[2] - 1]
            int j = trip[2] - 1;
            // 进行区间操作
            df.increment(i, j, val);
        }

        int[] res = df.result();

        // 客车自始至终都不应该超载
        for (int i = 0; i < res.length; i++) {
            if (capacity < res[i]) {
                return false;
            }
        }
        return true;
    }

    // 差分数组工具类
    class Difference {
        // 差分数组
        private int[] diff;

        /* 输入一个初始数组，区间操作将在这个数组上进行 */
        public Difference(int[] nums) {
            assert nums.length > 0;
            diff = new int[nums.length];
            // 根据初始数组构造差分数组
            diff[0] = nums[0];
            for (int i = 1; i < nums.length; i++) {
                diff[i] = nums[i] - nums[i - 1];
            }
        }

        /* 给闭区间 [i, j] 增加 val（可以是负数）*/
        public void increment(int i, int j, int val) {
            diff[i] += val;
            if (j + 1 < diff.length) {
                diff[j + 1] -= val;
            }
        }

        /* 返回结果数组 */
        public int[] result() {
            int[] res = new int[diff.length];
            // 根据差分数组构造结果数组
            res[0] = diff[0];
            for (int i = 1; i < diff.length; i++) {
                res[i] = res[i - 1] + diff[i];
            }
            return res;
        }
    }

}
```

### 基本思路

相信你已经能够联想到差分数组技巧了：**`trips[i]` 代表着一组区间操作，旅客的上车和下车就相当于数组的区间加减；只要结果数组中的元素都小于 `capacity`，就说明可以不超载运输所有旅客**。

这题还有一个细节，一批乘客从站点 `trip[1]` 上车，到站点 `trip[2]` 下车，呆在车上的站点应该是 `[trip[1], trip[2] - 1]`，这是需要被操作的索引区间。



## 1109. Corporate Flight Bookings

```java
class Solution {
    public int[] corpFlightBookings(int[][] bookings, int n) {
        int[] labels = new int[n];
        Difference df = new Difference(labels);
        for (int[] booking : bookings) {
            int value = booking[2];
            int i = booking[0] - 1;
            int j = booking[1] - 1;
            df.increment(i, j, value);
        }
        int[] res = df.result();
        return res;  
    }
    class Difference {
        int[] diff; 
        public Difference(int[] nums) {
            diff = new int[nums.length];
            diff[0] = nums[0];
            for (int i = 1; i < diff.length; i++) {
                diff[i] = nums[i] - nums[i - 1];
            }
        }
        
        public void increment(int i, int j, int x) {
            diff[i] += x;
            if (j + 1 < diff.length) {
                diff[j + 1] -= x;
            }
        }
        public int[] result() {
            int[] res = new int[diff.length];
            res[0] = diff[0];
            for (int i = 1; i < diff.length; i++) {
                res[i] = res[i - 1] + diff[i];
            }
            return res;
        }
    }
}
```



## 370. Range Addition

### 1. Using Difference Class

```java
class Solution {
    public int[] getModifiedArray(int length, int[][] updates) {
        int[] arr = new int[length];
        Difference df = new Difference(arr);
        for (int[] update : updates) {
            int i = update[0];
            int j = update[1];
            int val = update[2];
            df.increment(i, j, val);
        }
        int[] res = df.result();
        return res;
    }
    
    class Difference{
        private int[] diff;
        public Difference(int[] nums) {
            diff = new int[nums.length];
            diff[0] = nums[0];
            for (int i = 1; i < diff.length; i++) {
                diff[i] = nums[i] - nums[i - 1];
            }
        }
        
        public void increment(int i, int j, int x) {
            diff[i] += x;
            if (j + 1 < diff.length) {
                diff[j + 1] -= x;
            }
        }
        
        public int[] result() {
            int[] res = new int[diff.length];
            res[0] = diff[0];
            for (int i = 1; i < res.length; i++) {
                res[i] = res[i - 1] + diff[i];
            }
            return res;
        }
    }
}
```



## 1996. The Number of Weak Characters in the Game

### 1. Sorting

```java
class Solution {
    public int numberOfWeakCharacters(int[][] properties) {
        Arrays.sort(properties,(a, b) -> (a[0] == b[0]) ? (b[1] - a[1]) : (a[0] - b[0]));
        int maxDefense = 0;
        int count = 0;
        for (int i = properties.length - 1; i >= 0; i--) {
            if (properties[i][1] < maxDefense) {
                count++;
            }
            maxDefense = Math.max(maxDefense, properties[i][1]);
        }
        return count;
    }
}
```

**Algorithm**

1. Sort the list of pairs `properties` in ascending order of their attack and then in descending order of the defense value. We can achieve it using the custom comparator.

2. Initialize the maximum defense value achieved `maxDefense` to `0`.

3. Iterate over the pairs from right to left and for each pair:

   a. If the `maxDefense` is greater than the defense value of the current character, increment the value `weakCharacters`.

   b. Update the value of `maxDefense` if it's smaller than the current defense value.

4. Return `weakCharacters`.





# Comparator and Comparable (比较器)

## 252. Meeting Rooms

### 1. Sort (Lambada)

```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        Arrays.sort(intervals, (a , b) -> Integer.compare(a[0], b[0]));
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i - 1][1] > intervals[i][0]) {
                return false;
            }
        }
        return true;
    }
}
```



### 2. Sort (外部Comparator普通写法)

```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        Arrays.sort(intervals, new MyCompare());
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i - 1][1] > intervals[i][0]) {
                return false;
            }
        }
        return true;
    }
    
    class MyCompare implements Comparator<int[]>{
        @Override
        public int compare(int[] a, int[] b) {
            return Integer.compare(a[1], b[1]);
        }
    }
}
```



