# Java Reference와 GC

## GC 과정
- 힙에 있는 객체 중 가비지를 찾아냄
- 찾아낸 가비지를 메모리에 반환


JDK 1.2부터 제한적으로 GC에 관여할 수 있게 됨
4가지 참조 방식을 제공하기 시작하였음.
* Strong reference
* soft
* weak
* phantom


GC를 위해 객체마다 reachability라는 상태 정보를 가지고 있음.
- reachable : root set으로 부터 시작한 참조 있을 때
- unreachable:  root set으로 부터 시작한 참조가 없을 때 

root set: 유효한 최초의 참조


## 힙의 객체 참조는 4 종류
1. 힙내 다른 객체 참조
2. Java 스택 메서드 실행 시에 사용하는 지역 변수와 파라미터들에 의한 참조
3. 네이티브 스택. JNI에서 생성한 객체 참조
4. 메소드 영역의 정적 변수 참조


이중 2,3,4 는 root set 이다. 이로써 reachability를 판단하는 기준이 된다.


## Soft, Weak, Phantom Reference
java.lang.ref.WeakReference 는 참조 대상 객체를 캡슐화한 reference를 생성한다.

```
// wr이 실제 객체를 가리킨다.
WeakReference<Product> wr = new WeakReference<Product>( new Product());  
// product도 실제 객체를 가리킨다.
Product product = wr.get();  
...
// product의 레퍼런스를 사라짐.
product = null;  
```

SoftReference, WeakReference, PhantomReference 클래스로 생성한 객체는 object reference
참조되는 객체는 referent.

위의 예제처럼 WeakReference로 연결된 객체는 unreachable객체와 함께 GC 시점에 메모리가 회수된다.
GC 시점에 WeakReference로 연결된 객체를 null로 바꾸고 메모리를 회수한다.
이런 것처럼 참조 가능하지만 반드시 유효할 필요는 없는 LRU 캐시를 쉽게 만들 수 있다.

아래 예제 같은 경우는 Strong reference 상태로 GC 대상이 아니다.

```
WeakReference<Product> wr = new WeakReference<Product>( new Product());  
Product product = wr.get();  
```
## Strengths of Reachability

* Reachability의 5가지 종류. GC 객체를 처리하는 기준이 된다.
* 1개의 객체 참조 수나 참조 형태는 제한이 없다. 

1. strongly reachable: root set으로 부터 시작해 어떤 reference도 끼지 않은 경우
2. softly reachable: strongly reachable이 아닌 것 중 weak reference, phantom reference 없이 soft reference만 통과하는 참조가 있는 경우
3. weakly reachable: strongly reachable, softly reachable도 아닌 중에 phantom reference 없이 weak reference만 통과하는 참조가 있는 객체
4. phantomly reachable: finalize되었지만 회수 되지 않은 객체 
5. unreachable: root set으로부터 참조 되지 않은 객체 

## softly reachable 
- softly reachable 객체는 힙에 남아 있는 메모리의 크기와 해당 객체의 사용 빈도에 따라 GC 여부가 결정된다. GC가 동작할 때마다 회수되지 않으며 자주 사용될수록 더 오래 살아남게 된다. 
  + Oracle HotSpot VM에서는 softly reachable 객체의 GC를 조절하기 위해 다음 JVM 옵션을 제공
  ```
  // 기본값은 1000
  -XX:SoftRefLRUPolicyMSPerMB=<N>
  ```
  + softly reachable 객체의 GC 여부 결정 공식
```
(마지막 strong reference가 GC된 때로부터 지금까지의 시간) > (옵션 설정값 N) * (힙에 남아있는 메모리 크기)
``` 
    + 남은 메모리가 작을수록 우변 값이 작아지므로 힙이 거의 소진되면 대부분 softly reachable 객체는 GC 타임에 모두 메모리에 회수된다.
## Weakly Reachable 객체
* Weakly Reachable 객체는 GC 타임마다 회수 대상이다. 하지만 GC마다 작동 방식이 달라서 GC 마다 회수되진 않는다. unreachable , Soft reference도 마찬가지.
  + LRU 캐시 구현으로는 Weak reference가 알맞다. 힙에 메모리가 많을 수록 회수 가능성이 낮기 때문이다.
    + 그래서 soft reference가 힙에 많으면 전체 메모리 사용량이 높아지고 GC가 자주 걸린다. 


## ReferenceQueue
* SoftReference 객체나 WeakReference 객체가 참조하는 객체가 GC 대상이되면 참조는 null로 전환하고 ReferenceQueue에 넣는다.
 + GC가 자동으로  enqueue한다.
 + ReferenceQueue에 enqueue는 GC가 수행한다.
 + GC가 queue를 확인해서 soft, weak 객체가 GC되었는지를 파악할 수 있다.
 + WeakHashMap 은 ReferenceQueue와 WeakReference를 이용해서 구현

## PhantomReference
* ReferenceQueue를 반드시 사용한다.
```
ReferenceQueue<Object> rq = new ReferenceQueue<Object>(); 
PhantomReference<Object> pr = new PhantomReference<Object>(referent, rq);  
```
1. PhantomReference는 객체 내부의 참조를 null로 설정하지 않는다.
2. 참조된 객체를 phantomly reachable 객체로 만듬 
3. ReferenceQueue에 enqueue된다.

## Phantomly Reachable과 PhantomReference
+ GC 대상 객체를 처리하는 작업과 메모리를 회수하는 작업도 연속된 작업이 아니다. 
  - 객체의 파이널라이즈 작업이 이루어진 후에 GC 알고리즘에 따라 할당된 메모리를 회수
   - phantomly reachable은 finalize와 메모리 회수 사이에 관여한다. 
   - finalize 먼저 하고 phantomly reachable로 간주된다.
   - 객체 참조가 PhantomReference만 남으면 객체는 바로 파이널라이즈된다. 

### GC 객체 처리 순서
 
1. soft references
2. weak references
3. 파이널라이즈
4. phantom references
5. 메모리 회수

위 순서로 진행한다. 만약 PhantomReference가 있다면 phantomly reachable로 간주한다. 그 뒤에 PhantomReference를 ReferenceQueue에 넣는다. 그 후 파이널라이즈 이후 작업을 애플리케이션이 수행하게 하고 메모리 회수는 지연시킨다.

- PhantomReference의 get() 메서드는 항상 null을 반환한다. 
- phantomly reachable로 판명된 객체는 더 이상 사용될 수 없게 된다.
- clear() 등을 호출하지 않으면 메모리가 바로 회수 안된다.
- finalize 이후에 처리할 정리 작업이 있다면 유용하게 사용할 수 있다. 


## 요약

1.  GC는 GC 대상 객체를 찾고, 대상 객체를 finalization 하고 할당된 메모리를 회수하는 작업으로 구성된다.
2. 애플리케이션은 사용자 코드에서 객체의 reachability를 조절하여 Java GC에 일부 관여할 수 있다.
3. 객체의 reachability를 조절하기 위해서 java.lang.ref 패키지의 SoftReference, WeakReference, PhantomReference, ReferenceQueue 를 사용한다.
4. WeakReference 나 WeakHashMap을 주로 사용한다.

# 참고 
http://d2.naver.com/helloworld/329631
