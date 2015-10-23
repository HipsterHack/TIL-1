## [Rails] mini test를 사용한 모델 테스트 팁

* mini 테스트는 Fixture를 사용한다.

(즉 DB를 이용해서 테스트를 하는데 test case마다 한번 전체 테이블의 데이터를 지우고 Fixture를 한번 로딩하는 반복을 이용한다.)

* mini test에서는 test case 마다 transaction의 begin, rollback 을 사용한다.


대부분 DB(MS SQL 빼고)에서 중첩 transaction을  지원하지 않으므로 테스트 케이스 테스트 중에 오류가 발생할 수 있다.

트랜잭션이 걸려 있는 경우, 

1. transaction을 스킵하는 기능을 만듬
2. [AOP를 적용](http://blog.arkency.com/2013/07/ruby-and-aop-decouple-your-code-even-more/) 
3. transaction을 사용하지 않는다.
4. helper class를 통해서 테스트 범위 밖으로 밀어버린다.

위의 4가지 방법을 이용해서 트랜잭션 중첩을 회피 해야한다.

* fixture 를 DB로 로딩 하는것으로 인해 테스트 시간이 길다.
* Fixture가 싫다면, 일종의 mock를 만들어주는 [FactoryGirl](https://github.com/thoughtbot/factory_girl)를 쓰자.)
* fixture yml에 id를 지정안하면 mini test가 알아서 id를 만들어버린다. (대신 유니크는 보장한다)
* fixture yml에 null로 설정하면 빈값(nil)이 들어간다



### 참고자료

* [효과적인 fixture 사용법](http://blog.bigbinary.com/2014/09/21/tricks-and-tips-for-using-fixtures-in-rails.html)
* [AOP for ruby](http://blog.arkency.com/2013/07/ruby-and-aop-decouple-your-code-even-more/)
* [FactoryGirl](https://whatdoitest.com/getting-friendly-with-fixtures)
