### Docker로 배포하기

#####- 준비사항

1. Gitpod.io 계정 생성

   - Gitpod Free는 퍼블릭 레포만 작업 가능 / 월 50시간 제한
   - Gitpod Student는 프라이빗도 레포도 작업 가능 / 월 100시간 제한

2. Gitpod 설정 수정

   - Gitpod -> settings
     + Feature Preview > Enable Feature Preview 체크

   - Default IDE Theia 선택
     - Theia : Eclipse팀이 만든 IDE
     - Code : VSCode의 웹 버전 IDE (호환성 문제 있음)

3. Gitpod 인스턴스 생성

   1. 본인 Github 레포지토리로 이동
   2. 레포지토리 주소 앞에 `gitpod.io/#` 을 붙여서 이동

4. DockerHub 계정 생성

#####Requirements 설치

1. `pip install -r requirements.txt`

#####Whitenoise 설치

1. ```pip install whitenoise```
2. MIDDLEWARE 에서 SecurityMiddleware 바로 아래에 내용 추가
   - ```whitenoise.middleware.WhiteNoiseMiddleware```

#####Gunicorn 설치

1. `pip install gunicorn`

#####Dockerfile 생성

-> Docker 한테 뭘 해야하는지 알려주는 파일

##### run.sh 파일 생성

- python manage.py migrate

- python manage.py collectstatic

- gunicorn 프로젝트명.wsgi -b 0.0.0.0:8000

##### Docker 나머지 setting

1. ```pip freeze > requirements.txt``` #파일 생성
2. ```sudo docker -up```
3. ```Crtl + Shift + `  ``` 키로 새로운 터미널 열기
4. ```docker bulid -t 도커아이디/django-app .``` #도커 이미지 생성
5. ```docker run -it -p 8000:8000 도커아이디/django-app``` #생성된 도커 이미지 실행
6. ```docker login``` #Docker Hub에 로그인 (이미지 업로드용)
7. ```docker push 도커아이디/django-app``` #Docker Hub에 생성된 이미지 업로드
8. https://hub.docker.com/r/DockerHubld/django-app 에서 내 앱이 업로드 된 것을 확인

