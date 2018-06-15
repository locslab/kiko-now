---
layout: post
title: "메트릭 어그리게이션(Metric Aggregation)"
tags: [Elastic Search]
comments: true
---

**Metric Aggregation이란?**


elastic search안에 있는 `document 안에서 조합을 통해서 어떠한 값을 도출할때 쓰이는 방법으로 그 중 metric aggregations는 산술할 때` 쓰입니다.

예를 들면 최댓값, 최솟값, 평균값 등을 구할때 이용합니다. 



![frozen Lake WorldS](../images/taehyun_image01.png)








**엘라스틱서치 매핑(Mapping)**


매핑은 관계형데이터베이스에서 스키마 개념과 동일합니다.

데이터 매핑을 지정하는 방법은 아래와 같습니다.


**1) 먼저 매핑을 지정할 인덱스를 하나 생성합니다.**

![frozen Lake WorldS](../images/taehyun_image02.png)


**2) mappingTest.json 파일로 매핑을 지정합니다.**

- mappingTest.json 파일 내용입니다.

![frozen Lake WorldS](../images/taehyun_image03.png)


- 매핑 지정 명령어입니다.

![frozen Lake WorldS](../images/taehyun_image04.png)









- 매핑된 결과 출력입니다.

![frozen Lake WorldS](../images/taehyun_image05.png)


**3) bulk(json형태로 한번에 입력)을 사용하여 해당 인덱스에 데이터를 입력합니다.**

![frozen Lake WorldS](../images/taehyun_image06.png)


- 데이터가 들어갔는지 확인합니다.

![frozen Lake WorldS](../images/taehyun_image07.png)

