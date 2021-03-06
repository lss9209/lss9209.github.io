---
title: "ElasticSearch 설치하기"
date: 2021-03-06 08:10:47 -0400
categories: ElasticSearch
---
# 엘라스틱 서치 설치하기
---
## 사전 조건

1. 자바 환경이 설치되어있어야 한다. Java -version으로 확인해보고 미설치되어있으면 설치하자.
2. rpm, yum, brew 등의 패키지매니저들 중 하나가 설치되어 있어야 한다. 그리고 물론 리눅스 혹은 맥OS가 설치되어 있어야 한다.

필자는 맥OS 환경에서 Brew 패키지매니저로 설치하였다. 만약 다른 패키지매니저를 쓴다면 명령어 형식이 조금 다를 수도 있음을 유의바란다.

이런 조건이 갖춰 졌다면
<pre>
<code>
brew install elasticsearch
</code>
</pre>
명령어를 통해 간단히 설치 할 수 있다.

설치 완료 후 
<pre style="background-color:red">
<code>
elasticsearch
</code>
</pre>
명령어를 통해 실행 할 수 있다.

실행되었는지 확인해보려면 웹브라우저로 localhost:9200으로 접속해본다. 만약
<pre>
<code>
{
  "name" : "isangseung-ui-MacBookPro.local",
  "cluster_name" : "elasticsearch_brew",
  "cluster_uuid" : "7Ys68iiJQhiAtEOTrSJ3vw",
  "version" : {
    "number" : "7.10.2-SNAPSHOT",
    "build_flavor" : "oss",
    "build_type" : "tar",
    "build_hash" : "unknown",
    "build_date" : "2021-01-16T01:41:27.115673Z",
    "build_snapshot" : true,
    "lucene_version" : "8.7.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
</code>
</pre>
와 같이 뜬다면 설치 및 실행이 잘 된 것이다.

참고서적: 기초부터 다지는 ElasticSearch 운영 노하우 (박상헌 등 저, 프로그래밍 인사이트)
