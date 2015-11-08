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
