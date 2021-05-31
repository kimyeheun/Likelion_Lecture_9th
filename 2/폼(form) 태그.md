# 폼(form) 태그

: 폼에 포함되는 다양한 입력 양식 태그들을 감싸 줌

+ action :데이터를 보낼 URL을 지정

+ method : 보내는 방식을 지정

  -> 브라우저에서 폼에 입력한 데이터를 서버로 보내는 기능. 

  - method="get"  : 데이터 조회만을 목적으로 할 때 주로 쓰임.

    -> 데이터를 URL  끝에 붙여 <ins>눈에 보이게</ins> 보냄

  - method="post"  :  서버에 있는 데이터를 쓰거나 수정, 삭제 할 때 주로 사용.

    -> 데이터를 메세지 <ins>내부(body)부분에 숨겨서</ins> 보냄

    (로그인, 회원가입, 게시물 작성)

<실습>

<h2>회원가입</h2>
<form action="my_app" method="get"> 
    <div>
    	<label for="userid"> 아이디 :</label>
    	<input type="text" id="userid" name="id" placeholder="아이디를 입력하세요." value="snow"> 
    </div>
    <div>
        <label for="userpassword"> 비밀번호 :</label>
    	<input type="password" password="userpassword" name="password" placeholder="비밀번호를 입력하세요."> 
    </div>
    <div>
        <label for="gender">성별 :</label>
        <select name="gender" id="gender">
            <option value="male">남성</option>
            <option value="male">여성</option>
        </select>
    </div>
    <div>
        <label for="job">직업 :</label>
        <select name="job" id="job">
            <option value="male">프로그래머</option>
            <option value="male">개발자</option></option>
        </select>
    </div>
    <div>
        <label for="introduce">자기소개 :</label>
        <textarea name="introduce" id="introduce">
        </textarea>
        </select>
    </div>
	<div>
        <button type="submit">제출</button>
        <button type="reset">리셋</button>
	</div>
</form>



----------------------------------

# input

: 사용자에게 입력을 받기위해 사용되는 태그  (빈 태그=종료태그 없음)



### placeholder="아이디를 입력하세요."

: input에 아무 값도 입력되지 않았을 때 나타나는 텍스트. 초기값이 아닌, 가이드 문구.

### value = "soonho"

: 실제 할당되는 값, 우리가 데이터를 넣으면 이 속성에 값이 들어감. 초기값처럼 둘 수 있음. 

### label<>

: 해당하는 라벨을 클릭 시 input 태그가 활성화 됨.

<input id="password">

<label for="password">비밀번호:</label>

### div<>

: 태그들을 구분 짓고 나구기 위해 사용되는 태그 (division의 약자)

----------------------------

# select

: 여러개의 선택지를 제시하고 싶을 때 씀

"<select name="gender">" : input 태그의 name 속성과 동일하게 작동

"<option value="male"> 남성 </option> : input 태그에서 입력한 값과 동일한 역할

-----------------------------

# textarea

:한번에 많은 글을 입력받을 때 사용. 

# button

: input 태그의 버튼타입과 동일하게 버튼을 생성

---------------------------------------------





