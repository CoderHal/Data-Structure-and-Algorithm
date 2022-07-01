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

