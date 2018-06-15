---
layout: post
title: "메트릭 어그리게이션(Metric Aggregation)"
tags: [Elastic Search]
comments: true
---

**Metric Aggregation이란?**


`Document 안에서 조합을 통해 특정(산술) 값을 도출할때 쓰이는 방법`

ex) 최대값, 최소값, 평균값 등을 구할때 사용합니다. 


**샘플 데이터(json) 내용은 아래와 같습니다.**

![frozen Lake WorldS](../images/ELK_posts_image00.png)



**아래는 Aggregation의 포맷입니다.**

![frozen Lake WorldS](../images/ELK_posts_image01.png)



**1. 평균값을 구하는 aggregation 설정 파일(avg_points_aggs.json) 내용을 살펴보겠습니다.**


![frozen Lake WorldS](../images/ELK_posts_image02.png)


![frozen Lake WorldS](../images/ELK_posts_image03.png)


**2. 최대값을 구하는 aggregation 설정 파일(max_points_aggs.json) 입니다.**


![frozen Lake WorldS](../images/ELK_posts_image04.png)


![frozen Lake WorldS](../images/ELK_posts_image05.png)



**3. 최소값을 구하는 aggregation 설정 파일(min_points_aggs.json) 입니다.**


![frozen Lake WorldS](../images/ELK_posts_image06.png)


![frozen Lake WorldS](../images/ELK_posts_image07.png)


**4. 합계를 구하는 aggregation 설정 파일(sum_points_aggs.json) 입니다.**


![frozen Lake WorldS](../images/ELK_posts_image08.png)


![frozen Lake WorldS](../images/ELK_posts_image09.png)



**5. 다음은 위에서 수행한 모든 계산값들을 한번에 구하는 파일(stats_points_aggs.json) 입니다.**


![frozen Lake WorldS](../images/ELK_posts_image10.png)


![frozen Lake WorldS](../images/ELK_posts_image11.png)

