# Heap

## 1046. Last Stone Weight

### 1. Heap Solution

```java
class Solution{
  public int lastStoneWeight(int[] stones){
    PriorityQueue<Integer> heap=new PriorityQueue<>(Comparator.reverseOrder());
    //Store the data into heap: nlogn
    for(int s:stones){
      heap.add(s);
    }
    //n*2*logn
    while(heap.size()>1){
      int s1=heap.remove();
      int s2=heap.remove();
      if(s1!=s2){
        s1=s1-s2;
        heap.add(s1);
      }
    }
    //Time 3nlogn -> O(nlogn)
    //Space O(N): Create the PriorityQueue
    return heap.size()==1?heap.peek():0;
  }
}
```

### 2. Sorted Array-Based Simulation (模拟)

```java
class Solution{
  public int lastStoneWeight(int[] stones){
    List<Integer> res=new ArrayList<>();
    for(int s:stones){
      res.add(s);
    }
    Collections.sort(res);
    while(res.size()>1){
      int s1=res.remove(res.size()-1);
      int s2=res.remove(res.size()-1);
      if(s1!=s2){
        int newS=s1-s2;
        int index=Collections.binarySearch(res,newS);
        if(index<0){
          res.add(-index-1,newS);
        }else{
          res.add(index,newS);
        }
      }
    }
    return res.size()==1?res.remove(0):0;
  }
  //Time O(n^2)
  //Space O(n)
}
```

#### Collections

https://blog.51cto.com/u_14954398/2559204

If Collections.binarySearch(list, num) is not exist, just like {1,2,3,4}-> Collections.binarySearch(list, 5) -> return -4-1=-5 

## 703. Kth Largest Element in a Stream

### 1. Heap

```java
class KthLargest {
    PriorityQueue<Integer> heap=new PriorityQueue<>();
    int index=0;
    public KthLargest(int k, int[] nums) {
         for(int c:nums){
             heap.add(c);
         }
        index=k;
        while(heap.size()>index){
            heap.remove();
        }
    }
    
    public int add(int val) {
        heap.add(val);
        while(heap.size()>index){
            heap.remove();
        }
        return heap.peek();
    }
  //Space O(n)
  //Time O(nlogn+mlogk) m=the number of calls to add()
}
```

![image-20220307215240763](/Users/youhao/Library/Application Support/typora-user-images/image-20220307215240763.png)

### 2. ArrayList

```java
class KthLargest{
  List<Integer> res=new ArrayList<>();
  int key=0;
  public KthLargest(int k, int[] nums){
    for(int c: nums){
      res.add(c);
    }
    Collections.sort(res);
    key=k;
  } 
  public int add(int val){
    int index=Collections.binarySearch(res,val);
    if(index<0){
      res.add(-index-1,val);
    }else{
      res.add(index,val);
    }
    return res.get(res.size()-key);
    //Time O(n^2)
    //Space O(n)
  }
}
```



## 1405. Longest Happy String

### 1. Heap

```java
class Solution {
  public String longestDiverseString(int a, int b, int c) {
    StringBuilder sb = new StringBuilder();
    PriorityQueue<Pair> heap = new PriorityQueue<Pair>(
				(count1, count2) -> Integer.compare(count2.count, count1.count));
    //Store the data into heap: nlogn
    if (a > 0) {
      heap.add(new Pair('a', a));
    }
		if (b > 0) {
      heap.add(new Pair('b', b));      
    }
    if (c > 0) {
      heap.add(new Pair('c', c));
    }
    if (heap.size() == 1) {
        if (heap.peek().count >= 2) {
            sb.append(heap.peek().ch);
            sb.append(heap.peek().ch);
        } else {
            sb.append(heap.peek().ch);
        }
        return sb.toString();
    }
    while(heap.size() > 1){
      Pair p1 = heap.poll();
      if(p1.count >= 2) {
        sb.append(p1.ch);
        sb.append(p1.ch);
        p1.count -= 2;
      } else {
        sb.append(p1.ch);
        p1.count -= 1;
      }
      Pair p2 = heap.poll();
      
      if (p2.count >= 2 && p2.count > p1.count) {
        sb.append(p2.ch);
        sb.append(p2.ch);
        p2.count -= 2;
      } else {
        sb.append(p2.ch);
        p2.count -= 1;
      }
      
      if (p1.count > 0) {
        heap.add(new Pair(p1.ch, p1.count));
      }
      if (p2.count > 0) {
        heap.add(new Pair(p2.ch, p2.count));
      }
    }
    while (!heap.isEmpty()){
      Pair p3 = heap.poll();
      if(sb.charAt(sb.length() - 1) != p3.ch && p3.count >= 2) {
        sb.append(p3.ch);
        sb.append(p3.ch);
      } else {
          sb.append(p3.ch);
      }
    }
    return sb.toString();
  }
   static class Pair {
		public Character ch;
		int count;
     public Pair(Character ch, int count) {
			this.ch = ch;
			this.count = count;
		}
	}
}
```

![69901654202105_.pic](/Users/youhao/Library/Containers/com.tencent.xinWeChat/Data/Library/Application Support/com.tencent.xinWeChat/2.0b4.0.9/5c30e188b46dd764edba02dc2a7d8db0/Message/MessageTemp/debebe7def205d28f6eae426b86e9ea2/Image/69901654202105_.pic.jpg)