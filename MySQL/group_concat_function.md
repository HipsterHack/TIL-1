# 여러 row를 하나로 묶어주는 함수 
* mysql에는 `GROUP_CONCAT()`이라는 함수가 있다. 
* 이 함수의 역할은 여러 row를 하나의 row로 보여주는 역할을 한다.

예를 들면 

|article_id|comment_id_list|
|----------|---------------|
|1         |1,2,3,4        |
|2         |1              |


위와 같은 예제를 만들 수 있게 해준다.

```
SELECT DISTINCT article_id GROUP_CONCAT(DISTINCT comment_id ORDER BY comment_id) comment_id_list
FROM article
GROUP BY article_id
ORDER BY article_id
```

기본 separator는 `,` 으로  바꾸고 싶다면 `SEPARATOR` 를 이용하면 된다.

```
SELECT DISTINCT article_id GROUP_CONCAT(DISTINCT comment_id ORDER BY comment_id SEPARATOR '|') comment_id_list
FROM article
GROUP BY article_id
ORDER BY article_id
```

## 주의점

* 설정 변수인 `group_concat_max_len` 최대값이 달라진다. (기본은 1024)
* 아래처럼 최대값을 증가시킬 수 있다.

```
set @@group_concat_max_len = 90000;
```
