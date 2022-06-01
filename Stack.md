# Stack

## 20. Valid Parentheses(括号)

### 1. Solution 1

```java
class Solution{
  public boolean isValid(String s){
    Map<Character,Character> map= Map.of('(',')',
                                         '[',']',
                                         '{','}');
    Stack<Character> stack=new Stack<>();
    for(char c : s.toCharArray()){
      if(map.containsKey(c)){
        stack.push(c);
      }else{
        if(stack.isEmpty()){return false;}
        char open = stack.pop();
        if(map.get(open)!=c){
          return false;
        }
      }
    }
    return stack.isEmpty();
    //Time O(n)
    //Space O(n)
  }
}
```

### 2. Solution 2

```java
class Solution{
  public boolean isValid(String s){
    Stack<Character> stack=new Stack();
    for(char c: s.toCharArray()){
      if(c=='('){
        stack.push(')');
      }
      else if(c=='['){
        stack.push(']');
      }
      else if(c=='{'){
        stack.push('}');
      }else{
        if(stack.isEmpty()||stack.pop()!=c){
          return false;
        }
      }
    }
    return stack.isEmpty();
    //Time O(n)
    //Space O(n)
  }
}
```

![image-20220223161343235](/Users/youhao/Library/Application Support/typora-user-images/image-20220223161343235.png)

## 1047. Replace All Adjacent Duplicates in String

### 1. Stack Solution

```java
class Solution{
  public String removeDuplicates(String S){
    StringBuilder sb=new StringBuilder();
    for(char c: S.toCharArray()){
      int size=sb.length();
      if(size>0&&sb.charAt(size-1)==c){
        sb.deleteCharAt(size-1);
      }else{
        sb.append(c);
      }
    }
    return sb.toString();
    //Time O(n)
    //Space O(n)
  }
}
```

![image-20220223200944313](/Users/youhao/Library/Application Support/typora-user-images/image-20220223200944313.png)

### 2. Two Pointers 

```java
class Solution{
  public String removeDuplicates(String s){
    int i=0;
    int j=0;
    char[] res=s.toCharArray();
    for(i=0;i<res.length();i++){
      res[j]=res[i];
      if(j>0&&res[j]==res[j-1]){
        j-=2;
      }
      j++;
    }
    return new String(res,0,j);
  }
}
```

![image-20220223203128958](/Users/youhao/Library/Application Support/typora-user-images/image-20220223203128958.png)

## 232. Implement Queue using Stacks

### 1. Solution 1

```java
class MyQueue{
  Stack<Integer> s1=new Stack<>();
  Stack<Integer> s2=new Stack<>();
  public MyQueue(){
  
  }
  void push(int x){
    s1.push(x);
    //Time O(1)
    //Space O(n)
  }
  int pop(){
    if(!s2.isEmpty()){
      return s2.pop();
    }else{
      while(!s1.isEmpty()){
        s2.push(s1.pop());
      }
      return s2.pop();
    }
    //Time O(1), worst-case O(n)
    //Space O(1)
  }
  int peek(){
    if(!s2.isEmpty()){
      return s2.peek();
    }else{
      while(!s1.isEmpty()){
        s2.push(s1.pop());
      }
      return s2.peek();
    }
    //Time O(1)
    //Space O(1)
  }
  boolean empty(){
    return s1.isEmpty()&&s2.isEmpty();
    //Time O(1)
    //Space O(1)
  }
}
```

![image-20220224154408401](/Users/youhao/Library/Application Support/typora-user-images/image-20220224154408401.png)

### PoP(Time Analysis)

![image-20220224160214829](/Users/youhao/Library/Application Support/typora-user-images/image-20220224160214829.png)

## 155. Min Stack

### 1. Stack of Value/Minimum Pairs

```java
class MinStack{
  Stack<int[]> stack=new Stack<>();
  public MinStack(){
    
  }
  public void push(int val){
    if(stack.isEmpty()){
      stack.push(new int[]{val,val});
    }else{
      int min=stack.peek()[1];
      stack.push(new int[]{val,Math.min(val,min)});
    }
  }
  public void pop(){
    stack.pop();
  }
  public int top(){
    return stack.peek()[0];
  }
  public int getMin(){
    return stack.peek()[1];
  }
  //All operation Time complexity O(1);
  //Space O(n);
}
```

Idea

Push{val, min} into stack. if execute pop()function, min will also pop.

### 2. Double Stack

```java
class MinStack{
  Stack<Integer> stack1=new Stack<>();
  Stack<Integer> stack2=new Stack<>();
  public MinStack(){
    
  }
  public void push(int val){
    stack1.push(val);
    if(stack2.isEmpty()||stack2.peek()>=val){
      stack2.push(val);
    }
  }
  public void pop(){
    int val=stack1.pop();
    if(val==stack2.peek()){
      stack2.pop();
    }
  }
  public int top(){
    return stack1.peek();
  }
  public int getMin(){
    return stack2.peek();
  }
  // Time: O(1) for all operation
  // Space: O(n)
}
```

Idea

Use double stack. Stack1 store the value. Stack2 store the min. The principle of Stack2 is when val <= peek, we need to push val into stack2.



## 225. Implement Stack using Queues.

### 1. Using One Queue to solve the problem

```java
class MyStack {
    Queue<Integer> queue1=new LinkedList<>();
    public MyStack() {
        
    }
    
    public void push(int x) {
        queue1.add(x);
        int size= queue1.size();
        for(int i=size;i>1;i--){
            queue1.add(queue1.poll());
        }
        //Time O(n)
        //Space O(1)
    }
    
    public int pop() {
       return queue1.poll();   
        //Time O(1)
        //Space O(1)
    }
    
    public int top(){
        return queue1.peek();
        //Time O(1)
        //Space O(1)
    }
    public boolean empty() {
        return (queue1.isEmpty());
    }
}
```

![image-20220227182542377](/Users/youhao/Library/Application Support/typora-user-images/image-20220227182542377.png)

### 2. Using Two Queue 

```java
class MyStack {
    Queue<Integer> queue1=new LinkedList<>();
  	Queue<Integer> queue2=new LinkedList<>();
    public MyStack() {
        
    }
    
    public void push(int x) {
        queue2.add(x);
        while(!queue1.isEmpty()){
          queue2.add(queue1.poll());
        }
        Queue<Integer> tamp=new LinkedList<>();
      	tamp=queue1;
      	queue1=queue2;
      	queue2=tamp;
        //Time O(n)
        //Space O(1)
    }
    
    public int pop() {
       return queue1.poll();   
        //Time O(1)
        //Space O(1)
    }
    
    public int top(){
        return queue1.peek();
        //Time O(1)
        //Space O(1)
    }
    public boolean empty() {
        return (queue1.isEmpty());
    }
}
```

![image-20220227200029378](/Users/youhao/Library/Application Support/typora-user-images/image-20220227200029378.png)

## 1021. Remove Outermost Parenthese

### 1. No Stack Solution

```java
class Solution{
  public String removeOuterParentheses(String s){
    int counter=0;
    StringBuilder sb=new StringBuilder();
    for(char c: s.toCharArray()){
      if(c=='('){
        if(counter++>0){
          sb.append(c);
        }
      }else{
        if(--counter>0){
          sb.append(c);
        }
      }
    }
    return sb.toString();
  }
  //Time O(n)
  //Space O(n)
}
```

![image-20220228123413336](/Users/youhao/Library/Application Support/typora-user-images/image-20220228123413336.png)

### 2. One Stack Solution

```java
class Solution{
  public String removeOuterParentheses(String s){
    Stack<Character> stack=new Stack<>();
    StringBuilder sb=new StringBuilder();
    for(char c: s.toCharArray()){
      if(c=='('){
        if(stack.size()==0){
          stack.push(c);
        }else{
          sb.append(c);
          stack.push(c);
        }
      }else{
        if(stack.size()==1){
          stack.pop();
        }
        else if(stack.size()>1){
          stack.pop();
          sb.append(c);
        }
      }
    }
    return sb.toString();
    //Time O(n)
    //Space O(n)
  }
}
```

![image-20220228124032479](/Users/youhao/Library/Application Support/typora-user-images/image-20220228124032479.png)

## 682. Baseball Game

### 1. Stack 

```java
class Solution{
  public int calPoints(String[] ops){
    Stack<Integer> stack=new Stack<>();
    for(String c: ops){
      if(c.equals("C")){
        stack.pop();
      }
      else if(c.equals("D")){
        stack.push(2*stack.peek());
      }
      else if(c.equals("+")){
        int top=stack.pop();
        int newTop=top+stack.peek();
        stack.push(top);
        stack.push(newTop);
      }else{
        stack.push(Integer.valueOf(c));
      }
    }
    int total=0;
    while(!stack.isEmpty()){
      total+=stack.pop();
    }
    return total;
  }
}
```

### 2. Deque Solution

```java
class Solution{
  public int calPoints(String[] ops){
    Deque<Integer> stack=new ArrayDeque<>();
    for(String c: ops){
      if(c.equals("C")){
        stack.pop();
      }
      else if(c.equals("D")){
        stack.push(2*stack.peek());
      }
      else if(c.equals("+")){
        int top=stack.pop();
        int newTop=top+stack.peek();
        stack.push(top);
        stack.push(newTop);
      }else{
        stack.push(Integer.valueOf(c));
      }
    }
    int total=0;
    while(!stack.isEmpty()){
      total+=stack.pop();
    }
    return total;
  }
}
```

### 3. Deque VS Stack

https://blog.csdn.net/qq_44013629/article/details/106461200

https://blog.csdn.net/linysuccess/article/details/109038453?spm=1001.2101.3001.6650.13&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-13.queryctrv4&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-13.queryctrv4&utm_relevant_index=22

Java中[Stack](https://so.csdn.net/so/search?q=Stack&spm=1001.2101.3001.7020)类从Vector类继承，底层是用数组实现的**线程安全**的栈。栈是一种后进先出(LIFO)的容器，常用的操作`push/pop/peek`

不过Java中用来表达栈的功能（`push/pop/peek`），更适用的是使用双端队列[接口](https://so.csdn.net/so/search?q=接口&spm=1001.2101.3001.7020)Deque，并用实现类`ArrayDeque/LinkedList`来进行[初始化](https://so.csdn.net/so/search?q=初始化&spm=1001.2101.3001.7020)。

#### 1. 不用Stack至少有以下两点原因

1、从性能上来说应该使用Deque代替Stack。

Stack和Vector都是线程安全的，其实多数情况下并不需要做到线程安全，因此没有必要使用Stack。毕竟保证线程安全需要上锁，有额外的系统开销。

2、Stack从Vector继承是个历史遗留问题，JDK官方已建议优先使用Deque的实现类来代替Stack。

Stack从Vector继承的一个副作用是，暴露了set/get方法，可以进行随机位置的访问，这与Stack只能从尾巴上进行增减的本意相悖。

此外，Deque在转成ArrayList或者stream的时候保持了“后进先出”的语义，而Stack因为是从Vector继承，没有这个语义。

#### 2. Deque: ArrayList or LinkedList？

ArrayList: Store the data by Array

LinkedList: Store the data by LinkedList

Recommendation: ArrayList (more fast)

## 844. Backspace String Compare

### 1. Use Stack (Space Complexity O(M+N))

```java
class Solution{
  public boolean backspaceCompare(String s, String t){
    Deque<Character> stack1=new ArrayDeque<>();
    Deque<Character> stack2=new ArrayDeque<>();
    for(char c: s.toCharArray()){
      if(c=='#'&&!stack1.isEmpty()){
        stack1.pop();
      }else if(c=='#'&&stack1.isEmpty()){
          continue;
      }else{
        stack1.push(c);
      }
    }
    for(char c: t.toCharArray()){
      if(c=='#'&&!stack2.isEmpty()){
        stack2.pop();
      }else if(c=='#'&&stack2.isEmpty()){
          continue;
      }else{
        stack2.push(c);
      }
    }
    while(!stack1.isEmpty()&&!stack2.isEmpty()){
      if(stack1.pop()!=stack2.pop()){
        return false;
      }
    }
    return true&&stack1.isEmpty()&&stack2.isEmpty();
    //Time O(m+n)
    //Space O(m+n)
  }
}
```

### 2. Use Two Pointer(O(1))

```java
class Solution {
    public boolean backspaceCompare(String S, String T) {
        int i = S.length() - 1, j = T.length() - 1;
        int skipS = 0, skipT = 0;

        while (i >= 0 || j >= 0) { // While there may be chars in build(S) or build (T)
            while (i >= 0) { // Find position of next possible char in build(S)
                if (S.charAt(i) == '#') {skipS++; i--;}
                else if (skipS > 0) {skipS--; i--;}
                else break;
            }
            while (j >= 0) { // Find position of next possible char in build(T)
                if (T.charAt(j) == '#') {skipT++; j--;}
                else if (skipT > 0) {skipT--; j--;}
                else break;
            }
            // If two actual characters are different
            if (i >= 0 && j >= 0 && S.charAt(i) != T.charAt(j))
                return false;
            // If expecting to compare char vs nothing
            if ((i >= 0) != (j >= 0))
                return false;
            i--; j--;
        }
        return true;
    }
  //Time O(M+N)
  //Space O(1)
}
```

![image-20220302151529448](/Users/youhao/Library/Application Support/typora-user-images/image-20220302151529448.png)

# Monotonous Stack

## 496. Next Greater Element I

### 1. Better Brute Force O(m*n)

```java
class Solution{
  public int[] nextGreaterElement(int[] nums1,int[] nums2){
    HashMap<Integer,Integer> map=new HashMap<>();
    int[] res=new int[nums1.length];
    int max=0;
    for(int i=0;i<nums2.length;i++){
      map.put(nums2[i],i);
    }
    for(int i=0;i<nums1.length;i++){
      int j=map.get(nums1[i]);
      for(j=map.get(nums1[i])+1;j<nums2.length;j++){
        if(nums1[i]<nums2[j]){
          max=nums2[j];
          break;
        }else{
          max=-1;
        }
      }
     res[i]=max;
     if(map.get(nums1[i])==nums2.length-1){
        res[i]=-1;
     }
    }
    return res;
  }
}
```

### 2. Monotonous Stack

```java
class Solution{
  public int[] nextGreaterElement(int[] n1,int[] n2){
    Deque<Integer> stack=new ArrayDeque<>();
    HashMap<Integer,Integer> map=new HashMap<>();
    int[] res=new int[n1.length];
    for(int cur: n2){
      while(!stack.isEmpty()&&cur>stack.peek()){
        map.put(stack.pop(),cur);
      }
      stack.push(cur);
    }
    for(int i=0;i<n1.length;i++){
      if(map.containsKey(n1[i])){
        res[i]=map.get(n1[i]);
      }else{
        res[i]=-1;
      }
    }
    return res;
    //Time O(m+n+n)---> O(m+n)
    //Space O(n)
  }
}
```

![image-20220303164637253](/Users/youhao/Library/Application Support/typora-user-images/image-20220303164637253.png)

## 739. Daily Temperatures 

### 1. Monotonous Stack

```java
class Solution{
  public int[] dailyTemperatures(int[] temperatures){
    Deque<Integer> stack=new ArrayDeque<>();
    int[] res=new int[temperatures.length];
    for(int i=0;i<temperatures.length;i++){
      while(!stack.isEmpty()&&temperatures[i]>temperatures[stack.peek()]){
        res[stack.peek()]=i-stack.pop();
      }
      stack.push(i);
    }
    return res;
    //Time O(2*n) -> O(n)
    //Space O(n)
  }
}
```

### 2. Array, Optimized(使最优化) Space 

```java
class Solution{
  public int[] dailyTemperatures(int[] temperatures){
      int hottest=0;//if temperatures>hottest, hottest=temperatures, which means this temperature dont have the hotter number in next, because we loop from end to front.
      int n=temperatures.length;
      int[] res=new int[n];
      for(int i=n-1;i>=0;i--){
          int curT=temperatures[i];
          if(curT>=hottest){
              hottest=curT;
              continue;
          }
          int days=1;
          while(temperatures[i+days]<=curT){
              days+=res[i+days];
          }
          res[i]=days;
      }
      return res;
    //Time O(n)
    //Space O(1)
  }
}
```

![image-20220303212959244](/Users/youhao/Library/Application Support/typora-user-images/image-20220303212959244.png)

## 402. Remove K Digits

### 1. Greedy with Stack

```java
class Solution{
  public String removeKdigits(String num,int k){
    Deque<Character> stack=new ArrayDeque<>();
    StringBuilder sb=new StringBuilder();
    for(char c: num.toCharArray()){
      while(!stack.isEmpty()&&k>0&&stack.peek()>c){
        stack.pop();
        k--;
      }
      stack.push(c);
    }
    /* remove the remaining digits from the tail. */
    while(k>0){
      stack.pop();
      k--;
    }
    while(!stack.isEmpty()){
      sb.append(stack.pop());
    }
    // build the final string, while removing the leading zeros.
    for(int i=sb.length()-1;i>=0;i--){
      if(sb.charAt(i)=='0'){
        sb.deleteCharAt(i);
      }else{
        break;
      }
    }
    sb.reverse();
    //return the final string
    if(sb.length()==0) return "0";
    return sb.toString();
  }
  //Time O(n)
  //Space O(n)
}
```

![image-20220306113022500](/Users/youhao/Library/Application Support/typora-user-images/image-20220306113022500.png)

## 1124. Longest Well-Performing Interval

### 1. Using HashMap

```java
class Solution{
  public int longestWPI(int[] hours){
    HashMap<Integer,Integer> map=new HashMap<>();
    int s=0; //calculate the corresponding sum of each number of array
    int res=0; // calculate the result
    for(int i=0;i<hours.length;i++){
      s+=hours[i]>8?1:-1;
      if(s>0){
        res=i+1;
      }else{
        if(!map.containsKey(s)){
          map.put(s,i);
        }
        if(map.containsKey(s-1)){
          res=Math.max(res, i-map.get(s-1));
        }
      }
    }
    return res;
    //Time O(n)
    //Space O(n)
  }
}
```

![image-20220306202010127](/Users/youhao/Library/Application Support/typora-user-images/image-20220306202010127.png)

# 