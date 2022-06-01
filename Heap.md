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

# 