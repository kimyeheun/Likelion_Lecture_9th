# CSS (Cascading Style Sheets)

### = Style Sheets

예시

p {

​	front-family:  '맑은 고딕';

​	front-size: 18px;

​	color: blue;

}

* p: **선택자(Selector)** : 스타일을 적용하고자 하는 HTML 요소를 선택하는 역할

* front-size, color : **속성(Property)** : 지정할 스타일의 속성명에 해당 속성: 값;이 한 단위 ;(세미콜론)을 이용하여 구분
* 18px, blue : **값(Value)** : 키워드나 특정 단위를 이용하여 원하는 스타일을 적용 속성(property)과 쌍을 이룸
* {} : **선언 블록(Declaration block)**
* 속성+값 : **선언(Declaration)** , 선언은 ;(세미콜론)으로 구분, 한 줄로 써도 띄어쓰기와 ; 만 주의하면 됨

-------------------

# HTML에 CSS를 적용하는 방법

css는 html과 무조건 같이 쓰여야 한다. 

+ Link style : HTML 외부에 있는 CSS파일을 불러옴

<link rel="stlysheet" href="문서 url">

"문서 url" =>  h1{color: red;}

+ Embedding style : HTML의 <head>에 <style>를 이용하여 CSS를 작성

<head>
    <style>
        h1 {color: red;}
    </style>
</head>

+ lnline style : HTML요소에 직접 style 속성(Attributes)을 이용하여 CSS를 작성

<h! style="color: red;"> </h1>