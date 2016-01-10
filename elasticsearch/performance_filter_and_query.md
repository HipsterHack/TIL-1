# Elasticsearch

## query
* full text search를 하거나 문서의 관련도에 따라 데이터를 조회하는 경우에 query라 한다.

```
curl -XGET http://es.google.com:9200/_search -d '
{
  "query": {
    "bool": {
      "must": [
        {
          "match_all": {}
        },
        {
          "term": {
            "user.id": "5681810"
          }
        }
      ]
    }
  }
}'
```
## Filter
* 정확한 값으로 비교해서 데이터를 조회하는 경우
```
curl -XGET http://es.google.com:9200/_search -d '
{
  "query": {
    "filtered": { 
      "filter": {
      	"term": {
	        "user.id": "5681810",
	        "_cache": "true"
	  }   
      }
    }
  }
}'

```

* 기본적으로 Filter 는 Query 보다 빠르다. Filter는 document간 유사도 조회를 하지 않으니까 당연하다.
* 그리고 Filter를 Cache 저장하면 쿼리보다 1000배쯤 빠르다.(응답 속도가 1/10 까지 내려간다)
`indices.cache.filter.size` (디폴트는 10%)로 필터의 캐시 크기를 지정할 수 있다.

## 참고
* [쿼리와 필터](https://www.elastic.co/guide/en/elasticsearch/guide/current/_queries_and_filters.html)
* [필터 캐시](https://www.elastic.co/guide/en/elasticsearch/reference/1.4/index-modules-cache.html#filter)
