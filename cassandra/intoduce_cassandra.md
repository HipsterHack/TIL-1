# 카산드라

scalability와 high availability에 최적화한 Data storage.
Consistent hashing 과 ring 구조, 구조와 Gossip protocol을 구현하였다
샤딩을 고려할 필요 없고 노드 장비 추가 제거가 쉽다.


## 데이터 구조

keyspace => 일종의 database.
keyspace안에 table이 있고. 테이블은 여러개의 row로 구성되어 있다. 
RDBMS 구조와 비슷하다. 

## 데이터 저장 방식

* 링형구조. 
* 링을 구성하는 각 노드에 partition key 혹은 row key에 데이터의 hash값을 기준으로 분산저장한다.
* 이 해쉬값을 token이라고 부른다.

## 예제

```
 CREATE TABLE test_keyspace.test_table_ex1 ( 
    code text, 
    location text, 
    sequence text, 
    description text, 
    PRIMARY KEY (code, location)
);
INSERT INTO test_keyspace.test_table_ex1 (code, location, sequence, description ) VALUES ('N1', 'Seoul', 'first', 'AA');
INSERT INTO test_keyspace.test_table_ex1 (code, location, sequence, description ) VALUES ('N1', 'Gangnam', 'second', 'BB');
INSERT INTO test_keyspace.test_table_ex1 (code, location, sequence, description ) VALUES ('N2', 'Seongnam', 'third', 'CC');
INSERT INTO test_keyspace.test_table_ex1 (code, location, sequence, description ) VALUES ('N2', 'Pangyo', 'fourth', 'DD');
INSERT INTO test_keyspace.test_table_ex1 (code, location, sequence, description ) VALUES ('N2', 'Jungja', 'fifth', 'EE');

Select * from test_keyspace.test_table_ex1;
```
* 데이터가 정상 출력됨을 확인할 수 있다.

```
use test_keyspace;
list test_table_ex1;

Using default limit of 100
Using default cell limit of 100
-------------------
RowKey: N1
=> (name=Gangnam:, value=, timestamp=1452481808817684)
=> (name=Gangnam:description, value=4242, timestamp=1452481808817684)
=> (name=Gangnam:sequence, value=7365636f6e64, timestamp=1452481808817684)
=> (name=Seoul:, value=, timestamp=1452481808814357)
=> (name=Seoul:description, value=4141, timestamp=1452481808814357)
=> (name=Seoul:sequence, value=6669727374, timestamp=1452481808814357)
-------------------
RowKey: N2
=> (name=Jungja:, value=, timestamp=1452481808833644)
=> (name=Jungja:description, value=4545, timestamp=1452481808833644)
=> (name=Jungja:sequence, value=6669667468, timestamp=1452481808833644)
=> (name=Pangyo:, value=, timestamp=1452481808829751)
=> (name=Pangyo:description, value=4444, timestamp=1452481808829751)
=> (name=Pangyo:sequence, value=666f75727468, timestamp=1452481808829751)
=> (name=Seongnam:, value=, timestamp=1452481808823137)
=> (name=Seongnam:description, value=4343, timestamp=1452481808823137)
=> (name=Seongnam:sequence, value=7468697264, timestamp=1452481808823137)

2 Rows Returned.
Elapsed time: 67 ms.
```
위에서 살펴보면 
CQL과는 달리 단지 2개의 Row가 있고, 각 Row마다 속해있는 Column의 개수가 다르다
Cassandra는 'N1'과 'N2'를 각각 Hashing 하여 Token을 계산한다.
그 다음에 Token이 Cassandra 노드 중에서 범위에 해당하는 노드를 찾아 데이터를 만들고 삭제하고 하는 것이다



1. Cassandra는 Row Key의 Hash 값을 이용하여 데이터를 분산한다.
2. Cassandra Data Layer에서의 Row key = CQL partition key의 value이다. (복수의 partition key라면 해당 Column value들과 ":"문자의 조합이다.)
3. C assandra Data Layer에서의 Column Name = CQL cluster key의 Column value와 primary key에 속하지 않은 Column Name들 및 ":" 문자의 조합이다.
