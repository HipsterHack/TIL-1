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
