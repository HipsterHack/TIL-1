## Presto Query 팁1

Presto는 서브쿼리를 지원하지 않는다.

예를 들면 

````
SELECT t.id,
       (SELECT c.val FROM code c WHERE c.relate_id = t.id) AS code
FROM table t
````
위와 같은 경우는 지원하지 않는다.
대신 아래와 같은 형태로 조인으로 조회해야 한다.

```
SELECT t.id,
      c.val AS code
FROM table t
INNER JOIN  code c 
ON t.id = c.relate_id 
```

조인을 써야하지만 서브 쿼리 대안으로 쓸 수 있는 INNER VIEW를 지원한다. 
예는 아래에 있다.

```
SELECT t.id,
      c.val AS code
FROM table t
INNER JOIN  (SELECT id, 
                    related_id 
             FROM code
             LIMIT 1
            ) c 
ON t.id = c.relate_id 
```

