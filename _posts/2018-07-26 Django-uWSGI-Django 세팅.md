---
layout: post
title: "Django-uWSGI-Django 세팅"
tags: [Django]
comments: true
---

## Django 설치 및 준비
### 1. 파이썬 및 가상환경 설치

```bash
sudo apt-get update
sudo apt-get install python3-pip
sudo pip3 isntall virtualenv
```

### 2. 파일 디렉토리 생성

```bash
mkdir PNU_voting_system
```

### 3. 가상환경 세팅

```bash
cd PNU_voting_system
virtualenv venv
source /venv/bin/activate	=> python 가상환경 활성화
```

### 4. python 라이브러리 설치

```bash
pip3 install django==2.0.7      # django 2.0.7 install
pip3 install uwsgi==2.0.17      # uwsgi 2.0.17 install
```

### 5. django project 생성

```bash
django-admin.py startproject conf
```

### 6. test.py 생성

```python
def application(env, start_response):
    start_response('200OK', [('Content-Type','text/html')])
    return [b"Hello World"]
```

---

## uWSGI 세팅

**uWSGI**란 Web Server Gateway Interface의 약자로, 웹서버(Nginx)에서 받은 요청이 서버 쪽에서 처리가 필요한 경우, uWsgi가 서버사이드 애플리케이션을 실행하여 담당하게 된다. 

**즉 웹서버와 웹 어플리케이션이 어떤 방식으로 통신하는가에 관한 인터페이스를 의미한다.**  웹서버와 웹어플리케이션 간의 소통을 정의해 어플리케이션과 서버가 독립적으로 운영될 수 있게 돕는다.

WSGI 어플리케이션은 uWSGI라는 컨테이너에 담아 어플리케이션을 실행하게 되며, uWSGI가 각각의 웹서버와 소통하도록 설정하면 끝이다. Flask, django와 같은 프레임워크는 이미 WSGI 표준을 따르고 있기 때문에 바로 웹서버에 연결해 사용할 수 있다.

###  1. uWSGI 실행 확인

```bash
uwsgi --http :8000 --wsgi-file test.py
```

- http://127.0.0.1:8000 실행창이 hello world 가 나오는 것을 확인
- **Web Client <-> uWSGI <-> Python** 구조로 통신

### 2. django 실행 확인

```bash
python manage.py runserver 0.0.0.0:8000
```

- 위의 명령어를 사용하여 django가 제대로 작동되는지 확인

```bash
cd PNU_voting_system/conf
uwsgi --http :8000 --module conf.wsgi
```

- uWSGI를 통해 django가 실행되는지 웹 상에서 확인

---

##  Nginx 세팅

### 1. nginx 설치
```bash
sudo apt-get install nginx      # nginx install
```

### 2. nginx.conf 파일 설정

```bash
cd PNU_voting_system
vim PNU_voting_system_nginx.conf
```

- `vim PNU_voting_system/PNU_voting_system_ngnix.conf` 파일을 만들어서 nginx 설정파일을 만들어준다

```vim
# vim PNU_voting_system_nginx.conf

upstream django {
        server unix:///tmp/PNU_voting_system.sock; #2번	(소켓)
        # server 127.0.0.1:8001;		   #1번	(포트)
}

server {
        # the port your site will be served on
        listen 80;
        server_name www.도메인.com;
        charset utf-8;
        client_max_body_size 75M;

        location /media {
                alias /home/locs/Workspace/PNU_voting_system/conf/media;
        }

        location /static {
                alias /home/locs/Workspace/PNU_voting_system/conf/static;
        }

        location / {
                uwsgi_pass django;
                include /home/locs/Workspace/PNU_voting_system/conf/uwsgi_params;
                # include /etc/nginx/uwsgi_params;
        }
}
```

- 위 설정을 통해 nginx로 들어오는 요청들을 uWSGI로 보내고 또 돌려받아 클라이언트로 전달을한다
- **Web Client <-> nginx <-> uWSGI <-> django** 구조로 통신

### 3. uwsgi_parms

```bash
cd PNU_voting_system/conf
vim uwsgi_params
```

- nginx에서 uwsgi를 사용하게 해주기위해 `vim PNU_voting_system/conf/uwsgi_param`에 아래와 같이 입력 

```vim
# vim uwsgi_params

uwsgi_param  QUERY_STRING       $query_string;
uwsgi_param  REQUEST_METHOD     $request_method;
uwsgi_param  CONTENT_TYPE       $content_type;
uwsgi_param  CONTENT_LENGTH     $content_length;

uwsgi_param  REQUEST_URI        $request_uri;
uwsgi_param  PATH_INFO          $document_uri;
uwsgi_param  DOCUMENT_ROOT      $document_root;
uwsgi_param  SERVER_PROTOCOL    $server_protocol;
uwsgi_param  REQUEST_SCHEME     $scheme;
uwsgi_param  HTTPS              $https if_not_empty;

uwsgi_param  REMOTE_ADDR        $remote_addr;
uwsgi_param  REMOTE_PORT        $remote_port;
uwsgi_param  SERVER_PORT        $server_port;
uwsgi_param  SERVER_NAME        $server_name;
```

### 4. ngnix에 django 사이트 등록

```bash
cd /etc/nginx/sites-enabled
sudo ln -s /home/locs/Workspace/PNU_voting_system/PNU_voting_system_nginx.conf /etc/nginx/sites-enabled/
```

- `cd /etc/nginx/sites-enabled`안에는 연결하고 자 하는 서비스에 대한 사이트 정보를 정의해야됨
- 심볼릭 링크를 통해 `PNU_voting_system/PNU_voting_system_ngnix.conf`를 `cd /etc/nginx/sites-enabled`에 연결

### 5. settings.py 파일 수정

```vim
STATIC_ROOT = os.path.join(BASE_DIR, "static/") # 맨 아래에 추가
```

```vim
DEBUG	= False					# 디버깅 모드 끄기
```

```vim
ALLOWED_HOSTS = ['*']				# 접속허가 호스트 ALL
```

- `PNU_voting_system/conf/settings.py` 파일을 통해 django의 설정값을 바꿔줄 수 있음
- settings.py 데이터 안에 위와 같이 수정

### 6 static folder 생성

```bash
python manage.py collectstatic
```

---

## Nginx-uWSGI-Django 연동

### 1. 연동 Test

```bash
uwsgi --socket :8001 --wsgi-file test.py
```

- 앞서 만들어 놓은 test.py로 nginx와 uWSGI 연동을 확인하는 명령어
- 웹에서 8000포트로 확인하면 됨
- 8000포트를 접속하면 작성한 PNU_voting_system.conf 파일 내용과 같이 소켓을 통해 8001을 호출
- **the web client <-> the web server <-> the socket <-> uWSGI <-> python**

### 2. Unix socket을 사용한 uWSGI

- 1번에서 uWSGI를 사용한 방법은 TCP port socket을 사용한 방법 이방법을 사용하는 이유는 간단하게 세팅 가능
- 하지만 포트를 사용하는 것보다는 유닉스 소켓을 사용하는것이 오버헤드를 잘 건딜수 있음

```bash
server unix:///tmp/PNU_voting_system.sock;	=> 2번	(소켓)
```

- `PNU_voting_system/PNU_voting_system_ngnix.conf` 내용을 위와 같이 변경

```bash
uwsgi --socket /tmp/PNU_voting_system.sock --wsgi-file test.py --chmod-socket=666
```

- 위 명령어를 실행
- **the web client <-> the web server <-> the unix socket <-> uWSGI <-> python** 이 구조로 실행되는 것
- 만약 에러코드가 나면  `/var/log/nginx/error.log`에서 확인

### 3. Unix Socket을 사용해 Django 실행

```bash
uwsgi --socket /tmp/PNU_voting_system.sock --module conf.wsgi --chmod-socket=666
```

### 4. .ini파일을 사용하여 uWSGI 실행

```bash
cd PNU_voting_system/conf
vim PNU_voting_system.ini
```

```vim
# PNU_voting_system.ini file
[uwsgi]
# Django-related settings
# the base directory (full path)
chdir   =       /home/locs/Workspace/PNU_voting_system/conf
# Django wsgi file
module  =       conf.wsgi
# the virtualenv (full path)
home    =       /home/locs/Workspace/PNU_voting_system/venv
# process-related settings
# master
master  =       true
# maximum number of worker processes
processes       =       5
socket  =       /tmp/PNU_voting_system.sock
# ... with appropriate premissions - may be needed
chmod-socket    =       666
# clear environment on exit
vacuum  =       true
```

- .ini파일을 설정하여 사용하는 이유는 uWSGI를 실행시키기 위해 공통적으로 사용되는 옵션들을 정리하여 보다 쉽게 구성하기 위함

### 3. 최종 연동

```bash
uwsgi --ini PNU_voting_system.ini
uwsgi --ini PNU_voting_system.ini &             # 백그라운드 실행
```

- 위 명령어를 실행후 웹상에서 잘 동작하는지 확인

---

## ※ 참고자료

- http://brownbears.tistory.com/16
- https://www.haruair.com/blog/1900
- https://mango-tree.github.io/2017/03/27/uWSGI-%EC%99%80-Python3%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC-Nginx%EB%A1%9C-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0/
