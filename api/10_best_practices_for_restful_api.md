# RESTful API Best Practices

## 1. 명사를 사용하라. 동사 쓰지 말라 

* 쉽게 이해할 수 있게 구조를 잡아라 
* 아래는 예제다. 
* 
| Resource | GET(read)              | POST(Create)         | PUT(update)                  | DELETE                 |
| -------- |:----------------------:|:--------------------:|:----------------------------:|:----------------------:|
| /cats    | 고양이 목록을 리턴한다 | 새 고양이를 만든다   | 여러 고양이들을 업데이트한다 | 모든 고양이를 삭제한다 |
| /cats/1  | 1번 고양이 정보를 준다 | 허용안된 method(405) | 1번 고양이를 업데이트한다    | 1번 고양이를 삭제한다  | 

* 나쁜예
```
/deleteAllRedCats
/createNewCats
```

## 2. GET method와 쿼리 파라미터로 상태를 바꾸지 말아라.

* GET 대신 PUT, POST, DELETE를 사용해라

## 3. 복수형 명사를 사용해라 

* 복수 명사만 접근을 허용해라.
```
/cats
/products
/settings
```

## 4. 관계를 표현하려면 하위 resource를 사용해라 

```
GET /products/141/orders/ 141 상품에 대한 주문 목록을 return 한다
```

## 5. 직렬화 포맷을 위한 HTTP 헤더를 사용해라 
* client나 서버나 통신을 위한 포맷이 존재한다. 

예를 들어 Content-Type은 요청 포맷이고, Accept 는 수락가능한 응답 포맷 리스트를 정의한다.


## 6. HATEOAS를 사용해라 
* HATEOAS는 Hypermedia as the Engine of Application State의 약자다.
* API를 통해서 좀더 나은 찾기가 가능하게 하이퍼링크가 쓰여야한다.
* 응답에 리소스에 대한 정보가 있으면 그에 대한 링크를 넣어서 찾기가 가능하다
```
{
  "id": 711,
  "manufacturer": "bmw",
  "model": "X5",
  "seats": 5,
  "drivers": [
   {
    "id": "23",
    "name": "Stefan Jauker",
    "links": [
     {
     "rel": "self",
     "href": "/api/v1/drivers/23"
    }
   ]
  }
 ]
}
```


## 7. 필터링, 정렬, 필드 선택, 페이징을 제공해라 
### Filtering

* 필터링을 위한 고유 파라미터를 사용해라 
```
GET /cats?color=black 검은 고양이 목록을 구한다.
GET /cars?seats<=2 의자가 2보다 작거나 같은 차 목록을 구한다.
```

### Sorting
* 오름차순은 +로 내림차순은 -로 표현하고 컬럼 단위로 정렬을 표현한다.
```
GET /cars?sort=-manufactorer,+model
```

### Field 선택

* 모바일은 URL이 길게 볼 수 없다. 그래서 delimter등을 도입해서 같은 파라미터를 하나의 parameter로 만든다. 아래가 그 예.
```
GET /cars?fields=manufacturer,model,id,color
```

### 페이징 

* limit , offset을 이용해라. limit는 페이지당 보여줄 개수. offset은 처음부터 지금 데이터까지의 위치.
* 예 
```
GET /cars?offset=10&limit=5
```

## 8. API version을 적용해라 
* API버전은 prefix로 v, 뒤에는 숫자로 구성한다. 소수점은 쓰지 말아야 한다.

```
/blog/api/v1
```

## 9. 에러 핸들링을 위해 HTTP code를 써라 

* 표준 API 코드를 이용해서 통신하자

```
200 – OK – 모든게 정상. 잘 동작함Eyerything is working
201 – OK – 새 리소스가 만들어짐New resource has been created
204 – OK – 성공적으로 지워짐 The resource was successfully deleted

304 – Not Modified – 클라이언트가 캐시 데이터를 이용한다.The client can use cached data

400 – Bad Request – 잘못된 request가 들어왔다. invalid json이 들어왔거나 하면 나타남. The request was invalid or cannot be served. The exact error should be explained in the error payload. E.g. „The JSON is not valid“
401 – Unauthorized – request가 user 인증이 필요함. The request requires an user authentication
403 – Forbidden – 접근이 허용되지 않음. The server understood the request, but is refusing it or the access is not allowed.
404 – Not found – 찾을수 없음 There is no resource behind the URI.
422 – Unprocessable Entity – 처리를 할 수 없는 요소. 이미지 포맷처리를 못하거나 필수 필드가 빠졌을때 나타나는 오류. Should be used if the server cannot process the enitity, e.g. if an image cannot be formatted or mandatory fields are missing in the payload.

500 – Internal Server Error – 서버 내부 오류. API developers should avoid this error. If an error occurs in the global catch blog, the stracktrace should be logged and not returned as response.
```
### json형태의 payload를 제공하라 

모든 예외에 대해 아래처럼 에러가 있으면 자체 오류를 제공한다. 
```
{
  "errors": [
   {
    "userMessage": "Sorry, the requested resource does not exist",
    "internalMessage": "No car found in the database",
    "code": 34,
    "more info": "http://dev.mwaysolutions.com/blog/api/v1/errors/12345"
   }
  ]
} 
```

## 10. http method 오버라이딩을 허용하라
*  X-HTTP-Method-Override를 이용해서 RESTful API지원하지 않는 메소드를 구현할때 사용한다.
* 예를 들면 FETCH는 지원하지 않는 경우가 많다. 

# 참고 
* http://blog.mwaysolutions.com/2014/06/05/10-best-practices-for-better-restful-api/
