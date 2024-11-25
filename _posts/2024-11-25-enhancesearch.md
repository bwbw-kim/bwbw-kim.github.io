---
title: Elastic Search 검색 개선 하기
date: 2024-11-25
categories:
  - misc
tags:
  - ElasticSearch
  - boost
  - 2token
render_with_liquid: true
mermaid: true
---
#### 저번에 검색 개선한것이 효과가 있다
---
불과 5일전에 검색 성능을 개선하기 위해서 and 일 경우 boost 한것이 물론 효과는 있었지만 반례가 있음을 알았다. 

예를 들어 "A B C D" 라고 검색했을 경우 모든 토큰이 포함된 document가 없으면 사실상 의미가 없는 쿼리라는것이 허점이었다. 나는 그런 상황에서도 A, B ,C 3개가 포함된 친구는 나왔으면 하는 바람이 있었다.

따라서 쿼리를 조금 추가하기로 했다.

#### 2개의 토큰만 때와서 and 점수를 부여하자
---
기존의 쿼리는 다음과 같다
```json
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "title": {
	            "query" : "Token1 Token2 Token3 Token4",
	            "operator" : "or"
            }
          }
        },
        {
          "match": {
            "title": {
              "query": "Token1 Token2 Token3 Token4",
              "operator": "and",
              "boost": 5
            }
          }
        }
      ]
    }
  }
}
```

여기에서 쿼리를 만들때 이부분을 추가해주려고 한다

```json
{
  "match": {
	"title": {
	  "query": "Token1 Token2",
	  "operator": "and",
	  "boost": 5
	}
  }
},
{
  "match": {
	"title": {
	  "query": "Token1 Token3",
	  "operator": "and",
	  "boost": 5
	}
  }
}
...
```
총 N 개의 토큰이라면 N(N-1)/2 만큼의 and 쿼리를 추가한다. 물론 이 쿼리도 boost를 준다!
이렇게 되면 A B C 가 있는 문서가 훨씬 높은 score를 받을수가 있을 것이라고 생각한다.!

실제 동작도 보면 검색 성능이 나아진 것 처럼 보인다.


#### 개선점이 또 있을까?
---

위의 쿼리는 잘 동작하는것 처럼 보인다 하지만... O^2 만큼 query가 길어져서 검색 토큰에 제한이 있을 것 같다. 

계속해서 발전 시킬수 있는 방법을 찾아봐야겠다.

