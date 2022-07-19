# Math

## 9. Palindrome Number

```java
public class Solution {
    public bool IsPalindrome(int x) {
        // Special cases:
        // As discussed above, when x < 0, x is not a palindrome.
        // Also if the last digit of the number is 0, in order to be a palindrome,
        // the first digit of the number also needs to be 0.
        // Only 0 satisfy this property.
        if(x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }

        int revertedNumber = 0;
        while(x > revertedNumber) {
            revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }

        // When the length is an odd number, we can get rid of the middle digit by revertedNumber/10
        // For example when the input is 12321, at the end of the while loop we get x = 12, revertedNumber = 123,
        // since the middle digit doesn't matter in palidrome(it will always equal to itself), we can simply get rid of it.
        return x == revertedNumber || x == revertedNumber/10;
    }
}
```

## 125. Valid Palindrome

### 1. Two Pointer

```java
class Solution{
  public boolean isPalindrome(String s){
    s = s.toLowerCase();
    int left = 0, right = s.length() - 1; // set two pointer
    // Iterate until left meet right
    while (left < right){
      //Make the left pointer point the lowercase letter or numbers
      while ((left < right) && (!Character.isLetterOrDigit(s.charAt(left)))){
        left++;
      }
      //Make the right pointer point the lowercase letter or numbers
      while ((left < right) && (!Character.isLetterOrDigit(s.charAt(right)))){
        right--;
      }
      //Judge the left pointer whether if is equal to right
      if (s.charAt(left) != s.charAt(right)){
        return false;
      }
      left++;
      right--;
    }
    return true;
    // Time O(n)
    // SPace O(1)
  }
}
```



## 13. Roman to Integer

### HashTable

```java
class Solution {
  public int romanToInt(String s) {
    Map<String, Integer> map = new HashMap<>();
    map.put("I", 1);
    map.put("V", 5);
    map.put("X", 10);
    map.put("L", 50);
    map.put("C", 100);
    map.put("D", 500);
    map.put("M", 1000);
    map.put("IV", 4);
    map.put("IX", 9);
    map.put("XL", 40);
    map.put("XC", 90);
    map.put("CD", 400);
    map.put("CM", 900);
    if (s.length() == 0) {
      return 0;
    }
    int n = s.length();
    int sum = 0;
    int next = 0;
    for (int i = 0; i < n; i++) {
      if (i + 1 < n && map.containsKey(s.substring(i, i + 2))) {
        sum += map.get(s.substring(i, i + 2));
        i++;
      } else {
        sum += map.get(s.substring(i, i + 1));
      }
    }
    return sum;
  }
}
```



## 12. Integer to Roman

### 1. Array

```java
class Solution {
  public String intToRoman(int num) {
    String[] ones =new String[]{"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};
    String[] tens =new String[]{"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
    String[] hun =new String[]{"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
    String[] tho =new String[]{"", "M", "MM", "MMM"};
    
    int t = num /1000;
    int h = num % 1000 / 100;
    int b = num % 100 / 10;
    int a = num % 10;
    return tho[t] + hun[h] + tens[b] + ones[a];
  }
}
```

## 38. Count and Say

### 1. String

```java
class Solution {
  public String countAndSay(int n) {
    String res = "1";
    int say = 1;
    for (int i = 2; i <= n; i++) {
      int l = res.length();
      int count = 1;
      char pre = res.charAt(0);
      StringBuilder newR = new StringBuilder();
      for (int j = 1; j < l; j++) {
        if (res.charAt(j) == pre) {
          count++;
        } else {
          newR.append(count);
          newR.append(pre);
          pre =  res.charAt(j);
          count = 1;
        }
      }
      newR.append(count);
      newR.append(pre);
      res = newR.toString();
    }
    return res;
  }
}


```



## 6. Zigzag Conversion

### 1. String 

```java
class Solution {
  public String convert(String s, int numRows) {
    if (numRows == 1) {
      return s;
    }
    StringBuilder sb = new StringBuilder();
    int zig = 2 * numRows - 2;
    int l = s.length();
    int group = 0;
    if (l % zig != 0) {
      group = l / zig + 1;
    } else {
      group = l / zig;
    }
    for (int i = 0; i < numRows; i++) {
      for (int j = 0; j < group; j++) {
        if (j * zig + i >= l) {
            continue;
        }
        sb.append(s.charAt(j * zig + i));
        if (i != 0 && i != numRows - 1 && j * zig + zig - i < l) {
           sb.append(s.charAt(j * zig + zig - i));
        }
      }
    }
    return sb.toString();
  }
}
```

![image-20220524205140955](/Users/youhao/Library/Application Support/typora-user-images/image-20220524205140955.png)

## 1822.  Sign of the Product of an Array

### 1. Standardization the data

```java
class Solution {
    public int arraySign(int[] nums) {
        int res = 1;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) {
                nums[i] = 1;
            }
            else if (nums[i] == 0) {
                return 0;
            } else {
                nums[i] = -1;
            }
            res *= nums[i];
        }
        return res >= 0 ? res : -1;
    }
}
```



## 7. Reverse Integer

## 1. Math

```java
class Solution {
  public int reverse(int x) {
    if (x == 0) {
      return x;
    }
    int res = 0;
    while(x != 0){ 
      int pop = x % 10;
      x /= 10;
      if (res > 0) {// Consider the res exceed the boundary of int 
        // res should < (max value of int) / 10 
        // or in the case of res = (max value of int) / 10, pop should < (max value of int) % 10
        // in order to make res * 10 + pop < max value
        if (res > Integer.MAX_VALUE / 10 || (res == Integer.MAX_VALUE / 10 && pop > Integer.MAX_VALUE % 10)){
          return 0;
        }
      } else {// Consider the res exceed the boundary of int 
        // res should > (min value of int) / 10 
        // or in the case of res = (min value of int) / 10, pop should > (min value of int) % 10
        // in order to make res * 10 + pop > min value
        if (res < Integer.MIN_VALUE / 10 || (res == Integer.MIN_VALUE / 10 && pop < Integer.MIN_VALUE % 10)) {
          return 0;
        }
      }
      res = res * 10 + pop;
    }
    return res;
  }
}

```



## 66. Plus One

### 1. Math

```java
class Solution {
  public int[] plusOne(int[] digits) {
    if (digits.length == 0) {return digits;}
    for (int i = digits.length - 1; i >= 0; i--) {
      if (digits[i] < 9) {
        digits[i]++;
        return digits;
      }
      digits[i] = 0;
    }
    digits = new int[digits.length + 1];
    digits[0] = 1;
    return digits;
  }
}
```

if the array are all 9's, the array have to increase 1 space, and the first index is 1, others are 0.



## 202. Happy Number

### 1. HashTable

```java
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> set = new HashSet<>();
        set.add(n);
        while (true) {
            int sum = 0;
            while (n != 0) {
                int digit = n % 10;
                sum = sum + digit * digit;
                n /= 10;
            }
            if (sum == 1) {return true;}
            if (set.contains(sum)) {return false;}
            set.add(sum);
            n = sum;
        }
    }
}

```



## 263. Ugly Number (relative with P.264)

### 1. Math

```java
class Solution {
    public boolean isUgly(int n) {
        if (n == 0) {return false;}
        if (n == 1) {return true;}
        while (n != 1) {
            if (n % 2 == 0) {n /= 2;} // n can divide by 2, so we can discuss another multipulier
            else if (n % 3 == 0) {n /= 3;} // n can divide by 3, so we can discuss another multipulier
            else if (n % 5 == 0) {n /= 5;} // n can divide by 5, so we can discuss another multipulier
            else {
                return false;
            }
        }
        return true;
    }
}
```

14 :  2 & 7 -> 7 : % 2 != 0 return false;

12:  2 & 6 - > 6:  2 & 3 - > 3: 3 & 1 -> jump out of loop return true;



## 50. Pow(x, n)

### 1. Iterative

```java
class Solution {
  public double myPow(double x, int n) {
    if(x == 1 || n == 0) {return 1;}
    if(n == 0) {return x;}
    double res = 1;
    if(n < 0) {
      if (n == Integer.MIN_VALUE) {
          x = 1 / x;
          n = - (n + 1);
          res *= x;
      } else {
          x = 1 / x;
          n = - n;
      }
    }

     while (n > 1) {
       if (n % 2 == 1) {
         n--;
         res *= x;
       }
       n /= 2;
       x *= x;
     }
     res *= x;
    return res;
  }
}
```



### 2. Recursive 

```java
class Solution {
  private double res;
  public double myPow(double x, int n) {
    if (n == 0 || x == 1) return 1;
    if (n == 1) {return x;}
    res = 1;
    if(n < 0) {
      if (n == Integer.MIN_VALUE) {
        res *= (1 / x);
        x = 1 / (x * x);
        n = - (n + 1);
      } else {
        x = 1 / x;
        n = - n;
      }
    }
      return helper(x, n, res);
  }
      
  public double helper(double x, int n, double ans) {
    if (n == 1) {return x;}
   
    if (n % 2 == 0) {
      ans *= helper(x * x, n / 2, ans);
    } else {
      ans = ans * x * helper(x * x, n / 2, ans);
    }
    return ans;
  }
}
```

![image-20220718232215732](/Users/youhao/Library/Application Support/typora-user-images/image-20220718232215732.png)
