# String

## 5. Longest Palindromic Substring

### 1. Simple Go Through

```java
class Solution {
  int maxLen, left;
  public String longestPalindrome(String s) {
    if (s.length() == 0) {return s;}
    for (int i = 0; i < s.length(); i++) {
      helper(s, i, i); // discuss the case of odd
      helper(s, i, i + 1);// discuss the case of even
    }
    return s.substring(left, left + maxLen);
  }
  public void helper(String s, int l, int r) {
    while(l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)) {
      if (maxLen < r - l + 1) {
        maxLen = r - l + 1;
        left = l;
      }
      l--;
      r++;
    }
  }
}
```

![image-20220621160627333](/Users/youhao/Library/Application Support/typora-user-images/image-20220621160627333.png)

## 14. Longest Common Prefix

```java
class Solution {
  public String longestCommonPrefix(String[] strs) {
    if (strs.length() <= 1) {
      return strs;
    }
    String preStr = strs[0];//Store the first String
    //From the first to the end, compare the pre and cur, update the pre
    for (int i = 1; i < strs.length(); i++) {
      int index = 0;
      String cur = strs[i];
      // Compare with the preString
      while (index < preStr.length() && index < cur.length() && preStr.charAt(index) == cur.charAt(index)){
        index++;
      }
      if(index == 0) {
        preStr = "";
        return preStr;
      }
      preStr = cur.substring(0, index);
    }
    return preStr;
  }
}
```