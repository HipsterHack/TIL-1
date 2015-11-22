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
    var numberMap: Map[Int,Boolean] = Map()
    
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
    var numberMap: Map[Int,Boolean] = Map()
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
* 유클리드 호제법 적용으로 a,b 중 작은 값으로 
