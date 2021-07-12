# Django 배포 준비

- 환경변수

시스템에 저장되어있는 변수 

보통 비밀키 등 유출되면 안되는 정보나 환경에 차이를 둘 때 사용(테스트/프로덕션 구별 등)

os.envrion 에서 dict 형식으로 불러올 수 있음

os.envrion.get('변수명','기본값') 으로 사용

- requriements

내 파이썬 (장고)앱을 실행하기 위해 우선 설치되어야 하는 패키지들(Django, Pillow 등)

패키지명==버전 으로 보통 requirements.txt 파일에 저장

pip freeze 명령어는 해당 환경에 설치된 모든 패키지를 보여줌

'>' 는 프로그램의 출력을 파일에 저장한다는 뜻

pip freeze > requirements.txt 로 생성

- IAM

Identity and Access Managemen 의 줄임말

IAM에서 계정을 만든 후 해당 계정 로그인 정보 (엑세스 키 & 시크릿 키)를 이용하여 AWS의 API활용

보안을 위해 원한을 최대한 보수적으로 잡음

- S3

Simple Storage Service 의 줄임말

AWS에서 제공하는 구글 드라이브 정도로 생각할 수 있음

최초 용량 지정 없이 사용한 만큼만 과금되므로 용량예측이 필요 없음

여러 서버에서 동시에 접속 가능 (부하 분산 유리)

----------------------------

1.  settings.py 

   SECRET_KEY = os.environ.get('변수명','기본값' ) 으로 수정

2.  터미널 창

   (pip freeze를 치면 현재 가상화면에 설치된 모든 패키지 표시됨)

   pip install boto3

   pip freeze > requirements.txt 입력

   -> requirements.txt 파일 생성

3.  AWS 

   aws.amazon.com  사이트 이동 (또는 aws console으로 검색) -> 로그인 

   1) 오른쪽 상단의 지역 확인하기 seoul로 바꿈

   2) 검색창에 IAM검색

   3) 왼쪽 바에 Access management의 Users 선택

   4) Add user (Access type == programmatic access)

   5) Set permission의 Attach existing policies directly 에서 AmazonS3FullAccess 선택

   6) 완료. Access Key ID는 이 페이지에서만 볼 수 있으니 다운로드 해 놓을 것!

4.  S3 연동

   1) 터미널 창에 pip install django-storages 입력하여 설치

   2) pip freeze > requirements.txt 입력하여 파일 생성(수정)

   3) 구글에 django-storages 으로 검색

   4) settings의 url을 settings.py 에 붙여넣기 (AWS_ACCESS_KEY_ID)

5.  AWS Bukets

   aws.amazon.com  사이트 이동

   create buket 선택

   buket 만들기 

   * AWS Region == seoul

   * Block all public access 해제 

6. setting.py

   AWS_SECRET_ACCESS_KEY

   AWS_STORAGE_BUKET_NAME  입력

---------------------------

