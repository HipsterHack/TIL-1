## 객체지향에 대한 정리

### 객체로 보는 객체지향
* 객체는 상태를 가지고 있다
* 객체는 행동을 가진다. 행동은 책임으로 말할 수 있다.
* 객체는 행동만 같으면 다른 형태의 데이터를 가져도 무방하다. 다형성의 개념이 이곳에서 나온다.
* 데이터는 상관없이 행동만 고려해야한다. 
* 책임 주도 설계: 외부에 제공하는 책임(=행동)을 먼저 결정하고 이 행동의 책임에 적합한 데이터를 나중에 결정한 뒤 데이터를 외부 인터페이스 내부로 숨긴다.
 
### 책임 

* 오래전 밥 아저씨가 했던 명언 "변해야할 이유."

* 책임은 객체가 해야할 행위(behavior)을 말한다. 
* 책임은 여러 개의 메시지(1..n 관계)가 될 수 있다.(보통 하다보면 한개로 하다 말테지만)
* 아주 추상적인 책임이나 너무 어떻게 하라는 식의 책임을 주면 안된다.
* 이러면, 객체에서 원하는 결과가 확실해야 한다. 이를 바탕으로 인풋과 결과중심의 계약기반 개발이 나오게 된듯하다.
* 일종의 외주 기반 개발(....) -_-;
* 적절히 추상화된 책임은 객체에 자율성을 부여한다. 말한다. 

### 역할

* 한 객체를 다른 객체로 대체 가능하도록 객체의 공통 부분을 묶고 단순화 하여 추상화 시킨 것, 즉 '역할'이다.
* 인터페이스와 비슷할 수 있지만 좀 다르다. 인터페이스는 좀 더 구체화시킨 개념
* 공통의 책임을 수행할 수 있는, 즉 공통의 메시지를 받을 수 있는 것이다.

### 협력(Collaboration)

* 아름다운 콜라보(...)
* 시스템의 기능을 만들기 위해 여러 역할(객체)간의 의사소통 과정을 묶고 단순화 시키고 공통으로 묶어 추상화 한다. 이를 협력이라고 한다.
* RUP에서 협력은 여러 sequence diagram 과 collaboration diagram으로 나타내고 한단어의 표현한다. 이것이 바로 '협력'
* 개별 객체에 초점을 맞춘다기 보단 객체 사이에 주고 받는 메시지의 협력관계를 보는 것. 

### 책임 주도 설계(Responsibility Driven Development )

* 시스템의 기능을 시스템 책임으로 본다. 이 시스템 책임을 나눠 여러 책임으로 도출한다. 
* 그리고 이 책임을 완수하려면 어떤 메시지가 필요할지 고민한다. 
* 그 다음에 메시지를 모아보고, 메시지 수신 인터페이스나 객체를 선정한다.
* 이런 수신체를 모아 인터페이스나 객체 만들고 할당한다.
* 만약 책임 하나를 수행하기 위해 다른 객체에 어떤 값이 필요하다고 시시콜콜 물어본다면, 
그 책임은 시시콜콜 물어보는 객체에 이양되야 한다는 신호다(객체는 다른 객체의 결정에 간섭하면 안된다.)
* 메시지 결정 시점엔 메시지 필요여부만 고민하자. 이후에 객체를 선정해도 된다.
* 간단히 메시지가 객체를 선정한다 보자.
* "묻지말고 시켜라" 라는 것을 명심하자. 불필요한 메시지를 줄여준다.

#### 특징 
*  추상적 인터페이스
* 최소 인터페이스
* 인터페이스와 구현의 분리. 


### 유스케이스

유스케이스는 유저와 시스템간의 상호작용 시나리오의 집합이다.
시스템의 기능에 대해 기술한다. 보통 문서형태로 존재한다.
블랙박스로 시스템과 유저 간의 상호작용의 패턴을 보는 작업이다.

### 도메인 모델
도메인 모델은 이해당사자 누구나 공유하는 비즈니스의 멘탈모델이다.
(라고는 하지만 현실에선 그렇지 않지) 
 만들고자 하는 시스템의 개념과 개념의 관계를 이해하기 위해 표현하는 단계이다. 
이들은 추후에 구현 단계의 클래스가 될 가능성이 높다. 
UML을 사용하면 클래스 다이어그램 형태로 나타낼 수 있겠다. RUP에 경우에도 concept 스테레오 타입을 이용해서 나타냈다.
이전에는 스프레트 시트나 문서등의 형태나 ERD의 논리 레벨로 도메인 모델을 만들어 냈었다.


* 이 도메인 모델(ERD 논리 레벨의 형태가 되겠지)을 기준으로 삼아, 저장할 데이터를 정규화 작업을 거쳐 물리 DB 테이블구조 형태로 만드는 방식이 데이터 모델링 중심의 개발 방법이다. 
* 반대로 도메인 모델과 유스케이스를 가지고  협력을 만들어 나가면서 객체를 식별하고 메시지를 정의하는 방식의 설계,. 
* 유스케이스의 일부나 전체를 가볍게 작성해놓고, 도메인 모델을 대략 만들어서 시스템 설계라 여기고, 시스템 책임을 Step by step으로 테스트를 만들면서 개발을 하는게 TDD. 핵심 테스트 포인트는 객체지향의 협력과정이 되겠지. 
* TDD는 문서로 만들어질 명세를 테스트로 만들고, 설계를 가급적 단순화 시킨 것이다. 
* TDD 처럼 발전한 형태의 개발을 하려면 이전에 기반이 되는 객체지향 개념과 설계에 익숙하고 일정이상의 경험치가 있는 사람이 있어야 시간이 많이 든다거나 힘들지 않다. 그래서 일반인이 첨부터 하기엔 부담이 만배인거지.-_-;

### 마틴파울러의 객체지향 분석 설계의 3가지 관점(in UML Distilled)

#### 개념 관점

* 도메인에 존재하는 개념과 개념 관계를 표현. 클래스 다이어그램으로 관계를 이해하기 위해 만든다.(도메인 모델)

#### 명세 관점

* 소프트웨어에 초점을 맞춤. 객체가 협력을 위해 무엇을 할 수 있는가에 초점을 맞춘다. 
시스템 책임을 작은 책임으로 나눈다. 그리고 이 작은 책임을 하기 위해 협력을 설계한다. 
협력을 설계하기 위해선 최초로 시스템에 인입되는 메시지가 필요하다. 
이 메시지를 어떻게든 찾아내거나 유스케이스에서 힌트를 얻어 가져온다. 
그리고  이 메시지를 어느 객체에게 보낼지 찾아낸다. (처음엔 도메인 모델에 있는 객체 중에 적당한 녀석을 고른다. 없다면? 적당한걸로 만들어내야 한다.) 

이 단계에서 인터페이스가 나타난다. 인터페이스는 랭귀지의 클래스로 public method를 이용해서 나타내거나 인터페이스 자체로 나타낼 수 있다. 

#### 구현관점

실제 동작하는 코드를 작성하는 관점. 
객체들이 책임을 수행하는데 필요한 코드를 작성한다. 
이 단계에선 초기 설계에 예측하지 못했던 객체 등이 나타날 수 도 있다.
개념->명세->구현단계로 소프트웨어 개발을 하도록한다.

### 3가지 관점에 대한 첨언 

* 전체적으로 최근의 기조는 가벼운 설계, 빠른 구현 단계로의 진행을 전제로 한다. 그리고 설계 변경이 생길거 같으면 돌아가도록 코드를 작성하고 리팩토링을 한다. 
* 설계 단계인 명세 관점에서 객체를 식별한다. 이때 도메인 모델을 사용한다. 비즈니스 로직이 바뀌어서, 도메인 모델에 변경이 발생했을때 코드를 좀 더 수월하게 수정하기 위함이다. 각 단계간 간극을 줄이자는 얘기.
* 인터페이스와 구현을 분리해야한다. 명세 관점과 구현관점의 분리명세는 클래스의 안정적 관점을 유지해야하기 때문.

### 추상화 기법이야기 
나중에 추가 예정
* 분류
* 타입
* 집합,분해
* 상속
* 서브타입
** 리스코프 치환 원칙 - 행위적 순응을 해야한다., 서브타입은 슈퍼타입을 행위적으로 대체 해야한다.



### 참고

* 5,6,7장 in [객체지향의 사실과 오해](http://www.yes24.com/24/goods/18249021?scode=032&OzSrank=1)


