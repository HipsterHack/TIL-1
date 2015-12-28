
# 암시적 변환 

라이브러리를 사용하는 코드와 우리네가 만든 코드와는 극명한 차이가 있다. 
우리코드는 확장이 쉽지만 라이브러리는 규칙에 따라 사용한다.

언어는 이 문제를 해결해려고 여러 방법을 썼다.
루비는 모듈이 있고 스몰톡은 패키지가 서로 클래스를 추가할 수 있다.

어쨌든 이런 문제를 해결하려고 스칼라는 암시적 변환과 암시적 파라미터를 사용했다.

## 장점
* 주로 연동 고려가 아예 안된 소프트트웨어가 함께 동작해야 할 때 도움이 된다. 
* 타입 변경을 위한 많은 explicit conversion을 줄여준다. 즉 코드가 간결해진다.

## Swing 예제
* 스윙을 쓸때  자바랑 비슷함
```
val button = new JButton
button.addActionListener( 
  new ActionListener {
    def actionPerformed(event: ActionEvent) {
      println("pressed!")
    }
  }
)
```
* 실제 쓰는건 아래 코드 정도다.
```
button.addActionListener(
  (_ : ActionEvent) => println("pressed!")
)
```

* 근데 동작을 안한다. 그럼 이걸 동작하게 하려면 어떻게 해야할까?
* 간단하다. ActionListener를 대신할 암시적 변환 함수를 만들어야 한다.
```
implicit def function2ActionListener(f: ActionEvent => Unit) = 
  new ActionListener {
    def actionPerformed(event: ActionEvent) = f(event)
  }
```

### 동작 방식 
1. 컴파일러가 코드를 있는 그대로 컴파일한다.
2. 타입 오류가 발생
3. 암시적 변환으로 해결할 수 있는지 체크해본다. 
4. 시도해보고 동작하면 컴파일을 계속 한다.


## 암시 규칙
* 암시적 정의는 컴파일러가 컴파일 중에 타입 오류를 고치기 위한 정의들을 말한다.

### Marking Rule: implicit을 쓴 정의만 사용 가능

* 변수, 함수, 객체에 모두 `implicit`을 달 수 있다.
```
implict def intToString(x: Int) = x.toString
```
* `x + y`에 타입 오류가 발생했으면 scope 내의 implicit 함수를 찾아서 convert를 해본다.
* 이런 규칙을 사용함으로써 만약 컴파일러가 랜덤으로 함수를 사용했다면 발생했을 혼란을 막을 수 있다.
* 컴파일러는 명시적으로 implicit를 선언한 정의만 사용한다.

### Scope Rule: 삽입할 implicit 변환은 scope 안에서 단일 식별자, 변환의 결과나 원래 타입과 연관이 있어야 한다. 
* 스칼 컴파일러는 scope안에서 암시적 변환만 고려해서 사용한다.
* 단일 식별자로 존재해야한다. 
* 예외가 있다. source 타입이나 변환 결과 타입의 companion 객체의 암시적 정의도 살핀다. (companion 객체에 있는 암시적 정의를 사용하게 되면 연관이 있다고 한다)
* scope는 한 파일이내지만 import 한 케이스는 다른 파일을 참조할 수 있다. 

### One-at-a-time Rule: 하나의 implicit 선언만 사용
* x + y는 convert1(convert2(x)) + y로 변환되지 않는다.
* 그랬다간 프로그래머의 의도와 프로그램이 달라질 수 있다.

### Explicit-First Rule: 코드가 그 상태로 타입 검사를 통과 하면 암시 변환은 사용하지 않음.
* 컴파일러는 이미 잘 동작하는 코드는 변경하지 않는다. 이때는 명시적 선언으로 암시적 식별자를 대신할 수 있다.
* 코드가 장황하면 암시적 변환을 사용하고
* 코드가 모호하면 명시적 변환을 추가한다. 

## 이름짓기 
* 아무 이름이나 붙일 수 있다.

### 메소드 호출시 명시적으로 변환을 사용 

### 프로그램의 특정 지점에서 사용가능한 암시적 변환이 어떤게 있는가?
* 암시적 변환이 여러개가 있다면 특정 암시변환만 임포트를 해서 쓴다.

## 암시가 쓰이는 부분 
* 컴파일러가 원하는 타입으로 변환
예) String이 있는데..이걸 IndexedSeq[Char]을 받는 함수에 넘기고 싶다고 해보자..이런 경우 implicit을 탄다.
* 어떤 선택의 호출 대상 객체를 변환
예) "abc".exists가 stringWrapper("abc").exists로 변환된다. 왜냐하면 exists method가 String에는 없지만 IndexedSeq에 있다
* 암시적 파리머터를 지정하는 부분
예) 함수 호출시 caller가 원하는 추가 정보를 함수에 제공할때 사용한다. generic에서 특히 유용하다.

## 예상타입으로 암시적 변환 
* 컴파일러가 Y 타입이 필요한 위치에 X 타입을 봤으면 X를 Y로 변하는 암시적 함수를 찾는다.
* `scala.Predef`에 더 큰 크기의 변환은 암시변환으로 선언되어 있다. 그래서 Int to Double은 큰 문제가 없이 코딩할 수 있다.
## 호출 대상 객체 변환 
### 용도 
* 호출 대상 객체 변환을 이용해서 새 클래스를 기존 클래스 계층 구조에 손쉽게 통합할 수 있다.
* 언어안에서 DSL을 만드는 일을 지원한다. 

### 새 타입과 통합
* 어떤 객체나 클래스 등에 타입변환에 대한 기능이 없으면 자동으로 추가해준다.
```
class Rational(n:Int, d: Int) {
  def + (that: Rational): Rational = ...
  def + (that: Int): Rational = ...
}
val oneHalf = new Rational(1, 2)
```
* 위 예제에서 `oneHalf + 1`은 가능하지만 `1 + oneHalf`는 오류가 발생한다.
* 이런걸 하려면 
```
implicit def intToRational(x: Int) = new Rational(x, 1)
```
위 암시적 변환을 만들어 참조해야한다. 

### 새로운 문법 흉내 

* Map의 `->`는 문법이 아니다 메소드다!
```
Map ( 1 -> "one", 2 -> "two")
```
* 프리엠블 `scala.Predef`의 ArrowAssoc 클래스 메소드다. 아래는 그 정의다. 
```
package scala
object Predef {
	class ArrowAssoc[A](x: A){
	  def -> [B](y: B): Tuple2[A, B] = Tuple2(x, y)	  
	}
	implicit def any2ArrowAssoc[A](x: A): ArrowAssoc[A] = new ArrowAssoc(x)
}
```

* 이런 풍부한 래퍼는 DSL을 쓰기 위한 밑거름이다. 
