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

### 1. Array.sort

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        Arrays.sort(strs);
        String firstS = strs[0];
        String lastS = strs[strs.length - 1];
        int index = 0;
        while ((index < Math.min(firstS.length(), lastS.length())) && firstS.charAt(index) == lastS.charAt(index)) {
            index++;
        }
        return firstS.substring(0, index);
    }
  //Time O(m * nlogn) m be the average # of characters for the strings in the str array
  //n be the number of elements in the str array.
}
```



### 2. Horizonal Scanning

```java
class Solution {
  public String longestCommonPrefix(String[] strs) {
    if (strs.length <= 1) {
      return strs[0];
    }
    String preStr = strs[0];
    for (int i = 1; i < strs.length; i++) {
      int index = 0;
      String cur = strs[i];
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
  // Time O(m * n) m 平均长度 n length of strs
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





## 293. Flip Game

### 1. Loop

```java
class Solution {
    public List<String> generatePossibleNextMoves(String currentState) {
        List<String> res = new ArrayList<>();
        if (currentState.length() < 2) {return res;}
        for (int i = 1; i < currentState.length(); i++) {
            if (currentState.charAt(i) == '+' && currentState.charAt(i - 1) == '+')
            res.add(currentState.substring(0, i - 1) + "--" + currentState.substring(i + 1));
        }
        return res;
    }
}
```



## 1268. Search Suggestions System

### 1. Binary Search

```java
class Solution {
    public List<List<String>> suggestedProducts(String[] products, String searchWord) {
        int n = products.length;
        List<List<String>> res = new ArrayList<>();
        Arrays.sort(products);
        String prefix = "";
        int start = 0;
        for (char c : searchWord.toCharArray()) {
            prefix += c;
            int bsStart = lowerBound(products, prefix, start);
            int preLength = prefix.length();
            List<String> cur = new ArrayList<>();
            for (int i = bsStart; i < Math.min(n, bsStart + 3); i++) {
                if (preLength > products[i].length() || ! products[i].substring(0, preLength).equals(prefix)) {
                   break;
                }
                 cur.add(products[i]);
            }
            res.add(cur);
            start = bsStart; // 方便下次循环从bsStart开始查，减少运行复杂度
        }
        return res;
    }
    
    public int lowerBound(String[] products, String prefix, int start) {
        int mid = 0;
        int end = products.length;
        while (start < end) {
            mid = (start + end) / 2;
            if (products[mid].compareTo(prefix) >= 0) {
                end = mid;
            } else { //prefix的字母要在当前products的后面， 所以start 需要在mid后面一位
                start = mid + 1;
            }
        }
        return start;
    }
}
```

- Time complexity : O(nlog(n)) + O(mlog(n))*O*(*n**l**o**g*(*n*))+*O*(*m**l**o**g*(*n*)). Where `n` is the length of `products` and `m` is the length of the search word. Here we treat string comparison in sorting as O(1)*O*(1). O(nlog(n)comes from the sorting and O(mlog(n)) comes from running `binary search` on products `m` times.
  - In Java there is an additional complexity of O(m^2)*O*(*m*2) due to Strings being immutable, here `m` is the length of `searchWord`.

### 2. Trie Tree +DFS (未解决)

```java
List<List<String>> res=new ArrayList<List<String>>();
class TrieNode{
    boolean endOfWord;
    TrieNode children[]=new TrieNode[26];
    public TrieNode(){
        children=new TrieNode[26];
    }
}

public void dfs(String pref, TrieNode root, List<String> temp,String curr){
    if(root!=null){
        if(temp.size()==3){
            return;
        }
        if(root.endOfWord){
            temp.add(pref+curr);
        }
        
        for(int i=0;i<26;i++){
            if(root.children[i]!=null){
                dfs(pref,root.children[i],temp,curr + (char)(i+'a'));
            }   
        }
    }
}

public void insert(TrieNode root,String word){
    for(int i=0;i<word.length();i++){
        int x=word.charAt(i)-'a';
        if(root.children[x]==null){
            root.children[x]=new TrieNode();
        }
        root=root.children[x];
    }
    root.endOfWord=true;
}

public void search(TrieNode root,String pref){
    List<String> temp=new ArrayList<String>();
    for(int i=0;i<pref.length();i++){
        Character xchar = pref.charAt(i);
        int x = xchar-'a';
        if(root.children[x]!=null){
            root=root.children[x];        
        }else{
            res.add(temp);
            return;
        }
    }
    dfs(pref,root,temp,"");    //collect all strings.
    res.add(temp);
    
}

public List<List<String>> suggestedProducts(String[] products, String searchWord) {
    TrieNode root=new TrieNode();
    for(int i=0;i<products.length;i++){
        insert(root,products[i]);
    }
    
    int sn=searchWord.length();
    String pref = "";
    for(int i=0;i<sn;i++){
        pref+= searchWord.charAt(i);
        search(root,pref);
    }
    
    return res;
}
```

