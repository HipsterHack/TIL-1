# 자바 기본 메소드 

## Array & String

```
toCharArray() //get char array of a String
charAt(int x) //get a char at the specific index
length() //string length
length //array size 
substring(int beginIndex) 
substring(int beginIndex, int endIndex)
Integer.valueOf()//string to integer
String.valueOf()/integer to string
Arrays.sort()  //sort an array
Arrays.toString(char[] a) //convert to string
Arrays.copyOf(T[] original, int newLength)
System.arraycopy(Object src, int srcPos, Object dest, int destPos, int length)
Collections.swap(List[T] src, int i, int j);
```
## Stack 

```
isEmpty();
pop() // T 
push(T item)
T peek() 
```

## Queue 
* LinkedList를 사용한다

```
Queue // LinkedList
isEmpty();
offer(T item)
pop() //T  
T peek()
```

## Hashtable
* HashMap을 사용한다. HashMap키는 자기 마음대로 Key 순서가 바뀐다. 
* 키가 입력한 순서대로 조회할 필요가 있으면 `LinkedHashMap`을 사용한다
```
put(K key, V value)
isEmpty()
get(K key)// V
size() // int
```

## Tree 
* TreeMap을 이용한다.(Red-black Tree으로 구현됨)
```
TreeMap<K, V> // red-black tree
put(K key, V value) 
isEmpty()
get(K key)// V
firstEntry() // 가장 작은 entry 값
lastEntry() // 가장 큰 entry 값  Map.Entry<Integer, String> entry 
lowerEntry(K key) // key 보다 작은 entry
higherEntry(K key) //key 보다 큰 entry
floorEntry(K key) //k 랑 같거나 작은 값 
ceilingEntry(K key) // k랑 같거나 바로 위 값
pollFirstEntry() // 가장 작은 값 제거 
pollLastEntry()  // 가장 큰 값 구하고 제거
```

## Set
* 유일한 값 + 정렬된 값 데이터를 구할땐  `HashSet`을 이용한다.
```
boolean add(E e)
boolean contains(Object o)
isEmpty() 
Iterator<E> iterator()
int size()
retainAll(Collection c) // 모두 포함되었는지 여부
clear()  
remove(Object o)
retainAll
```

* TreeSet - Set의 일종 
```
 E first() 제일 낮은 객체를 리턴
 E  last() 제일 높은 객체를 리턴
 E lower(E e)  주어진 객체보다 바로 아래 객체를 리턴
 E higher(E e)  주어진 객체보다 바로 위 객체를 리턴
 E floor(E e)  주어진 객체와 동등한 객체가 있으면 리턴,없으면 주어진 객체의 바로 아래의 객체를 리턴
 E ceiling(E e) 주어진 객체와 동등한 객체가 있으면 리턴, 없으면 주어진 객체의 바로 위의 객체를 리턴
 E pollFirst() 제일 낮은 객체를 꺼내오고 컬렉션에서 제거
 E pollLast() 제일 높은 객체를 꺼내오고 컬렉션에서 제거
 Iterator<E> descendingIterator() 내림 차순으로 정렬된 Iterator를 리턴
retainAll
```


## 클래스의 import 방법

```
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.TreeMap;
import java.util.Random;
import java.util.Stack;

```


## Swap without temporary variable
### 1차 솔루션
```
int a, b;
a = a + b;
b = a - b; // (a + b) - b
a = a - b; // (a + b) - a
```
* 단 이것은 a,b값이 Integer.MAX_VALUE 값을 넘어 갈 수 있다.

### 2차 솔루션
* XOR을 이용한다.
* a와 b의 xor값을 다시 xor 시키면 원하는 값을 추출할 수 있다. 
```
int a, b;
a = a ^ b; // 1000 XOR 0101 = 1101
b = a ^ b; // 1101 XOR 1000 = 0101
a = a ^ b; // 1101 XOR 0101 = 1000
```
