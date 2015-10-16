# 파티션 재생성 (Recover Partition)

 오늘 회사에서 하이브를 설치할 기회가 있었다.  

테스트 데이터로 쓰기 위해 운영 데이터 일부를 개발 HDFS에 직접 파일을 넣었다. 

그런데, 테이블 구조도 동일하고, 데이터도 있는데 hive콘솔에서 조회하면 0건으로 나오는 것이었다.

알고보니 하이브는 메타스토어(주로 MySQL)에 테이블의 파티션 정보(이 테이블엔 어떤 파일들에 데이터가 저장되어 있는가...하는 정보)를 저장하기 때문이었다. 

즉 직접 데이터를 때려 넣었기 때문에 메타 스토어에서 파티션 정보가 없다. 그러니 하이브는 내가 밀어 넣은 데이터는 없다고 얘기하는 것이다. 이렇게 직접 파일로 데이터를 넣는 경우 파티션을 재생성 해줘야 hive에서 문제 없이 조회할 수 있었다. 

아래와 같은 명령어를 사용해서 테이블의 파티션 정보 업데이트를 한다.

## 예제

```
MSCK REPAIR TABLE table_name;
```

혹은 

```
ALTER TABLE table_name RECOVER PARTITIONS;
```

아랫쪽이 RDBMS 문법에 가깝다.


## 참고

* [Apache Hive 매뉴얼 DDL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-RecoverPartitions(MSCKREPAIRTABLE))