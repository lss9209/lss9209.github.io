title: "ElasticSearch 성능확인과 ELK Stack의 활용"
date: 2021-03-06 15:10:47 -0400
categories: ElasticSearch
---
# 엘라스틱 성능 확인하기
---
## 상태정보 확인
### 클러스터 상태정보 확인
curl -s "http://localhost:9200/_cat/health?format=json&pretty"로 클러스터의 상태 확인 가능 <br>
이 중 status는 green, yellow, red이렇게 3개의 상태값을 갖는다. <br>
green:모든 샤드가 정상적 동작 <br>
yellow:프라이머리 샤드는 모두 정상적 동작, 그러나 일부 혹은 모든 레플리카 샤드가 동작하지 않음 <br>
red: 프라이머리 샤드의 일부도 동작하지 않음 <br>

### 노드 상태정보 확인
curl -s "http://localhost:9200/_cat/nodes?v"로 확인가능한데 <br>
이 노드도 개별적으로 status값을 가지며 위의 기준에 따라 이 값이 분류된다. <br>
이 때 하나의 노드라도 yellow 혹은 red이면 그 클러스터의 상태은 yellow 혹은 red다. 따라서 클러스터 상태정보를 보고 yellow 혹은 red를 발견했다면 노드개별검색으로 구체적으로 어떤 노드가 이상인지 파악할 수 있다. <br><br>

## 색인 성능과 검색 성능 확인
### 색인 성능 확인
curl -s http://localhost:9200/_stats?pretty | more API 입력 -> 원하는 샤드(프라이머리만 혹은 모든 샤드)의 index_total값과 index_time_in_mills값 기록 -> 10초정도 후 똑같은 Api 입력 <br>
->여기서 나온 index_total값과 index_time_in_mills를 앞의 두 값에서 각각 빼줌 그리고 그렇게 얻은 index_total의 차를 index_time_in_mills의 차로 나눠주면 건당 색인 속도를 확인가능하다. <br>

### 검색 성능 확인
위와 같은 api를 호출하되 이번엔 "search" 밑의 "qeury_total"과 "query_time_in_millis"의 차에 주목한다.

---

# ELK Stack

## filebeat
로그 수집 대상이 되는 서버컴퓨터에 설치해서 그 로그들을 logstash로 보내는 역할을 한다. 상황에 따라 logstash를 거치지 않고 filebeat단계에서부터 로그를 json형태로 가공할 수도 있다. <br>

## logstash
filebeat로 부터 수거된 로그를 jSON형태로 가공하여 ElasticSearch에 저장한다. filebeat.yml의 output.logstash값을 로드밸런서의 가상 ip로 설정해주면 각 logstash 서버별로 로드밸런싱도 가능하며 안정성도 높일 수 있다.<br>

## elasticsearch
logstash로 부터 받은 jSON들의 저장소. 클러스터 형태로 구성되어 안정성이 높다.<br>

## kibana
logstash의 조회 및 검색 및 분석내용들을 그래피컬하게 시각화 해주는 프로그램. 보통 동일한 ElasticSearch를 바라보는 두대의 Kibana서버를 두어 한 대에 장애가 발생했을 경우를 대비한다. <br>

