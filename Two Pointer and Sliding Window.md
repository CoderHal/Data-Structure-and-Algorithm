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

## 26. Remove Duplicates from Sorted Array

### 1. Two Pointers

```java
class Solution {
  public int removeDuplicates(int[] nums) {
    int n = nums.length;
    if (n <= 1) {
      return n;
    }
    int count = 1;
    for (int i = 1; i < n; i++) {
      if (nums[i] != nums[count - 1]) {
        nums[count++] = nums[i];
      }
    }
    return count;
  }
}
```



## 80. Remove Duplicates from Sorted Array II

### 1. Two Pointers

```java
class Solution {
  public int removeDuplicates(int[] nums) {
    int n = nums.length;
    if (n <= 2) {
      return n;
    }
    int p = 2;
    for (int i = 2; i < n; i++) {
      if(nums[p - 2] != nums[i]) {
        nums[p++] = nums[i];
      }
    }
    return p;
  }
}
```

### 总结

无论是保证数字的重复次数是多少次，P的范围内永远是合理的。如果重复的数字超出了限制， p会停止在超出限制的第一位上，等待下一个合理的数字，然后进行置换。

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



## 27. Romove Element

### 1. Two Pointer

```java
class Solution {
  public int removeElement(int[] nums, int val) {
    int n = nums.length;
    if (n == 0) {
      return 0;
    }
    int left = 0;
    int right = n;
    while (left < right) {
     if (nums[left] == val) {
       nums[left] = nums[right - 1];
       right--;
     } else {
       left++;
     }
    }
    return right;
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

## 88. Merge Sorted Array

### 1. Two Pointers

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        if (n == 0) {
            return;
        }
        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;
        while (i >= 0 && j >= 0) {
            if (nums1[i] > nums2[j]) {
                nums1[k--] = nums1[i--];
            }else{
                nums1[k--] = nums2[j--];
            }
        }
        while (j >= 0) {
            nums1[k--] = nums2[j--];
        }
        return;
    }
}

```



## 75. Sort Color

### 1. Two-Pass

```java
class Solution {
    public void sortColors(int[] nums) {
        int zero = 0;
        int two = 0;
        int one = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) {
                zero++;
            }
            else if (nums[i] == 1) {
                one++;
            }
            else {
                two++;
            }
        }
        for (int i = 0; i < nums.length; i++) {
            if (zero != 0) {
                zero--;
                nums[i] = 0;
            } 
            else if (one != 0) {
                one--;
                nums[i] = 1;
            }
            else {
                two--;
                nums[i] = 2;
            }
        }
        return;
    }
}

```

### 2. Two Pointers

```java
class Solution {
  public void sortColors(int[] nums) {
    int p0 = 0;
    int p1 = nums.length - 1;
    int i = 0;
    while (p1 >= i) {
      if (nums[i] == 0) {
        swap(nums, i, p0);
        i++;
        p0++;
      }
      else if (nums[i] == 1) {
        i++;
      }
      else {
        swap(nums,i, p1);
        p1--;
      }
    }
    return;
  }
  public void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
  }
}
```

![69421654046617_.pic](/Users/youhao/Library/Containers/com.tencent.xinWeChat/Data/Library/Application Support/com.tencent.xinWeChat/2.0b4.0.9/5c30e188b46dd764edba02dc2a7d8db0/Message/MessageTemp/debebe7def205d28f6eae426b86e9ea2/Image/69421654046617_.pic.jpg)
