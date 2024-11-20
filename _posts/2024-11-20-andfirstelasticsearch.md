---
title: Elastic search 에서 And 조건의 document에 점수 많이 주기
date: 2024-11-20
categories:
  - misc
tags:
  - ElasticSearch
  - boost
  - and
render_with_liquid: true
mermaid: true
---
#### 검색 시스템을 개선하고 싶다!
---
우리 플랫폼이 SW Q&A 플랫폼이다 보니 검색 성능에 대해서 개선을 상당히 긴 시간 동안 못하고 있었다... 사실은 핑계인거 같지만!

예전에 검색을 개선한다고 Elastic Search 를 도입하고 모든 post를 indexing하고 다음과 같은 query를 날려서 검색결과를 가져오고 있었다.

title 과 content를 저장 하고 같은 검색어로 검색 하려고 하기때문에 multi match를 사용했다.

```json
{
  "query": {
    "multi_match": {
      "query": "Token1 Token2 Token3 Token4",
      "fields": ["title^3", "content^1"]
    }
  }
}
```


#### 문제가 있다!!
---
그런데 이렇게 하면 어떤 post가 Token1 만 가지고 있다고해도 글 길이가 짧고 또 Token1의 개수가 많아지면 중요도가 높아지면서 상위에 랭크되는 결과를 가져왔다.

우리 플랫폼이 글 개수가 엄청 많은 것이 아니라서 더 부각되어 보이는 문제가 있어서 Token 개수가 다양하게 일치하는 친구에게 더 큰 점수를 부여하고 싶었다!

사용자들 또한 이러한 문제를 제일 많이 우리에게 건의했었다. 다 포함되어있는 친구가 제일 위로 오면 좋겠다고!


#### And 조건에 더 큰 점수를 부여하는 방법은 어떨까?
---
따라서 나는 모든 token을 가지고 있는 경우에 점수를 상당히 후하게 주는 쿼리로 수정하기로 결정했다.

한국어, 중국어의 경우 조사라는게 있기 때문에 쿼리를 날리기 전에 그런 친구들을 모두 제거해주는 작업을 우선적으로 거치고 뒤의 과정을 진행했다.

match 를 우선 나누고 and operator를 사용한 쿼리를 추가한다. 그리고 해당 쿼리에 boost 점수를 부여했다.

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
            "content":  {
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
        },
        {
          "match": {
            "content": {
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

이런식으로 쿼리를 날리면 모든 token이 들어간 post가 큰 점수를 가지게 된다!

#### 사용자가 보기에는 제목에서 검색어가 많이 등장하면 느낌이 달라진다
---
여러번 테스트를 해봤는데 사용자의 입장에서 검색을 해봤을때 본문에 원하는 검색결과가 있는 것 보다 제목에서 바로 보이는 것이 훨씬 더 만족도가 좋았다. 따라서 위의 쿼리를 조금 수정 하였다.

```json
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "title": {
	            "query" : "Token1 Token2 Token3 Token4",
	            "operator" : "or",
	            "boost" : 5
            }
          }
        },
        {
          "match": {
            "content":  {
	            "query" : "Token1 Token2 Token3 Token4",
	            "operator" : "or",
	            "boost" : 1
            }
          }
        },
        {
          "match": {
            "title": {
              "query": "Token1 Token2 Token3 Token4",
              "operator": "and",
              "boost": 25
            }
          }
        },
        {
          "match": {
            "content": {
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