# 자동완성 ES 예제.

* 문자열을 1개부터 10개까지 토큰화 시켜서 저장해둠. 
 + 이거 멘션 초대에서 쓰는거 아니었으면 용량이 대체 얼마야?(...)
 + plain text니 좀 낫겠지.
 + 앞으로 찾고 뒤로 찾고..
   + 이 두개를 bool쿼리로 찾고 boost로 옵션을 주면 적당히 자동완성이 만들어진다?!
* 아래 쿼리 예제 두개를 bool로 묶고 boost등 을 통해서 우선순위를 정해주면 원하는 순서대로 출력할 수 있다.
   
## 데이터 구조 

```
PUT /autocomplete
{
  "settings" : {
    "index" : {
        "analysis" : {
            "analyzer" : {
                "edge_ngram_analyzer" : {
                	"type" : "custom",
                    "tokenizer" : "edge_ngram_tokenizer",
                    "filter" : ["lowercase", "trim"]
                },
                "edge_ngram_analyzer_back" : {
                	"type" : "custom",
                    "tokenizer" : "edge_ngram_tokenizer",
                    "filter" : ["lowercase", "trim", "edge_ngram_filter_back"]
                }
            },
            "tokenizer" : {
                "edge_ngram_tokenizer" : {
                    "type" : "edgeNGram",
                    "min_gram" : "1",
                    "max_gram" : "10",
                    "token_chars": [ "letter", "digit", "punctuation", "symbol" ]
                }
            },
            "filter" : {
            	"edge_ngram_filter_front" : {
            		"type" : "edgeNGram",
            		"min_gram" : "1",
                    "max_gram" : "10",
                    "side" : "front"
            	},
            	"edge_ngram_filter_back" : {
            		"type" : "edgeNGram",
            		"min_gram" : "1",
                    "max_gram" : "10",
                    "side" : "back"
            	}
            }
        }
    }
  },
  "mappings": {
    "personnel" : {
      "properties": { 
      "id" :  { "type" : "long", "index" : "not_analyzed" },
              			"search_fields_front" : {
              			  "analyzer" : "edge_ngram_analyzer", 
              			  "type" : "string", 
              			  "store" : "no", 
              			  "index" : "analyzed",
              			  "omit_norms" : true, 
              			  "index_options" : "offsets", 
              			  "term_vector" : "with_positions_offsets", 
              			  "include_in_all" : false
              			  },
        			      "search_fields_back" : {
        			        "analyzer" : "edge_ngram_analyzer_back", 
        			        "type" : "string", 
        			        "store" : "no", 
        			        "index" : "analyzed", 
        			        "omit_norms" : true, 
        			        "index_options" : "offsets", 
        			        "term_vector" : "with_positions_offsets", 
        			        "include_in_all" : false
        			        }
      }
    }
  }
}
```

## 예제 데이터

```
POST /autocomplete/personnel
{
  "id" : 1,  
    "search_fields_back": ["박용택", "YongTak Park", "ytpark@test.com", "fire.park"],
    "search_fields_front": ["박용택", "YongTak Park", "ytpark@test.com", "fire.park"]
}


POST /autocomplete/personnel
{
  "id" : 2,  
    "search_fields_back": ["삼신택", "ShinTak Sam", "stsam@test.com", "threeGods"],
    "search_fields_front": ["삼신택", "ShinTak Sam", "stsamk@test.com", "threeGods"]
}

POST /autocomplete/personnel
{
  "id" : 3,  
    "search_fields_back": ["박진성", "JinSung Park", "jspark@test.com", "JiRoo.Park"],
    "search_fields_front": ["박진성", "JinSung Park", "jspark@test.com", "JiRoo.Park"]
}

```


## 조회 쿼리

### Prefix 검색
```
GET /autocomplete/personnel/_search
{
  "query" : {
  "term": { 
    "search_fields_front" : "박"
    }  
  }
}
```

#### 조회 결과
```
  "hits": {
    "total": 3,
    "max_score": 1,
    "hits": [
      {
        "_index": "autocomplete",
        "_type": "personnel",
        "_id": "AVZGLZiMrEE23MLBCElc",
        "_score": 1,
        "_source": {
          "id": 2,
          "search_fields_back": [
            "박진성",
            "JinSung Park",
            "jspark@test.com",
            "JiRoo.Park"
          ],
          "search_fields_front": [
            "박진성",
            "JinSung Park",
            "jspark@test.com",
            "JiRoo.Park"
          ]
        }
      },
      {
        "_index": "autocomplete",
        "_type": "personnel",
        "_id": "AVZGLY7HrEE23MLBCEla",
        "_score": 0.30685282,
        "_source": {
          "id": 1,
          "search_fields_back": [
            "박용택",
            "YongTak Park",
            "ytpark@test.com",
            "fire.park"
          ],
          "search_fields_front": [
            "박용택",
            "YongTak Park",
            "ytpark@test.com",
            "fire.park"
          ]
        }
      }
    ]
  }
}    
    
```

### Postfix 검색

```
GET /autocomplete/personnel/_search
{
  "query" : {
  "term": { 
    "search_fields_back" : "택"
    }  
  }
}
```

#### 결과

```
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 5,
    "successful": 5,
    "failed": 0
  },
  "hits": {
    "total": 2,
    "max_score": 1.4054651,
    "hits": [
      {
        "_index": "autocomplete",
        "_type": "personnel",
        "_id": "AVZGLZNXrEE23MLBCElb",
        "_score": 1.4054651,
        "_source": {
          "id": 2,
          "search_fields_back": [
            "삼신택",
            "ShinTak Sam",
            "stsam@test.com",
            "threeGods"
          ],
          "search_fields_front": [
            "삼신택",
            "ShinTak Sam",
            "stsamk@test.com",
            "threeGods"
          ]
        }
      },
      {
        "_index": "autocomplete",
        "_type": "personnel",
        "_id": "AVZGLY7HrEE23MLBCEla",
        "_score": 0.30685282,
        "_source": {
          "id": 1,
          "search_fields_back": [
            "박용택",
            "YongTak Park",
            "ytpark@test.com",
            "fire.park"
          ],
          "search_fields_front": [
            "박용택",
            "YongTak Park",
            "ytpark@test.com",
            "fire.park"
          ]
        }
      }
    ]
  }
}
```
