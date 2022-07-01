## 395. Longest Substring with At Least K Repeating Characters

### 1. Divide and Conquer

```java
class Solution {
  public int longestSubstring(String s, int k) {
    return helper(s, 0, s.length(), k);
  }
  public int helper(String s, int start, int end, int k) {
    if (end < k) {return 0;}
    int[] ch = new int[26];
    for (int i = start; i < end; i++) {
      ch[s.charAt(i) - 'a']++;
    }
    for (int mid = start; mid < end; mid++) {
      //如果该位置的字母在这段序列中个数 >= k，则继续循环
      if (ch[s.charAt(mid) - 'a'] >= k) {continue;}
      //如果 < k, 则将序列分段讨论
      int midNext = mid + 1;
      while(midNext < end && ch[s.charAt(midNext) - 'a'] < k) midNext++;
      return Math.max(helper(s, start, mid, k), helper(s, midNext, end, k));
    }
    return (end - start);
  }
}
```

![image-20220701180952409](/Users/youhao/Library/Application Support/typora-user-images/image-20220701180952409.png)