# DFS一般使用场景

1. 模版DFS， mask举例dfs
2. 外部空间dfs（用stack写成iterative way， for example tree traversal
3. dfs + memo （DP剪枝）
4. 使用在模拟流程，寻找所有情况全排列



## 94. Binary Tree Inorder Traversal

Tips：

（1）二叉树中序遍历的顺序是，左 --- 中 --- 右



# Backtracking 模版

```java
class Solution {
    private List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> subsets(int[] nums) {
        if (nums.length == 0) return res;
        helper(nums, 0, new ArrayList<>());
        return res;
    }
    public void helper(int[] nums, int index, List<Integer> path) {
            res.add(new ArrayList<>(path));
        for (int i = index; i < nums.length; i++) {
            path.add(nums[i]);
            helper(nums, i + 1, path);
            path.remove(path.size() - 1);
        }
    }
}
```





## 78. Subsets

### 1. Backtracking

Tips:

（1） 直接套用Backtracking 模版

（2）这个数组是不重复的，所以在遍历时不需要添加条件限制

（3） 在递归中，做选择时，选择当前位置，然后传到下一步时，需要当前位置+ 1

（4）递归后，需要去掉最后一个

### 2. Mask

Tips：

（1） 生成Mask去找到所有的解

（2） 计算出数组的长度n，2^n代表所有的排列组合总数

![image-20220919165513876](/Users/youhao/Library/Application Support/typora-user-images/image-20220919165513876.png)

（3）从小到大开始，对当前的value利用 &方式去查找每一位是否是1，将是1的位置记录在path中，说明当前value中该位置的数组被选择，然后loop完当前的value，就能将path加入到res中

Time n * 2^n





## 90. Subsets II

### 1. Backtracking

Tips:

（1）先将Arrays排序，这样可以将相同的数放在一起

（2）和Subset一样套用backtracking模版

（3） 但是在loop中，除了在index的第一个节点不用讨论，其他节点不能和它前一个节点相同。因为这样会出现重复的递归。

因为在这一层中 比如， 1， 2， 2，我们只需要讨论 1， 2就行



Time n * 2^n



## 46. Permutations

### 1. Backtracking

Tips:

（1）规定好结束条件，当path的长度和nums一致时，就加入到res中

（2）其余的在loop中进行path添加。每一层添加时，如果碰到相同值，就跳过

![image-20220919191917093](/Users/youhao/Library/Application Support/typora-user-images/image-20220919191917093.png)

Time N！



## 47. Permutations II

### 1. Backtracking

Tips:

（1）由于有重复数字，我们需要把数组sort一下

（2）我们需要借用boolean[] used去记录一次path中，nums中位置i的数有没有被使用过

（3）在一条path中，如果nums中位置i的数被使用过，就需要continue。

（4）在同一个level中，如果nums中位置i - 1被使用过，而且nums[i] == nums[i - 1]，说明是重复且在这个level下已经被添加，也需要跳过

Time N！



## 77. Combinations

### 1.Backtracking

![image-20220919200244881](/Users/youhao/Library/Application Support/typora-user-images/image-20220919200244881.png)

Tips：

（1）每一层的level要从index开始

![image-20220919200449214](/Users/youhao/Library/Application Support/typora-user-images/image-20220919200449214.png)





## 37. Sudoku Solver

### 1. Backtracking

Tips. 

（1） 利用Valid Sudoku来判断加入数字之后是否是合理的Sudoku

（2） 找到value为'.' 进行讨论。为该位置赋予1 ～ 9, 如果合理的话，就进入下一层，将后面的数字填满，如果能够在后续层中满足整个的Sudoku我们就返回true。否则就让该位置变成'.'，然后loop到下一个值，继续探索。

（3） 如果 1 ～ 9都尝试了一遍之后，仍然没有合适的，结束了循环。就return false。

（4）该功能的结尾return true. 表示遍历完了整个的board，board的'.'都被数字填充，返回return true；





# Summary

## 1. BFS vs DFS

BFS：对于解决最短或最少问题特别有效， 寻找深度小，缺点内存消耗大

DFS：对于解决遍历和求所有问题有效， 对于问题搜索深度小的时候速度快，深度大的时候处理速度慢



## 2. DFS的优点

内存开销小，每次只需要维护一个节点

能处理子节点较多或树层次过深的情况

一般用于解决连通性

### 3. DFS的缺点

只能寻找有解但无法找到最优解

