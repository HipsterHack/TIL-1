# JVM 메모리 구조와 GC


## 메모리 구조

* JVM 전통적인 메모리 구조. 
* G1 도입이후 크게 변했다. 

![JVM 메모리 Image](http://cfile5.uf.tistory.com/image/212B844E566D318C2F37A3)

### Young Generation
* 메모리가 할당 되면 이 영역에 할당된다.
* 힙의 일부
* 메모리가 최초에 할당되는 곳. GC 이후에 survivor 영역으로 이동하다가 공간이 없으면 old Generation으로 promotion한다.
* Minor GC가 이곳에서 일어난다.

#### Eden

* 객체를 만들면 최초로 할당되는 곳.

#### Survivor 

* GC를 거치면서 참조가 있어 살아남는 객체가 있는 곳. 
* 참조가 있는 객체만 survivor 영역을 왔다 갔다하면서 한곳이 비워지는 시스템이다.
* 일정이상 GC에서 살아남으면 Old Gen으로 인정받는다.

### Old Generation

* `-XX:MaxTenuringThreshold=15`의 minor GC를 견디고 살아남은 객체가 promotion해서 가는 영역. 
* Major GC에서 삭제가 가능했다.

#### Card Table
* OldGen으로부터 YoungGen으로 참조가 생기면 Young Gen의 참조 정보를 가지는 테이블.

### permanent Generation

* Class 메타정보
* Method 메타정보
* Static Object
* 상수화된 String Object
* Class관련 배열 메타정보
* JVM내부 객체와 최적화컴파일러(JIT)최적화 정보 등 포함
* HotSpot VM으로 올려둔 정보도 포함한다.

## GC
* 참조가 사라진 객체의 메모리를 회수한다. 

### GC 종류에 따른 분류

#### Minor GC

1. 에덴 영역이 가득차면 GC가 발생. 
2. Survivor0 영역에 Eden에서 살아남은 객체 복사.
3. Survivor0 영역 제외하고 나머지 영역을 비움.
4. Eden:Survivor0 영역의 메모리 비율이 일정 이상이면 (`XX:SurvivorRatio` 옵션으로 지정가능하다)
  Eden과 Survivor0의 참조중인 객체를 뒤진다. 
5. 이제 참조 객체는 Survivor1로 이동시킨다. 나머지는 삭제.
6. 일정카운트(`-XX:MaxTenuringThreshold=15` JDK8부터 15이상 지정하면 JVM 시작이 안됨.)이상 참조되고 있는 객체들을 Old영역으로 이동
(MaxTenuringThreshold 수치까지 왔다리 갔다리 합니다.) (Survivor1 영역에서 Old 영역으로 이동)
7. 반복한다. 

#### Major GC
* Old 영역 부족, Perm 영역 부족에 의하여 발생
* 보통 Mark-Sweep-Compat 과정을 거친다. 상대적으로 단순. (참조가 없는 객체를 표시하고, 표시된 객체를 삭제하며, 한곳에 몰아넣는 조각모음 같은 일을 함)
* Stop the world를 일어나게한다.
* Old Gen 영역에서 할때 Reference가 없는걸 찾아서 해제하면서 주로 일어난다.
* Minor GC + Major GC가 합쳐서 full gc라고 하고 보통 같이 일어나기 때문에 Major GC가 Full GC라고 여기는 경우가 많다. 

### 알고리즘

#### Serial GC

* 쓰레드 1개로 MSC 알고리즘을 이용해서 GC를 수행하는 경우이다.
* `-XX:+UseSerialGC`를 이용한다.

#### Parallel GC

* 다중쓰레드로 Young Gen 영역을 GC하는 옵션. Old Gen은 다중 쓰레드를 사용하지 않는다.
* `-XX:+UseParallelGC`

#### Parallel Old GC

* OldGen 영역을 다중쓰레드로 GC하는 옵션.
* Mark-Summary-Compaction 알고리즘을 이용한다.
* `-XX:+UseParallelOldGC`

#### CMS GC(Concurrent Mark Sweep GC) 
* 위의 3가지 GC와의 다른 점은 Compaction 작업을 수행하지 않는다.
* memory사용과 cpu 사용이 높다. 메모리 단편화가 발생해서 큰 크기의 객체를 만들지 못할 수도 있다.
```
-XX:+UseConcMarkSweepGC // CMS GC 옵션
-XX:+UseParNewGC // Young 영역의 GC를 Parallel로 수행할지 지정 (단 UseParallelGC 옵션과 함께 사용하면 안된다.)
-XX:+CMSParallelRemarkEnabled // UseParNewGC하고만 같이 사용 해야함.
-XX:CMSInitiatingOccupancyFraction=value // Old 영역의 사용량에 따른 GC시기 지정(%). 이 값을 잘 지정하면, Full GC 회수를 줄일 수 있음(Default = 68)
-XX:+UseCMSInitiatingOccupancyOnly // CMSInitatingOccupancyFraction 옵션에 따른 GC만 수행하도록 지정
```

##### GC 과정
1. initial Mark : Stop the world, ClassLoader에 가장 가까운 객체 중 살아있는 객체를 찾는다,
2. concurrent mark: initial Mark한 객체를 따라가면서 참조 객체를 찾아간다.(다중쓰레드로 동작)
3. remark: 2번 단계에서 추가되거나 참조가 끊긴 객체를 확인한다.(Stop the world)
4. concurrent sweep: 3에서 확인한 참조가 끊긴 객체를 삭제한다.

#### G1 GC
* 아래는 G1 GC를 사용했을때의 메모리구조다.
![G1메모리구조](http://cfile2.uf.tistory.com/image/22778D3E55F2310D14F298)
* region이라는 영역으로 나눈다. 영역을 2048개 정도로 만든다.(1~32MB사이 정도쯤)
* 응답속도가 빨라야 하고, GC 속도가 적어야 하는 경우에 쓴다. 보통 8GB이상으로 메모리를 할당할 때 사용한다.
* 튜닝을 해도 효과가 잘 없다.
* region 타입은 Eden, Survivor, Old, humougous(큰 크기의 영역), unused가 있다.
* 사용자가 지정한 PauseTime(기본값 200ms)안에 GC를 끝내려고 노력한다.

##### Minor GC(Evacuation Phase)

* Stop the world가 발생, 다중쓰레드로 동작한다.
* Eden, Survivor region 크기가 변경될 수 있다.
* live object가 구 GC 동작처럼 적당한 영역으로 이동한다.
* 각 region은 Live data connecting 정보를 가지고 있다. 
* Young GC는 하나의 단계로 구성된다.
* `-XX:ParallelGCThreads=8` 과 같이 옵션을 통해 Young GC를 수행하는 thread 갯수를 조정할 수 있음. 대부분의 old generation GC의 copy나 cleanup 등 stop-the-world 방식 동작들(initial mark, remark, cleanup, copying, young)은 이 thread들이 piggyback하여 실행한다.
* gc log에는 `[GC pause (young)]`으로 표시


##### Old Generation GC

* Old Generation GC는 다섯 단계로 구성된다.
* 대부분의 단계는 필요 시에 Young GC 실행 시에 piggyback되어 실행되므로 Young GC의 parallel gc thread들이 실행한다.
* 다만 시간이 많이 걸릴 수 있고 stop-the-world가 필요없는 mark 단계는 별도의 쓰레드를 통해 실행된다.
* `-XX:ConcGCThreads=8` 와 같이 옵션을 통해 Old generation marking 단계에 사용되는 GC 쓰레드 갯수를 조정한다.
* gc log에는 young GC에 piggyback되는 초기 mark 단계는 `[GC pause (young) (initial-mark)]`으로 표시되고, 마찬가지로 young GC에 piggyback되는 copy/cleanup 단계는 `[GC pause (mixed)]`로 표시된다.
* `-XX:NewRatio=n` 비율에 따라서 OldGen GC가 수행된다.

1. Initial Marking Phase (Stop-the-world)
  + 다중쓰레드로 동작한다. 
  + Reference를 가지는 Survivor Regions(Root Region)을 Mark. 
  + Old GC가 필요해지면 Young GC 때 함께 실행
  + `[GC pause (young) (initial-mark)]`
2. Concurrent Marking Phase
  + 다중쓰레드로 동작, reachable, live 객체를 마킹한다. 
  + 빈 region들을 찾아 표기하고 region별 live object 비율을 계산해둔다.
  + 이 region들은 바로 다음 Remark 단계에서 제거
  + 별도의 concurrent GC thread들이 실행
3. Remark Phase (Stop-the-world)
  + 빈 region들은 삭제해서 free로 만든다.
  + 전체 region들의 live object 비율이 계산된다.
  + G1은 SATB(Snapshot-At-The-Beginning) 알고리즘을 사용
4. Copying/Cleanup Phase (Stop-the-world)
  + 가장 빨리 청소가 가능한 live object 비율이 낮은 region들을 선택한다.
  + Young GC 때 선택한 region들을 청소한다.
  + [GC pause (mixed)]
5. After Copying/Cleanup Phase
  + 선택된 region들의 compaction이 완료된 시점.
  + young과 old generation이 모두 cleanup되고 선택된 region들은 모두 새로운 region으로 compaction되어 위치한다.


##### Full GC

* G1 GC는 가능하면 Full GC를 회피하기 위해 여러 가지 Young GC와 Old GC를 적절한 시점에 실행한다.
* Young GC는 일상적으로 실행되며, Old GC는 young area와 old area 비율이 일정 값 이하로 떨어졌을 때 (즉, young area가 부족하게 되었을 때) 트리거되어 실행된다.
   + 예를 들어 기본값으로 young과 old는 1:2의 비율을 가지고 있다. 따라서 old area 비율이 늘어나서 young area의 2배를 가지게 되면 old generation gc가 실행된다.
   + 만일 Old GC를 통해서도 필요한 young area를 확보하지 못하게 되면, 어쩔 수 없이 Full GC를 실행하게 된다.

* 특징은 Single-threaded, Stop-the-world 이때 permanent area cleanup도 실행 (즉, classloader unloading은 이때 일어남)
   + 참고 : G1GC에서 class unloading 관련한 이슈가 있다. Hot Deploy를 많이 할 경우 JDK 7 G1 GC에서는 perm generation 문제가 발생할 수 있다
   

# 참고

* G1: http://logonjava.blogspot.kr/2015/08/java-g1-gc-full-gc.html
* http://yckwon2nd.blogspot.kr/2014/04/garbage-collection.html
