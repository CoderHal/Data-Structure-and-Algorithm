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

negative 存储的永远是在当前位置中，只有一个负数情况下最长的序列（序列中包括了偶数个负数，因为两个负数✖️大于0，所以把序列中的负数看成正数。

##  1014. Best Sightseeing Pair

### 1. DP

```java
class Solution {
  public int maxScoreSightseeingPair(int[] values) {
    int n = values.length;
    if (n <= 1) {
      return 0;
    }
    int preId = 0;
    int res = 0;
    for (int i = 1; i < n; i++) {
      res = Math.max(res, values[preId] + preId + values[i] - i);
      if (preId + values[preId] <= values[i] + i) {
        preId = i;
      }
    }
    return res;
    //Time O(n)
    //Space O(1) 
  }
}
```

![image-20220503151948468](/Users/youhao/Library/Application Support/typora-user-images/image-20220503151948468.png)

## 122. Best Time to Buy and Sell Stock II

### 1. DP 

```java
class Solution {
  public int maxProfit(int[] prices) {
    int n = prices.length;
    if (n <= 1) {
      return 0;
    }
    int pre = 0;
    int cur = 0;
    int sum = 0;
    for (int i = 1; i < n; i++) {
      cur = prices[i];
      pre = prices[i - 1];
      if (cur > pre) {
        sum += (cur - pre);
      }
    }
    return sum;
    // Time O(n)
    // Space O(1)
  }
}
```

![image-20220503155348568](/Users/youhao/Library/Application Support/typora-user-images/image-20220503155348568.png)



## 309. Best Time to Buy and Sell Stock with Cooldown

### 1. DP

```java
class Solution {
  public int maxProfit(int[] prices) {
    int n = prices.length;
    int hold = Integer.MIN_VALUE;// this value is important! If this hold is bigger than prices[i], the first hold is not correct
    int cooled = 0;
    int sold = 0;
    for (int i = 0; i < n; i++) {
      int hold2 = hold, cooled2 = cooled, sold2 = sold;
      int p = prices[i];
      hold = Math.max(hold2, cooled - p);
      sold = hold2 + p;
      cooled = Math.max(cooled2, sold2);
    }
    return Math.max(sold, hold);
  }
}
```

the current state  only has three case: 

Hold:  max(hold, cooled - p)

Sold:  hold + p

Cooled: max(cooled, hold)



### 714. Best Time to Buy and Sell Stock with Transaction Fee

### 1. DP

```java
class Solution {
  public int maxProfit(int[] prices, int fee) {
    int n = prices.length;
    if(n <= 1) {return 0;}
    int cash = 0;
    int hold = -p[0];// this value is important! this value is important! If this hold is bigger than prices[i], the first hold is not correct
    for (int i = 0; i < n; i++) {
      int p = prices[i];
      int cash2 = cash;
      int hold2 = hold;
      cash = Math.max(cash2, hold2 + p - fee);
      hold = Math.max(hold2, cash2 - p);
    }
    return cash;
  }
}
```

The current state only has two cases: 

Cash : max(cash,  hold + p - fee)

hold: max(hold, cash - p)

cash stands who the value we have now

hold stands how the stock we have bought



Hold: max(hold, stay, sold + p - fee)

Sold: hold - p

Stay: 

## 139. Word Break

### 1. DP

```java
class Solution {
  public boolean wordBreak(String s, List<String> wordDict) {
    Set<String> set = new HashSet<>(wordDict);
    boolean[] dp = new boolean[s.length() + 1];
    dp[0] = true;
    for (int i = 1; i <= s.length(); i++){
      for (int j = 0; j < i; j++) {
        if (dp[j] && set.contains(s.substring(j, i))) {
          dp[i] = true;
          break;
        }
      }
    }
    return dp[s.length()];
    //O(n^3) 2loops and the substring is the 3rd loops
  }
}
```

![image-20220505152402816](/Users/youhao/Library/Application Support/typora-user-images/image-20220505152402816.png)

## 42. Trapping Rain Water

### 1. Finding the Heighest 

```java
class Solution {
    public int trap(int[] height) {
        int mostHeightIndex = 0;
        int n = height.length;
        for (int i = 0; i < n; i++) {
            if (height[i] >= height[mostHeightIndex]) {
                mostHeightIndex = i;
            }
        }
        int leftTrap = 0;
        int leftMost = 0;
        for (int i = 0; i < mostHeightIndex; i++) {
            leftMost = Math.max(leftMost, height[i]);
            leftTrap += leftMost - height[i];
        }
        int rightMost = 0;
        int rightTrap = 0;
        for (int j = n - 1; j > mostHeightIndex; j--) {
            rightMost = Math.max(rightMost, height[j]);
            rightTrap += rightMost - height[j];
        }
        return rightTrap + leftTrap;
      //Time O(n) 
      //Space O(1)
    }
}
```

![image-20220505160317702](/Users/youhao/Library/Application Support/typora-user-images/image-20220505160317702.png)

### 2. Monotonous Stack

```java
class Solution {
  public int trap(int[] height) {
    int current = 0;
    int res = 0;
    Stack<Integer> stack = new Stack<>();
    while (current < height.length) {
      while (!stack.isEmpty() && height[stack.peek()] < height[current]) {
        int pre = stack.pop();
        if (stack.isEmpty()) {
          break;
        }
        int pre2 = stack.peek();
        res += (Math.min(height[pre2], height[current]) - height[pre]) * (current - pre2 - 1);
      }
      stack.push(current++);
    }
    return res;
    // Time O(n)
    // Space O(n)
  }
}
```

![image-20220505164353209](/Users/youhao/Library/Application Support/typora-user-images/image-20220505164353209.png)



## 413. Arithmetic Slices

### 1. DP

```java
class Solution {
  public int numberOfArithmeticSlices(int[] nums) {
    int n = nums.length;
    if (n <= 1) {
      return 0;
    }
    int res = 0;
    int[] dp = new int[n];
    dp[0] = 0;
    dp[1] = 0;
    for (int i = 2; i < n; i++) {
      if (nums[i] - nums[i - 1] == nums[i - 1] - nums[i - 2]) {
        dp[i] = dp[i - 1] + 1;
      }
      res += dp[i];
    }
    return res;
  }
}
```

### 2. DP with constant space

```java
class Solution {
  public int numberOfArithmeticSlices(int[] nums) {
    int n = nums.length;
    if (n <= 1) {
      return 0;
    }
    int res = 0;
    int pre = 0;
    int cur = 0;
    for (int i = 2; i < n; i++) {
      if (nums[i] - nums[i - 1] == nums[i - 1] - nums[i - 2]) {
        cur = pre + 1;
      } else {
          cur = 0;
      }
      pre = cur;
      res += cur;
    }
    return res;
  }
}
```

![image-20220509210313599](/Users/youhao/Library/Application Support/typora-user-images/image-20220509210313599.png)

## 91. Decode Ways

### 1. DP

```java
class Solution {
  public int numDecodings(String s) {
    int n = s.length();
    if (n == 0 || s.charAt(0) == '0') {
      return 0;
    }
    int[] dp = new int[n + 1];
    dp[0] = 1;
    dp[1] = 1;
    for (int i = 2; i <= n; i++) {
      if (s.charAt(i - 1) != '0') {
        dp[i] += dp[i - 1];
      }
      int cur = Integer.valueOf(s.substring(i - 2, i));
      if (cur >= 10 && cur <= 26) {
        dp[i] += dp[i - 2];
      }
    }
    return dp[n];
  }
  // O(n)
  // O(n)
}
```

![image-20220510170118998](/Users/youhao/Library/Application Support/typora-user-images/image-20220510170118998.png)

## 118. Pascal's Triangle

### 1. DP 

```java
class Solution {
  public List<List<Integer>> generate(int numRows) {
    List<List<Integer>> res = new ArrayList<>();
    if (numRows == 0) {
      return res;
    }
    List<Integer> pre = new ArrayList<>();
    pre.add(1);
    res.add(new ArrayList<>(pre));
    for (int i = 1; i < numRows; i++) {
      List<Integer> cur = new ArrayList<>();
      cur.add(1);
      for (int j = 1; j < i; j++) {
        int curNode = pre.get(j - 1) + pre.get(j);
        cur.add(curNode);
      }
      cur.add(1);
      pre = cur;
      res.add(new ArrayList<>(cur));
    }
    return res;
  }
}
```

cur: index = i    Therefore, the value of this index: pre[j - 1] + pre[j] 



## 119. Pascal's Triangle II

### 1. DP

```java
class Solution {
  public List<Integer> getRow(int rowIndex) {
    List<Integer> res = new ArrayList<>();
    if (rowIndex < 0) {
      return res;
    } 
    List<Integer> pre = new ArrayList<>();
    pre.add(1);
    res = pre;
    for (int i = 1; i <= rowIndex; i++) {
      List<Integer> cur = new ArrayList<>();
      cur.add(1);
      for (int j = 1; j < i; j++) {
        cur.add(pre.get(j - 1) + pre.get(j));
      }
      cur.add(1);
      pre = cur;
      res = cur;
    }
    return res;
  }
}
```

## 62. Unique Paths

### 1. DP

```java
class Solution {
  public int uniquePaths(int m, int n) {
    int paths = 0;
    int[][] matrix = new int[m][n];
    matrix[0][0] = 1;
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (i + j != 0) {
          if (i - 1 < 0) {
            matrix[i][j] = matrix[i][j - 1];
          }
          else if (j - 1 < 0) {
            matrix[i][j] = matrix[i - 1][j];
          }
          else {
            matrix[i][j] = matrix[i - 1][j] + matrix[i][j - 1];
          }
        }
      }
    }
    return matrix[m - 1][n - 1];
  }
}
```

