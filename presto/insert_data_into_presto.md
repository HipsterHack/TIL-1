# Insert Data into Presto

* insert 문은 있으나 하이브를 지원하진 않는다.
* presto는 hive의 insert를 지원하지 않는다.(0.115 버전현재)
* 데이터를 생성하려면 Create table as 구문을 사용해야한다

예제


```
CREATE TABLE visit_log AS
SELECT log_date, detail 
FROM raw_log
```

## 참고

* [CREATE AS presto 문서](https://prestodb.io/docs/current/sql/create-table-as.html)
