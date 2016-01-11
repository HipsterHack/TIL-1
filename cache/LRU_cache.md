## LRU Cache 

* LRU Cache는 Least Recently Used Cache의 약자다. 
영문 뜻 의미 그대로의 캐시다. 저장 허용 용량이 넘어가면 가장 오래된 데이터부터 삭제한다.

* 캐시 hit 율을 높이기 위해서 이렇게 만든게 아닐까 싶다.

### Java Version
* Java의 구현 방법은 두 가지.
* WeakedHashMap을 이용하는 방법과 DoubleLinkedList와 HashMap 을 이용해서 구현하는 방법이 있다.
* 다만 `WeakedHashMap`을 이용하면 한번 get하면 reference count - 1 이 된다. 그 때문에 순삭이 될 수 있다.
* 아래 구현 예제는 후자의 구현 방법 이다.

```
public class Node {
  int key;
  int value; 
  Node prev;
  Node next;
}

public class LRUCache { 
  private Map<Integer, Node> map = new HashMap<Integer, Node>();
  private Node head;
  private Node end;
  private int capacity = 10;
  public LRUCache(int capacity){
    this.capacity = capacity;
  }
  
  public int get(int key) {
    if ( map.hasKey(key)) {
      Node n = map.get(key);
      remove(n);
      setHead(n);
      return n.value;
    }
    return -1;
  }
  public void remove(Node n){
     if( n == null ) return ;
     Node n = map.get(key);
     if ( n.prev != null ){
       n.prev.next = n.next;
     }
     else{
       head = n.next;
     }
     if ( n.next != null ){
       n.next.prev = n.prev;
     }
     else {
       end = n.prev;
     }
     
  }
  public void setHead(Node current){
     current.next = head;
     current.prev = null;
     if ( head != null ){
        current.next = head;
     }
     head = current;
     if ( end == null ){
        end = head;
     }
  }
  public void set(int key, int value) {
    if(map.containsKey(key)){
       Node old = map.get(key)
       old.value = value;
       remove(old);
       setHead(old);
    }
    else {
      Node n = new Node();
      n.key = key;
      n.value = value;
      
      if (map.size() >= capacity) {
        map.remove(end.key);
        remove(end);
        setHead(n);
      }
      else{
        setHead(n);
      }
      
      map.put(key, n);
      
    }
  }
}
```
* 안쓰는 키가 map 에선 remove 안되는데 문제가 없을까?



