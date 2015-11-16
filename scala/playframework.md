# Play Framework

* 자바, 스칼라 기반의 웹 서비스 프레임워크
* Rails, Django, Flask 같은 경량 프레임워크 계통이다.

## 설치 

1) 최신 play 설치부터 typesafe의 activater를 사용하기 시작했다.
2) activator new 하면 play scala 예제를 선택하고 프로젝트 이름을 입력하자. 귀찮으면
```
  activator new hello-app play-scala
```
3) 신규 프로젝트가 생겼을 것이다.
4) actvator run 을 실행하자.
5) 끝.

### 설치 트러블 슈팅

#### 여러 개의 JDK 설치 한 경우
* Windows에서 발생했다.
* 최근에 설치한 JDK(1.8)와 JAVA_HOME Path(1.7)가 다르면 버전 지원 불가능하다는 문제가 발생한다
** Unsupported major.minor version 52.0이런 오류가 발생한다.
* 발생하는 원인은 JRE와 Play 라이브러리가 충돌을 일으키는 듯하다.
* 커맨드로 띄운 JRE(1.8)가 JAVA_HOME 설정을 사용하는 녀석과 버전 차이가 생기니 발생하는 듯.

