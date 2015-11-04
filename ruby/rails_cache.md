## Rails Cache 

### SQL Cache

* 모든 쿼리는 request 단위로 SQL질의 결과를 서버 메모리에서 캐싱을 하고 있다. 
* hibernate 같은 ORM이 하는 기능을 기본적으로 하고 있다.


### low level cache

Rails.cache를 이용해서 코딩한다.
아래와 같이 cache를 작성하면 마지막에 호출한 return 한 값은 cache로 들어간다.
(굳이 write를 쓸 필요가 없다. 코딩시 참고하자)

```
 result = Rails.cache.fetch(cache_key, expires_in: 1.hour) do
  Article.all
 end
```
 
 
### 참고 

[SQL 캐시](http://guides.rubyonrails.org/caching_with_rails.html#sql-caching)
