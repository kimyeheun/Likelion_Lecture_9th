# Docker 이미지 자동 생성

##### 준비사항

1. Docker로 배포하기가 끝난 레포
2. Github 계정

#####세팅

1. 깃헙의 내 레포로 이동하기

2. Actions 버튼 선택

3. set up a workflow yourself -> 버튼 클릭하기

4. 이름을 docker-publish.yml 로 수정

5. 아래의 코드 작성

   ``` python
   name: Docker Publish
   
   on:
    push:
      branches: [main]
    workflow_dispatch:
   jobs:
     build:
       runs-on:ubuntu-latest
       steps:
         -uses: actions/checkout@v2
         -name: Docker Build
          run: docker build -t ${{ secrets.DOCKER_USERNAME}}/my-app .
         -name: Docker Push
          run: |
           docker login -u ${{ secrets.DOCKER_USERNAME}} -p ${{ secrets.DOCKER_PASSWORD}}
           docker push ${{ secrets.DOCKER_USERNAME}}/my-app 
   ```

6. start commit 누른 후 저장

7. Settings 에서 Secrets 로 이동해서 `DOCKER_USERNAME` 이랑 `DOCKER_PASSWORD` 등록하기

8. 다시 Actions로 가서 Re-run jobs 실행

