# Heroku 배포하기

1. Heroku 회원가입

   - https://www.heroku.com

2.  Heroku CLI 설치

   - https://devcenter.heroku.com/articles/heroku-cli

3.  환경변수 적용

   - Debug 는 아래 값 사용.
     +  ```DEBUG = (os.environ.get('DEBUG', 'True') != 'False' ) ```

4.  .gitignore 파일 적용

   - gitignore.io 에서  Django 선택(입력) 후 ' 생성' 클릭
   - 페이지에 나온 텍스트를 모두 복사 후 .gitignore 파일로 저장

5.  Heroku 용 파일 작성

   - Procfile 이라는 파일을 만들어 아래 내용 작성
     - ``` web: gunicorn 프로젝트명.wsgi --log-file -```
   -  runtime.txt 파일에 아래 내용을 작성
     - python-3.9.1 (파이썬 현재 버전)

6.  필요한 Dependency 설치

   - ``` pip install gunicorn whitenoise dj-database-url psycopg2-binary```

7.  setting.py 수정

   - Whitenoise 설치

     - MIDDLEWARE 에서 SecurityMiddleware 바로 아래 내용 추가

       + ```'whitenoise.middleware.WhiteNoiseMiddleware'```

     - ALLOWED_HOSTS 수정

       - ```ALLOWED_HOSTS = []``` 를 ```ALLOWED_HOSTS = ['*']```로 수정

     - DB 관련 코드 수정 

       - settings.py 제일 밑에 아래 내용 추가

       - ```python
         import dj_database_url
         db_from_env = dj_database_url.config(conn_max_age=500)
         DATABASE['default'].update(db_from_env)
         ```

     - requirements.txt 생성

       - ```pip freeze > requirements.txt```

     - git에 수정된 파일들 추가

       - ```git add -A```
       - ```git commit -m "add files for deploying to heroku"```

     - Heroku 관련 명령어들 실행

       - ```heroku login ```
       - ```heroku create```
       - ```git push heroku main ```
       - ```heroku run python manage.py migrate```
       - ```heroku run python manage.py createsuperuser```
       - ```heroku open```

     -  Heroku 에서 환경 변수 설정

       - https://dashboard.heroku.com에서 앱 선택
       - Settings
       - Config Vars > Reveal Config Vars
       - KEY VALUE 값에 입력 후 저장

-----------------------

