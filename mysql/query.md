# MySQL Queries

* MySQL용 쿼리 집합. 
* 리마인드 차원에서 기록한다.


## 중복 처리
*  INSERT시 중복 키 처리 방법은 3가지가 있다.

1. `INSERT IGNORE`
2. `REPLACE INTO`
3. `INSERT INTO .. ON DUPLICATE UPDATE`

* unique index가 걸려있거나 primary key를 기준으로 duplicated key error가 발생한다.

### INSERT IGNORE
* duplicated key error가 발생하면 무시하고 오류를 안넣는 방법이다.
```
INSERT IGNORE INTO employees VALUES('kang', 10, 20, NOW());
```
### REPLACE INTO
* duplicated key error가 발생하면 그 데이터를 삭제하고 INSERT 한다.

```
REPLACE INTO employees VALUES ('kang', 10, 20, NOW());
```
### INSERT INTO .. ON DUPLICATE UPDATE
*  duplicated key error가 발생하면 대신 업데이트를 한다.
```
INSERT INTO employees VALUES('kang', 10, 20, NOW())
ON DUPLICATE KEY UPDATE name = 'kang', depart_id = 10, salary_amount = 20
```
* `INSERT INTO ... SELECT` 를 사용할수도 있다.
```
   INSERT INTO states
   (name, state_id, created_at)
   SELECT  
   	u.name,
   	10 AS state_id, 
    now() AS created_at
   FROM users u
   WHERE u.type = 'VIP'
   ON DUPLICATE KEY UPDATE 
   	name = u.name
    state_id = 10, 
   	created_at = now(),
   	updated_at = now()   
```

## UPDATE where 에서 자기 테이블 서브쿼리 방법

* 조건절에서 조건 컬럼을 업데이트 대상 테이블로 본다. 그래서 그냥 서브쿼리는 수행할 수 없다.

```
UPDATE
  users
SET state = 'perferences_input'
WHERE id IN (
   SELECT u.id  FROM users WHERE state IS NULL
)
```

* 한번 더 감싸서 VIEW로 만들어야 업데이트 가능하다.

```
UPDATE
  users us
SET us.state = 'perferences_input'
WHERE us.id IN (
   SELECT target.id 
   FROM (
   	 SELECT u.id  
   	 FROM users 
   	 WHERE state IS NULL
   ) target
)
```



### 참고 
* http://dev.mysql.com/doc/refman/5.1/en/insert-on-duplicate.html 공식문서 
* http://c.tistory.com/163 UPDATE에 서브쿼리
