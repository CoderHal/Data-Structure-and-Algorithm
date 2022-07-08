## 146. LRU Cache

### 1. Using Hashmap + DoubleLinkedList

```java
class LRUCache {
  class Node{
    int key;
    int value;
    Node pre;
    Node next;
    public Node(int key, int value) {
      this.key = key;
      this.value = value;
    }
  }
  private HashMap<Integer, Node> map;
  private int capacity;
  private Node tail;
  private Node head;
  public LRUCache(int capacity) {
    map = new HashMap<>();
    this.capacity = capacity;
    head = null;
    tail = null;
  }
  public int get(int key) {
    if (!map.containsKey(key)) {return -1;}
    Node cur = map.get(key);
    if (cur != tail) {
      if (cur == head) {
        head = head.next;
      } else {
        cur.pre.next = cur.next;
        cur.next.pre = cur.pre;
      }
      tail.next = cur;
      cur.pre = tail;
      cur.next = null;
      tail = cur;
    }
    return tail.value;
  }
  public void put(int key, int value) {
    Node node = map.get(key);
    //如果存在节点
    if (node != null) {
      node.value = value;
      if (node != tail) {
        if (node == head) {
          head = head.next;
        } else {
          node.pre.next = node.next;
          node.next.pre = node.pre;
        }
        tail.next = node;
        node.next = null;
        node.pre = tail;
        tail = node;
      }
    } else {
      //如果不存在，插入新节点
      Node newNode = new Node(key, value);
      if (capacity == 0) {
        Node temp = head;
        head = head.next;
        map.remove(temp.key);
        capacity++;
      } 
      if (head == null) {
        head = newNode;
      } else {
        tail.next = newNode;
        newNode.pre = tail;
        newNode.next = null;
      }
      tail = newNode;
      map.put(key, newNode);
      capacity--;
    }
  }
}
```



## 348. Design Tic - Tac- Toe

### 1. Using Two Arrays and Two variables

```java
class TicTacToe {
    int[] rows;
    int[] cols;
    int diagonal;
    int antiDiagonal;
    private int[][] player2;
    public TicTacToe(int n) {
        rows = new int[n];
        cols = new int[n];
        diagonal = 0;
        antiDiagonal = 0;
    }
    
    public int move(int row, int col, int player) {
        int n = rows.length;
        int cur = (player == 1) ? 1 : -1;
        rows[row] += cur;
        cols[col] += cur;
        if (row == col) {
            diagonal += cur;
        }
        if (row + col == rows.length - 1){
            antiDiagonal += cur;
        }
        if (Math.abs(rows[row]) == n || Math.abs(cols[col]) == n || Math.abs(diagonal) == n || Math.abs(antiDiagonal) == n) {
            return player;
        } else {
            return 0;
        }
    }
}

/**
 * Your TicTacToe object will be instantiated and called as such:
 * TicTacToe obj = new TicTacToe(n);
 * int param_1 = obj.move(row,col,player);
 */
```

