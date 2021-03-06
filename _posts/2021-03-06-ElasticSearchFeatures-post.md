---
title: "ElasticSearch의 기능 살펴보기"
date: 2021-03-06 13:44:47 -0400
categories: ElasticSearch
---
# 엘라스틱 기능 살펴보기
---
## 엘라스틱 서치의 기능들
엘라스틱 서치는 크게 세가지 기능, 색인 및 조회기능과 검색기능 그리고 분석기능을 갖추고 있다. 하나씩 살펴보자.

## 색인과 조회 기능

<div style="background-color:#add8e6; border-radius: 25px;">
<pre>
<code>
isangseung@isangseung-ui-MacBookPro ~ % curl -X PUT "localhost:9200/user/_doc/1?pretty" -H 'Content-Type: application/json' -d'
quote> {
quote> "username": "sangseung.lee"
quote> }
quote> '
{
  "_index" : "user",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
</code>
</pre>
</div>

위와 같이 curl을 통해서 리눅스 명령어를 통해 API를 호출하면 색인 기능이 동작한다. 색인 기능 사용시 기억할 것들은 이러하다.
1.인덱스가 없다면 인덱스를 생성하고 타입또한 자동 생성된다. 다만 이때 타입을 바탕으로 동적으로 생성된 스키마가 이 인덱스의 스키마가 되며 이후 이 스키마와 구조가 다른 문서가 들어오면 에러가 발생된다.
2.스키마충돌이 없다면 해당 문서아이디가 존재하는지 여부를 살피고 존재하지 않는다면 신규로 저장하고 아이디가 존재한다면 기존 문서를 업데이트 한다.

<div style="background-color:#add8e6; border-radius: 25px;">
<pre>
<code>
isangseung@isangseung-ui-MacBookPro ~ % curl -X GET "localhost:9200/user/_doc/1?pretty"
{
  "_index" : "user",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 0,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "username" : "sangseung.lee"
  }
}
</code>
</pre>
</div>
조회 코드는 이러하다. 삭제코드도 REST API의 DELETE키워드를 통해 가능하며 삭제한 문서도 조회는 된다. 다만 found값이 false로 나온다.

스키마 검색은 아래와 같이 해볼 수 있다.

<div style="background-color:#add8e6; border-radius: 25px;">
<pre>
<code>
isangseung@isangseung-ui-MacBookPro ~ % curl -s "http://localhost:9200/contents/_mappings?pretty"
{
  "contents" : {
    "mappings" : {
      "properties" : {
        "author" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "title" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        }
      }
    }
  }
}
</code>
</pre>
</div>

