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

### URI pattern
* URI 패턴은 request를 지정한 규칙에 맞게 라우트한다.
#### static path
* 아래는 정적 경로 예제다.
```
GET   /clients/all          controllers.Clients.list()
```

#### 동적 패턴
* regexp `[^/]+`처럼 만들수 있다.
* 또한 `:{글자}`로 쓸수도 있다. 이건 바로 URI에 매칭된다.
```
GET   /clients/:id          controllers.Clients.show(id: Long)
```
* 동적 패턴을 사용하려면 `*name`을 형태를 사용한다. 아례 예제를 보자.

```
GET   /files/*name          controllers.Application.download(name)
```
* files 뒤에 어떤 내용이 오든 name에 들어가게 된다. 예를 들어 `/files/images/test.png`이면 `name`에는 `images/test.png` 값이 들어간다.

##### 커스텀 regexp
* `$id<regex>`를 사용한다. id는 파라미터로 넘어갈 값의 이름 
```
GET   /items/$id<[0-9]+>    controllers.Items.show(id: Long)
```

##### Action 생성기 메소드 호출

* 만약 action에서 파라미터를 안받을 거면 
```
GET   /                     controllers.Application.homePage()
```
* 파리미터를 받을거면 play는 요청 URI에서 값을 찾거나 query string에서 찾는다.
```
# /1 -> 1page
GET   /:page                controllers.Application.show(page)
```
이거나

```
# /?page=1 -> 1page
GET   /                     controllers.Application.show(page)
```

###### 파라미터 타이핑 

파라미터 타이핑은 옵션이다. `String`이 기본이다. 타입을 결정하고 싶거든 명시적으로 타입을 지정해라

```
GET   /clients/:id          controllers.Clients.show(id: Long)
```
* 위에서 설정한 메소드의 선언부이다. 
```
def show(id: Long) = Action {
  Client.findById(id).map { client =>
    Ok(views.html.Clients.display(client))
  }.getOrElse(NotFound)
}

```
###### 파라미터 타이핑 

* 고정된 값을 넘기고 싶다면?
```
# 고정된 값인 home이 넘어간다.
GET   /                     controllers.Application.show(page = "home")
GET   /:page                controllers.Application.show(page)
```

* 기본값을 지정하려면? 아래처럼 하자.
```
GET   /clients              controllers.Clients.list(page: Int ?= 1)
```

* 옵션 파라미터면? Option을 활용하자.

```
GET   /api/list-all         controllers.Api.list(version: Option[String])
```


##### 라우팅 우선순위

* 두개의 라우팅이 충돌나면 먼저 선언한걸 우선으로 사용한다.

##### 역 라우팅 

* 컨트롤러에서 route에 있는 URI로 변환 할 수 있다. 
* RoR의 `_path`시리즈와 같은 것이다. 
* redirect 에서 주로 사용한다.

* 예를 들어 
```
package controllers

import play.api._
import play.api.mvc._

class Application extends Controller {

  def hello(name: String) = Action {
    Ok("Hello " + name + "!")
  }

}
```
처럼 컨트롤러를 선언하고 아래처럼 URL을 매핑했다면 
```
GET   /hello/:name          controllers.Application.hello(name)
```
* 컨트롤러의 다른 메소드에서 `/hello/Bob`을 아래처럼 호출할 수 있다. 

```
def helloBob = Action {
  Redirect(routes.Application.hello("Bob"))
}
```



## Result 값을 변화시키기 

암묵적으로 response body에 따라 설정되어 있다.
예를 들면 
```
val textResult = Ok("Hello World!")
```
이면 `Content-Type` 헤더는 `text/plain`이다  
반면, 

```
val xmlResult = Ok(<message>Hello World!</message>)
```

이면 XML이 result이다.

그러면 수동으로 지정하려면 어떻게할까?

as(newContentType)을 사용하면 된다.

```
val htmlResult = Ok(<h1>Hello World!</h1>).as("text/html")
```
이나 
```
val htmlResult2 = Ok(<h1>Hello World!</h1>).as(HTML)
```
위 두가지 방식이 있다. 

### Http Header 변경
* `withHeaders`를 호출하면 된다.

```
val result = Ok("Hello World!").withHeaders(
  CACHE_CONTROL -> "max-age=3600",
  ETAG -> "xx")
```

#### 쿠키 설정

* `withCookies`로 설정하고 `discardingCookies`로 쿠키를 제거한다.
* 쿠키 생성 
```
val result = Ok("Hello world").withCookies(
  Cookie("theme", "blue"))
```
* 쿠키 제거 
```
val result2 = result.discardingCookies(DiscardingCookie("theme"))
// 쿠키를 두개 설정하고 나머지 하나는 제거
val result3 = result.withCookies(Cookie("theme", "blue")).discardingCookies(DiscardingCookie("skin"))
```


### Session 와 Flash scopes
* 여러 HTTP request에 공통으로 사용해야하는 데이터가 있는 경우 session이나 flash 를 사용한다.
* flash는 다음 request까지만 유효할 때 사용한다.
* 세션과 플래시 scope은 cookie 매커니즘을 이용해 구현한다. 
* session은 `PLAY_SESSION` 라는 이름으로 쿠키에 저장된다.
* application.conf의 `session.cookieName`으로 바꿀 수 있다.

#### 세션
* 아래처럼 전체 세션을 대체할 수 있다.
```
Ok("Welcome!").withSession(
  "connected" -> "user@gmail.com")
```
* 단순히 추가하려면
```
Ok("Hello World!").withSession(
  request.session + ("saidHello" -> "yes"))
```
* 삭제 하려면?
```
Ok("Theme reset!").withSession(
  request.session - "theme")
```
* 세션 값 읽기 
```
def index = Action { request =>
  request.session.get("connected").map { user =>
    Ok("Hello " + user)
  }.getOrElse {
    Unauthorized("Oops, you are not connected")
  }
```
* 전체 세션값을 삭제 하려면
```
Ok("Bye").withNewSession
```

####  Flash scope

* 데이터를 다음 request까지만 유지한다
* 유저가 수정할 수 있다.
* 단순히 알림이나 오류 성공 메시지 전달할때만 쓰자.
* 아래는 그 예제다 
```
def index = Action { implicit request =>
  Ok {
    request.flash.get("success").getOrElse("Welcome!")
  }
}

def save = Action {
  Redirect("/home").flashing(
    "success" -> "The item has been created")
}
```

* 뷰에서 flash scope의 값을 보고 싶을 때는 implicit Flash parameter로 설정 해야한다.
```
@()(implicit flash: Flash)
...
@flash.get("success").getOrElse("Welcome!")
...
```

* 액션에서 `implicit request =>`를 쓰는 경우 뷰에 implicit request에 기반한  implicit Flash가  제공될 것이다. 
```
def index = Action { implicit request =>
  Ok(views.html.index())
}
```


#  Play 템플릿 사용법

## play 템플릿은  Twirl 엔진을 사용한다.

play 스칼라 템플릿은 작은 scala 코드이다.
`views/Application/index.scala.html` 파일이면 코드에서는 `views.html.Application.index` 클래스로 만들어진다.

아래는 예제이다.

```
@(customer: Customer, orders: List[Order])

<h1>Welcome @customer.name!</h1>

<ul>
@for(order <- orders) {
  <li>@order.title</li>
}
</ul>
```

위에걸 코드에서 사용하고 싶다면
```
val content = views.html.Application.index(c, o)
```

## `@` 문자

`@`는 템플릿용 문자다. 이 문자를 prefix로 해서  분기, 반복, 값 할당 등을 한다.

```
Hello @customer.name!
```

```
Hello @(customer.firstName + customer.lastName)!
```

```
Hello @{val name = customer.firstName + customer.lastName; name}!
```
 escape는 `@@`으로 쓰면 된다
```
My email is bob@@example.com
```

## 템플릿 파라미터 

함수처럼 사용한다. 파라미터가 필요하면 템플릿 가장 위에 선언하자.
```
@(customer: Customer, orders: List[Order])
// 기본값 사용
@(title: String = "Home")

// 여러개 
@(title: String)(body: Html)
```

## 반복 
`for`키워드를 사용하면 loop를 사용 할 수 있다.
```
<ul>
@for(p <- products) {
  <li>@p.name ($@p.price)</li>
}
</ul>
```

## if 

```
@if(items.isEmpty) {
  <h1>Nothing to display</h1>
} else {
  <h1>@items.size items!</h1>
}
```
## 재사용 block

```
@display(product: Product) = {
  @product.name ($@product.price)
}

<ul>
@for(product <- products) {
  @display(product)
}
</ul>
```
* 메소드 형태로 묶을 수 있지만 복잡한 로직을 넣는 것은 아니다.

## defining 문법

* 재사용 값을 선언하는 방법이다. `defining`을 사용한다.

```
@defining(user.firstName + " " + user.lastName) { fullName =>
  <div>Hello @fullName</div>
}
```


## import 문장

`@import` 로 템플릿으로 가져올 수 있다.

```
@(customer: Customer, orders: List[Order])

@import utils._

...
```

`_root_` prefix를 사용해서 절대 경로로 가져올 수 있다.
```
@import _root_.company.product.core._
```

## 주석

주석은 다음과 같이 쓸수 있다
```
 @* *@
```

## String 외삽(interpolation)

* String에서 value 값을 대체해서 넣으려면 `$`를 사용한다.
```
import play.twirl.api.StringInterpolation

val name = "Martin"
val p = html"<p>Hello $name</p>"
```

## 참고

(https://playframework.com/documentation/2.4.x/ScalaTemplates)
