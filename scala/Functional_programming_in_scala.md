# 스칼라로 배우는 함수형 프로그래밍

스칼라로 배우는 함수형 프로그래밍 책을 보고 내용을 정리하는 페이지다.


## 2장 연습문제 풀이 ##

### 2.2 ###
#### 문제 ####
Array[A]가 주어진 비교함수에 의거하여 정렬되어 있는지를 체크하는 isSorted[A]를 구현해라
```
  def isSorted[A](as: Array[A], ordered: (A, A) => Boolean): Boolean
```

#### 풀이 ####
* tailrec annotation을 사용해서 tail recursion을 사용하는지 체크했다
* loop 내장함수를 사용해서 루프를 구현했다.
```
import scala.annotation.tailrec

object ArrayUtils {
  def isSorted[A](as: Array[A], ordered: (A, A) => Boolean): Boolean = {
    @tailrec
    def loop(n: Int, acc: Boolean): Boolean = 
      if (n == as.length - 1) acc 
      else loop(n + 1, acc && ordered(as(n), as(n + 1)))
    loop(0, true)
  }

  def main(args: Array[String]) = {
    val arrs:Array[Int] = Array[Int](1,2,3,4,5,6,7)
    val notSorted:Array[Int] = Array[Int](1,2,4,3,5,6,7)
    val sorted:Array[Int] = Array[Int](1,2,4,4,5,5,7)
    
    println(isSorted[Int](arrs, {(a, b) =>
            a <= b 
          }))
    println(isSorted[Int](notSorted, ((a, b) => a <= b )))
    println(isSorted[Int](sorted, ((a, b) => a <= b )))
  }
}
```


### 2.3, 2.4,2.5 ###
#### 문제 ####
curry , uncurry, compose를 구현해라
```
  def curry[A, B, C](f: (A, B) => C): A => (B => C) 
  def uncurry[A, B, C](f: A => B => C): (A, B) => C 
  def compose[A, B, C](f: B => C, g: A => B): A => C
```
#### 풀이 ####
* 고차 함수의 특성을 이용했다.

```
object Curry extends App {
  def curry[A, B, C](f: (A, B) => C): A => (B => C) =
    (a: A) => (b: B) => f(a, b)

  def uncurry[A, B, C](f: A => B => C): (A, B) => C =
    (a: A, b: B) => f(a)(b)

  def compose[A, B, C](f: B => C, g: A => B): A => C =
    (a: A) => f(g(a))

  println(curry[Int, Int, Int]((a: Int, b: Int) => a + b)(10)(20))
  println(uncurry[Int, Int, Int] { a: Int => (b: Int) => a + b }(10, 20))
  println(compose[Int, Int, Int]({ a: Int => a * 10 }, { b: Int => b + 1 })(2))
}
```

## 3장 함수적 자료구조 정리 ##
* 함수적 자료구조는 immutable 이다.
* 그리고 자료구조 내에서 같은 값의 자료는 공유한다. (일종의 design pattern 중 flyweight 패턴과 비슷)

### sealed 키워드 ###
* 클래스나 trait 앞에 붙는다
```
sealed trait Gender
object Male extends Gender
object Female extends Gender
```
* 위와 같이 사용한다. 
* 상속을 같은 파일 내에서만 하도록 제한할 때 사용한다.

### 공변(covariant) ###
* generic에 [+A], [-A], [A] 3가지가 있다.
* [+A]는 컴파일러가 지정한 타입 하위 타입으로 간주한다
```
 val list:List[Animal] = List[Dog]()
```
* [A]는 어떤 타입이든지 개의치 않으며..
* [-A]는 지정한 타입 상위 타입으로 간주 한다는 것이다.


### 패턴 매칭 ###
* companion 객체는 패턴 매칭을 사용한다.
* 패턴 매칭 규칙은 식별자와 자료 생성자로 구성된다. 이 모든 것이 매칭되는 케이스가 있으면, 그걸 수행한다.
* 구조적으로 동등하면 매칭된다고 본다.
* 생성자 패턴과 변수를 함께 묶는다. 
* 매치가 안되면? MatchError 오류를 발생한다. 
* 예
```
 def sum(ints: List[Int]): Int = ints match {
   case Nil => 0
   case Cons(x, xs) => x + sum(xs)
 }
 
 def product(ds: List[Double]): Double = ds match {
   case Nil => 1.0
   case Cons(0.0, _) => 0.0
   case Cons(x, xs) => x * product(xs)
 }
 
 List (1,2,3) match { case Cons(h, _) => h }
 // 위와 상등은            Cons(1, Cons(2, Conms(3, Nil))) 이다
 // 즉 위 값은 1이다.
```

### 3.3 문제 풀이 ###
#### 문제 ####
* dropWhile을 구현하라.
* 첫 요소부터 시작한다
* f가 true 일때까지 요소를 지우고 돌려준다
#### 풀이 ####
```
package fpinscala.datastructures

sealed trait List[+A]
case object Nil extends List[Nothing]
case class Cons[+A](head: A, tail: List[A]) extends List[A]

object List {
  def apply[A](as: A*): List[A] =
    if (as.isEmpty) Nil
    else Cons(as.head, apply(as.tail: _*))

  def tail[A](l: List[A]): List[A] = l match {
    case Nil        => Nil
    case Cons(h, t) => t
  }

  def dropWhile[A](l: List[A], f: A => Boolean): List[A] = l match {
    case Nil        => Nil
    case Cons(h, t) => if (f(h)) dropWhile(tail(l), f) else l
  }
}

object ListTest extends App {
  println(List.dropWhile[Int](List(1, 1, 3, 4), _ % 2 != 0))
  println(List.dropWhile[Int](List(1, 2, 3, 4), _ % 2 != 0))
}
```


## 참고 자료 
* [스칼라 기본 타입](https://twitter.github.io/scala_school/ko/type-basics.html)
* [FP in Scala 답](https://github.com/fpinscala/fpinscala)
