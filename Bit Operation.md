# Bit Operation

## 136. Single Number

### 1. HashSet

```java
class Solution{
  public int singleNumber(int[] nums){
    int sum=0,sumSet=0;
    Set<Integer> set=new HashSet<>();
    for(int c:nums){
      if(!set.contains(c)){
        set.add(c);
        sum+=c;
      }
      sumSet+=c;
    }
    return sum*2-sumSet;
    //Time O(n)
    //Space O(n)
  }
}
```

![image-20220317162611367](/Users/youhao/Library/Application Support/typora-user-images/image-20220317162611367.png)

### 2. List

```java
class Solution{
  public int singleNumber(int[] nums){
    List<Integer> res=new ArrayList<Integer>();
    for(int c: nums){
      if(!res.contains(c)){
        res.add(c);
      }else{
        res.remove(new Integer(c));
      }
    }
    return res.get(0);
    //Time O(1)
    //Space O(n)
  }
}
```

Be careful! If you want to remove the Integer "c", you need to be on this way: res.remove(new Integer(c));



### 3. Bit Operation

```java
class Solution{
  public int singleNumber(int[] nums){
    int a=0;
    for(int b: nums){
      a^=b;
    }
    return a;
    //Time O(n)
    //Space O(1)
  }
}
```

![image-20220317163415468](/Users/youhao/Library/Application Support/typora-user-images/image-20220317163415468.png)

## 191. Number of 1 Bits

### 1. Bits Operating 

``` java
class Solution{
  public int hammingWeight(int n){
    int mask=1;
    int bits=0;
    for(int i=0;i<32;i++){
      if((mask & n)!=0){
        bits++;
      }
      mask<<=1;
    }
    return bits;
    //Time O(1)
    //Space O(1)
  }
}
```

![image-20220318134158716](/Users/youhao/Library/Application Support/typora-user-images/image-20220318134158716.png)

### 2. Bit Manipulation Trick 

```java
class Solution{
  public int hammingWeight(int n){
    int sum=0;
    while(n!=0){
      sum++;
      n = & (n-1);
    }
    return sum;
    //Time O(1)
    //Space O(1)
  }
}
```

![image-20220318135839692](/Users/youhao/Library/Application Support/typora-user-images/image-20220318135839692.png)

## 169. Majority Element

## 1. Array Sort

```java
class Solution{
  public int majorityElement(int[] nums){
    Arrays.sort(nums);
    return nums[nums.length/2];
    //Time O(nlogn);
    //Space O(1);
  }
}
```

Time complexity of Arrays.sort() is O(nlogn) in Java and Python.

### 2. HashMap

```java
class Solution{
  public int majorityElement(int[] nums){
    HashMap<Integer, Integer> map = new HashMap<>();
    HashSet<Integer> set=new HashSet<>();
    for(int c : nums){
      if(map.containsKey(c)){
        int i = map.get(c);
        map.put(c, ++i);
      }else{
        map.put(c, 1);
      }
      set.add(c);
    }
    for(int c : set){
      if(map.get(c) > nums.length/2){
        return c;
      }
    }
    return -1;
    //Time O(n)
    //Space O(n)
  }
}
```

### 3. Randomization

```java
class Solution{
  public int randomIndex(Random random, int min, int max){
    return random.nextInt(max-min)+min;
  }
  public int numCount(int index, int[] nums){
    int c=0;
    for(int i=0; i < nums.length; i++){
      if(nums[i] == nums[index]){
        c++;
      }
    }
    return c;
  }
  public int majorityElement(int[] nums){
    Random random = new Random();
    int midCount = nums.length/2;
    while(true){
      int index = randomIndex(random, 0, nums.length);
      if(numCount(index, nums) > midCount){
        return nums[index];
      }
    }
    // Time O(∞)
    // Space O(1)
  }
}
```

![image-20220320160257920](/Users/youhao/Library/Application Support/typora-user-images/image-20220320160257920.png)

### 4. Boyer- Moore Voting 

```java
class Solution{
  public int majorityElement(int[] nums){
    int count = 0;
    Integer candidateNum = null;
    for(int c : nums){
      if(count == 0){
        candidateNum = c;
      }
      count += (c == candidateNum) ? 1 : -1;
    }
    return candidateNum;
  }
  // Time O(n);
  // Space O(1);
}
```

![image-20220320160044856](/Users/youhao/Library/Application Support/typora-user-images/image-20220320160044856.png)

## 190. Reverse Bits

### 1. Bit Operation

```java
class Solution{
  public int reverseBits(int n){
    int ans = 0;
    for(int i=0; i < 32; i++){
      ans = (ans << 2) | (n & 1);
      n >>= 1;
    }
    return ans;
    //Time O(logn)
    //Space O(1)
  }
}
```

![image-20220320175639083](/Users/youhao/Library/Application Support/typora-user-images/image-20220320175639083.png)

![image-20220320175920239](/Users/youhao/Library/Application Support/typora-user-images/image-20220320175920239.png)

**Using fixed fixed bits 32 bits to process the "0" at front bits. Therefore, we just need to loop 32 times**

### Bit Operation ( Base-10, Base-2)

#### 1. Base-10 Time Case

ans = ans * 10 + n % 10;

n / = 10;

#### 2. Base-2 Time Case (Postive Integers) 

ans = ans * 2 + n % 2;

n /= 2;

#### 3. Base-2 Time Case (Postive & Negative Integers)

ans = (ans << 1) | (n & 1);

n >>= 1;

## 231. Power of Two

### 1. Using Loop

```java
class Solution{
  public boolean isPowerOfTwo(int n){
    if(n <= 0){return false;}
    int count=0;
    for(int i = 0; i < 32; i++){
      if((n & 1) == 1){
        count++;
      }
      n >>= 1;
    }
    return (count > 1) ? false : true;
    //Time O(logn)
    //Space O(1)
  }
}
```

### 2. Bitwise Operators : Get the Rightmost 1-bit

```java
class Solution{
  public boolean isPowerOfTwo(int n){
    return (n > 0) && ((n * (-n)) == n);
    //Time O(1)
    //Space O(1)
  }
}
```

![image-20220320185209718](/Users/youhao/Library/Application Support/typora-user-images/image-20220320185209718.png)

### 3. Bitwise operators : Turn off the Rightmost 1-bit

```java
class Solution{
  public boolean isPowerOfTwo(int n){
    return (n > 0) && ((n & (n-1)) == 0);
    //Time O(1)
    //Space O(1)
  }
}
```

![image-20220320190011750](/Users/youhao/Library/Application Support/typora-user-images/image-20220320190011750.png)

## 389. Find the Difference

### 1. Sorting

```java
class Solution{
  public char findTheDifference(String s, String t){
    char[] sortedS = s.toCharArray();
    char[] sortedT = t.toCharArray();
    Arrays.sort(sortedS);
    Arrays.sort(sortedT);
    int i=0;
    for(i=0; i < s.length(); i++){
      if(sortedS[i] != sortedT[i]){
        return sortedT[i];
      }
    }
    return sortedT[i];
    //Time O(nlogn)
    //Space O(n) sort function will occupy O(n)
  }
}
```

![image-20220320201610850](/Users/youhao/Library/Application Support/typora-user-images/image-20220320201610850.png)

### 2. HashMap

```java
class Solution{
  public char findTheDifference(String s, String t){
    HashMap<Character,Integer> map = new HashMap<>();
    for(char c : s.toCharArray()){
      if(!map.containsKey(c)){
        map.put(c, 1);
      }else{
        int i = map.get(c);
        map.put(c, ++i);
      }
    }
    for(char c : t.toCharArray()){
      if(map.containsKey(c)){
        if(map.get(c) == 0){
          return c;
        }else{
          int i = map.get(c);
          map.put(c, --i);
        }
      }else{
        return c;
      }
    }
    return 0;
    //Time O(n)
    //Space O(1) Because the map space is fixed. We only have 26 lowercase letter. Therefore the HashMap space isn't relate with String's length;
  }
}
```

![image-20220320201620293](/Users/youhao/Library/Application Support/typora-user-images/image-20220320201620293.png)

### 3. Bits Manipulation

```java
class Solution{
  public char findTheDifference(String s, String t){
    char c = 0;
    for(char cur : s.toCharArray()){
      c ^= cur;
    }
    for(char cur : t.toCharArray()){
      c ^= cur;
    }
    return c;
    //Time O(n)
    //Space O(1)
  }
}
```

![image-20220320201631621](/Users/youhao/Library/Application Support/typora-user-images/image-20220320201631621.png)

## 461. Hamming Distance

### 1. Bit Shift

```java
class Solution{
  public int hammingDistance(int x, int y){
    int s = x ^ y;
    int count = 0;
    for(int i = 0; i < 32; i++){
      if((s & 1) == 1){
        count++;
      }
      s >>= 1;
    }
    return count;
    //Time O(1)
    //Space O(1)
  }
}
```

### 2. Brian Kernighan's Algorithm

```java
class Solution{
  public int hammingDistance(int x, int y){
    int xor = x ^ y;
    int count = 0;
    while(xor != 0){
      xor &= (xor - 1);
      count++;
    }
    return count;
    // Time O(1)
    // Space O(1)
  }
}
```

![image-20220321123433179](/Users/youhao/Library/Application Support/typora-user-images/image-20220321123433179.png)

## 405. Convert a Number to Hexadecimal

```java
class Solution{
  public String toHex(int num){
    if(num == 0) return "0";
    char[] map={'0','1','2','3','4','5','6','7',
                '8','9','a','b','c','d','e','f'};
    StringBuilder sb = new StringBuilder();
    while(num != 0){
      int hex = 15;
      hex &= num;
      sb.append(map[hex]);
      num >>>= 4;
    }
    return sb.reverse().toString();
    //Time O(1);
    //Space O(1);
  }
}
```

In java, >>>  is operated to unsigned Integer, >> is operated to signed Integer

### 268. Missing Number

### 1. Sorting

```java
class Solution{
  public int missingNumber(int[] nums){
    int n = nums.length;
    int[] numSorted = nums;
    Arrays.sort(numSorted);
    for(int i = 0; i < n+1; i++){
      if(i == n){
        return i;
      }else{
        if(numSorted[i] != i){
          return i;
        }
      }
    }
    return -1;
    //Time O(nlogn)
    //Space O(n)
  }
}
```

### 2. Bit Manipulation

```java
class Solution{
  public int missingNumber(int[] nums){
    int n = nums.length;
    int res = 0;
    int i = 0;
    for(i = 0; i < n; i++){
      res = res ^ i ^ nums[i];
    }
    return res ^ i;
    //Time O(n)
    //Space O(1)
  }
}
```

### 3. Gauss' Formula 

```java
class Solution{
  public int missingNumber(int[] nums){
    int n = nums.length;
    int sumN = n * (n+1) / 2;
    int sumNum = 0;
    for(int i = 0; i < n; i++){
      sumNum += nums[i];
    }
    return sumN - sumNum;
    //Time O(n)
    //Space O(1)
  }
}
```

![image-20220321144110903](/Users/youhao/Library/Application Support/typora-user-images/image-20220321144110903.png)

## 89. Gray Code （死记硬背）

### 1. Fomula

```java
class Solution {
  public List<Integer> grayCode(int n) {
    List<Integer> res = new ArrayList<>();
    for(int i = 0; i < (1 << n); i++) {
      res.add(i ^ (i >> 1));
    }
      return res;
  } 
}
```



# 