# 스칼라 정리
이 페이지에서는 스칼라 공부하면서 배운 내용을 정리합니다.

## 함수형 프로그래밍
* 스칼라로 함수형 프로그래밍을 하면 모듈성이 증가한다
* 이 때문에 일반화 병렬화 가 쉽고 모듈성이 증가한다.
* 다만, 재귀호출을 자주 사용하므로 언어레벨에서 지원하는 최적화가 중요하다

### 함수형 프로그래밍 방법
* 메소드는 부수효과가 없어야 한다.
* 메소드는 return 값으로만 소통해야한다.( Unit의 리턴값이 있으면 부수효과 메소드다.)
* 참조 투명성을 가져야 한다. (언제나 새로운 값을 리턴한다) => 스칼라 코딩이 val 위주인 이유다
* 메소드는 참조 투명한 표현식의 특성을 가진다.
* 스칼라 코드는 재사용 가능한 구성요소로 구성되며 이들끼리 합성할 수 있다
* 대부분의 스칼라 API는 immutable 이며 mutable은 따로 import해야한다.
*  break나 continue없이 살자 => 재귀를 이용해서 풀자. 
*  아래는 FP in scala 인용
``` 
모든 프로그램 P에 에 대해 표현식 e의 모든 출현을 e의 평과 결과로 치환해도 p에 아무영향이 없다면 e는 참조에 투명하다. 
f(x)가 참조에 투명한 모든 x에 대해 참조에 투명하면 f는 순수하다.
```

#### 부수효과가 나오는 행위
* 변수를 수정한다
* 자료구조를 제 자리에서 수정한다
* 객체의 필드를 설정한다
* 예외를 던지거나 오류를 내면서 실행을 중단한다
* 콘솔에 출력하거나 사용자의 입력을 읽어들인다
* 화면에 그린다.

#### 기타 코딩 팁

* match - case를 사용할 때는 if를 case 안으로 넣어서 평가문의 if를 줄이자.
* 아니면 분기처리가 많아져서 읽기가 어려워진다. 
```
 case _ if loop(l, sub) => true
```

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

## 메소드 작성시 유의점
* return 을 명시적으로 사용하지 말아라
* 메소드가 하나의 값을 계산하는 표현식으로 여겨라. (리턴을 한개만 만들어라)
* 이 결과가 return 값.
* 메소드를 작게 만들 수 있다. 여러 메소드로 나누게 됨
* 중괄호를 없는쪽으로 만들어라



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



### 메소드 호출 축약법

처음엔 정말 황당했다. 이 표현법은 대체 무엇일까?
스칼라가 허용한 파격적인 문법일까?

```
request OK as.String
```

이건

```
request.OK(as.String)
```
의 다른 표현 이었던 것이다!(두둥)

개발자로 하여금 쓰기 편하게 만든 문법적 축약이다.  개인적으로 비호감인 축약형태. 
최근에 만들어진 언어의 영향을 많이 받은듯 싶다.


## 데이터 타입 
* Int
* BigInt
* Double
* Float
* String
( 추후 정리 예정)

## 클래스
```
// 클래스 파라미터 n, d 
class Rational(n :Int, d: Int){
  // 여기서부터는 생성자로 들어간다.
  // require는 클래스 파라미터의 오류를 체크한다.
  require(d != 0)
  private val g = gcd(n.abs, d.abs)
  val numer = n / g
  val denom = d / g

  def this(n: Int) = this(n, 1)

  def + (that: Rational): Rational =
    new Rational( 
           numer * that.denom + that.numer * denom,
           denom * that.denom
    )
  
  override def toString = numer + "/" + denom

  private def gcd(a: Int, b: Int): Int = 
    if (b == 0) a else gcd(b, a % b)
}
```

* 클래스 파라미터(위 코드에서 n,d)는 생성자에서 사용한다 
* def 가 없이 선언된 변수는 public로 접근이 가능하다.
* requre() 는 false면 IllegalArgumentException을 발생한다.
* 생성자를 추가하려고 할땐 this를 이용한다
* 재귀 함수 호출을 지향한다. 다만 tail recursion 을 이용하면 스칼라 내부에서 최적화한다.
* override할떈 override 키워드를 이용한다
* ->, <- 는 한개의 리터럴로 인식한다.
* x.add(y)는 x add y 로 표현할 수 있다
* val는 할당이 불가능한거다. 내부 상태는 변할 수 있다. 대체로 장점을 가진다. 하지만 레퍼런스가 여러 개로 연결되어서 복사할게 많으면 멸망이다.

### 메소드 오버로드

* 오버로드는 자바의 오버로드와 비슷하다

### 암시적 타입 변환
* 2 + r 은 안되는 형태(Int.+(r)이기 때문)
* implicit을 이용하여 메소드를 선언하면 변환을 할 수 있다.
* 암시적 변환을 이용할땐 import scala.language.implicitConversions 를 해서 문제를 해결하는게 좋다.
* 클라이언트가 간결하며 이해하기 쉬우면 좋지만 지나치면 나쁘다.

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
다만, 트레잇은 클래스 파라미터를 가질 수 없다
자바의 인터페이스와 비슷한 용도로 추상 클래스와 트레잇 중에서 어떤 걸 쓸지 고민 하는 경우가 있다.
문서에서의 추천은 기승전 트레잇!
스칼라 언어 문법상 클래스는 1개만 상속(extend)할 수 있지만, 트레잇은 여러 트레잇을 믹스인 할 수 있다.
다만, 생성자 매개변수가 필요한 경우라면 추상 클래스를 사용하란다.
추상 클래스의 생성자는 매개변수를 받을 수 있다 하지만 트레잇의 생성자는 매개변수를 못받는다.
예를 들어 trait t(i: Int) {} 에서 i 매개변수는 문법상 못쓴다.

### 트레잇(Trait) 사용 팁

* 트레잇은 간결한 인터페이스를 확장해 풍부한 인터페이스로 사용할 수 있다.
메소드를 구현해서 넣어도 되기 때문이다.
* 나머지는 쌓을수 있는 변경을 정의 하는 것.


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
* 외부의 변수를 바인딩 시킨 경우 정확하게 클로저라 할 수 있다.
* 외부 변수를 가져와 포획하여 바인딩 하면서 closing을 한다

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
### Tail recursive
* 한 메소드 내에서 같은 메소드 호출시만 Tail Recursive 가능.
* 래핑 된거나 여러개의 재귀호출이면 불가능함
* JVM이 지원하는게 아님. 스칼라 언어에서 제공하는 최적화
* 최적화 코드는 같은 메소드 스택을 사용한다. 그래서 재귀호출이 안되는것처럼 보임

### curry method
* 학자이름에서 따왔다
* 두 개 이상의 함수를 하나의 함수처럼 호출 하는 경우를 커리라고 부른다.
```
def curriedSum (x: Int)(y: Int) = x + y
```
* 여기서 curriedSum(1)로 호출하면 파라미터 한개를 바인딩하고 함수 값을 return 한다.

#### 일반함수에서 커리 함수 사용으로 예제
* 파일 내용을 PrintWriter로 화면에 뿌려줄때의 메소드
```
def withPrintWriter(file :File, op: PrintWriter => Unit){
// 중략
}
```
* 커리 함수로 변환하자. 좀더 쓰기 편해진다.
```
def withPrintWriter(file :File) (op: PrintWriter => Unit){
// 중략
}

withPrintWriter(file) {
  writer => writer.println("file.name")
}
```
##### 주의점
* 위에서 {}는  인자가 하나일때 사용가능하다.
* 두개이면? 에러. 클라이언트 프로그래머에게 함수 리터럴 사용을 권장하기 위함.




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
## Option

* Option은 값의 존재를 알려주는 데이터 타입이다.
* Some[T] or None의 값을 가진다. T는 임의의 템플릿이다.

* Map을 사용할때 주로 Option을 이용한다.
* 값이 없을때 기본 값으로 리턴할 때 쓴다. 

```
object Test {
  def main (args : Array[String]){
    val params = Map("page" -> 1)

    // page 키가 있으면 page 값 출력 없으면 기본값출력
    println ("page=" + defaultPage(params.get("page")))
    // None이 출력
    println ("pageSize=" + params.get("pageSize").getOrElse(10))
  }

  def defaultPage(param:Option[Any]) = param match {
    case Some(something) => something
    case None => 1
  }
}
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

## 예외처리

* try-catch 절을 사용한다.

```
try{
 new URL(path)
} catch {
  case e: MalformedURLException => new URL("http://comic.naver.com")
}finally{
}
```
* finally엔 리턴을 명시하지 않으면 try, catch절에 값을 덮어씌운다.
* 자바와는 다르다 자바와는!

## match 

* match는 switch - case 와 유사하다
* 값을 기준으로 동작을 바꾸기 위해 사용한다.
* 그리고 값을 조건에 따라 다르게 할당할 때도 사용한다.
```
firstArg match {
 case "salt" => println("pepper")
 case _ => println("what?")
}

val friend = firstArg match {
 case "salt" => println("pepper")
 case _ => println("what?")
}

println(friend)
```

## 공부하기 좋은 자료들
* [Scala school](https://twitter.github.io/scala_school/ko/basics.html)
* [Effective scala](http://twitter.github.io/effectivescala/)
* [Scala Tutorial](http://www.tutorialspoint.com/scala) : 쉽게 볼 수있는 스칼라 튜토리얼, 문법을 찬찬히 살펴볼때 좋다. 
