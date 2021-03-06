---
title: "ElasticSearch의 환경설정"
date: 2021-03-06 13:48:47 -0400
categories: ElasticSearch
---
# 엘라스틱 서치의 환경설정
---

<div style="background-color:#add8e6; border-radius: 25px;">
<pre>
<code>
# ======================== Elasticsearch Configuration =========================
#
# NOTE: Elasticsearch comes with reasonable defaults for most settings.
#       Before you set out to tweak and tune the configuration, make sure you
#       understand what are you trying to accomplish and the consequences.
#
# The primary way of configuring a node is via this file. This template lists
# the most important settings you may want to configure for a production cluster.
#
# Please consult the documentation for further information on configuration options:
# https://www.elastic.co/guide/en/elasticsearch/reference/index.html
#
# ---------------------------------- Cluster -----------------------------------
#
# Use a descriptive name for your cluster: #1 클러스터의 이름 이것이 같은 노드들은 같은 클러스터에 속한다!
#
cluster.name: elasticsearch_brew
#
# ------------------------------------ Node ------------------------------------
#
# Use a descriptive name for the node:
#
#node.name: node-1 #2 노드네임 노드네임은 클러스터내에서 유일해야 하며 ${HOSTNAME}을 쓰면 호스트명이 대입되어 자동으로 다른노드와 겹치지 않게 된다.
#
# Add custom attributes to the node:
#
#node.attr.rack: r1 #3 사용자 정의된 rack값을 등록해서 각 노드를 커스터마이징 할 수 있는 항목.
#
# ----------------------------------- Paths ------------------------------------
#
# Path to directory where to store the data (separate multiple locations by comma):
#
path.data: /usr/local/var/lib/elasticsearch/ #4 노드가 가진 문서들을 저장할 경로
#
# Path to log files:
#
path.logs: /usr/local/var/log/elasticsearch/ #5 엘라스틱서치의 어플리케이션 로그들을 저장할 경로
#
# ----------------------------------- Memory -----------------------------------
#
# Lock the memory on startup:
#
#bootstrap.memory_lock: true #6 스왑메모리 사용하지 않겠다는 설정 이 설정을 On할시 성능이 향상되나 메모리 부족 에러를 일으킬 가능성이 증가한다.
#
# Make sure that the heap size is set to about half the memory available
# on the system and that the owner of the process is allowed to use this
# limit.
#
# Elasticsearch performs poorly when the system is swapping the memory.
#
# ---------------------------------- Network -----------------------------------
#
# Set the bind address to a specific IP (IPv4 or IPv6):
#
#network.host: 192.168.0.1 #7 EL이 사용할 IP주소로 0,0,0,0으로 설정시 서버에서 사용하는 IP와 로컬에서만 사용가능한 127.0.0.1이 동시에 적용되는 효과가 있어서 권장된다. 이 경우 서버 내외부에서 모두 접속이 가능하다.
#
# Set a custom port for HTTP:
#
#http.port: 9200 #8 EL이 사용할 포트번호
#
# For more information, consult the network module documentation.
#
# --------------------------------- Discovery ----------------------------------
#
# Pass an initial list of hosts to perform discovery when this node is started:
# The default list of hosts is ["127.0.0.1", "[::1]"]
#
#discovery.seed_hosts: ["host1", "host2"] #9 클러스터링을 위한 다른노드들의 정보를 나열한다. 특정 클러스터에 속한 노드들 중 하나 이상을 나열하면 그 노드가 속한 클러스터에 클러스터링 된다. 보통 변경이 잦지 않은 마스터 노드를 기입할 것이 권장된다.
#
# Bootstrap the cluster using an initial set of master-eligible nodes:
#
#cluster.initial_master_nodes: ["node-1", "node-2"] #10 클러스터 구축을 위한 최소 마스터노드 갯수, SplitBrain현상을 막기 위해 전체 마스터노드 갯수의 과반수로 구성한다.
#
# For more information, consult the discovery and cluster formation module documentation.
#
# ---------------------------------- Gateway -----------------------------------
#
# Block initial recovery after a full cluster restart until N nodes are started:
#
#gateway.recover_after_nodes: 3 #11 클러스터 내 노드를 전부 재시작 할 때 최소 몇개의 노드가 정상적인 상태일 때 복구를 시작할 지 결정하는 변수
#
# For more information, consult the gateway module documentation.
#
# ---------------------------------- Various -----------------------------------
#
# Require explicit names when deleting indices:
#
#action.destructive_requires_name: true #12 인덱스를 와일드카드 표현식의나 _all로 삭제할 수 없도록 막아놓은 설정 이를 통해 실수로 인한 데이터 유실을 막을 수 있다.
</code>
</pre>
</div>

