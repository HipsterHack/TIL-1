# 자바 초심자를 위한 30가지 조언
* null 대신 빈 컬렉션을 리턴해라 => 이 때문에 
* String은 신중하게 사용해라(String 객체 생성 비용은 비싸다)
* 불필요한 객체는 생성마라(자바의 메모리 사용은 비싸기 때문에 가급적 초기화할때 한번만 생성하자.)
* 배열과 ArrayList는 용도에 따라 사용하자 
** 배열은 개수가 고정인 대신 액세스가 빠르고 ArrayList는 개수는 동적이면서 추가 수정이 쉽다.
** 배열은 다차원 액세스가 1차원
* finally도 실행되지 않는 경우가 있다.
** System.exit 실행하면 finally에 안걸린다. 
** Runtime.addShutdownHook이나 Finalizer로 등록하자.
* Odd(홀수) 체크 로직을 체크해라
** `num % 2 == 1` <- 이거 정확하게 동작할거 같지? 
* '와 "의 차이. 
** + 연산할때 '로 묶인 경우는 문자의 값을 더한다.
** 'H' + 'a' 는 각 문자 코드값의 합이다.
* 메모리릭피하기
** DB 커넥션은 쿼리끝나고 항상 release해라
** finally 블록은 종종 쓸 수 있다.
** static table에 저장된 인스턴스는 릴리즈해라
* 데드락을 피해라
** 일반적으로 데드락은 synchronized object가 다른 synchronized object에 의해 락걸린걸 기다릴때 나타난다.
* 자바를 위해 메모리를 확보해라
** 아니면 느려질 수 있다.
* 시스템 시간 함수는 어떻게 써야 하는가?
** System.currentTimeMillis() : 1/1000초 ~ 15/1000초 사이를 소비한다
** System.nanoTime(): 1/1000,000초를 소비한다.
** System.currentTimeMillis()는 1970년 1월 1일 0시 0분 기분, nanoTime은 아니다.
* Float, Double중 선택?
** 대체로 double을 쓴다. 수행시간은 거의 똑같은데 dobule 소수점 정확도가 높다
* 제곱(power) 연산 방법
** 제곱한 결과 연산을 재활용해라.
** Math.pow는 느리다 ,필요할때만 써라
* NullPointerException 핸들링
** 미리 체크해서 NPE가 발생하지 않게 하자
* JSON 인코딩, 디코딩
** JSON 참 많이 쓴다.
** 적절한 라이브러리를 써라(예: https://code.google.com/p/json-simple/)
* 문자열 검색법
** String.indexOf는 검색 문자열이 있으면 첫번째 인덱스를 return 한다. 없으면 -1
* 디렉토리의 내용을 리스팅하는 법
** 요정돈 탐색할줄 알아야 한다
* 간단한 파일 IO 사용법
** 파일 읽고 쓰고는 할줄 알아야 한다. FileInputStream , FileOutputStream이 있다
* shell command 를 자바에서 실행하는 법
** Runtime rt = Runtime.getRuntime(); rt.exec("실행파일");
** 등등
* Regex 사용법
** 오라클 웹 사이트를 참고해라
** API 문서 http://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html
* 간단한 Swing 예제
** 난 안써(...)
* 자바로 소리 play 어떻게 하는지
* PDF Export
** 생각보다 많이 필요하다
** itextpdf 라이브러리가 쉽게 만들 수 있게 해준다
* 자바로 이메일 어떻게 보내야 하는가?
** 표준 라이브러리의 java mail을 쓰자
** javax.mail 패키지참고
* 메소드 시간 측정
** 간이 성능 테스트할때 자주 쓰인다.
** System.currentTimeMillis, nanoTime(비교적 정확하다)이용
* 이미지 리사이즈
** BMP나 JPG은 자바 표준 라이브러리를 이용하자
** javax.imageio 패키지와 awt의 Graphics2D등을 이용한다
* 마우스 hover 위치 캡쳐
** Swing, AWT 얘기
** MouseMotionListner 구현해서 마우스 이벤트를 캡쳐할 수 있다.
** mouseMoved의 인자로 들어오는 MouseEvent 에 좌표정보가 있다
*  FileOutputStream Vs. FileWriter
** 둘중 뭘쓸까?
** FileOutputStream 는 이미지 같은 raw byte 데이터 를 쓸때
** FileWriter는 문자열 쓸때 사용하자
** API 문서에 잘 나와 있네~
## 추가 추천사항
30.1 Use Collections

30.2 10-50-500 Rule
코드 유지보수는 꽤나 점점 힘들어진다. 코드를 깔끔하게 만드는 간단한 룰이다.

10: 한개 패키지에 10개 이상 클래스를 넣지마라
50: 메소드에 50 라인 이상 쓰지마라
500: 클래스 코드가 500라인 넘어가지 마라

30.3 SOLID Class Design Principles
- 밥아저씨(Robert Martin)가 제안한 디자인원칙
- Single responsibility principle: 클래스는 한개의 일이나 책임을 가진다. 한개 이상이면 혼란이 온다
- Open/closed principle: 개발자는 소프트웨어 독립체(software entities)를 수정보단 확장하는데 포커스를 맞춰라
- Liskov substitution principle: 파생클래스는 베이스 클래스를 대체할 수 있어야 한다.
- Interface segregation principle: SRP와 유사하다. 하지만 인터페이스얘기다. 각 인터페이스는 구체적인 일(task)에 대한 책임이 있다. 개발자는 필요하든 안하든 꼭 인터페이스 메소드를 구현해야한다
- Dependency inversion principle: 구체적인게 아닌 추상에 의존해라.

30.4 디자인 패턴을 사용해라
- 말이필요 없지 않냐 최고의 best practice다. -.-
30.5 아이디어를 문서로 써라

30.6 `==`보단 equals
- 내용비교할땐 equals를 써라.
- `==` 는 object 자체가 같은지 체크할때 쓴다.
30.7 부동 소수점 숫자는 피해라.
- 아무래도 소숫점 정확도 문제가 좀 있다.
