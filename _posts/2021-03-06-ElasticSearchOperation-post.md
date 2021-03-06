---
title: "ElasticSearch 운영하기"
date: 2021-03-06 13:44:47 -0400
categories: ElasticSearch
---
# 엘라스틱 서치 운영하기
---
## 엘라스틱 서치 버전 업그레이드

엘라스틱 서치는 두가지 방식으로 업그레이드 할 수 있다.<br>
1. Full cluster restart : 모든 노드를 동시에 재시작, 서비스가 중단되지만 빠르게 버전업그레이드 가능. <br>
2. Rolling Restart: 노드를 순차적으로 한 대씩 재시작하여 전체 업그레이드 시간은 오래 걸리지만 서비스 중단 없이 버전 업을 하는 방법. <br>

## 샤드 배치방식 변경

샤드 배치방식을 변경하은 API들은 아래와 같은 것들이 있다.<br>
1. reroute: 샤드 하나하나를 특정 노드에 수동적으로 배치할 때 사용(자동 배치기능을 어기는 강제성이 있으므로 자동 배치에 의한 최적화가 깨질 수도 있다.) <br>
2. allocation : 샤드 배치방식을 클러스터 전체적으로 변경할 때 사용. <br>
3. rebalance: 샤드 재분배방식을 클러스터 전체적으로 변경할 때 사용. <br>
4. filtering : 특정 조건에 해댱하는 샤드들을 특정 노드에 배치할 떄 사용. <br><br>

이때 2.allocation과 3.rebalance의 방식은 아래와 같은 것들이 있다. <br>
all: 프라이머리랑 레플리카 샤드 모두 배치 허용<br>
primaries: 프라이머리 샤드만 배치 허용<br>
new_primaries: 새롭게 생성되는 인덱스에 한해 프라이머리 샤드만 배치 허용<br>
none:모든 샤드의 배치작업 비활성화<br><br>

그리고 샤드 재배치의 기준은 아래와 같은 옵션을 통해 줄 수 있다.<br>
cluster.routing.allocation.disk.watermark.low: 이 임계치를 넘어간 노드에는 생성되어있는 노드들의 할당를 허용하지 않음. 단, 새로 생성된 노드의 재배치는 허용함.<br>
cluster.routing.allocation.disk.watermark.high: 이 임계치를 넘어간 노드에는 모든(새로 생성된 노드 포함) 샤드를 할당하지 않음.<br>
cluster.routing.allocation.disk.watermark.flood_stage: 이 임계치를 넘어간 노드에 존재하는 샤드를 포함하는 인덱스들은 모두 read only처리에서 추가 색인이 아예 불가능하게 막음.<br>

## 인덱스 API

1.open/close : 인덱스를 색인과 검색이 가능한 상태로 만들고 그렇지 않게 만드는 api <br>
2.aliases: 인덱스들에 별칭 부여, 서로 다른 각기 인덱스에 하나의 별칭을 부여해 하나의 별칭에 대한 api호출로 여러 인덱스에 영향을 줄 수 있다. <br>
3.rollover: 분기하는 api, 인덱스가 특정 조건을 만족하면 새로운 인덱스를 만들고 거기다 데이터를 모두 옮기고 앞으로 그 인덱스로 요청을 받게끔 한다. <br>
4.refresh: 메모리 버퍼캐시를 강제로 비워서 세그먼트로 저장하는 api <br>
5.forcemerge: 세그먼트들을 강제로 병합하는 api <br>
6.reindex: 한 인덱스를 이를 테면 그 인덱스의 analyzer변경을 위해 다른 인덱스로 마이그레이션 해야 할때 쓰이는 api<br><br>
