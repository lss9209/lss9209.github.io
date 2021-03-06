---
title: "ElasticSearch의 환경설정"
date: 2021-03-06 13:48:47 -0400
categories: ElasticSearch
---
# 엘라스틱 서치의 환경설정
---

## elasticsearch.yml 
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
이 외에도 노드의 역할주기또한 이 파일에서 할 수 있다.
</code>
</pre>
</div>

## JVM.Options 

<div style="background-color:#add8e6; border-radius: 25px;">
<pre>
<code>
## JVM configuration

################################################################
## IMPORTANT: JVM heap size
################################################################
##
## You should always set the min and max JVM heap
## size to the same value. For example, to set
## the heap to 4 GB, set:
##
## -Xms4g
## -Xmx4g
##
## See https://www.elastic.co/guide/en/elasticsearch/reference/current/heap-size.html
## for more information
##
################################################################

# Xms represents the initial size of total heap space
# Xmx represents the maximum size of total heap space

-Xms1g #1 JVM에서 사용할 힙메모리 크기를 부여하는 옵션 이값만큼 최소로 확보했다가 필요에 따라 아래의 #2에 설정한 만큼 힙메모리가 늘어난다. 그러나 동적으로 늘리는 과정에서 성능이 낮아질 수 있으므로 두 값을 같게 설정하는 것이 권고된다.
-Xmx1g #2

################################################################
## Expert settings
################################################################
##
## All settings below this section are considered
## expert settings. Don't tamper with them unless
## you understand what you are doing
##
################################################################

## GC configuration
8-13:-XX:+UseConcMarkSweepGC #3 GC방식을 설정하는 변수, 웬만하면 기본값에서 바꾸지 않는다.
8-13:-XX:CMSInitiatingOccupancyFraction=75 #4 힙메모리의 어느정도가 차면 old GC를 실행할 것인지 결정한다. 낮게 설정하면 자주 old GC가 발생하고 높게 하면 빈도는 낮으나 한번의 old GC의 수행시간이 증가한다. 참고로 old GC 수행중에는 stop-the-world가 발생하여 클러스터의 동작이 멈춰 서비스 진행이 불가해진다.
8-13:-XX:+UseCMSInitiatingOccupancyOnly #5 4의 설정만을 기준으로 old GC를 수행하겠다는 설정, 이 값을 설정안하면 #4의 기준외의 별도의 기준으로 old GC가 동작한다.

## G1GC Configuration #6 디폴트 GC인 위의 CMS GC가 아닌 다른 GC를 쓰려고 할때 하는 설정인데 다양한 이슈가 발생될 수 있으므로 전문적인 테스트 하에 이 설정으로 변경을 결정하여야 하며 웬만하면 디폴트 설정인 위의 CMS GC를 쓰는 것이 권장된다.
# NOTE: G1 GC is only supported on JDK version 10 or later
# to use G1GC, uncomment the next two lines and update the version on the
# following three lines to your version of the JDK
# 10-13:-XX:-UseConcMarkSweepGC
# 10-13:-XX:-UseCMSInitiatingOccupancyOnly
14-:-XX:+UseG1GC
14-:-XX:G1ReservePercent=25
14-:-XX:InitiatingHeapOccupancyPercent=30

## JVM temporary directory
-Djava.io.tmpdir=${ES_TMPDIR}

## heap dumps

# generate a heap dump when an allocation from the Java heap fails
# heap dumps are created in the working directory of the JVM
-XX:+HeapDumpOnOutOfMemoryError

# specify an alternative path for heap dumps; ensure the directory exists and
# has sufficient space
-XX:HeapDumpPath=data
</code>
</pre>
</div>

