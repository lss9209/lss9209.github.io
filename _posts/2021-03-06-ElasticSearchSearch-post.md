---
title: "ElasticSearch의 개념"
date: 2021-03-13 10:00:47 -0400
categories: ElasticSearch
---
# 엘라스틱서치에서 검색 활용하기
---

## 역 색인

![Alt text](https://gblobscdn.gitbook.com/assets%2F-Ln04DaYZaDjdiR_ZsKo%2F-LntL_BGpuFbNXy_sFtK%2F-LntLbibpXHABupWvXtu%2F6.1-03.png?alt=media&token=d2726f20-a7ea-4219-bcb0-340cbe1d21f1)


위와 같이 ES는 여타 다른 RDBMS와 달리 역 색인 형태로 문서들을 색인한다. 그래서 일일히 한 로우씩 찾아야만 하는 RDBMS에 비해 ES는 한 로우로 그 term을 포함하는 문서들의 서치가 끝나기 때문에 ES가 RDBMS보다 성능이 빠르다.<br>
검색에는 analyzer가 사용된다. 이 analyzer는 인덱스별로 사용자 정의의 analyzer를 추가할 수도 있다.<br>
관련 사항은 https://esbook.kimjmin.net/06-text-analysis/6.3-analyzer-1/6.4-custom-analyzer 에 정리가 잘 되어 있다.<br>
기본적으로 anlyzer는 charecter filter - tokenizer - token fileter로 구분되는데 간단히만 해보자면 'Dogs and Cats are Friend Each Others'라는 문장은 캐릭터필터에서 대소문자 구분이 없어진 'dogs and<br>
cats are friend each others'로 바뀐다. 그리고 토크나이저에서 dogs, and, cats, are, friend, each, others라는 토큰들로 분류되어 역색인 될 준비를 합니다. 마지막으로 토큰필터에서는 검색에 의미가<br>
없는 'and'와 'are'를 제거하는 한편 형태소 처리로 복수형 명사는 대표 단수형 명사로 처리되게 하여(한글은 'Nori'가 이 역할을 하고 영어는 'Snowball'이 이 역할을 함 최종적으로 'dog', 'cat', 'friend',<br>
'each' 'others'가 역색인이 된다. 물론 이는 한 사례일 뿐 다양한 커스터마이징 설정을 통해 공백이 아닌 기준으로 토큰을 나눌 수도 있고 토큰필터 및 캐릭터 필터에 더 다양한 적용사항을 추가할 수도 있다.<br>
<br>
Query질의와 Filter질의의 가장 큰 차이점은 Query는 얼마나 문서가 조건에 부합하는지에 대한 스코어를 매긴다. 다만 Filter는 오로지 일치/불일치 여부만 판단하므로 score처리가 없어 빠르다. 그리고 Filter의 term쿼리는 <br>
anylize과정이 없다. 이 점이 query의 match와 뚜렷이 구분되는 점이다. 가령 query의 match로 질의를 하면 Mice는 mice -> mouse로 변환되어서 검색된다. 그러나 filter의 term으로 질의하면 mice 그대로 질의가 된다.<br>

여러가지 검색질의문들을 활용하는 방안은 https://esbook.kimjmin.net/05-search에 설명이 아주 잘되어있으니 참고하면 좋겠다.

