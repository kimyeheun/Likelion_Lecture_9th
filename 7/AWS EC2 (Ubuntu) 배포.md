# AWS EC2 (Ubuntu) 배포

1. EC2 인스턴스 만들기

   - https://console.aws.amazon.com/

   - 우측 상단 지역이 Seoul인지 확인

   - EC2 검색

   - Instances > Launch Instances

   - Ubuntu Server 20.04 LTS (64-bit x86)

   - t2.micro (Free tier eligible)

   - Storage 설정 (Free tier 은 30GB까지)

   - Security Group > Add Rule

     - HTTP
     - HTTPS
     - Custom -> TCP -> 8000 - > 0.0.0.0/0, ::/0

   - Review and Launch > Launch

     - Create a new key pair
     - 이름은 원하는 이름 설정
     - Dowload Key Pair
     - Lanch Instance

   - Elastic IP 받기

     - Network & Security > Elastic IPs
     - Allocate Elastic IP address
     - Allocate
     - Associate Elastic IP adress
     - Instance > 아까 생성된 인스턴스 선택
     - Associate

   -  SSH 연결하기

     - ```ssh ubuntu@아까_받은_Elastic_IP -i 다운받은_PEM_FILE```

   -  필요한 패키지 설치

     - ```sudo apt update && sudo apt -y upgrade```
     - ```sudo apt install -y python3 python3-pip python-dev python3-venv build-essential libpq-dev vim git```
     - ```sudo reboot``` #업그레이드 도중 일부 시스템 파일이 변경되므로 재부팅

   - 패키지 설치하는 동안 settings.py 수정

     - 환경 변수 적용
       - Debug 는 아래 값 사용
         - ```DEBUG = (os.environ.get('DEBUG', 'True') != 'False' )```
       - ```ALLOWED_HOSTS = []``` 를 ```ALLOWED_HOSTS = ['*']```로 수정
     - ```pip freeze > requirements.txt```
     - ```git add -A```
     - ```git commit -m "edit for deployment"```

   - 우리가 매번 하던것들

     - ```python3 -m venv venv```
     - ```git clone 내_GitHub_레포지토리 django-app```
     - ```cd django-app```
     - ```source ../venv/Scripts/activate```
     - ```pip install -r requirements.txt```
     - ```python manage.py runserver 0.0.0.0:8000```
     - http://내_Elastic_IP:8000 로 접속해서 Django 화면이 표시되는지 확인
     - ```deactive``` #가상화면 밖으로 탈출

   - PostgreSQL 설치

     - ```sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'```
     - ```wegt --quiet -0 - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt -key add -```
     - ```sudo apt update```
     - ```sudo apt -y install postgresql```
     - ```sudo systemctl enable --now postgresql@13-main`` #시스템 시작시 자동 실행
     - ```sudo -u postgres createdb django``` #djgango 는 디버명 - 원하는 이름 입력
     - ```sudo -u postgres psql django``` #디비 접속
     - ```create user django with password 'p@ssword!':``` #원하는 username과 password 입력
     - ```alter role django set client_encoding to 'utf-8':``` #인코딩 수정 (한글깨짐 방지)
     - ```alter role django set timezone to 'Asia/Seoul':```#시간 수정
     - ```grant all privileges on database django to django:``` # django 데이터베이스의 모든 권한을 django 유저에게 부여

   - Settings.py 에 DB 설정

     - 내 컴퓨터에서 VSCode에서 settings.py 열기

     - DATABASES = 부분을 아래와 같이 수정

     - ```python 
       DATABASE = {
           'default' : {
               'ENGINE': 'django.db.backends.postgresql_psycopg2',
               'NAME': os.environ.get('DB_NAME'),
               'USER': os.environ.get('DB_USER'),
               'PASSWORD': os.environ.get('DB_PASSWORD'),
               'HOST': os.environ.get('DB_HOST'),
               'PORT': '',
           }
       }
       ```

     - ```git add -A```

     - ```git commit  -m "edit database"```

     - ```git push```

   ------------------------

   - 업데이트된 설정 서버에 가져오기 (서버에서 실행)

     - ```cd /home/ubuntu/django-app``` #git 레포 클론한 위치로 가기
     - ```git pull```
     - ```source ../venv/Scripts/activate```
     - ```pip install psycopg2``` #PostgreSQL 접속에 필요한 패키지 설치

   - 환경 변수 설정(임시/테스트 용)

     - ```export DB_NAME=django```
     - ```export DB_USER=django```
     - ```export DB_PASSWORD='p@ssword!'```
     - ```export DB_HOST=localhost```

   - 잘 작동하는지 테스트

     - ```python manage.py makemigrations```
     - ```python manage.py migrate```
     - ```python manage.py runserver 0.0.0.0:8000```
     - http://내_Elastic_IP:8000 로 접속해서 Django 화면이 표시되는지 확인

   - gunicorn 으로 Django App실행

     - ```pip install gunicorn``` (반드시 venv안에서 실행)
     - ```deactivate``` #일단 가상환경 탈출
     - ```../venv/Scripts/gunicorn 프로젝트명.wsgi -b 0.0.0.0```
     - http://내_Elastic_IP:8000 로 접속해서 Django 화면이 표시되는지 확인 (사진 등 static files는 표시 안되는게 정상)

   - systemd 에 gunicorn 등록

     - vi 는 에디터 (윈도우에서 메모장 정도의 느낌)

     - 파일을 연 후 ```i```키 또는 ```insert```키 누르면 내용 입력 가능

     - 수정한 사항을 저장하고 싶으면 ```Esc```->```:w```->```Enter```

     - 종료하고 싶으면 ```Esc```->```:q```->```Enter```

     - 합하면 ```Esc```->```:wq```->```Enter``` 로 저장&종료

     - 저장하지 않고 강제로 종료하려면 ```Esc```->```:q!```->```Enter``` (!는 강제)

     - ```sudo vi /etc/systemd/system/gunicorn.service```

     - ```python text
       [Unit]
       Description=gunicorn deamon
       Requires=gunicorn.socket
       After=network.target
       
       [Service]
       Type=notify
       RuntimeDirectory=gunicorn
       WorkingDirectory=/home/uduntu/django-app
       ExecStart=/home/ubuntu/venv/Scripts/gunicorn 프로젝트명.wsgi
       ExecReload=/Scripts/kill -s HUP $MAINPID
       KillMode=mixed
       TimeoutStopSec=5
       PrivateTmp=true
       EnvironmentFile=/etc/gunicorn/env.conf
       
       [Install]
       WantedBy=multi-user.target
       ```

     - ```sudo vi /etc/systemd/system/gunicorn.socket```

     - ```python text
       [Unit]
       Description=gunicorn socket
       
       [Socket]
       ListenStream=/run/gunicorn.sock
       SocketUser=www-data
       
       [Install]
       WantedBy=multi-user.target
       ```

     - sudo mkdir /etc/gunicorn

     - sudo vi /etc/gunicorn/env.conf

     - ```python
       DB_NAME=django
       DB_USER=django
       DB_PASSWORD='p@ssword!'
       DB_HOST=localhost
       #기타 환경변수 여기에 입력
       ```

     ------------------------

     1. 생성한 서비스를 실행

     - sudo systemctl deamon-reload
     - sudo systemctl enable --now gunicorn.socket
     - sudo systemctl enable --now gunicorn

     2. 테스트

     - curl --unix-socket /run/gunicorn.sock http
     - 실행시 HTML코드가 나오면 성공

     3. Nginx 설치
        - Nginx는 웹 서버로 외부 세상과 Gunicorn 을 연결하는 역할을 함

     - sudo apt -y install nginx

     - sudo vi /etc/nginx/sites-available/django_app

     - ```python
       server {
           listen 80:
           server_name 내_Elastic_IP_주소:
           
           location = /favicon.ico { access_log off: log_not_found off: }
           location /static/ {
               root /home/ubuntu/django-app:
           }
           location / {
               include proxy_params:
               proxy_pass http://unix:/run/gunicorn.sock:
           }                     
       }
       ```

       - ```sudo in -s /etc/nginx/sites-available/django_app /etc/nginx/sites-enabled```
       - ```sudo nginx -t```
       - ```sudo systemctl restart nginx```
       - http://내_Elastic_IP 접속시 Django 화면이 보이면 성공!
       - ```source ../venv/Scripts/activate```
       - ```python manage.py collectstatic``` #이미지나 css 등의 static 파일들 보임

--------------------------------

