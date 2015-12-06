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
### 대수적 자료 형식 ###
* 일종의 복합 타입이다.(Tuple이나 record가 예)
* 1개 이상의 data 생성자만 있다. 데이터 생성자의 파라미터는 필드라 부른다.
* 이런 data 형식을 data 생성자의 sum합 또는 합집합union이라 부른다.
* 각 data 생성자는 생성자가 가진 인수의 곱product 이라 부른다.


### 참고 ###

* [대수적자료형식 위키피디아](https://en.wikipedia.org/wiki/Algebraic_data_type)


### 3.3 문제 풀이 ###
#### 문제 ####
* drop, dropWhile을 구현하라.
* 첫 요소부터 시작한다
* f가 true 일때까지 요소를 지우고 돌려준다( dropWhile만)
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

  def drop[A](l: List[A], n: Int): List[A] = {
    def go(next: List[A], cnt: Int): List[A] = 
      if (cnt == n || next == Nil) next
      else go(tail(next), cnt + 1)    
    go(l, 0)
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

### 3.6 문제풀이 
#### 문제
* 맨 뒤 요소를 빼는 init을 구현한다. 
### 해결책
* O(N)으로 밖에 구현이 안된다. 
* 링크드 리스트고 탐색하는데 걸리는시간이 O(N)이기 때문이다. 
```
  def init[A](l: List[A]): List[A] = l match {
    case Nil => Nil
    case Cons(_,Nil) => Nil
    case Cons(h, t) => Cons(h, init(t))
  }
```

### 3.7
#### 문제
* fold로 구현한게 0.0을 만났을때 재귀를 멈추는가?
#### 해결책
* 구현하지 않았다. 
* 0을 만났을 때 fold를 중단하는 방식을 찾으면 된다.

### 3.9
#### 문제
* length를 구현하라
#### 해결책
```
def length[A](ns: List[A]) =
    foldRight[A, Int](ns, 0)((x: A, y: Int) => y + 1)

```
### 3.10
#### 문제
* foldLeft를 구현하라
* stack safe하도록 구현해라

#### 해결책
* Tail Recursion과 가산 변수를 메소드에 추가하자
* 이러면 가산 변수에 결과값을 넣으면 tail recursion을 한다.
```
  def foldLeft2[A, B](as: List[A], z: B)(f: (B, A) => B): B = {
    @tailrec
    def fold[A, B](l: List[A], b: B, fx: (B, A) => B, acc: B): B = l match {
      case Nil          => b
      case Cons(h, Nil) => fx(acc, h)
      case Cons(h, t)   => fold(t, b, fx, fx(acc, h))
    }
    fold(as, z, f, z)
  }
```

### 3.11
#### 문제
* sum, product, length를 foldLeft로 구현해라

#### 해결책
```
  def sumByFoldLeft(ns: List[Int]): Int =
    foldLeft2(ns, 0)(_ + _)

  def productByFoldLeft(ns: List[Double]): Double =
    foldLeft2(ns, 1.0)(_ * _)
    
  def lengthByFoldLeft[A](ns: List[A]): Int = 
    foldLeft2[A, Int](ns, 0)((x: Int, y: A) => x + 1)
    
```

### 3.12 
#### 문제
* reverse 를 구현해라. 
* 단 fold메소드를 사용해서 구현할 것

#### 해결책 

* 빈 리스트 (apply로 문제 해결)
```
  def reverse[A](as: List[A]): List[A] = 
    foldLeft(as, List[A]())((acc,el) => Cons(el, acc))
```

### 3.13
#### 문제
* FoldLeft를 foldRight로 구현해라
 
#### 해결책
* reverse로 뒤집는다.
```
  def foldRight2[A, B](as: List[A], z: B)(f: (A, B) => B): B =
    foldLeft(reverse(as), z)((b, a) => f(a, b))
```
### 3.16
#### 문제
* 정수 List에서 1을 더해서 목록을 변환하는 함수를 작성해라
* 단 새 List를 돌려주어야 한다.
#### 해결책
```
  def plusOne(l: List[Int]): List[Int] =
    foldRight(l, List[Int]())((a, b) => Cons(a + 1, b))
```

### 3.17
#### 문제
* List[Double]을 String으로 변환하는 함수를 작성해라 

#### 해결책
* foldRight를 활용하여 문제를 풀었다.
```
  def convertDoubleToString(l: List[Double]): List[String] =
    foldRight(l, List[String]())((a, b) => Cons(a.toString(), b))
```

### 3.18
#### 문제
* map을 구현해라
```
def map[A, B](l: List[A])(f: A => B): List[B]
```
#### 해결책
```
import scala.collection.mutable.ListBuffer
  def map[A, B](l: List[A])(f: A => B): List[B] = l match {
    case Nil        => Nil
    case Cons(h, t) => Cons(f(h), map(t)(f))
  }
  
  def map2[A, B](l: List[A])(f: A => B): List[B] = {
    val buffer = new ListBuffer[B]()
    def go(l: List[A]): Unit = l match {
      case Nil => ()
      case Cons(h, t) => {
          buffer += f(h)
          go(t)
      }
    }
    go(l)
    List(buffer.toList: _*)    
  }
```
### 3.19
#### 문제
* filter를 구현해라
```
def filter[A](l: List[A])(f: A => Boolean): List[A]
```
#### 해결책
* ListBuffer 또는 FoldRight를 이용해서 구현한다.
```
import scala.collection.mutable.ListBuffer
  def filter[A](l: List[A])(f: A => Boolean): List[A] = {
    val buffer = new ListBuffer[A]()
    def go(as: List[A]): Unit = as match {
      case Nil => ()
      case Cons(h, t) => {
        if (!f(h)) buffer += h
        go(t)
      }
    }
    go(l)
    List(buffer.toList: _*)
  }

  def filterViaFoldRight[A](l: List[A])(f: A => Boolean): List[A] =
    foldRight(l, Nil: List[A])((h, t) => if (f(h)) Cons(h, t) else t)
```
### 3.20
#### 문제
* flatMap을 구현해라
```
 List.flatMap(List(1,2,3))(i => List(i, i))
```

* 아래는 함수 시그니쳐이다. 
```
  def flatMap[A, B](l: List[A])(f: A => List[B]): List[B]
```
#### 해결책
* 리스트와 리스트를 붙여주는 append 를 이용한다
```
  def flatMap[A, B](l: List[A])(f: A => List[B]): List[B] = l match {
    case Nil        => Nil
    case Cons(h, t) => append(f(h), flatMap(t)(f))
  }
```
### 3.21
#### 문제
* filter를 이용해서 flatMap을 구현해라
```
def filterViaFlatMap[A](l: List[A])(f: A => Boolean): List[A]
```
#### 해결책
```
  def filterViaFlatMap[A](l: List[A])(f: A => Boolean): List[A] =
    flatMap(l)(v => if (f(v)) Nil else List(v))  
```


### 3.22
#### 문제
* 두 리스트를 더하는 메소드를 만들어라
* List(1,2,3) 와 List(4,5,6) 을 더하면  List(5,7,9)라는 결과가 나오는 메소드를 만들어라 

#### 해결책
* 패턴 매치를 두 개 인자를 대상으로 사용할 수 있다.
```
  def plusLists(l: List[Int], r: List[Int]): List[Int] = (l, r) match {
    case (Nil, Nil)                   => Nil
    case (Cons(h, t), Nil)            => Cons(h, t)
    case (Nil, Cons(h, t))            => Cons(h, t)
    case (Cons(lh, lt), Cons(rh, rt)) => Cons(lh + rh, plusLists(lt, rt))
  }
```

### 3.23

#### 문제
* 3.22를 일반화 시켜서 zipWith 메소드를 만들어라 

#### 해결책
* 커링을 이용해 연산용 메소드를 받는다.
```
  def zipWith[A, B, C](l: List[A], r: List[B])(f: (A, B) => C): List[C] = (l, r) match {
    case (Nil, Nil)                   => Nil
    case (Cons(h, t), Nil)            => Nil
    case (Nil, Cons(h, t))            => Nil
    case (Cons(lh, lt), Cons(rh, rt)) => Cons(f(lh, rh), zipWith(lt, rt)(f))
  }
```
### 3.24

#### 문제
* List가 또다른 List를 부분 순차열로 담고 있는지 체크하는 메소드를 구현한다
* 예를 들어 List(1,2,3,4)는 List(4), List(1,2) 등이 부분 순차열이다.

#### 해결책
* 한 번에 모두 풀려고 하지 말자
* sup 파라미터의 현재 위치에서 주어진 부분 순차열(sub)에 포함되는가의 메소드(loop)
* go는 sup을 뒤로 조금씩 이동하면서 loop가 true를 돌려주는지를  체크한다. 
* 개선점이 있다면 loop나 go는 내부 함수로 두지 않아도 된다.
* case () if를 이용해서 특정 조건일때만 case를 수행되게 하고 나머지는 false처리 할 수 있다.
* 가산 변수 acc를 사용하지 않아도 된다.
```
  def hasSubsequence[A](sup: List[A], sub: List[A]): Boolean = {
    @tailrec
    def loop(target: List[A], subList: List[A], acc: Boolean): Boolean = (target, subList) match {
      case (_, Nil)                     => acc
      case (Cons(lh, lt), Cons(rh, rt)) => if (acc) loop(lt, rt, lh == rh && acc) else acc
      case _                            => false
    }

    @tailrec
    def go(l: List[A]): Boolean = l match {
      case Nil        => false
      case Cons(_, t) => if (loop(l, sub, true)) true else go(t)
    }
    go(sup)
  }
```
* 개선 풀이

```
  def hasSubsequence[A](sup: List[A], sub: List[A]): Boolean = {
    @tailrec // acc 제거 
    def loop(target: List[A], subList: List[A]): Boolean = (target, subList) match {
      case (_, Nil)                                 => true
      case (Cons(lh, lt), Cons(rh, rt)) if lh == rh => loop(lt, rt)
      case _                                        => false
    }

    @tailrec
    def go(l: List[A]): Boolean = l match {
      // subList와 리스트가 같은 값으면 true
      case Nil               => sub == l
      // l이 어떤 값이 오든 현재의 l이 sub로 시작한다면 같은 값 
      case _ if loop(l, sub) => true
      // 다른 값이라 넘어감
      case Cons(_, t)        => go(t)
    }
    go(sup)
  }
```

### 모범답안
```
@annotation.tailrec
def startsWith[A](l: List[A], prefix: List[A]): Boolean = (l,prefix) match {
  case (_,Nil) => true
  case (Cons(h,t),Cons(h2,t2)) if h == h2 => startsWith(t, t2)
  case _ => false
}
@annotation.tailrec
def hasSubsequence[A](sup: List[A], sub: List[A]): Boolean = sup match {
  case Nil => sub == Nil
  case _ if startsWith(sup, sub) => true
  case Cons(_,t) => hasSubsequence(t, sub)
}
```
### 3.25

#### 문제
* 트리의 전체 개수를 구해라
#### 풀이 
```
  def size[A](tree: Tree[A]): Int = tree match {
    case Leaf(v)      => 1
    case Branch(l, r) => 1 + size(l) + size(r)
  }
```

### 3.26
#### 문제
* 가장 큰 Leaf의 value를 구해라
#### 풀이
```
  def maximum(tree: Tree[Int]): Int = tree match {
    case Leaf(v)      => v
    case Branch(l, r) => maximum(l) max maximum(r)
  }
```

### 3.27
#### 문제
* tree의 depth를 구해라
#### 풀이
```
  def depth[A](tree: Tree[A]): Int = tree match {
    case Leaf(_)      => 0
    case Branch(l, r) => 1 + depth(l).max(depth(r))
  }
```

## 참고 자료 
* [스칼라 기본 타입](https://twitter.github.io/scala_school/ko/type-basics.html)
* [FP in Scala 답](https://github.com/fpinscala/fpinscala)
