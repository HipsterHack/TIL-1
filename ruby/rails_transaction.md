## Rails 트랜잭션 

오늘은 Rails에서 트랜잭션을 사용하는 방법을 다뤄본다.

테스트는 레일즈 4.1.2 버전에서 진행했다.

###  기본적인 사용벙법

모델에 transaction method(ActiveRecord::ConnectionAdapters::DatabaseStatements 에 있다)를 호출하면 된다.

파라미터로 블록 안에 한번에 transaction 으로 처리해야 할 내용을 추가한다.(참 쉽죠?)

이제 예제를 보자.


```
Article.transaction do 
	@article.title = 'change.' 
	@article.save
end 
```

임의로 롤백을 하려면? 간단히 ActiveRecord::Rollback.new 를 raise허면 된다.

```
Article.transaction do 
	@article.title = 'change.' 
	unless @article.save do 
		# 롤백 처리 
		raise ActiveRecord::Rollback.new 
	end 
end 
```

### Isolation level


보통 DB에서 지원하는 일반적인 4가지 레벨을 지원한다


* READ UNCOMMITTED :  트랜잭션에서 처리중인 데이터를 다른 트랜잭션에서 읽을 수 있음(Dirty read 라부른다)
* READ COMMITTED:  트랜잭션에서 커밋된 데이터를 읽을 수 있음. Non-Repeatable Read, phantom read가 발생 할 수 있다.
* REPEATABLE READ: 앞선 트랜잭션이 읽은 데이터를 다음 트랜잭션이 갱신하거나 삭제할 수 없다.
* SERIALIZABLE: 조회한 모든 Record에 락을 걸어버린다.

* 하지만 레일즈는  Isolation level을 Rails 4부터 지원한다.

Rails 에서 사용 방법은 쉽다. transaction에 파라미터로 isolation을 hash로 지정하면 된다.
아래 예시를 참고한다.

```
Article.transaction (isolation: :repeatable_read)  do 
# 트랜잭션 처리
end 
```

아래 심볼을 이용해서 isolation 레벨을 지정한다.

* :read_uncommitted
* :read_committed
* :repeatable_read
* :serializable


### 중첩 transaction

DBMS에서 중첩 Transaction을 지원하는건 MS-SQL 뿐이다.
그래서 Rails 에선 내부 구현할땐 SavePoint를 이용해서 구현한다.

단 MySQL은 DDL transaction을 지원하지 않으므로 DDL(테이블 생성 등)을 쓰게 될때 조심하자. (근데 누가 이걸 사용해...--;)


###  참고 
* [레일즈 트랜잭션 in Stackoverflow](http://stackoverflow.com/questions/9127027/how-to-set-transaction-isolation-level-using-activerecord-connection-recommend)
* [Rails API](http://apidock.com/rails/ActiveRecord/ConnectionAdapters/DatabaseStatements/transaction)
* [SavePoint 설명](https://ko.wikipedia.org/wiki/SAVEPOINT)
