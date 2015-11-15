# 스칼라 정리
이 페이지에서는 스칼라 공부하면서 배운 내용을 정리합니다.

## 스칼라 언어의 특징
* 함수는 트레잇 집합이다.
* 클래스 메소드는 표현식 개념으로 생각하면 쉽다. def 옆에 할당 연산자 = 가 있는 이유가 있다.


## 스칼라 코딩 스타일
* val (데이터 값을 변경할 수 없는 경우)
* 명명시 _는 사용 안한다. 
* 스칼라는 표현식으로 코드를 구성하는 방식을 주로 사용한다.
* 포맷팅할때 1탭은 2칸 띄어쓰기.
* 스칼라는 함수와 람다로 표현하고 변수 이름을 짧게 한다.
* 원 라인으로 표현할때는 변수 이름은 짧게,두줄 이상은 변수이름을 자세하게 만든다.
* 축약어를 종종 쓰는 경우가 많다.

# 언어 사용 내용 정리
## object 

객체(object)는 싱글턴 이다. 주로 Main 함수나 factory에서 주로 사용한다.

```
object Main {
  def main(args: Array[String]) = {

  }
}
```
객체의 이름과 클래스 이름이 같으면 컴패니언 오브젝트라고 부른다.
컴패니언 오브젝트는 private method 등에 접근이 가능하다. 
C++ friend와 비슷한 특징이다.
apply method를 이용하여 팩토리를 만들때 유용하게 사용한다.

```
class Money(currency:String) 

object Money{
	def apply(currency:String) = new Money(currency)
}
```

## 데이터 타입 

( 추후 정리 예정)

## case class

* 쉽게 데이터를  저장하기 위해 만들어졌다. (자바빈 대신으로 쓸 수 있다?!)
* 그리고 그 데이터에 따라 match를 하고 싶을 때 사용한다.
* new 를 쓰지 않고 instance생성 할 수 있다.
* toString, equals 함수를 자동으로 생성한다.


### 예제
```
case class SmartPhone(brand: String, model:String)
val samasungS5 = SmartPhone("Samasung", "SG-180")
val ioPhone = SmartPhone("IO", "IO-B21")


def createModel(smartPhone:SmartPhone) = smartPhone match {
  case SmartPhone("Samasung", "SG-180") => "SamaSung S5"
  case SmartPhone("IO", "IO-B21") => "IoPhone 2"
  case _ => "Unknown this phone."
}
```





## 트레잇

인터페이스 대용으로 나온것이다. 여러개를 섞을 수 있다.
with로 합친다. 오른쪽에서 왼쪽 순서로 호출한다.
```
class Soinata extends Car with Hyundi with Tunable {
  
}
```
자바의 인터페이스와 비슷한 용도로 추상 클래스와 트레잇 중에서 어떤 걸 쓸지 고민 하는 경우가 있다.
문서에서의 추천은 기승전 트레잇!
스칼라 언어 문법상 클래스는 1개만 상속(extend)할 수 있지만, 트레잇은 여러 트레잇을 믹스인 할 수 있다.
다만, 생성자 매개변수가 필요한 경우라면 추상 클래스를 사용하란다.
추상 클래스의 생성자는 매개변수를 받을 수 있다 하지만 트레잇의 생성자는 매개변수를 못받는다.
예를 들어 trait t(i: Int) {} 에서 i 매개변수는 문법상 못쓴다.

## 스칼라 scope

스칼라는 access 수정자를 제공한다. 
private[], protected[]의 형태로 제공한다. 

이 access 수정자는 현재 패키지, 클래스 내부에서만 접근을 설정할 수 있다. 

```
package currency {
  package koreaCurrency {
    class KoreanWon {
      private[currency] var amount = null;
      private[koreanCurrency] var amountByWon = null;
      prviate[this] var original null;
     }
  }
}

```

위의 예시에서 
* amount는 currency 패키지 내부에서 접근이 가능하고, 
* amountByWon는 koreaCurrency 패키지 내부에서 접근 가능하며,
* this는 현재 object에서만 접근이 가능하다(같은 클래스인 다른 오브젝트에서 접근하려면 오류가 난다.)

### 루프

스칼라는 루프를 3개 제공한다. 
for, while, do-while.
Java와 다르게 for는 :대신 <-로 표현한다. 그리고 c, c++ 스타일의 :을 사용하는 대신,
to를 이용해서 루프를 돈다.

* for 예제 
```
  for(current <- args) {
    println(current)
  }
  for( idx <- 0 to 1000){
  	// 뭔가 처리함.
  }
```

* for yield 예제

```
numList = List(1,2,3,4,5,6,7,8,9,10)
var retVal = for { var a <- numList 
                   if != 3 ; if 1 < 8
                 } yield a
```

스칼라는 위 예제에서 retVal은 numList와 같은 타입으로 저장한다.

yield 옆의 a는 저장할 형태의 expression이다.


* while 예제
```
while ( i > 20 ){ 
  // 뭔가 처리함
  
}
```
List 같은 클래스는 foreach를 제공함으로써 함수형 프로그래밍 형태를 지원한다. 

```
 nums.foreach((i: Int) => i * 3)
```

#### Breaks

자바의 break문은 없다. break 객체가 있을 뿐이다.

breakable은 break 범위를 설정해주는 것. break 하면 어디로 갈지를 정해준다.

```
import scala.util.control._


var loop = new Breaks
val myList = List(1,2,3,4,5)
loop.breakable { 
  for ( i <- myList){
    if (i > 3)
      loop.break;
  }
}
```
### 스칼라 method and function

메소드는 클래스의 일부분이다. 목적을 이루기 위한  문장의 집합이 메소드.
함수는 method와 약간의 차이만 있다. object의 멤버이면서 변수에 할당되었으면 function. bytecode 레벨까지 내려가야 정확하게 함수, 메소드 구분을 할 수 있다.


#### 정의 

아래와 같은 형태로 정의 한다

```
def plus(a : Int, b : Int) : Int = a + b

def plus(a : Int, b : Int) : Int = {
  a + b
}
```

#### 기본값 있는 함수

```
def plus(a : Int = 1 , b : Int = 2) : Int = a + b


println( plus() ) // 3

```

#### nested function 중첩함수 

* 함수안의 함수.

```
object Test{
  def main( args : Array[String]){
    println( factorial(0))
  }	

  def factorial(i : Int) : Int = {
    def fact(i : Int, accumulator: Int): Int = {
      if ( i <= 1 )
        accumulator
      else 
      	fact(i - 1, i * accumulator)  
    }
  }
}
```

### 익명 함수 및 클로저


```
 var decrease = (x: Int, min: Int) => x - min
 decrease(6, 1) // 5
```

여기에 함수 밖에 선언된 변수명 등을 가져와서 사용하면 closure가 된다. (당연한가?)

```
 val min = 1
 var decrease = (x: Int) => x - min


 decrease(6) // 5
```


### call by name

call by name 순수 언어론자들이 가장 좋아하는 (물론 성능은 차치하고)방식의 파라미터 전달이다.
코드 블록을 호출한 함수로 전달하고, 전달 받았을때 파라미터에 접근하는 방식. 함수 실행 타이밍에 값을 계산한다.


```
def calculate () = {
  println("calculate..")
  1 + 1	
}
def delayedPass( calculate: => Int ) = {
  println("delayedPass..")
  println(calculate)	
}

// 실행함수에서
delayedPass(calculate())

```

### 부분 적용 함수

파라미터를 미리 bind할 수 있는 경우 사용한다.
아래 예제를 참고해보자.
(예제 설명: 키 생성할때, prefix가 있어서 무조건 반영해야할 경우, 아래처럼 미리 바인딩을 할 수 있다.)

```
  def createKey(prefix:String, postfix: String) = {
    prefix + postfix
  }
  // 실행함수 
  val prefix:String = "prefix_"
  val createKeyWithPrefixBound = createKey(prefix, _ : String)
  println(createKeyWithPrefixBound("1")) // prefix_1
  println(createKeyWithPrefixBound("2")) // prefix_2
  println(createKeyWithPrefixBound("3")) // prefix_3
```

## String

* 스칼라의 String은 java.lang.String  에서 가져왔다. 
* 그래서 스칼라 String은 immutable이다. 
* 두개의 호환이 자연스러운 편이다.

아래는 예제다.
```
object Main{
  def main(args : Array[String]){
    val greeting : String = "test"
    println(greeting)
  }
}
```

* 문자열 생성하는 법은 두가지이다. 한가지는 타입을 선언, 다른 한가지는 타입을 적지 않는 암묵적 방법이다.

```
  val greeting = "test"
  val greeting2: String = "test"
```
* 자바랑 다른 점은 length가 메소드로 제공한다.
* + 연산자와 concat도 제공한다.

## Collection

## Array

* 간단히 배열이다.
* 고정 크기 배열이다. 동일 타입의 고정 데이터를 만들때 주로 사용한다

```
var arr:Array[String] = new Array[String](10)

var arr2 = new Array[String](10)
```

* 요소에 접근 지정하는 방식이 자바랑 다르다.
* [] 대신 ()를 쓴다 (선언도 그래서 ()를 쓴다)

```
var arr3 = Array(1,2,3,4,5)
arr3(0) // 1
```

#### 다중 배열 

* Array.ofDim을 사용한다.
```
import Array._

  var matrix = ofDim[Int](10,10) 
  matrix(0,0) // 0, 0 번째 접근
```
#### 배열 두개 붙이기

* Array.concat을 이용한다.
```
import Array._
var list = Array(1,2,3,4)
var list2 = Array(5,6,7)
var list3 = concat(list, list2)
```

#### for 루프

* 아래 처럼 사용하면 된다. 

```
var list = Array(1,2,3,4)
var list2 = Array(5,6,7)

for ( data <- list ){
  println(" " + data)
}

for (data <- 0 to list2.length()){
  println(" " + data)
}

```


### 유용한 함수들(static 형태로 제공함)

### List
* 리스트는 자바의 리스트와는 달리 immutable이다.. 
* 아래와 같이 선언 가능하다.

```
val nums: List[String] = List(1,2,3,4,5)
val empty : List[Nothing] = List()
```

* 다차원 리스트도 만들수가 있다.

```
val = dim: List[List[Int]] = 
   List ( 
     List(1, 0, 0),
     List(0, 1, 0),
     List(0, 0, 1)
   )
```   

### :: 연산  ( 요소 붙이기 연산)
* 리스트를 만들때 :: 를 붙여서 만들수도 있다.
* 다만 맨 마지막에 Nil 을 붙여줘야한다.(괄호까지 붙여야 하니, 괄호치다 한 세월이겠다)
```
 val nums = 1 :: ( 2 :: ( 3:: Nil)))
```
### tabulate 함수
* 리스트를 만들어주는 일종의 factory이다
* 파리미터에 입력한 숫자 만큼의 개수의 리스트를 만든다.
* curried function 형태로 만들 수도 있음
* 0부터 입력이 시작된다.
```
//1,2,3,4,5
 val list = List.tabulate(5) {x => x+1} 
 
 //List(List(1, 1), List(2, 2), List(3, 3), List(4, 4), List(5, 5))
 val list = List.tabulate(5,2) {(x:Int, y:Int) => x+1}
 
```

### fill 함수
* 리스트를 만들어주는 일종의 factory이다
* 특정 값으로 채워진 함수로 만들 수 있음
* curried function 형태로 만들 수도 있음

```
 val list = List.fill(3)(false) // false,false,false
 val list2 = List.fill(2){ 1 } // 1, 1
 
```


### Map

* key value pair 를 가지고 있는 컬렉션이다.
* 이 pair를 찾는 기준은 key이다
* key는 unique해야한다.
* mutable, immutable 두 가지가 있다.
* import scala.collection.mutable.Map 를 사용할 수 있다.


#### 선언 방법

```
import scala.collection.mutable.Map 
//  중략
var lookupTable: Map[String,Int] = Map()

val colormap = Map ("black" => "#000000", "red" => "#FF0000")
```

위와 같다.

#### Map에 요소를 추가하는 방법

* + 연산자를 사용한다. 아래 예제를 보면 명확하다

```
lookupTable += ('max.pager.size' -> 10000)
lookupTable += ('window.size' -> 5)
```

#### 두개의 맵을 합치는 방법
* ++ 연산자를 이용한다. 
```
val colormap1 = Map ("black" => "#000000", "red" => "#FF0000")
val colormap2 = Map ("white" => "#FFFFFF")

var combinedColorMap = colormap1 ++ colormap2
var combinedColorMap2 = colormap1.++(colormap2)
```

* Map의 반복자는 foreach 메소드를 제공한다
* 이 foreach 메소드는 클로저를 이용한다. 

```
val colormap1 = Map ("black" => "#000000", "red" => "#FF0000")

colormap1.keys.foreach {
	i =>  print("Key" + i + " value="  + colormap1(i))
}
```

#### Map 참고

[http://www.tutorialspoint.com/scala/scala_maps.htm](http://www.tutorialspoint.com/scala/scala_maps.htm)


### Tuple

* 자바에는 기존에 튜플이 존재하지 않았다. 만들려면 신규로 object로 만들어야 했다.
* 이름이 없거나 알수 없는 데이터의 묶음을 이 자료형으로 만드는것도 괜찮다.
* 튜플은 고정 개수 요소가 합쳐 하나로 보는 것이다 
* 선언은 아래와 같이 할 수 있다
* 요소가 두개면 Tunple, 세개면 Tuple3

```
val helloInfo = (10, "hello", 100)
val helloInfo2 = new Tuple3(10, "hex", 100)
```

* 이 자료구조를 기본으로 spark 가 만들어졌다

#### 튜플의 각 요소에 access 하는 법

* _숫자 형태로 각 요소에 접근한다.

```
helloInfo._1 // 10
helloInfo._2 // "hello"
```

#### 리스트처럼 순차적으로 접근하는 방법

* productIterator 메소드를 이용한다

```
val helloInfo2 = new Tuple3(10, "hex", 100)
helloInfo2.productIterator.foreach { i => println("value = " + i)}
```


#### Map 참고

[http://www.tutorialspoint.com/scala/scala_maps.htm](http://www.tutorialspoint.com/scala/scala_maps.htm)




### Set

* 중복 없는 리스트다. 
* 리스트에서 중복을 제거해야할 때 set을 이용한다.
* 스칼라 리스트와의 차이점은 mutable, immutable 두가지 형태가 존재한다.
* import scala.collection.mutable.Set

```
// Empty set of integer type
var s : Set[Int] = Set()

// Set of integer type
var s : Set[Int] = Set(1,3,5,7)


var s2 = Set(1,3,5,7)
```

* head, tail, isEmpty 함수를 기본으로 제공한다.
* 최대 최소를 구하는 메소드를 제공한다. min, max


#### Set 끼리 합치기
++ 연산을 사용한다.

```
val money = Set("cash", "check")
val money2 = Set("bitCoin")

var concated_set = money ++ money2

for (el <- concated_set){
  println(el)
}
```

* 두 Set의 차집합 연산(-)도 제공한다. 

#### 공통요소 찾기

* 두 Set의 공통 요소를 찾을 수 있다.
* intersect, & 연산자를 이용해서 공통 요소를 뽑을 수 있다.

```
 val numset1 = Set(1,2,3,4,5)
 val numset2 = Set(3)

 println(numset1.&(numset2))
 println(numset1.intersect(numset2))
```

## sbt를 사용한 이클립스용 스칼라 세팅 

* Scala 2.11.7, sbt 0.14.0 기준

1) 맥이라면 brew로 scala 와 sbt를 설치하자 
```
brew install scala
brew install sbt
```
2) 프로젝트용 폴더와 필요한 sbt파일 만든다.
```
# 프로젝트 폴더 생성
$ mkdir project_name
$ touch build.sbt
$ mkdir project_name/project
$ touch project_name/plugins.sbt
```
3) plugins.sbt 파일에 eclipse 플러그인을 설정한다

```
addSbtPlugin("com.typesafe.sbteclipse" % "sbteclipse-plugin" % "4.0.0")
```
4) 프로젝트 루트로 이동한뒤 프로젝트를 생성한다
```
sbt eclipse
```
5) eclipse를 실행해서 File > Import 를 누르고 Existing Projects into Workspace


### 주의사항 
sbt, scala는 버전이 맞지 않으면 정상 동작 하지 않는다.
가능하면 최신 버전으로 업데이트 하자.

## 공부하기 좋은 자료들
* [Scala school](https://twitter.github.io/scala_school/ko/basics.html)
* [Effective scala](http://twitter.github.io/effectivescala/)
* [Scala Tutorial](http://www.tutorialspoint.com/scala) : 쉽게 볼 수있는 스칼라 튜토리얼, 문법을 찬찬히 살펴볼때 좋다. 
