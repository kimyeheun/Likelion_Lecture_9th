# User 확장과 인증

1. A와 B가 서버에 요청을 보냄. 
2. 서버가 사용자에 따라 다른 정보를 보냄
3. 이때 다른 사용자에게 다른 정보를 제공하기 위해 user table이 필요함. => Django에서 지원

-> 장고에서 제공하는 클래스를 임의로 대체하여 사용할 수 있음



Authentication : 인증

1. 클라이언트가 서버에 회원가입 정보와 함께 가입요청
2. 서버에서 user table에 저장함
3. 클라이언트가 로그인 요청(token)을 함.  request(user)
4. table에 정보가 있는지 확인하고(authenticate) , 맞으면 적절한 응답을 해줌(login). 
5. 로그아웃 요청을 함(logout).



Paginator : 블로그 객체를 잘라서 보내준다. 





