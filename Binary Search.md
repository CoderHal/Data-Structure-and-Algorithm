# BinarySearch

# 算法思想

# 二分查找框架

```java
int binarySearch(int[] nums, int target) {
    int left = 0, right = ...;

    while(...) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            ...
        } else if (nums[mid] < target) {
            left = ...
        } else if (nums[mid] > target) {
            right = ...
        }
    }
    return ...;
}
```

尽量不要使用else，把每个条件都写得清清楚楚 left + (right - left) / 2能够有效防止溢出，(right + left) / 2有可能发生溢出

## 1. 寻找一个数

```java
int binarySearch(int[] nums, int target) {
    int left = 0, right = nums.length - 1;//注意

    while(left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid + 1; // 注意
        } else if (nums[mid] > target) {
            right = mid - 1; //注意
        }
    }
    return -1;
}
```

### (1) 为什么不是(left < right) ?

因为right 为 nums.length - 1, 所以搜索区间为 [left, right], 两端都是闭区间

### (2) 终止条件

while(left <= right) 意味着终止条件为 left = right + 1, 区间为[right + 1, right], 不可能出现这种区间，所以结果可以直接返回 -1，表示没找到

while(left < right) 意味着终止条件为 left = right, 区间为[right, right], 此时不是非空区间。我们需要讨论nums[right] 是否等于target，再决定是否返回-1

### (3) 为什么是left = mid + 1, right = mid - 1? 

首先要明确搜索区间，如果nums[mid]不是target目标，说明我们不需要从mid为边界了，就是[left, mid - 1] 或者 [mid + 1, right]



## 2. 寻找左侧边界的二分搜索

```java
int left_bound(int[] nums, int target) {
  int left = 0;
  int right = nums.length;//注意
  
  while (left < right) {
    int mid = left + (right - left) / 2;
    if (nums[mid] == target) {
      right = mid;
    } else if (nums[mid] < target) {
      left = mid + 1;
    } else if (nums[mid] > target) {
      right = mid; //注意
    }
  }
  return left;
}
```

### (1) 为什么while不是 <= 而是 <

还是看搜索区间，此时right = nums.length，所以应该是[left, right)， 而循环终止条件是 left = right也就是 [left,  left)是空的，所以可以终止。

### (2) 没有返回-1操作？如果不存在target怎么办

返回的时候判断nums[left] 是否等于 target就行了。 在判断之前，需要讨论left 是否越界

```java
while (left < right) {
    //...
}
// 此时 target 比所有数都大，返回 -1
if (left == nums.length) return -1;
// 判断一下 nums[left] 是不是 target
return nums[left] == target ? left : -1;

```

### (3) 为什么是left = mid + 1， right = mid? 

因为是左闭右开区间 [left, right), 所以如果nums[mid] 被检索了之后， [left, mid)， 和 [mid + 1, right)是符合的，避免了重复检测mid

### (4) 为什么该算法能够搜索左侧边界

因为我们要求nums[mid] = target时, right = mid, 也就是从[left, mid)中继续搜索，不断的向左收缩，就能达到左侧边界

### (5) 返回left， right 都一样

因为终止条件是left = right

### (6) 按照 while (left <= right) 来修改模版

```java
public int left_bound(int[] nums, int target) {
  int left = 0;
  int right = nums.length - 1;
  while (left <= right) {
    int mid = left + (right - left) / 2;
    if (nums[mid] == target) {
      right = mid - 1;
    } else if (nums[mid] > target) {
      right = mid - 1;
    } else if (nums[mid] < target) {
      left = mid + 1;
    }
  }
  if (left == nums.length) return -1;
  // 判断一下 nums[left] 是不是 target
  return nums[left] == target ? left : -1;
}
```



## 3. 寻找右侧边界的二分搜索

```java
public int right_bound(int[] nums, int target) {
  int left = 0; 
  int right = nums.length; 
  while (left < right) {
    int mid = left + (right - left) / 2;
    if (nums[mid] == target) {
      left = mid + 1;//注意
    }
    else if (nums[mid] < target) {
      left = mid + 1;
    }
    else if (nums[mid] > target) {
      right = mid;
    }
  }
  return left - 1;//注意
}
```

### (1) 为什么到达右侧边界

当nums[mid] == target 时， 我们需要将left 往右收缩 --> left = mid + 1 去寻找我们的右边界

### (2) 为什么返回left - 1 而不是 left

首先终止条件是 left = right, 和左侧边界不同，左侧中一直是right = mid所以能等于target，只需要返回left。 因为我们更新的是left = mid + 1, 也就是说nums[left] 肯定不等于target了 (参考if条件语句)，所以我们要返回left - 1，去寻找我们的右边界

### (3) 如果不存在target

和左侧一样，我们讨论nums[left - 1]是否等于target

```java
while (left < right) {
    // ...
}
// 判断 target 是否存在于 nums 中
// 此时 left - 1 索引越界
if (left - 1 < 0) return -1;
// 判断一下 nums[left] 是不是 target
return nums[left - 1] == target ? (left - 1) : -1;
```

### (4) 按照While (left <= right)来修改

```java
public int right_bound(int[] nums, int target) {
  int left = 0;
  int right = nums.length - 1;
  while (left <= right) {
    int mid = left + (right - left) / 2;
    if (nums[mid] == target) {
      // 这里改成收缩左侧边界即可
      left = mid + 1;
    } else if (nums[mid] < target) {
      left = mid + 1;
    } else if (nums[mid] > target) {
      right = mid - 1;
    }
  }
  // 最后改成返回 left - 1
    if (left - 1 < 0) return -1;
    return nums[left - 1] == target ? (left - 1) : -1;
}
```

## 4. 三种情况的统一模版

```java
int binary_search(int[] nums, int target) {
    int left = 0, right = nums.length - 1; 
    while(left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1; 
        } else if(nums[mid] == target) {
            // 直接返回
            return mid;
        }
    }
    // 直接返回
    return -1;
}

int left_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，锁定左侧边界
            right = mid - 1;
        }
    }
    // 判断 target 是否存在于 nums 中
    // 此时 target 比所有数都大，返回 -1
    if (left == nums.length) return -1;
    // 判断一下 nums[left] 是不是 target
    return nums[left] == target ? left : -1;
}

int right_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，锁定右侧边界
            left = mid + 1;
        }
    }
    // 此时 left - 1 索引越界
    if (left - 1 < 0) return -1;
    // 判断一下 nums[left] 是不是 target
    return nums[left - 1] == target ? (left - 1) : -1;
}

```



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

## 240. Search a 2D Matrix II (Not Binary Search)

### 1. Search Space Reduction

```java
class Solution {
  public boolean searchMatrix(int[][] matrix, int target) {
        int n = matrix.length;
        int m = matrix[0].length;
        int row = 0;
        int col = m - 1;
        while (row < n && col >= 0) {
            int cur = matrix[row][col];
            if (target > cur) {
                row++;
            } else if (target < acur) {
                col--;
            } else {
                return true;
            }
        }
        return false;
    }
}
```

![image-20220704152225376](/Users/youhao/Library/Application Support/typora-user-images/image-20220704152225376.png)



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

## 378. Kth Smallest Element in a Sorted Matrix

### 1. Binary Search

```java
class Solution {
  public int kthSmallest(int[][] matrix, int k) {
    int n = matrix.length;
    int matrixMin = matrix[0][0];
    int matrixMax = matrix[n - 1][n - 1];
    while (matrixMin < matrixMax) {
      int mid = matrixMin + (matrixMax - matrixMin) / 2;
      int count = checkCount(matrix, mid);
      if (count < k) {
        matrixMin = mid + 1;
      } else {
        matrixMax = mid;
      }
    }
    return matrixMax;
  }
  public int checkCount(int[][] matrix, int mid) {
    int n = matrix.length;
    int row = 0;
    int col = n - 1;
    int count = 0;
    while (row < n && col >= 0) {
      if (matrix[row][col] <= mid) {
        count += col + 1;
        row++;
        
      } else {
        col--;
      }
    }
    return count;
  }
}
```

![image-20220702203305433](/Users/youhao/Library/Application Support/typora-user-images/image-20220702203305433.png)

## 528. Random Pick with Weight

### 1. Binary Search

```java
class Solution {
    private int[] prefixSum;
    private Random rand = new Random();
    public Solution(int[] w) {
        prefixSum = new int[w.length + 1];
        for (int i = 1; i < prefixSum.length; i++) {
            prefixSum[i] = w[i - 1] + prefixSum[i - 1];
        }
    }
    
    public int pickIndex() {
        int target = rand.nextInt(prefixSum[prefixSum.length - 1]) + 1;
        return leftBound(prefixSum, target) - 1;
    }
    public int leftBound(int[] pS, int t) {
        int left = 0;
        int right = pS.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (pS[mid] == t) {
                return mid;
            } else if (pS[mid] > t) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(w);
 * int param_1 = obj.pickIndex();
 */
```





## 4. Median of Two Sorted Arrays

### 1. Merge Sort

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int[] merge = new int[nums1.length + nums2.length];
        int l1 = 0; 
        int l2 = 0;
        int index = 0;
        while (l1 < nums1.length && l2 < nums2.length) {
            if (nums1[l1] > nums2[l2]) {
                merge[index] = nums2[l2];
                l2++;
                index++;
            } else {
                merge[index] = nums1[l1];
                l1++;
                index++;
            }
        }
        if (l1 != nums1.length) {
            for (int i = l1; i < nums1.length; i++) {
                merge[index] = nums1[i];
                index++;
            }
        } else {
            for (int i = l2; i < nums2.length; i++) {
                merge[index] = nums2[i];
                index++;
            }
        }
        
        int n = merge.length;
        if (n % 2 == 1) {
            return (double)merge[(n - 1) / 2];
        } else {
            int r1 = merge[n / 2];
            int r2 = merge[n / 2 - 1];
            return (double)(r1 + r2) / 2;
        }
    }
}
```



### 2. Binary Search

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if(nums1.length > nums2.length) {
            int[] temp = nums1;
            nums1 = nums2;
            nums2 = temp;
        }
        int n1 = nums1.length;
        int n2 = nums2.length;
        int low = 0;
        int high = nums1.length;
        int partition1 = 0;
        int partition2 = 0;
        while (low <= high) {
            partition1 = (low + high) / 2;
            partition2 = (n1 + n2 + 1) / 2 - partition1;
            int maxLeftX = (partition1 == 0) ? Integer.MIN_VALUE:nums1[partition1 - 1];
            int maxLeftY = (partition2 == 0) ? Integer.MIN_VALUE:nums2[partition2 - 1];
            int minRightX = (partition1 == n1) ? Integer.MAX_VALUE: nums1[partition1];
            int minRightY = (partition2 == n2) ? Integer.MAX_VALUE: nums2[partition2];
            if (maxLeftX <= minRightY && maxLeftY <= minRightX) {
                if ((n1 + n2) % 2 == 0) {
                    int r1 = Math.max(maxLeftX, maxLeftY);
                    int r2 = Math.min(minRightX, minRightY);
                    return ((double)(r1 + r2) / 2);
                } else {
                    return (double) (Math.max(maxLeftX, maxLeftY));
                }
            } else if (maxLeftX > minRightY) {
                high = partition1 - 1;
            } else {
                low = partition1 + 1;
            }
        } 
        return -1;
    }
}
```

![image-20220802213543112](/Users/youhao/Library/Application Support/typora-user-images/image-20220802213543112.png)



## 1011. Capacity To ship Package Within D days

### 1. Binary Search

```java
class Solution {
  public int shipWithinDays(int[] weights, int days) {
    int left = 0; 
    int right = 0;
    for (int weight : weights) {
      left = Math.max(left, weight); //求最大天数，每天只能运一个
      right += weight; //最小天数，一天全运完
    }
    while (left <= right) {
      int mid = left + (right - left) / 2;
      int needDays = helper(weights, mid);
      if (needDays == days) {
        right = mid - 1;
      }
      else if (needDays > days) {
        left = mid + 1;
      }
      else if (needDays < days) {
        right = mid - 1;
      }
    }
    return left;
  }
  
  public int helper(int[] weights, int capacity) {
    int cur = capacity;
    int res = 1;
    for (int weight : weights) {
      if (weight > cur) {
        day++;
        cur = capacity;
      }
      cur -= weight;
    }
    return res;
  }
}
```

找到最大装载量和最小装载量， 在这个范围内利用二分查找去找和target days 相等的天数，并且要靠左边界搜索，因为我们要求最小装载量



## 410. Split Array Largest Sum

### 1. Binary Search

```java
class Solution {
    public int splitArray(int[] nums, int m) {
        int left = 0;
        int right = 0;
        for (int num : nums) {
            left = Math.max(left, num);
            right += num;
        }
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int needGroup = split(nums, mid);
            if (needGroup == m) {
                right = mid - 1;
            }
            else if (needGroup > m) {
                left = mid + 1;
            }
            else if (needGroup < m) {
                right = mid - 1;
            }
        }
        return left;
    }
    
    public int split(int[] nums, int cap) {
        int cur = cap;
        int res = 1;
        for (int num : nums) {
            if (num > cur) {
                cur = cap;
                res++;
            }
            cur -= num;
        }
        return res;
    }
}
```

Same as the 1011. 计算一个数组的最大容量和最小容量， 以[最小容量，最大容量]为范围，去搜索左侧边界寻找在能分成m组的情况下，最小的容量。



## 875. Koko Eating Bananas

### 1. Binary Search

```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int left = 1;
        int right = 0;
        for (int pile : piles) {
            right = Math.max(right, pile);
        }
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int needHours = helper(piles, mid);
            if (needHours <= 0) {//进行溢出处理
                left = mid + 1;
            }
            else if (needHours == h) {
                right = mid - 1;
            }
            else if (needHours > h) {
                left = mid + 1;
            }
            else if (needHours < h) {
                right = mid - 1;
            }
        } 
        return left;
    }
    
    public int helper(int[] piles, int k) {

        int res = 0;
        for (int pile : piles) {
            int thisPile = pile / k;
            res += thisPile; //这个地方超出了上界 溢出之后变成了负数
            if (pile % k > 0) {
                res++;
            }
        }
        return res;
    }
}
```

这道题一定要考虑int类型溢出问题

