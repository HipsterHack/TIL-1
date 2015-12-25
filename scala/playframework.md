# Play Framework

* 자바, 스칼라 기반의 웹 서비스 프레임워크
* Rails, Django, Flask 같은 경량 프레임워크 계통이다.
* 2.4 기준이다.
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
* Unsupported major.minor version 52.0이런 오류가 발생한다.
* 발생하는 원인은 JRE와 Play 라이브러리가 충돌을 일으키는 듯하다.
* 커맨드로 띄운 JRE(1.8)가 JAVA_HOME 설정을 사용하는 녀석과 버전 차이가 생기니 발생하는 듯.
* 참고: (http://stackoverflow.com/questions/22489398/unsupported-major-minor-version-52-0)


## 템플릿을 이용해 App을 생성
* typesafe activator를 실행해서 새 play app을 만든다.

```
activator new my-first-app play-scala
```

## 서버 실행 방법

* 매우 간단하다. activator에 파라미터로 run 넣고 실행한다.

```
activator run 
```

## 생성 폴더 

* app : controllers, models, views 등이 들어가는 폴더
* conf: 설정 파일. DB 설정이나, i18N, 로그, routes 설정이 들어가는 폴더 
* conf/messages : 기본 메시지 파일이다. 
* target : generate된 파일이 들어간다


## Controller

* 먼저 생성되는 controller는 Application 이다. 

```
class Application extends Controller {

  def index = Action {
    Ok(views.html.index("Hello World"))
  }
 }
```
* 위 예제를 살펴보자. 
* 위 예제에서 views.html.index는 app/views/index.scala.html 을 매핑한 것이다. 
* 모든 컨트롤러는 Controller를 상속받는다. 또한 URL에 매핑할 메소드를 가지고 있다. 
* 메소드는 Action을 리턴값으로 생성한다. 
* `play.api.mvc.Action` 은 기본적으로 `play.api.mvc.Request => play.api.mvc.Result` 함수지만 생략하는 경우도 있다.
```
val echo = Action { request =>
  Ok("Got request [" + request + "]")
}
```
* Ok 는 HTTP 응답값으로 200을 응답하는 `play.api.mvc.Result`이다
### Http response 
* 위 얘기의 연장선으로 설명해보겠다. Http 응답값은 아래 처럼 클래스로 매핑되어 있다. 
* 정의가 없는건 Status로 임의로 내려줄 수 있다. 
```
val ok = Ok("Hello world!")
val notFound = NotFound
val pageNotFound = NotFound(<h1>Page not found</h1>)
val badRequest = BadRequest(views.html.form(formWithErrors))
val oops = InternalServerError("Oops")
val anyStatus = Status(488)("Strange response type")
```
### Redirect
* 브라우저에 새 URL로 redirect 하는 방법은 아래와 같다.
```
def index = Action {
  Redirect("/user/home")
}
```
* 기본값은 `303 SEE_OTHER`이지만 304 같은  코드도 정의되어 있다.
```
def index = Action {
  Redirect("/user/home", MOVED_PERMANENTLY)
}
```

### TODO 
더미 페이지로 나중에 만들 건 아래처럼 지정할 수도 있다.

```
def index(name:String) = TODO
```


## HTTP Router
* http request를 action으로 매킹시켜주는 역할을 한다.

* 메소드 이름, request path, Action 메소드 세 부분으로 구성된다.
* conf/routes 파일에 저장되어 있다. 
```
GET     /                           controllers.Application.index
GET     /test                       controllers.Application.test 
POST    /person 					controllers.Application.addPerson

GET     /persons                 	controllers.Application.getPersons
# Map static resources from the /public folder to the /assets URL path
GET     /assets/*file               controllers.Assets.versioned(path="/public", file: Asset)

```


### Depndency Injection

* 플레이는 두 타입의 http router를 지원한다.
* 하나는 위에서 설명한 static router 
* 또하나는 컨트롤러 클래스 위에 `@` 심볼을 이용해 route 위치를 선언하는 방법이다.
* build.sbt에 아래 코드를 추가하면 injection을 이용할 수있다.
```
routesGenerator := InjectedRoutesGenerator
```


## Route file 문법
* Http method와 uri 패턴을 쓰고 그 뒤에 action 메소드를 지정한다.
* `:` 를 prefix로 쓰는건 action의 파라미터로 들어간다. 
```
GET   /clients/:id          controllers.Clients.show(id: Long)
```
* `#`는 커맨트이다. 
```
# Display a client.
GET   /clients/:id          controllers.Clients.show(id: Long)
```

### http method
* 아래 http method를 지원한다. 
```
GET, POST, PUT, DELETE, HEAD
```
