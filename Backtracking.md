# Backtracking 

## Template of Backtracking

Backtrack( )

1. Base Case

2. For each possibility p

   ​	a. Memorize current state

   ​	b. Backtrack(next_state)

   ​	c. Restore current state

## 78. Subsets

### 1. Cascading

```java
class Solution {
  public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    res.add(new ArrayList<Integer>());
    for (int num : nums) {
      List<List<Integer>> newSubsets = new ArrayList<>();
      for (List<Integer> cur : res) {
        newSubsets.add(new ArrayList<Integer>(cur){{add(num)}}));
      }
      for (List<Integer> curr : newSubsets) {
        res.add(new ArrayList<>(curr));	
      }
    }
    return res;
    // Time O(n * 2^n)
    // Space O(n * 2^n)
  }
}
```

![image-20220411181205504](/Users/youhao/Library/Application Support/typora-user-images/image-20220411181205504.png)

### 2. Backtracking

```java
class Solution {
  public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> temp = new ArrayList<>();
    backtrack(nums, res, temp, 0);
    return res;
  }
  public void backtrack(int[] nums, List<List<Integer>> res, List<Integer> temp, int index) {
    res.add(new ArrayList<>(temp));
    for (int i = index; i < nums.length; i++) {
      temp.add(nums[i]);
      backtrack(nums, res, temp, i + 1);
      temp.remove(temp.size() - 1);
    }
    //Time O(n * 2^n);
    //Space O(n) We are using O(N) space to maintain curr, and are modifying curr in-place with backtracking. Note that for space complexity analysis, we do not count space that is only used for the purpose of returning output, so the output array is ignored.
  }
}
```

![image-20220411181911963](/Users/youhao/Library/Application Support/typora-user-images/image-20220411181911963.png)

![image-20220411181925632](/Users/youhao/Library/Application Support/typora-user-images/image-20220411181925632.png)



## 90. Subsets II

### 1. Cascading

```java
class Solution {
  public List<List<Integer>> subsetsWithDup(int[] nums) {
    Arrays.sort(nums); 
    List<List<Integer>> res = new ArrayList<>();
    res.add(new ArrayList<Integer>());
    int subsetsSize = 0;
    for (int i = 0; i < nums.length; i++) {
      int startIndex = (i > 0 && nums[i] == nums[i - 1]) ? subsetsSize : 0; // record the pre size of res
      subsetSize = res.size();
      for (int j = startIndex; j < subsetSize; j++) {
        List<Integer> path = new ArrayList<>(res.get(j));
        path.add(nums[i]);
        res.add(path);
      }  
    }
    return res;
    // Time O(n * 2^n) // n * 2^n + nlogn
    // Space O(logn) // Sort logN
  }
}
```

The space complexity of the sorting algorithm depends on the implementation of each programming language. For instance, in Java, the Arrays.sort() for primitives is implemented as a variant of quicksort algorithm whose space complexity is O*(log*n*). 

### 2. Backtracking

```java
class Solution {
  public List<List<Integer>> subsetsWithDup(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> res = new ArrayList<>();
    backtracking(nums, res, new ArrayList<Integer>(), 0);
    return res;
  }
  public void backtracking(int[] nums, List<List<Integer>> res, List<Integer> path, int index) {
    res.add(new ArrayList<Integer>(path));
    for (int i = index; i < nums.length; i++) {
      if (i != index && nums[i] == nums[i - 1]) {
        continue;
      }
      path.add(nums[i]);
      backtracking(nums, res, path, i + 1);
      path.remove(path.size() - 1);
    }
    //Time O(n * 2^n)
    //Space O(n)
  }
}
```

The space complexity of the sorting algorithm depends on the implementation of each programming language. For instance, in Java, the Arrays.sort() for primitives is implemented as a variant of quicksort algorithm whose space complexity is O*(log*n*). 



## 46. Permutations

### 1. Backtracking

```java
class Solution {
  public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    Arrays.sort(nums);
    backtracking(nums, res, 0);
    return res;
  }
  public void backtracking(int[] nums, List<List<Integer>> res, int index) {
    if (index == nums.length) {
      List<Integer> permutation = new ArrayList<>();
      for (int c : nums) {
        permutation.add(c);
      }
      res.add(new ArrayList<>(permutation));
      return;
    }
    for (int i = index; i < nums.length; i++) {
      swap(nums, index, i);
      backtracking(nums, res, index + 1);
      swap(nums, index, i);
    }
  }
  public void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
  }
  // Time O(n * n!)
  // Space O(n * n!)
}
```

index = 0， it has n possibilities. And then, based on the specific permutation, index move to next index.

index = index + 1, the previous index has been fixed, so you need to focus on the follow number and find all possibilities at this index.

The recursion will stop until the index beyond the length

In each recursion, after swap the index, we need to swap again in order to keep the order original.

![image-20220413210829540](/Users/youhao/Library/Application Support/typora-user-images/image-20220413210829540.png)



## 47. Permutations II

### 1. Backtracking

```java
class Solution {
  public List<List<Integer>> permuteUnique(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(nums, res, 0);
    return res;
  }
  public void backtrack(int[] nums, List<List<Integer>> res, int index) {
    if (index == nums.length) {
      List<Integer> permutation = new ArrayList<>();
      for (int c : nums) {
        permutation.add(c);
      }
      res.add(new ArrayList<>(permutation));
    }
    HashSet<Integer> set = new HashSet<>();
    for (int i = index; i < nums.length; i++) {
      swap(nums, index, i);
      if (set.add(nums[index])) {
        backtrack(nums, res, index + 1);
      }
      swap(nums, index, i);
    }
  }
  public void swap(int[] nums, int i, int j) {
    int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
    //Time O(n * n!)
    // Space O(n * n!)
  }
}
```

When  we find all possiblities in  i = [(index), n),  we can use hashset to check out the same element whether if it has been used for many times. If we find the same element, we neet to skip current process.

![image-20220413213242044](/Users/youhao/Library/Application Support/typora-user-images/image-20220413213242044.png)

## 39. Combination Sum

### 1. Backtraking

```java
class Solution {
  public List<List<Integer>> combinationSum(int[] candidates, int target) {
    Arrays.sort(candidates);
    List<List<Integer>> res = new ArrayList<>();
    backtracking(candidates, res, new ArrayList<>(), 0, target);
    return res;
  }
  public void backtracking(int[] candidates, List<List<Integer>> res, List<Integer> path, int index, int remain) {
    if (remain == 0) {
      res.add(new ArrayList<>(path));
      return;
    }
    else if (index == candidates.length || remain < 0) {
      return;
    } else {
      for (int i = index; i < candidates.length; i++) {
        path.add(candidates[i]);
        backtracking(candidates, res, path, i, remain - candidates[i]);
        path.remove(path.size() - 1);
      }
    }
  }
}
```

![image-20220414125526451](/Users/youhao/Library/Application Support/typora-user-images/image-20220414125526451.png)

Time Complexity :

![image-20220414130615042](/Users/youhao/Library/Application Support/typora-user-images/image-20220414130615042.png)

Space Complexity : 

![image-20220414130644749](/Users/youhao/Library/Application Support/typora-user-images/image-20220414130644749.png)

## 40. Combination Sum II

### 1. Backtracking

```java
class Solution {
  public List<List<Integer>> combinationSum2(int[] candidates, int target) {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    Arrays.sort(candidates);
    backtracking(candidates, res, path, 0, target);
    return res;
  }
  public void backtracking(int[] candidates, List<List<Integer>> res, List<Integer> path, int index, int remain) {
    if (remain == 0) {
      res.add(new ArrayList<Integer>(path));
      return;
    }
    else if (remain < 0 || index == candidates.length) {
      return;
    }
    else {
      for (int i = index; i < candidates.length; i++) {
        if (i != index && candidates[i] == candidates[i - 1]) {
          continue;
        }
        path.add(candidates[i]);
        backtracking(candidates, res, path, i + 1, remain - candidates[i]);
        path.remove(path.size() - 1);
      }
    }
		// Time O(2^n) == 2^n + nlogn
    // Space O(N) 
  }
}
```

![image-20220415150635290](/Users/youhao/Library/Application Support/typora-user-images/image-20220415150635290.png)

![image-20220415150652691](/Users/youhao/Library/Application Support/typora-user-images/image-20220415150652691.png)

## 17. Letter Combinations of a Phone Number

### 1. Backtracking 

```java 
class Solution {
  public List<String> letterCombinations(String digits) {
    Map<Character, String> map = Map.of('2', "abc", '3', "def", '4', "ghi",
                                        '5', "jkl", '6', "mno", '7', "pqrs",
                                        '8', "tuv", '9', "wxyz");
    List<String> res = new ArrayList<>();
    if (digits.length() == 0) {
      return res;	
    }
    StringBuilder sb = new StringBuilder();
    backtrack(digits, res, sb, 0, map);
    return res;
  }
  public void backtrack(String digits, List<String> res, StringBuilder sb, int index, Map<Character, String> map) {
    if (sb.length() == digits.length()) {
      res.add(sb.toString());
      return;
    }
    String letter = map.get(digits.charAt(index));
    for (char c : letter.toCharArray()) {
      sb.append(c);
      backtrack(digits, res, sb, index + 1, map);
      sb.deleteCharAt(sb.length() - 1);
    }
    // Time O(4^N*N) 
    // Space O(n)
  }
}
```

![image-20220419135925097](/Users/youhao/Library/Application Support/typora-user-images/image-20220419135925097.png)

## 22. Generate Parentheses

### 1. Backtracking

```java
class Solution {
  public List<String> generateParenthesis(int n){
    List<String> res = new ArrayList<>();
    if (n == 0) {
      return res;
    }
    StringBuilder sb = new StringBuilder();
    backtrack(res, sb, n, n);
    return res;
  }
  public void backtrack(List<String> res, StringBuilder sb, int left, int right) {
    if (left == 0 && right == 0) {
      res.add(sb);
      return;
    }
    if (left > 0) {
      sb.append('(');
      backtrack(res, sb, left - 1, right);
      sb.deleteCharAt(sb.length() - 1);
    }
    if (right > left && right > 0) {
      sb.append(')');
      backtrack(res, sb, left, right - 1);
      sb.deleteCharAt(sb.length() - 1);
    }
  }
}
```

In the recursion solution, the first thing is add a '(' into sb, and the number of '(' will minus 1. Based on this, we can backtrack this method, only if the number of '(' is bigger than the number of ')', we can add the ')' in to sb. When the number of '(' and ')' are all equal to 0, we can add the result into res.

![image-20220419153558277](/Users/youhao/Library/Application Support/typora-user-images/image-20220419153558277.png)

## 79. Word Search 

### 1. Backtracking

```java
class Solution {
  boolean[][] visited;
  public boolean exist(char[][] board, String word) {
    int n = board.length;
    int m = board[0].length;
    visited = new boolean[n][m];
    for (int i = 0; i < n; i++) {
      for(int j = 0; j < m; j++) {
        if (board[i][j] == word.charAt(0) && searchWord(i, j, board, word, 0)) {
          return true;
        }
      }
    }
    return false;
  }
  public boolean searchWord(int i, int j, char[][] board, String word, int index) {
    if (index == word.length()) {
      return true;
    }
    if (i < 0 || i >= board.length || j < 0 || j >= board[0].length || visited[i][j] == true || word.charAt(index) != board[i][j]) {
      return false;
    }
    visited[i][j] = true;
    if (searchWord(i + 1, j, board, word, index + 1) ||
       	searchWord(i - 1, j, board, word, index + 1) ||
       	searchWord(i, j + 1, board, word, index + 1) ||
       	searchWord(i, j - 1, board, word, index + 1)) {
      return true;
    }
    visited[i][j] = false;
    return false;
  }
  //Time O(N * 3 ^ l) l : the length of word
  // Space O(L)
}
```



## 77. Combinations

### 1. Backtracking

```java
class Solution {
  public List<List<Integer>> combine(int n, int k) {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path = new ArrayList<>();
    combination(res, path, n, k, 1);
    return res;
  }
  public void combination(List<List<Integer>> res, List<Integer> path, int n, int k, int index) {
    if (path.size() == k) {
      res.add(new ArrayList<>(path));
      return;
    }
    for (int i = index; i <= n; i++) {
      path.add(i);
      combination(res, path, n, k, i + 1);
      path.remove(path.size() - 1);
    }
  }
}
```

![image-20220420151107219](/Users/youhao/Library/Application Support/typora-user-images/image-20220420151107219.png)

![image-20220420151919307](/Users/youhao/Library/Application Support/typora-user-images/image-20220420151919307.png)



## 93. Restore IP Address

### 1. Backtracking

```java
class Solution {
  public List<String> restoreIpAddresses(String s) {
    List<String> res = new ArrayList<>();
    storeIp(s, res, 4, 0, "");
    return res;
  }
  public void storeIp(String s, List<String> res, int remain, int index, String sb) {
    if (remain == 0) {
      if (sb.size() == s.length()) {
        res.add(sb);
      }
    }
    for (int i = 1; i <= 3; i++) {
      if (i + index > s.length()) break;
      if (i != 1 && sb.charAt(index) == '0') break;
      String cur = s.subString(index, index + i);// starting index is inclusive, ending index is exclusive
      int curN = Integer.valueOf(cur);
      if (curN <= 255) {
        storeIp(s, res, remain - 1, index + i, sb + cur + (remain == 1 ? "" : "."));
      }
    }
  }
}
```

Time Complexity :  27

![image-20220421135350778](/Users/youhao/Library/Application Support/typora-user-images/image-20220421135350778.png)

Space: 19

![image-20220421135656446](/Users/youhao/Library/Application Support/typora-user-images/image-20220421135656446.png)

# 