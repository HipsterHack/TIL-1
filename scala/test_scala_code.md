## 스칼라에서의 테스트 

스칼라 테스트 프레임워크는 TDD와 BDD를 다양하게 지원한다. BDD는 좀 생소할 수 있어 다른 절에서 설명하겠다.(일단 내가 잘 모르므로 저엉리 하겠다.)
덕분에 많은 스칼라테스트는 다양하게 테스트 코드를 만들 수 있다.
다양한 스타일은 장점이지만 단점이기도 하다. 그래서 어떤걸 써야할지 막막한 상황이 발생한다.

## 프레임워크 제작자가 추천하는 스타일

그리하여, 프레임워크 제작자는 이들 위해 추천안을 만들었으니.. 다음과 같다. 
Unit test에는  FlatSpec을, Integration test에는 FeatureSpec을 추천함!

## BDD (Behaviour Driven Development)

BDD는 비즈니스 요구사항에 집중하여 테스트 케이스를 개발하는거다. 
상세하게 설명하면, 유저 스토리를 우리가 쓰는 글처럼 쓰는 형태를 말한다.
여러 형태가 있지만 한가지 형태는 given when then 형태로 테스트를 하자는 것.
즉 영어를 잘해야 한다라는 것과 일맥상통.(어디가?)

### 설치 방법 

plugins.sbt 파일에 아래 내용을 추가하면 된다 (작성당시 최신버전)
```
libraryDependencies += "org.scalatest" % "scalatest_2.11" % "2.2.4" % "test"
```

### 테스트 스타일
여기 기록하는 코드는 스칼라 테스트 공식홈에서 퍼온 것이다.


#### FunSuite
- xUnit 스티일과 제일 비슷하다
```

import org.scalatest.FunSuite

class SetSuite extends FunSuite {

  test("An empty Set should have size 0") {
    assert(Set.empty.size == 0)
  }

  test("Invoking head on an empty Set should produce NoSuchElementException") {
    intercept[NoSuchElementException] {
      Set.empty.head
    }
  }
}
```
 
#### FlatSpec

* BDD 형태 

```
import org.scalatest.FlatSpec

class SetSpec extends FlatSpec {

  "An empty Set" should "have size 0" in {
    assert(Set.empty.size == 0)
  }

  it should "produce NoSuchElementException when head is invoked" in {
    intercept[NoSuchElementException] {
      Set.empty.head
    }
  }
}
```

#### FunSpec

* RSpec 과 비슷한 테스트 케이스
```
import org.scalatest.FunSpec

class SetSpec extends FunSpec {

  describe("A Set") {
    describe("when empty") {
      it("should have size 0") {
        assert(Set.empty.size == 0)
      }

      it("should produce NoSuchElementException when head is invoked") {
        intercept[NoSuchElementException] {
          Set.empty.head
        }
      }
    }
  }
}
```

#### WordSpec
* specsN 스타일 테스트라고 한다ㅣ

```
import org.scalatest.WordSpec

class SetSpec extends WordSpec {

  "A Set" when {
    "empty" should {
      "have size 0" in {
        assert(Set.empty.size == 0)
      }

      "produce NoSuchElementException when head is invoked" in {
        intercept[NoSuchElementException] {
          Set.empty.head
        }
      }
    }
  }
}
```
#### FreeSpec

* 어떤 가이드도 없는 프리한 형태의 테스트. BDD에 익숙한 사람에 추천한다고 한다.

```
import org.scalatest.FreeSpec

class SetSpec extends FreeSpec {

  "A Set" - {
    "when empty" - {
      "should have size 0" in {
        assert(Set.empty.size == 0)
      }

      "should produce NoSuchElementException when head is invoked" in {
        intercept[NoSuchElementException] {
          Set.empty.head
        }
      }
    }
  }
}
```
#### Spec

```
import org.scalatest.Spec

class SetSpec extends Spec {

  object `A Set` {
    object `when empty` {
      def `should have size 0` {
        assert(Set.empty.size == 0)
      }

      def `should produce NoSuchElementException when head is invoked` {
        intercept[NoSuchElementException] {
          Set.empty.head
        }
      }
    }
  }
}
```

#### PropSpec
* property 체크와 테스트를 분리할때 쓰기 좋음

```
import org.scalatest._
import prop._
import scala.collection.immutable._

class SetSpec extends PropSpec with TableDrivenPropertyChecks with Matchers {

  val examples =
    Table(
      "set",
      BitSet.empty,
      HashSet.empty[Int],
      TreeSet.empty[Int]
    )

  property("an empty Set should have size 0") {
    forAll(examples) { set =>
      set.size should be (0)
    }
  }

  property("invoking head on an empty set should produce NoSuchElementException") {
    forAll(examples) { set =>
       a [NoSuchElementException] should be thrownBy { set.head }
    }
  }
}
```

#### FeatureSpec
* acceptance test에 사용할때 편함

```
import org.scalatest._

class TVSet {
  private var on: Boolean = false
  def isOn: Boolean = on
  def pressPowerButton() {
    on = !on
  }
}

class TVSetSpec extends FeatureSpec with GivenWhenThen {

  info("As a TV set owner")
  info("I want to be able to turn the TV on and off")
  info("So I can watch TV when I want")
  info("And save energy when I'm not watching TV")

  feature("TV power button") {
    scenario("User presses power button when TV is off") {

      Given("a TV set that is switched off")
      val tv = new TVSet
      assert(!tv.isOn)

      When("the power button is pressed")
      tv.pressPowerButton()

      Then("the TV should switch on")
      assert(tv.isOn)
    }

    scenario("User presses power button when TV is on") {

      Given("a TV set that is switched on")
      val tv = new TVSet
      tv.pressPowerButton()
      assert(tv.isOn)

      When("the power button is pressed")
      tv.pressPowerButton()

      Then("the TV should switch off")
      assert(!tv.isOn)
    }
  }
}
```

## 스칼라 Mock 
* 스칼라에서도 mock을 이용해서 테스트 할 수 있다.
[스칼라 Mock](http://scalamock.org/quick-start/#testing-with-scalamock)

### 설치 방법
sbt로 아래 구문을 사용한다.

```
libraryDependencies += "org.scalamock" %% "scalamock-scalatest-support" % "3.2" % "test"
```

### 참고 
* [BDD](http://blog.jaigurudevaom.net/319)
* [TDD BDD 차이점](http://blog.aliencube.org/ko/2014/04/02/differences-between-bdd-and-tdd/)
* [스칼라 테스트 스타일](http://www.scalatest.org/user_guide/selecting_a_style)
* [테스트 픽스쳐](http://stackoverflow.com/questions/16367489/initialize-val-on-a-scalatest-fixture)
* [스칼라Mock](http://scalamock.org/quick-start/)

