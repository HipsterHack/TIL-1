# 클린코드

## 1장 

### 깨끗한 코드란?

#### 비야네 스트롭스트룹

* 우아하고 효율적인 코드
* 의존성을 최대한 줄여야 유지보수가 쉽다.
* 오류는 명백한 전략에 의거해서 처리한다.

깨끗한 코드란 세세한 사항까지 꼼꼼히 처리하는 코드다

#### 그래디 부치

* 단순하고 직접적이다.
* 잘쓴 문장처럼 읽힌다.
* 설계자의 의도를 숨기지 않는다.
* 명쾌한 추상화와 단순한 제어문을 사용한다. 

코드는 추측이 아니라 사실에 기반해야한다.

#### 데이브 토마스 

* 사람이 읽기 쉽고 고치기 쉽다.
* 단위 테스트와 인수 테스트 케이스가 존재한다.
* 그리고 의미 있는 이름이 붙어져 있다
* 특정 목적을 달성하는 방법은 한가지만 존재한다.,
* 의존성은 최소고, 각 의존성을 명확히 정의한다.
* 언어에 따라 필요한 모든 정보를 코드만으로 표현이 불가능하기에 문학적 표현이 마땅하다

#### 마이클 패더스

깨끗한 코드는 주의 깊게 짰다. 이 말은 손댈 곳이 없다는 말이다.

#### 론 제프리즈

* 중복이 없다
* 모든 테스트를 통과한다
* 시스템내 모든 설계 아이디어를 표현한다
* 클래스 메소드 함수를 최대한 줄인다.


#### 와드 커닝햄

* 코드를 읽으면서 짐작했던 기능을 각 루틴 그대로 수행한다면 깨끗한 코드.
* 코드가 문제를 풀기위한 언어처럼 보인다면 아름다운 코드다.

### 깨끗한 코드를 만들기 위한 방법

* 보이스카웃 규칙 
 + 캠프장은 처음왔을때보다 깨끗히 해놓고 떠나라
* 프리퀄과 원칙 
 + SRP
 + OCP
 + DIP 
 
깨끗한 코드를 만들려면 많은 연습이 필요하다

## 2장 의미 있는 이름

### 의도를 분명히 밝혀라

코드의 함축성을 높이면 코드 자체가 명시적으로 들어나지 않는다.

### 그릇된 정보를 피해라

* 일관성이 떨어지는 표기법은 그릇된 정보다
* 흡사한 이름을 사용하지 말아야 한다.
* 여러 계정을 하나로 묶을때 실제 List가 아니면 List라는 이름을 쓰지 않는다

### 의미 있게 구분하라 

* 연속적 숫자를 덧붙인 이름은 피하라(a1, a2...)
* 불용어(ProductInfo, ProductData)는 사용을 피해라.
* 중복을 피해라

### 발을하기 쉬운 이름을 써라

### 검색하기 쉬운 이름을 써라

* 문자 1개를 사용하는 이름이나 상수는 찾기 어렵다.

### 인코딩을 피해라

* 헝가리안식 표기법을 피해라
* 멤버변수 접두어 

### 자신의 기억력을 자랑하지 말아라

* 명료함이 최고임을 이해하라

### 클래스 이름 

* 명사나 명사구
* 동사는 쓰지 않는다.
* Manager, Processor, Data, Info는 피한다 

### 메소드 이름 

* 동사나 동사구를 써라(postPayment)
* 생성자를 중복정의할때는 정적 팩토리 메소드를 쓰자 
```
Complex fulcrumPoint = Complex.FromRealNumber(23.0);
```

### 기발한 이름을 피하자

### 1 개념당 1 단어를 쓰자

* 똑같은 메서드를 클래스마다 fetch, get, retrieve로 각각 쓰진 말자
* Manager, Conmtroller, Driver를 섞어 쓰면 곤란하다

### 말 장난을 하지 말자 

* 같은 맥락에서 일관성을 고려해서 이름을 붙이자

### 해법 영역에서 가져온 이름을 쓰자 
* 전산용어, 알고리즘 이름, 패턴이름 수학용오 등을 사용하자 
* 기술 개념엔 기술 이름을 쓴다.

### 도메인에서 가져온 이름을 쓴다
* 프로그래머 용어가 없다면 만들 분야의 전문가에게 의미를 물어 파악할 수 있다.
* 코드가 도메인과 관련이 깊은 경우엔 도메인 영역에서 가져와서 이름을 쓰자 

### 의미있는 맥락을 추가하라 

* 클래스, 메소드 ,이름공간에 이름을 추가해서 맥락을 부여한다. 
* 맨 마지막에 접두어를 쓴다. 

### 불필요한 맥락을 제거하라

* 의미가 분명한 경우에 한해서 짧은 이름이 좋다,
* 안되면 조금 긴 이름을 쓰자.

 # 3장 함수

 * 클린 코드로 가기 위한 함수를 만드는 방법이다.


## 작게 만들어라 

* 3줄 4줄 정도로 잛게 만들어라 

## 한가지만 해라

* 함수는 한가지만 잘해야 한다. 그 한가지만 해야한다. 
* 보통은 이게 안된다.

## 함수는 추상화 수준은 하나로

* 함수가 한가지 작업만 하려면 함수 내 모든 문장의 추상화 수준이 동일해야 한다.
* 코드 사이에 추상화가 섞이면 안된다.


### 위에서 아래로 코드 읽기: 내려가기 규칙

* 코드는 위에서 아래로 추상화가 내려가야 한다.

### Switch 문 

* 스위치문은 여러 일을 해야하므로 추상화 팩토리를 이용해서 추상화 시키자.
* 다형성을 이용해서 추상화 하는 방식을 많이 쓴다.


### 함수 인수 

* 함수 인수는 0개를 쓰는게 좋고, 안되면 1개. 3개 이상인 경우 인수 객체를 만들어서 1개로 만들어 추상화 시키자.

#### 동사와 키워드 

* 함수의 의도나 인수의 순서와 의도를 제대로 표현하려면 좋은 함수 이름이 필수다.
* 동사 명사 쌍을 제대로 이뤄야 한다.

### 부수 효과를 피해라
* 시간적 결합이나 순서 종속성을 초래할만한 코드는 피해라.
* 이름을 보고 그 함수를 나타낼수 있어야 한다.

### 인자를 출력으로 사용하지 말아라

### 명령과 조회를 분리해라

* 뭔가 답하거나 뭔가 수행하거나 둘중 하나만 하자. 
* 객체 상태를 변경하거나 객체 정보를 반환 하거나 둘중 하나다 둘다 하면 이상한 코드가 된다.
```
public boolean set(String name, String value);
```

### 오류 코드 보다 예외를 사용해라

### 반복하지마라
* AOP, COP 모드 중복 제거 전략이다. 

### 함수를 만드는 방법은?

- 글짓기랑 비슷하다
- 먼저 생각을 기록 후 읽기 좋게 다듬는다.
- 원하는대로 읽힐때까지 말을 다듬과 문장을 고치고 문단을 정리한다.

* 위와 마찬가지로 수행한다.
 

 - 처음엔 생각하는 바를 구현한다.
 - 코드를 다듬고 함수를 만들고 이름을 바꾸고 중복ㅇ늘 제거한다.
 - 클래스를 쪼갠다
 - 이 와중에 단위 테스트를 통과 시킨다


# 5장 형식 맞추기 

## 목적

* 보기가 편해진다.

### 적절한 행길이를 유지하라
* 작은 파일일 수록 좋다. 
* 적정 소스코드 라인수는 50~100 정도

### 신문기사처럼 작성하라
* 첫문단이 전체 기사를 요약하는 것처럼 마찬가지로 소스코드를 작성해야 한다
* 이름만 보고도 올바른 모듈을 보는지 알 수 있어야 한다.
* 제일 처음에 나오는 메소드는 가장 고차원적인 것이어야 한다.

### 개념은 빈행으로 분리해라

### 세로 밀집도

* 연관성을 의미한다. 서로 밀접한 코드는 가까이 놓는다.

### 수직거리

* 변수선언: 변수는 사용하는 위치에 가까이 선언한다.
* 인스턴스 변수: 클래스 맨 처음에 선언한다. 논쟁이 분분한 편
* 종속 함수: 한 함수가 다른 함수를 호출한다면 세로로 가까이 배치한다. 가장 먼저 호출하는 함수를 가깝게 그 다음 것은 그 다음으로 가깝게 위치시킨다.

* 호출되는 함수는 아래쪽으로 점점 아래로 위치 시킨다.

### 가로형식 맞추기 

* 80자 이하로 맞춘다.(100, 120자도 나쁘진 않다.)

### 가로공백과 밀집도

* 공백은 두가지 요소가 확실히 나뉜다는 걸 의미한다.

### 들여쓰기

* 보통 들여쓰기는 구조를 한눈에 볼 수 있게 해준다.


## 정리 

* 좋은 소프트웨어 코드는 읽기 좋은 문서로 이뤄진다.


# 6장 객체와 자료구조

## 자료 추상화

* 함수계층을 넣는다고 구현이 감춰지는것은 아니다. 특히 get set 메소드들..
 + 그래서 getter, setter를 넣는다고 객체가 되는건 아니다. 단지 감싼것뿐. 그래서 가장 나쁜 방법이다.

## 자료 객체 비대칭

### 자료구조

* 자료를 그대로 보여준다

### 객체

* 자료를 숨긴체 자료를 다루는 함수를 공개한다.

### 절차지향 객체지향 

절차코드는 기존  자료구조를 변경하지 않으면서 새 함수를 추가하기 쉽다.
객체지향 코드는 기존 함수를 변경하지 않으면서 새 클래스를 추가하기 쉽다.

절차코드는 새로운 자료구를 추가하기 어렵다. 그러려면 모든 함수를 고쳐야 한다
객체지향 코드는 새로운 함수를 추가하기 어렵다. 그러려면 모든 클래스를 고쳐야 한다.

## 디미터 법칙

* 클래스 C의 메서드 f는 다음과 같은 객체의 메소드만 호출해야 한다.

1. 클래스 C
2. f가 생성한객체
3. f가 인수로 넘어온 객체
4. C 인스턴스 변수에 저장한 객체

- 낯선 사람은 경계하고 친구하고만 놀라는 의미

## 잡종 구조

절반은 객체 절반은 자료구조인 형태는 객체지향과 절자코드의 단점을 모두 가진다.

## DTO

* 자료구조체의 다른 이름 
* 빈구조도 이것의 변형이다.

## 액티브 레코드

* DTO의 특수한 형태
* 공개변수 or 비공개 변수에 조회 설정함수 있지만 save, find도 제공한다. 
* 여기에 비즈니스 로직을 추가하면 잡종구조가 나온다.
* 자료구조로 취급해라.
* 
# 8장 경계

## 외부 코드 사용하기

특유의 긴장이 존재한다.
패키지 제공자나 프레임워크 제공자는 적용성을 높이려 한다.
하지만 사용자는 자신의 요구사항에 집중한다. 이래서 시스템 경계에 문제가 발생한다.

### 경계를 살피고 익히기
- 외부코드는 문서를 보고 답을 얻을 수도 있지만 학습 테스트를 이용하는게 더 유용하다.

### 아직 존재하지 않는 코드 사용하기 

- 우리가 원하는 형태로 인터페이스와 패키지를 분리해서 Fake 어댑터를 만들어서 분리하여 테스트 한다

# 9장 단위 테스트 

## TDD 3법칙 

1. 실패하는 단위 테스트를 작성할 때 까지 실제 코드를 작성하지 않는다.
2. 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다
3. 현재 실패하는 테스트를 통과할 정도만 실제코드를 만든다.


- 위 3가지 법칙을 따르면 대략 30초 주기로 묶인다. 테스트 코드와 실제 코드가 함께 나올뿐더러 테스트 코드가 실제 코드보다 불과 몇 초전에 나온다.
- 이렇게 일하면 매일 수십개 매달 수백개.의 테스트 케이스가 나온다.
- 이런 테스트 코드는 관리문제를 유발 시키기도 한다.

## 깨끗한 테스트 코드 유지하기
* 테스트 코드는 실제 코드 만큼 중요하다.
* 테스트는 유연성 유지보수성, 재사용성을 제공한다.
* 테스트케이스로 인해 변경이 쉬워진다.

* 깨끗한 테스트 코드를 만들려면 무조건 무조건 가독성에 신경을 써야 한다. 
  + 도메인 특화 언어를 사용해서 테스트 코드를 구현하면 도움이 된다. 
      + 테스트 코드가 어떤 의미인지 바로 알아챌수 있게 만드는게 중요하다.
  + 이중 표준을 만들어라.
  		+ 테스트 환경은 실제 환경과 다르다는걸 인지하고 만든다.  		
  + 한 개념당 가능하면 적은 assert 를 유지해라
    + 가능하면 줄여라. (아래는 예제다)
    + 중복이 생긴다면? template method 등으로 중복 코드를 뽑아내라. 테스트 코드를 일반 코드 수준으로 중복을 줄이라는 것이다.
```
public void testGetPageHierarchyAsXml() throws Exception {
   givenPages("PageOne", "PageOne.ChildOne", "PageTwo");
   whenRequestIsIssued("root", "type:pages");
   thenResponseShouldBeXML();
}

public void testGetPageHierarchyHasRightTags() throws Exception {
  givenPages("PageOne", "PageOne.ChildOne", "PageTwo");
  whenRequestIsIssued("root", "type:pages");
  thenResponseShouldContain(
    "<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>"
    );
```  

## Build Operate check 패턴
  - 테스트 자료를 만들고
  - 데이터를 조작하고 
  - 올바른 결과가 맞는지 확인한다.
 

## FISRT
* 깨끗한 코드를 만드는 5가지 원칙

### 빠르게 (Fast)
* 테스트는 빨라야 한다. 그래야 자주 실행 가능하니까.

### 독립적으로 (Independent)
* 각 테스트가 서로 의존하면 안된다. 
  + 의존성이 생기면 원인을 찾는게 어려워진다.

### 반복가능하게 (Repeatable)
* 어떤 환경에서도 반복가능해야한다.
  + 이런 상태를 유지하기 어렵다.

### 자가 검증하는 (Self-Validating)
* 테스트는 불 값으로 결과를 내야한다. 성공 아니면 실패다. 

### 적시에 (Timely)
* 테스트는 적시에 작성해야한다.
* 테스트하는 코드를 구현 직전에 만들어야 한다. 그게 아니라면 테스트 코드 만드는게 참 어려운일이 될 것이다. 

# 10장 클래스 

자바의 경우 아래 순서로 코드를 쓴다.
- 공개 정적 상수 목록
- 정적 비공개 변수
- 비공개 인스턴스 변수
- 공개 함수

## 캡슐화

* 변수와 유틸리티 함수는 가급적 비공개로 만들어야 한다.
* 같은 패키지 안에서 사용해야 한다면 protected 선언하여 테스트 코드가 접근할 수 있게 한다.
* 캡슐화를 풀어주는 것은 마지막에 해야한다.

## 클래스의 최소화

### 단일 책임 원칙

* 클래스나 모듈을 변경할 이유가 단하나 여야 한다. 책임이 1개라는 이야기.
* 책임을 파악하라다보면 코드 추상화가 쉬워진다.
* 클래스 이름은 클래스 책임을 기술해야 한다. 
  + processor, Manager, Super가 들어가면 클래스에 여러 책임이 있다는 증거다.
* 큰 클래스 몇개가 아니라 작은 클래스 여러개로 만들어진 시스템이 바람직하다. 
  + 작은 클래스는 책임이 한개고 다른 작은 클래스와 협력해서 시스템을 동작 시킨다.

### 응집도

* 클래스는 인스턴스 변수 수가 작아야 한다. 
* 각 클래스 메소드는 클래스 인스턴스 변수를 한개 이상 사용해야 한다.
* 일반적으로 메소드가 변수를 많이 사용할수록 메소드와 클래스는 응집도가 높다.
* 하지만 응집도가 가장 높은 클래스는 가능하지도 바람직하지도 않다. 
* 함수를 작게 매개변수 목록을 짧게 라는 전략을 쓰다보면 몇몇 메소드에서만 사용하는 인스턴스 변수가 많아진다. 
  + 이런 경우 대부분은 새로운 클래스로 쪼개야 한다는 거다.

#### 응집도를 유지하면 여러 작은 클래스가 나온다.

* 큰 함수를 작은 함수 여럿으로 나누기만 하면 클래스 수가 많아진다.
  + extract method를 한다고 생각하자.뽑을 위치의 코드는 큰 함수의 변수를 몇개 사용하는데 이를 매개변수로 큰 함수에서 넘겨야 할까?
  + 아니다. 인스턴스 변수로 승격시키자. 그러면 함수를 쪼개기 쉬워진다. 
  + 이러면 응집도가 떨어진다. 그러면? 독자 클래스로 분리 시키자.
  + 정리하면 extract method를 진행하다가 응집도가 떨어지기 시작한다면 클래스를 분리해내자.


```
package literatePrimes;

public class PrintPrimes {
	public static void main(String[] args) {
		final int M = 1000;
		final int RR = 50;
		final int CC = 4;
		final int WW = 10;
		final int ORDMAX = 30;
		int P[] = new int[M + 1];
		int PAGENUMBER;
		int PAGEOFFSET;
		int ROWOFFSET;
		int C;
		int J;
		int K;
		boolean JPRIME;
		int ORD;
		int SQUARE;
		int N;
		int MULT[] = new int[ORDMAX + 1];
		J = 1;
		K = 1;
		P[1] = 2;
		ORD = 2;
		SQUARE = 9;
		while (K < M) {
			do {
				J = J + 2;
				if (J == SQUARE) {
					ORD = ORD + 1;
					SQUARE = P[ORD] * P[ORD];
					MULT[ORD - 1] = J;
				}
				N = 2;
				JPRIME = true;
				while (N < ORD && JPRIME) {
					while (MULT[N] < J)
						MULT[N] = MULT[N] + P[N] + P[N];
					if (MULT[N] == J)
						JPRIME = false;
					N = N + 1;
				}
			} while (!JPRIME);
			K = K + 1;
			P[K] = J;
		}
		{
			PAGENUMBER = 1;
			PAGEOFFSET = 1;
			while (PAGEOFFSET <= M) {
				System.out.println("The First " + M + " Prime Numbers --- Page " + PAGENUMBER);
				System.out.println("");
				for (ROWOFFSET = PAGEOFFSET; ROWOFFSET < PAGEOFFSET + RR; ROWOFFSET++) {
					for (C = 0; C < CC; C++)
						if (ROWOFFSET + C * RR <= M)
							System.out.format("%10d", P[ROWOFFSET + C * RR]);
					System.out.println("");
				}
				System.out.println("\f");
				PAGENUMBER = PAGENUMBER + 1;
				PAGEOFFSET = PAGEOFFSET + RR * CC;
			}
		}
	}
}
```

위의 내용을 리팩토링하자. 서술적인 이름을 사용하고 클래스를 분리하자 

```
public class PrimePrinter {
	public static void main(String[] args) {
		final int NUMBER_OF_PRIMES = 1000;
		int[] primes = PrimeGenerator.generate(NUMBER_OF_PRIMES);
		final int ROWS_PER_PAGE = 50;
		final int COLUMNS_PER_PAGE = 4;
		RowColumnPagePrinter tablePrinter = new RowColumnPagePrinter(ROWS_PER_PAGE, COLUMNS_PER_PAGE,
				"The First " + NUMBER_OF_PRIMES + " Prime Numbers");
		tablePrinter.print(primes);
	}
}

import java.io.PrintStream;

public class RowColumnPagePrinter {
	private int rowsPerPage;
	private int columnsPerPage;
	private int numbersPerPage;
	private String pageHeader;
	private PrintStream printStream;

	public RowColumnPagePrinter(int rowsPerPage, int columnsPerPage, String pageHeader) {
		this.rowsPerPage = rowsPerPage;
		this.columnsPerPage = columnsPerPage;
		this.pageHeader = pageHeader;
		numbersPerPage = rowsPerPage * columnsPerPage;
		printStream = System.out;
	}

	public void print(int data[]) {
		int pageNumber = 1;
		for (int firstIndexOnPage = 0; firstIndexOnPage < data.length; firstIndexOnPage += numbersPerPage) {
			int lastIndexOnPage = Math.min(firstIndexOnPage + numbersPerPage - 1, data.length - 1);
			printPageHeader(pageHeader, pageNumber);
			printPage(firstIndexOnPage, lastIndexOnPage, data);
			printStream.println("\f");
			pageNumber++;
		}
	}

	private void printPage(int firstIndexOnPage, int lastIndexOnPage, int[] data) {
		int firstIndexOfLastRowOnPage = firstIndexOnPage + rowsPerPage - 1;
		for (int firstIndexInRow = firstIndexOnPage; firstIndexInRow <= firstIndexOfLastRowOnPage; firstIndexInRow++) {
			printRow(firstIndexInRow, lastIndexOnPage, data);
			printStream.println("");
		}
	}

	private void printRow(int firstIndexInRow, int lastIndexOnPage, int[] data) {
		for (int column = 0; column < columnsPerPage; column++) {
			int index = firstIndexInRow + column * rowsPerPage;
			if (index <= lastIndexOnPage)
				printStream.format("%10d", data[index]);
		}
	}

	private void printPageHeader(String pageHeader, int pageNumber) {
		printStream.println(pageHeader + " --- Page " + pageNumber);
		printStream.println("");
	}

	public void setOutput(PrintStream printStream) {
		this.printStream = printStream;
	}
}

import java.util.ArrayList;

public class PrimeGenerator {
	private static int[] primes;
	private static ArrayList<Integer> multiplesOfPrimeFactors;

	protected static int[] generate(int n) {
		primes = new int[n];
		multiplesOfPrimeFactors = new ArrayList<Integer>();
		set2AsFirstPrime();
		checkOddNumbersForSubsequentPrimes();
		return primes;
	}

	private static void set2AsFirstPrime() {
		primes[0] = 2;
		multiplesOfPrimeFactors.add(2);
	}

	private static void checkOddNumbersForSubsequentPrimes() {
		int primeIndex = 1;
		for (int candidate = 3; primeIndex < primes.length; candidate += 2) {
			if (isPrime(candidate))
				primes[primeIndex++] = candidate;
		}
	}

	private static boolean isPrime(int candidate) {
		if (isLeastRelevantMultipleOfNextLargerPrimeFactor(candidate)) {
			multiplesOfPrimeFactors.add(candidate);
			return false;
		}
		return isNotMultipleOfAnyPreviousPrimeFactor(candidate);
	}

	private static boolean isLeastRelevantMultipleOfNextLargerPrimeFactor(int candidate) {
		int nextLargerPrimeFactor = primes[multiplesOfPrimeFactors.size()];
		int leastRelevantMultiple = nextLargerPrimeFactor * nextLargerPrimeFactor;
		return candidate == leastRelevantMultiple;
	}

	private static boolean isNotMultipleOfAnyPreviousPrimeFactor(int candidate) {
		for (int n = 1; n < multiplesOfPrimeFactors.size(); n++) {
			if (isMultipleOfNthPrimeFactor(candidate, n))
				return false;
		}
		return true;
	}

	private static boolean isMultipleOfNthPrimeFactor(int candidate, int n) {
		return candidate == smallestOddNthMultipleNotLessThanCandidate(candidate, n);
	}

	private static int smallestOddNthMultipleNotLessThanCandidate(int candidate, int n) {
		int multiple = multiplesOfPrimeFactors.get(n);
		while (multiple < candidate)
			multiple += 2 * primes[n];
		multiplesOfPrimeFactors.set(n, multiple);
		return multiple;
	}
}

```

### 변경하기 쉬운 클래스

* 클래스 일부에서 사용하는 비공개 메소드는 코드를 개선할 여지를 얘기한다.
 + 하지만 실제로 개선해야할 시점은 시스템이 변해서라야 한다.
* OCP 원칙을 지원해야한다. 확장에는 개방적이고 수정에 폐쇄적이어야 한다.
* 새 기능을 추가하거나 기존 기능을 변경할때 건드릴 코드가 최소인 구조가 바람직하다.

아래는 OCP 원칙을 지키면서 클래스를 리팩토링 한것을 보여준다.

```
public class Sql {
	public Sql(String table, Column[] columns)	
	public String create()	
	public String insert(Object[] fields)	
	public String selectAll()	
	public String findByKey(String keyColumn, String keyValue)	
	public String select(Column column, String pattern)	
	public String select(Criteria criteria)	
	public String preparedInsert()	
	private String columnList(Column[] columns)	
	private String valuesList(Object[] fields, final Column[] columns)	
	private String selectWithCriteria(String criteria)	
	private String placeholderList(Column[] columns)
}
 ```

위의 개선버전은 아래와 같다.
새로 쿼리가 추가 되도 클래스만 추가하면 된다. 

```
abstract public class Sql {
	public Sql(String table, Column[] columns)
	abstract public String generate();
}

public class CreateSql extends Sql {
	public CreateSql(String table, Column[] columns)
	@Override 
	public String generate()
}
public class SelectSql extends Sql {
	public SelectSql(String table, Column[] columns)
	@Override 
	public String generate()
}

public class InsertSql extends Sql {
	public InsertSql(String table, Column[] columns, Object[] fields)
	@Override 
	public String generate()
	private String valuesList(Object[] fields, final Column[] columns)
}

public class SelectWithCriteriaSql extends Sql {
	public SelectWithCriteriaSql(
			String table, Column[] columns, Criteria criteria)
	@Override 
	public String generate()
}

public class SelectWithMatchSql extends Sql {
	public SelectWithMatchSql(
			String table, Column[] columns, Column column, String pattern)
	@Override 
	public String generate()
}
public class FindByKeySql extends Sql
	public FindByKeySql(
			String table, Column[] columns, String keyColumn, String keyValue)
	@Override 
	public String generate()
}

public class PreparedInsertSql extends Sql {
	public PreparedInsertSql(String table, Column[] columns)
	@Override 
	public String generate() 
	private String placeholderList(Column[] columns)
}

public class Where {
	public Where(String criteria)
	public String generate()
}

public class ColumnList {
	public ColumnList(Column[] columns)
	public String generate()
}
```

### 변경으로부터 격리

테스트가 가능할정도로 시스템 결합도를 낮추면 유연성과 재사용성이 높아진다.
결합도가 낮다는 것은 시스템 요소가 다른 요소와 변경으로부터 격리가 잘되어 있단 얘기.

* 클라이언트에게 구체적인 사실을 숨기고 테스트 하기 용이하려면 concrete 클래스에 의존하는걸 가능하면 피해야 한다. 
 + 인터페이스나 추상클래스를 사용하는건 이때 필요한 것이다.
 + 추상적인 개념만 가지고 온다고 생각하게 만든다.


# 11장 시스템 

* 시스템을 제작과 시스템 사용을 분리하라

## 관심사의 분리 

+ 대부분의 어플리케이션이 관심사의 분리를 하지 않는다.
+ Main 분리
  - 생성관련 코드를 main이나 main을 호출한 모듈로 옮긴다
  - 나머지는 모든 객체가 생성되었고 모든 의존성이 연결되었다고 가정한다.
+ 팩토리 
  - 객체 생성 시점을 어플리케이션이 결정하게 한다.
  - 추상 팩토리 메소드가 이 예제.
+ 의존성 주입 
  - 사용과 제작을 분리하는 형태. Spring이 가장 대표 적인 예
  - 제어 역전을 의존성 관리에 적용한 매커니즘이다.
  - 제어역전은 한 객체가 맡은 보조 책임을 새로운 객체에게 넘긴다. 새로운 객체는 넘겨받은 책임만 맡으므로 SRP를 지키게 된다.
  - 클래스가 의존성을 해결하지 않는다. setter나 생성자 인수를 DI 컨테이너가 작업한다.

## 확장

 + 관심사를 분리해서 적절히 관리하면 소프트웨어 아키텍처는 점진적으로 발전할 수 있다.

### 횡단 관심사 (Cross-cutting)
* 영속성 같은 관심사는 어플리케이션의 객체 경계를 넘나드는 경향이 있다. 
 - 모든 객체자가 전반적으로 동일한 방식으로 이용하게 만들어야 한다.
* AOP는 특정 관심사를 지원하려면 시스템에서 특정 지점들이 동작하는 방식을 일관성 있게 바꿔야 한다고 명시한다.

## 테스트주도 시스템 아키텍쳐 구성 
* Aspect로 관심사를 분리하는 건 코드 수준에서 아키텍쳐 관심사를 분리할 수 있다. 그러면 테스트 주도 아키텍쳐 구축이 가능하다. 
 - 단순하면서 멋지게 분리된 아키텍쳐로 소프트웨어 프로젝트를 진행해 출시하고 기반구조를 조금씩 확장해가도 괜찮다는 것.
 - 일반적인 범위, 목표, 일정, 결과로 내놓을 시스템의 일반적인 구조도 생각한다. 
 - 변하는 환경에 대처해서 진로를 변경할 능력도 유지해야 한다.
* Big Design Up Front : 구현을 시작하기 전에 앞으로 벌어질 모든 사항을 설계하는 기법. 주로 건축가들이 애용한다.

## 의사결정을 최적화하라 

* 가능한 마지막 순간까지 결정을 미루는 방법이 최선이다.
  + 최대한 정보를 모아서 최선의 결정을 내리기 위함이다. 
  + 너무 일찍 결정하면 고객 피드백, 고민, 구현 방안을 탐험할 기회를 놓친다. 

## 명백한 가치가 있을때 표준을 현명하게 사용하라

+ 가볍고 간단한 설계에는 간단한 표준을 써라 

## 시스템은 DSL이 필요하다.

* 도메인 개념과 그 개념을 구현한 코듯 사이에 의사소통의 간극을 줄여준다.
* DSL을 사용하면 고차원 정책에서 저차원 세부사항까지 모든 추상화와 모든 도메인을 POJO로 작성할 수 있다. 

## 시스템은 실제로 돌아가는 가장 단순한 수단을 사용해야 한다. 

# 12장 창발성 

착살하게 따르기만 하면 우수한 설계가 나오는 규칙이 4가지가 있다.


1. 모든 테스트를 실행한다.
2. 중복을 없앤다.
3. 프로그래머의 의도를 표현한다.
4. 클래스와 메소드의 수를 최소로 줄인다.

위 4가지 단순한 설계 규칙을 설명해본다.

## 모든 테스트를 실행하라.

* 테스트 가능하게 시스템을 만들면 설계품질이 높아진다.
  +  SRP를 준수하는 클래스는 테스트 하기가 훨씬 쉽다.
  + 결합도가 높으면 테스트가 어렵다.

## 피리팩토링 

* 테스트 케이스 작성후 코드와 클래스를 정리한다.
* 코드를 점진적으로 리팩토링한다.
  + 코드 몇줄 추가할때마다 잠시 멈추고 설계를 조감한다.
  + 이 단계에선 설계 품질을 높이는 어떤 기법을 적용해도 좋다. 왜? TC가 있다.
    - 응집도를 높인다
    - 결합도를 떯어뜨린다.
    - 관심사를 분리한다.
    - 시스템 관심사를 모듈별로 나눈다.
    - 함수와 클래스 크기를 줄인다. 
    - 더 나은 이름을 선택한다.

### 중복을 없애라.

* 중복은 추가작업, 추가 위험, 불필요한 복잡도를 뜻한다. 
  + 예를들면 동일한 코드가 나타나던가 비슷한 코드도 중복이다.
  + 템플릿 메소드는 고차원 중복을 제거하는 목적으로 사용한다.

### 표현하라 

* 개발자 의도를 표현해라. 
  + 좋은 이름을 고르고
  + 함수와 클래스 크기를 줄인다.
  + 표준 명칭을 사용한다.
    - DP는 Command, Visitor 같은 표준 패턴을 사용한다.
  + TC를 꼼꼼히 작성한다.
  + 노오력을 투자 한다. 
    - 나중에 읽을 사람을 생각한다. 읽기 좋게 만들자.

### 클래스와 메소드 수를 최소로 줄여라.

* SRP를 준수, 중복 제거, 의도 표현이 극으로 치달으면 득보다 실이 많아진다.
* 예를 들면 클래스마다 무조건 인터페이스를 생성하는 구현 표준은 안티 패턴이다.
* 하지만 이 우선순위는 제일 낮다. 

