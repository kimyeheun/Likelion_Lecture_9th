# Docker 서버에 배포하기

#####준비사항

- EC2 인스턴스(AWS EC2 참고)
- DockerHub 에 등록된 이미지

#####서버 세팅

1. Docker 설치

   - `curl -fsSL https://get.docker.com | bash`

2. Docker Compose(Docker 플러그인) 설치

   - ```python
     sudo curl -L "https://github.com/docker/compose/releases/download/1.28.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose 
     # docker-compose 바이너리 다운로드
     
     sudo chmod +x /usr/local/bin/docker-compose 
     # docker-compose 바이너리 실행 권한 부여
     ```

3. 앱 설치 폴더 지정

   - ```sudo mkdir /opt/django-app```
   - ```cd /opt/django-app```

4. docker-compose.yml 생성

   - ```vi docker-compose.yml```

     - ```YAML
       version:'3.9'
       
       services:
         db:
           image: postgres
           environment:
             -POSTGRES_DB=${DB_NAME}
             -POSTGRES_USER=${DB_USER}
             -POSTGRES_PASSWORD=${DB_PASSWORD}
         web:
           image: DockerHubld/django-app
           environment:
             -DB_NAME=${DB_NAME}
             -DB_USER=${DB_USER}
             -DB_PASSWORD=${DB_PASSWORD}
             -DB_HOST=db
             -DEBUG=${DEBUG}
           ports:
             -"8000:8000"
           depends_on:
           -db
       ```

   - DockerHubId 수정
   - 그대로 복붙시 띄어쓰기 문제가 발생할 수 있음
   - [pastebin.com](https://www.pastebin.com/) 등의 서비스에 업로드 후 wget 사용
   - ```sudo wget https://pastebin.com/raw/[pasteId] -0 docker-compose.yml```

##### .env 파일 생성

1. ```sudo vi .env```

2. 아래 내용 입력

   1. ```
      DB_NAME=django
      DB_USER=django
      DB_PASSWORD=django
      DEBUG=False
      ```

##### 실행

1. DB 최초 실행(DB 생성 및 사용자 등록 등 진행)
   - `sudo docker-compose up db`

2. 잘 작동 되는지 테스트
   + `sudo docker-compose up`
   + 내 `IP:8000` 이 접속 되는지 확인

3. 백그라운드 모드로 전환(콘솔창 없이 실행 모드)
   + ```sudo docker-compose up -d```

