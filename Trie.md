# Trie (前缀树)

Trie又叫字典树， 单词查找树

## 1⃣️ Trie树的工作原理

Map 和 Set 的底层实现原理是哈希表和二叉搜索树两种。

TreeMap / TreeSet和使用红黑树这种自平衡BST实现的



### （1）Trie树本质上就是一颗葱二叉树衍生出来的多叉树

二叉树节点实现：

```java
class TreeNode {
  int val;
  TreeNode left, right;
}
```

多叉树节点实现：

```java
class TreeNode {
  int val;
  TreeNode[] children;
}
```

Trie的性质：

1. 根节点不包含字符， 除根节点外的每个节点都仅包含一个字符
2. 从根节点到某一个节点路径上所经过的字符连接起来，即为节点对应的字符串
3. 任意节点的所有子节点所包含的字符都不相同

![image-20220903164215466](/Users/youhao/Library/Application Support/typora-user-images/image-20220903164215466.png)

## 2⃣️. Use Cases

![image-20220903164532191](/Users/youhao/Library/Application Support/typora-user-images/image-20220903164532191.png)

## 3⃣️. 模板

常用method

1. addWord(String word)
2. search(String word)
3. SearchPrefix(String prefix)

```java
class Trie {
  class TrieNode {
    TrieNode[] children;
    boolean isWord;
    public TrieNode() {
      children = new TrieNode[26];
    }    
  }
  TrieNode root;
  public Trie() {
    root = new TrieNode();
  }
  
  public void insert(String word) {
    TrieNode node = root;
    for (char c : word.toCharArray()) {
      int index = c - 'a';
      if (node.children[index] == null) {
        node.children[index] = new TrieNode();
      }
      node = node.children[index];
    }
    node.isWord = true;
  }
  
  public boolean search(String word) {
    TrieNode node = root;
    for (char c : word.toCharArray()) {
      int index = c - 'a';
      if (node.children[index] == null) {return false;}
      node = node.children[index];
    }
    return node.isWord;
  }
  
  public boolean startsWith(String prefix) {
    TrieNode node = root;
    for (char c : prefix.toCharArray()) {
      int index = c - 'a';
      if (node.children[index] == null) {return false;}
      node = node.children[index];
    }
    return true;
  }
}
```



## 211. Design Add and Search Words Data Structure

### 1. Trie 

```java
class WordDictionary {
    class TrieNode {
        boolean isWord;
        TrieNode[] children;
        public TrieNode() {
            children = new TrieNode[26];
        }
    }
    
    private TrieNode root;
    public WordDictionary() {
        root = new TrieNode();
    }
    
    public void addWord(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            if (node.children[index] == null) {
                node.children[index] = new TrieNode();
            }
            node = node.children[index];
        }
        node.isWord = true;
    }
    
    public boolean search(String word) {
        return helper(0, root, word);
    }
    
    public boolean helper(int l, TrieNode node, String word) {
        if (l == word.length()) {return node.isWord;}
        char cur = word.charAt(l);
        if (cur != '.') {
            return node.children[cur - 'a'] != null && helper(l + 1, node.children[cur - 'a'], word);
        } else {

            for (int i = 0; i < 26; i++) {
                if (node.children[i] != null && helper(l + 1, node.children[i], word)) {
                    return true;
                }
            }
        }
        return false;
    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */
```

对模版的改动： 只需要关注search中 " . "的情况。
