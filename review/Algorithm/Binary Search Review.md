# Binary Search定义

一般是一堆数中找出指定的数，“一堆数”必须存在以下特征：

存储在数组中

有序排列

所以链表存储的就无法用二分法查找了

# Binary Search 模版

## (1) 寻找一个数

```java
int left = 0;
int right = nums.length - 1;
while (left <= right) {
  int mid = left + (right - left) / 2;
  if (nums[mid] == target) {
    return mid;
  } else if (nums[mid] < target){
    left = mid + 1;
  } else if (nums[mid] > target) {
    right = mid - 1;
  }
  return -1;
}
```

## (2) 寻找左边界的target

```java
int left = 0;
int right = nums.length - 1;
while (left <= right) {
  int mid = left + (right - left) / 2;
  if (nums[mid] == target) {
    right = mid - 1;
  } else if (nums[mid] < target) {
    left = mid + 1;
  } else if (nums[mid] > target) {
    right = mid - 1;
  }
  return nums[left] == target ? left : -1;
}
```

## (3) 寻找右边界的target

```java
int left = 0;
int right = nums.length - 1;
while (left <= right) {
  int mid = left + (right - left) / 2;
  if (nums[mid] == target) {
    left = mid + 1;
  } else if (nums[mid] < target) {
    left = mid + 1;
  } else if (nums[mid] > target) {
    right = mid - 1;
  }
  return nums[right] == target ? right : -1;
}
```



## 278. First Bad Version

### 1. Binary Search

Tips ：

寻找左边界，直接套用模版

优化： 最终的target左边一定不是bad version，我们只需要讨论当前位置之前的数是否是bad version，就能直接return





## 4. Median of Two Sorted Arrays

### 1. Binary Search

![image-20220802213543112](/Users/youhao/Library/Application Support/typora-user-images/image-20220802213543112.png)

## 410. Split Array Largest Sum

### 1. Binary Search

![image-20220922214454608](/Users/youhao/Library/Application Support/typora-user-images/image-20220922214454608.png)
