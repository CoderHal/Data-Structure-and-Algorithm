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



## 28. Implement strStr()

### 1. String

```java
class Solution {
  public int strStr(String haystack, String needle) {
    if (needle.length() == 0) {
      return 0;
    }
    int n = haystack.length();
    int m = needle.length();
    for (int i = 0; i < n - m + 1; i++) {
      boolean correct = false;
      if (haystack.charAt(i) == needle.charAt(0)) {
        for (int j = 0; j < m; j++) {
          if (haystack.charAt(i + j) != needle.charAt(j)) {
            correct = false;
            break;
          } else {
            correct = true;
          }
        }
      }
      if (correct) {
        return i;
      }
    }
    return -1;
  }
}
```

## 412. Fizz Buzz

### 1. Use %

```java
class Solution {
  public List<String> fizzBuzz(int n) {
    List<String> res = new ArrayList<>();
    for (int i = 1; i <= n; i++) {
      if (i % 3 == 0 && i % 5 == 0){
        res.add("FizzBuzz");
      } else if (i % 3 == 0){
        res.add("Fizz");
      } else if (i % 5 == 0){
        res.add("Buzz");
      } else {
        res.add(i);
      }
    }
    return res;
  }
}
```

### 2. Don't use %

一般来说，对于CPU取余数的运算相对来说效率很低，上面解法避免了使用大量的求余数操作，提升了程序的性能。

```java
class Solution {
  public List<String> fizzBuzz(int n) {
    List<String> res = new ArrayList<>();
    for (int i = 1, fizz = 0, buzz = 0; i <= n; i++) {
      fizz++;
      buzz++;
      if (fizz == 3 && buzz == 5) {
        res.add("FizzBuzz");
        fizz = 0;
        buzz = 0;
      } 
      else if (fizz == 3) {
        res.add("Fizz");
        fizz = 0;
      }
      else if (buzz == 5) {
        res.add("Buzz");
        buzz = 0;
      } else {
        res.add(Integer.toString(i));
      }
    }
    return res;
  }
}
```



## 242. Valid Anagram

### 1. HashMap

```java
class Solution {
  public boolean isAnagram(String s, String t) {
    int n1 = s.length();
    int n2 = t.length();
    if (n1 != n2) return false;
    HashMap<Character, Integer> map = new HashMap<>();
    for (int i = 0; i < n1; i++) {
      char curS = s.charAt(i);
      char curT = t.charAt(i);
      map.put(curS, map.getOrDefault(curS, 0) + 1);
      map.put(curT, map.getOrDefault(curT, 0) - 1);
    }
    for (char cur : map.keySet()) {
      if (map.get(cur) != 0) {
        return false;
      }
    }
    return true;
  }
}
```

### 2. Using Character Array

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        int n1 = s.length();
        int n2 = t.length();
        if (n1 != n2) return false;
        int[] newS = new int[26];
        int[] newT = new int[26];
        for(int i = 0; i < n1; i++) {
            int curS = s.charAt(i) - 'a';
            int curT = t.charAt(i) - 'a';
            newS[curS]++;
            newT[curT]++;
        }
        int count = 0;
        while (count < 26) {
            if(newS[count] != newT[count]) {
                return false;
            }
            count++;
        }
        return true;
    }
}
```

## 344. Reverse String

### 1. Two Pointers

```java
class Solution {
    public void reverseString(char[] s) {
        int n = s.length;
        int left = 0;
        int right = n - 1;
        while (left < right) {
            char temp = s[left];
            s[left++] = s[right];
            s[right--] = temp;
        }
        return;
    }
}
```

## 171. Excel Sheet Column Number

### 1. Right to Left

```java
class Solution {
    public int titleToNumber(String columnTitle) {
        int n = columnTitle.length();
        int res = 0;
        int digit = 0;
        for (int i = n - 1; i >= 0; i--) {
            int cur = columnTitle.charAt(i) - 'A' + 1;
            int exp = 1;
            for(int j = 0; j < digit; j++){
                exp *= 26;
            }
            res = res + exp * cur;
            digit++;
        }
        return res;
    }
}
```

### 2. Left to Right

```java
class Solution {
  public int titleToNumber(String columnTitle) {
    int res = 0;
    int n = columnTitle.length();
    for (int i = 0; i < n; i++) {
      res *= 26;
      int cur = columnTitle.charAt(i) - 'A' + 1;
      res += cur;
    }
    return res;
  }
}
```

