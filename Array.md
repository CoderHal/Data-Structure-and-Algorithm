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

# 