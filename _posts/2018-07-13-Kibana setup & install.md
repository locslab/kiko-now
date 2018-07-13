---
layout: post
title: "Kibana setup & install"
tags: [Kibana]
comments: true
---

## **1. DEB 파일 다운로드**

아래 링크에서 DEB 파일을 먼저 다운로드 합니다.

<https://www.elastic.co/downloads/kibana>

## **2. Install Kibana**

키바나를 설치하기 위해 아래 명령어를 입력합니다.
다운로드한 DEB 파일의 버전에 따라서 명령어가 다를수 있습니다.

```
sudo dpkg -i kibana-5.0.2-amd64.deb
```

## **3. 테스트를 위한 설정**

간단한 테스트를 위하여 설정 파일을 아래 명령어로 실행합니다.
경로가 다를수 있습니다.

```
vi /etc/kibana/kibana.yml
```

파일 내용에서 아래 2가지를 아래와 동일하게 수정합니다.
```
#elasticsearch.url: “http://localhost:9200”
#server.host: “localhost”
```

위 내용은 로컬에서 테스트를 위한 설정입니다.

## **4. 실행 명령어**

키바나를 실행하기 위하여 아래 명령어를 입력합니다.
경로가 다를수 있습니다.

```
sudo /usr/share/kibana/bin/kibana
```

## **5. Kibana 실행 확인**

실행 명령어까지 수행을 했다면, 브라우저에 접속하여 아래 주소로 접속합니다.

```
http://localhost:5601/
```

기본적으로 5601번 포트를 사용합니다.
정상적으로 Kibana 페이지가 출력이 된다면 정상적으로 설정이 완료되었습니다. 
 
  
  *Kibana setup & Install에 대해서 알아보았고, 다음 포스팅은 Kibana management에 대해서 포스팅입니다.*

