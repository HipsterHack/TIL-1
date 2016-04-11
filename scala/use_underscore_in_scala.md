# 스칼라의 언더스코어 _ 용법


## import

자바에서의 `import *`와 같은 용법이다.

```
import collection.{ Map => _ , _ }

```

예제
```
import scala.util.control.Breaks._
breakable {
    for (i <- 0 to 10) { if (i == 5) break }
}
```


## 디폴트 지정값

```
var number : Int = _
var person : Person = _
```

## 사용하지 않는 변수들

아래 두 예제는 같은 문장이다. 

```
(1 to 5) foreach { (x:Int) => println("one more")}
(1 to 5) foreach { _ => println("one more")}
```

패턴 매칭에서도 많이 사용한다. 예를 들면 아래 두 함수는 같은 일을 하는 함수이다.

```
def inPatternMatching1(s:String) {
    s match {
        case "foo" => println("foo !")
        case x => println("not foo")
    }
}
 
def inPatternMatching2(s:String) {
    s match {
        case "foo" => println("foo !")
        case _ => println("not foo")
    }
}
```

## 익명 파라미터

* 들어갈 변수 대신에 익명 파라미터로 지정한다. 넘어온 타입을 그대로 생략하고 사용하는 경우 사용한다.

```
def f(i:Int) : String = i.toString

// 아래 3함수는 같다.
def g = (x:Int) => f(x)
def g2 = f _
def f2 = (_:String).toString
 
def u(i:Int)(d:Double) : String = i.toString + d.toString
def v = u _ // v: (Int) => (Double) => String
 
def w1 = u(4) _ // w1: (Double) => String
 
def w2 = u(_:Int)(2.0) // w2: (Int) => String 
```

## import 예외처리

* 아래 예제는 Date를 제외하고 import할때
```
import java.util.{
 Date => _,
 _ 
}
```

## 스칼라 확장 타입 

```
def size(objs:List[T]) forSome { type T } : Int = objs.size

// 아래 것과 위의 것은 같다.
def size2(objs: List[_]) : Int = objs.size

```
