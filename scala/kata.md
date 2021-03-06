# Kata

## 평균값 구하기 
### Question
* 입력 받은 데이터를 바탕으로 평균을 구한다.
* 다중배열(Double값)을 입력 받는다.
* 평균은 반올림한다.
* 평균은 array 로 나온다.
* 빈 배열은 받을 수 없다.

### Solution

* 성능은 N^2 
* Array는 고정타입이라, 다루기 쉽지 않다.
```
import scala.util._
import scala.math._

object Average {
    /* Usage: 
        Average.calculate(Array(Array(1.0,2.0,3.0,4.0),Array(5.0,5.0,5.0,5.0,5.0)))
     result: Array[Long](3, 5)
     */
    def calculate(scores: Array[Array[Double]]) = {
 			val result = scores.map{ one:Array[Double] =>
  			val sum = one.foldLeft(0.0)(_ + _)
  		  round(sum / one.length)
  		}
  		result
  	}    
}  	
```

#### 느낀점 또는 개선점 
* Type에 엄격하지 않는 collection을 찾아봐야 할 것 같다.(List가 있겠지)
* 입력 오류 처리가 필요하지 않을까? 블라블라 붙지 않도록 주의해야겠다
* Scala 배우고 나서 처음 코딩이다. 그래서 시행착오가 잦았다.
* 개선 방법은 비트 연산질을 하면 가능하다(나중에 구현해보자)
* [참고 - stack overflow](http://stackoverflow.com/questions/1020188/fast-average-without-division)


## Array에서 중복값 구하기
### Question
* n+1 크기의 integer array를 가진다.
* 1개의 중복을 가지고 있다
* 배열은 정렬 되어 있지 않다
* 예를 들어 [3,2,5,1,3,4] 의 중복은 3이다.
* 리턴은 1개의 integer 값을 돌려준다

### Solution
* Map을 써서 중복이 여부를 확인
* O(N)의 시간 복잡도를 가짐
* 공간 복잡도는 (Int byte + boolean byte + map 버킷 크기)  * N

#### imperative Style
```

import scala.collection.mutable.Map

object DuplicatedNumbers {
  def findDuplicatedNumber( numbers: Array[Int]) : Option[Int] = {
    val numberMap: Map[Int,Boolean] = Map()
    
    for ( num <- numbers){
      if (numberMap contains num ){
        return Some(num)  
      }
      else{
        numberMap(num) = true
      }
    }
    None
  }
  
  
  def main (args : Array[String]) = {
    println(findDuplicatedNumber(Array(1,2,3,4,4)).get)
    println(findDuplicatedNumber(Array(3,2,5,1,3,4)).get)
    
  }
}
```
#### 약간 더 functional style
```


import scala.collection.mutable.Map

object DuplicatedNumbers {
  def findDuplicatedNumber( numbers: Array[Int]) : Option[Int] = {
    val numberMap: Map[Int,Boolean] = Map()
    numbers.foreach(num => 
      if (numberMap contains num ){
        return Some(num)  
      }
      else{
        numberMap(num) = true
      }
    )
    None
  }

  def main (args : Array[String]) = {
    println(findDuplicatedNumber(Array(1,2,3,4,4)).get)
    println(findDuplicatedNumber(Array(3,2,5,1,3,4)).get)

  }
}
```

## 느낀점 또는 개선점 
* 코드를 좀 더 간단하게 만들 수 있을까?
* FoldLeft같은 걸 이용해서 성능을 개선할 수 있다(공간복잡도 문제 해결)
* Map을 안쓰고 만들 수 있는 방법은 없을까?

## Image Ratio 구하기
### Question
* 흔히 사용하는 Image ratio를 구한다. (16:9 , 5:4 등)
* 이미지 width, height중 하나라도 0이 들어가면 Exception을 던진다.
* 

### Solution
* 이미지 비율을 적당히 나눈다.
* 더 이상 나눠지지 않는 값을 만들어야 하므로 최대 공약수를 구해야한다.
* width, height 중에서 작은 값 만큼의 loop를 돌아서 값을 구한다
* O(N)의 시간 복잡도를 가짐
* 두개의 값을 묶으므로 Tuple을 사용해보자

#### imperative Style

* 문제 코드 
```
import scala.util.control._

object ImageRatio {
  def ratio(size: Tuple2[Long, Long]): Tuple2[Long, Long] = {
    checkException(size)
    val smallerValue: Long = size._1 min size._2
    val biggerValue: Long = size._1 max size._2
    var maxVal:Long = 1L

    for (i <- 2L to smallerValue ) {
      if (biggerValue % i == 0 && smallerValue % i == 0) {
        maxVal = i
      } 
    }
    Tuple2( size._1 / maxVal , size._2 / maxVal )
  }

  private def checkException(size: Tuple2[Long, Long]) = if (size._1 == 0 || size._2 == 0)
    throw new IllegalArgumentException("height or width is zero")
}
```

* 테스트 코드 
```
import org.scalatest.FunSuite
import org.scalatest._

class ImageRatioTest extends FunSuite {
  test("should make exception in case of width or  height zero in image ratio function") {
    intercept[IllegalArgumentException] {
      val ratio = ImageRatio.ratio((0, 0))
    }
  }
  test("should return correct result of ratio method.") {
    assert(Tuple2(5, 1) == ImageRatio.ratio((100, 20)))
    assert(Tuple2(16, 9) == ImageRatio.ratio((16, 9)))
    assert(Tuple2(5, 4) == ImageRatio.ratio((5, 4)))
    assert(Tuple2(5, 4) == ImageRatio.ratio((10, 8)))
    assert(Tuple2(2, 3) == ImageRatio.ratio((2902, 4353)))
  }
}
```

#### 느낀점 또는 개선점
* for loop를 벗어나서 함수형 스타일로 옮길 수 있을까?
* 소수가 아닌 이상 1부터 시작하면 값이 커질 수 있으므로 가장 큰 수에서 부터 돌면 어떨까?
* 유클리드 호제법을 사용하면 좀 더 성능이 좋아질 수 있다.

### 유클리드 호제법 적용 
```
object ImageRatio {
  def ratio(size: Tuple2[Long, Long]): Tuple2[Long, Long] = {
    checkException(size)
    val imageSize = swapByImageSize(size)
    val gcdValue = this.gcd(imageSize._1, imageSize._2)
    Tuple2( size._1 / gcdValue , size._2 / gcdValue )
  }
  
  private def swapByImageSize(size: Tuple2[Long,Long]): Tuple2[Long,Long] = if ( size._1 < size._2 ) size.swap else size
  
  private def gcd(left: Long, right: Long) : Long = if ( right == 0L ) left else gcd(right, left % right) 

  private def checkException(size: Tuple2[Long, Long]) = if (size._1 == 0 || size._2 == 0)
    throw new IllegalArgumentException("height or width is zero")
}
```

#### 느낀점 또는 개선점
* 유클리드 호제법 적용.  a,b 중 작은 값이 수행 시간을 결정한다.
* 시간 개선할 수 있는 방법이 있을지 고민 해야한다
* 재귀 호출이라 많이 수행하면 오버플로우가 발생할 수 있음.
```
while (right != 0){
  remain = left % right
  left = right
  right = remain
}
```
* [Range](http://www.scala-lang.org/api/current/index.html#scala.collection.immutable.Range) 를 이용하면 함수형으로 바꿀 수 있을 것 같다. 다시 한번 작성해보자.

## GPS 지점 기록을 이용한 최고 속도 구하기
* 존의 GPS는 s 초마다 처음 지점에서부터 이동한 거리를 기록한다
* 구간 속도의 최대값을 integer로 구하라 
* 측정값이 1보다 작으면 움직이지 않은 것이다.
* 예제: 결과는 74
```
x = [0.0, 0.19, 0.5, 0.75, 1.0, 1.25, 1.5, 1.75, 2.0, 2.25]
result = 74
```
* 문제 URL : http://www.codewars.com/kata/speed-control

### imperative Style

```
import scala.collection.mutable.ArrayBuffer

class SpeedMesure(s: Int) {
  require(s > 0)
  val intervalSecound = s
  val secoundAHour = 3600

  def measure(distances: Array[Double]) = {
    val speedList = ArrayBuffer[Double]()
    for( i <- 0 until distances.length - 1 ){
      val deltaDistance = distances(i + 1) - distances(i)
      val speedPerHour = calculateSpeedPerHour(deltaDistance)
      speedList += speedPerHour
    }
    val arrs = maxInArray(speedList.toArray)
  }
  
  private def calculateSpeedPerHour(deltaDistance: Double): Double = (deltaDistance / intervalSecound) * secoundAHour
  
  private def maxInArray(values: Array[Double]) = if (values.length > 1)  values.max
    else 0    
}
```
### 개선점
* FP로 바꾸기 참 어렵다.
* 적당한 Collection API는 없다. 그러므로 메소드 자체에서 하나씩 처리해가면서 tail recursion으로 처리해야 FP형태로 만들 수 있다. 
* 이건 책을 좀 찾아봐야겠다.

## Functional Style(?)

```
import scala.collection.mutable.ArrayBuffer
import scala.annotation.tailrec

class SpeedMesure(s: Int) {
  require(s > 0)
  val intervalSecound = s
  val secoundAHour = 3600

  def measure(distances: Array[Double]) = {
    require(distances.length > 1)
    val speedList = ArrayBuffer[Double]()
    
    @tailrec
    def loop(arrs:ArrayBuffer[Double], currentCursor:Iterator[Double], nextCursor:Iterator[Double]): Array[Double] = {      
      if ( !currentCursor.hasNext || !nextCursor.hasNext ) {
        arrs.toArray
      }
      else {
        arrs.append(calculateSpeedPerHour(nextCursor.next() - currentCursor.next()))
        loop(arrs, currentCursor, nextCursor)
      }
    }

    val it = distances.iterator
    val nextIter = distances.iterator
    nextIter.next()
    maxInArray(loop(speedList, it, nextIter)).toInt
  }
  
  private def calculateSpeedPerHour(deltaDistance: Double): Double = (deltaDistance / intervalSecound) * secoundAHour
  
  private def maxInArray(values: Array[Double]) = if (values.length > 1)  values.max
    else 0    
}

object SpeedMeasureTest extends App{
  val x = Array[Double](0.0, 0.19, 0.5, 0.75, 1.0, 1.25, 1.5, 1.75, 2.0, 2.25)
  val s = 15
  println(new SpeedMesure(s).measure(x))
}
```
### 알게된 점
* scala.annotation.tailrec 은 Tail Recursion 으로 컴파일러가 만들어준다
* 코드에 최적화 불가능의 케이스가 있다면 오류를 발생한다.
* iterator를 두 개 쓰고 있는데 다른 방법은 없을지 생각해 봐야겠다.
* 인덱스로 처리할 수 있는건 아닌듯하다.

## Imperative Style + Functional Style(?)
* 코드
```
import scala.collection.mutable.ArrayBuffer
import scala.annotation.tailrec

class Speed (s: Int) {
  require(s > 0)
  val intervalSecound = s
  val secoundAHour = 3600

  def measure(distances: Array[Double]) = {
    require(distances.length > 1)
   
    val groups = for {
      current <- 0 until distances.length - 1
    } yield Tuple2[Double, Double](distances(current), distances(current + 1))
                    
    groups.foldLeft(0.0) {
                      (maxValue:Double, el:Tuple2[Double,Double]) => 
                        val speed = calculateSpeedPerHour(el._2 - el._1)
                        if (speed >  maxValue) speed else maxValue
                    }.toInt
   }

  private def calculateSpeedPerHour(deltaDistance: Double): Double = (deltaDistance / intervalSecound) * secoundAHour

  private def maxInArray(values: Array[Double]) = if (values.length > 1) values.max
    else 0   
}
```
* 테스트 코드
```
import org.scalatest.FunSuite

class SpeedTest extends FunSuite {
  test("should measure GPS's speed") {
    val x = Array[Double](0.0, 0.19, 0.5, 0.75, 1.0, 1.25, 1.5, 1.75, 2.0, 2.25)
    val s = 15
    assert(74 == new Speed(s).measure(x))
  }

  test("should not measure the number of record to equals to 1") {
    val x: Array[Double] = Array[Double](0.0)
    val s: Int = 15
    intercept[IllegalArgumentException] {
      new Speed(s).measure(x)
    }
  }
  
   test("should not measure the number of record to equals to zero") {
    val x: Array[Double] = Array[Double]()
    val s: Int = 15
    intercept[IllegalArgumentException] {
      new Speed(s).measure(x)
    }
  } 
}
```

### 알게된점
* 리스트 결과값을 return 해줄 때는 recursion 으로 적절하게 만들기 어렵다. (보통은 파라미터로 결과값을 들고 다니면서 처리하는 경우가 많았다)
* 내가 함수형 사고에 익숙하지 않다.
* imperative로 개발하다보니 for 루프를 버리기 어렵다.


## LinkedList 
* 기본적인 링크드리스트 카타 .
* push 메소드와 length 메소드를 만들어라
* 언어를 확장 시켜서 ->를 연산자로 링크드 리스트를 만들어라.
* 아래 예제를 참고

```
var chained = null
chained = push(chained, 3)
chained = push(chained, 2)
chained = push(chained, 1)
push(chained, 8) === 8 -> 1 -> 2 -> 3 -> null
```

### Answer 
* 위의 요구조건대로라면 암묵적 타입 변환이 필요하다
* 아직 내가 잘 쓰지 못하므로 일단 배제.
* Node Class
```
package kata.linkedlist

class Node[T] (data:T, nextNode: Node[T] = null) {
  val value = data
  var nextValue:Option[Node[T]] = if ( nextNode != null) Option[Node[T]](nextNode) else None;
  
  def hasNext():Boolean = nextValue != None
  def next():Node[T] = nextValue.get
}
```
* LinkedList Object 일단 동작하고 분리시킴
```
package kata.linkedlist

import scala.annotation.tailrec

object LinkedList {
  def push[T](node:Node[T], value:T) : Node[T] = new Node[T](value, node)
  
  def length[T](node:Node[T]) : Int = count(node ,0)
  
  def printAll[T](root: Node[T]) = {
    printNode(root)
  }
  
  @tailrec
  def count[T](node: Node[T], acc : Int) :Int = 
    if (node == null) 0 else if (!node.hasNext()) acc + 1 else count[T](node.next , acc + 1)
  

  @tailrec
  def printNode[T](node: Node[T]) : Unit = {
    print(node.value)
    print("-> ")
    if (!node.hasNext()) 
      print("null")
    else 
      printNode[T](node.next)
  }
}
```
* Push Test case (scala test 를 안깔아서 App으로 대신하고 프린트로 눈으로 테스트 케이스 만듬.
```
package kata.linkedlist

object LinkedListPushTest extends App{
  var chained = LinkedList.push[Int](null, 3)
  chained = LinkedList.push[Int](chained, 2)
  chained = LinkedList.push[Int](chained, 1)
  LinkedList.printAll(LinkedList.push[Int](chained, 8))
}
```
* Length Test case
```
package kata.linkedlist

object LinkedListLengthTest extends App {
  println(LinkedList.length(null))
  var chained = LinkedList.push[Int](null, 3)
  chained = LinkedList.push[Int](chained, 2)
  chained = LinkedList.push[Int](chained, 1)
  println(LinkedList.length(chained))
}
```

#### 개선점
* LinkedList 자체에 앞으로 뒤에 달리는 것이 몇개 더 있다고 알려주면 O(1)이 걸릴듯.
* 다만 입력시 O(N)이 되겠다. 
* 이보단 링크드리스트를 참조해서 감싸는 클래스를 만드는 것이 좀 더 간단함.(대신 메모리를 더 차지하겠지)
* 언어 특성을 활용해서 문제 요구조건에 맞게 개발 가능함. (찾아봐야지)
#### 좋았던점
* 재귀함수 설계에 조금씩 익숙해져 가고 있음
