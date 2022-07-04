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