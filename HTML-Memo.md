HTML의 기초

홈페이지의 구조 

html (hyper text markup language - ooo.html 형태의 파일)
- 홈페이지의 구조를 표시해주는 언어

CSS (Cascading Style Sheets - ooo.css 형태의 파일)
- 홈페이지의 디자인을 도와주는 언어 

JS (Java Script - ooo.js 형태의 파일)
- 홈페이지의 동작을 도와주는 언어


HTML 기본 구조 

Tag(태그) : 
	-> <a></a 혹은 <br/> 형태로 구성되는 html의 구성 단위 
		- <a></a> 	: 내부의 내용 또는 다른 태그를 포함하는 경우  
		- <br/>		: 태그 자체로 역할이 끝나는 경우 

태그와 속성 : 
	- <a href="./url"></a> : 태그 내부에 들어 있는 설정
		- <a 설정이 들어가는 곳> 내용이 들어가는 곳 <a/>
		- 자주 쓰이는 태그들

			- 문서의 구조
			<!DOCTYPE html> 해당 태그 뒤로는 HTML 문서임을 알린다.(단일 태그 - 첫문장에만 쓰인다.)
			<html></html>	html의 시작과 끝을 알린다.
			<head></head>	문서의 기본 특성들이 들어있다.
			<title></title>	문서의 제목을 설정한다.
			<script/>,<script></script>   사용될 JS의 주소를 알려주거나 문서에 직접 작성할 경우.
			<link/>, <link></link> CSS 등의 외부 파일들의 주소를 알려준다.
			<style></style> CSS를 외부파일이 아니라 문서 내에 CSS를 사용할 경우
			<meta/>, <meta></meta>	메타 데이터를 설정한다.
			<body></body>	문서의 구조가 본격적으로 들어있는 곳이다.

			- 글자를 보여주기 위한 태그 
			<h1></h1> html 제목 형태로 글자를 보여준다. (숫자는 1부터 6까지 있다.)
			<p></p>	문단 형태로 글을 모여준다.
			<br/>	다음 태그는 다음 줄부터 시작을 한다.
			<!--내용 -->	주석이다.
			<b></b>	굵은 글자이다.

			- 입력과 출력
			<form></form>	포한되어있는 정보들을 묶기 위한 
			<input/>,<input></input>	입력을 받을 수 있는 태그 (글자 등..)
			<textarea/>,<textarea></textarea>	위 input은 한 줄만 받기 때문에 여러줄을 받을 때 사용
			<button/>,<button></button>	클릭이 되는 버튼
			<img/>, <img></img>	이미지를 넣기 위한 태그
			<a></a>	주소/위치 이동을 하기 위한 태그 (하이퍼 링크 - hyperlink)

			- 나열하기 
			<ul></ul>	무언가를 나열하기 위한 태그 (unordered list)
			<li></li>	나열할 내용이 들어있는 태그

			- 표의 작성
			<table></table>	테이블을 정의
			<th></th>	테이블의 헤더를 지정 
			<tr></tr>	테이블에서 가로열을 정의
			<td></td>	세로열 내에서 표의 단위를 정의
			<thead></thead>	헤더(th)들의 집함
			<tbody></tbody>	테이블의 내용이 들어있는 곳
			<tfoot></tfoot>	테이블의 아랫부분을 정의

			- 위치 지정 및 그룹화
			<div></div>	공간을 정의한다.
			<span></span>	공간을 정의하지만 주로 위보다는 작은 개념으로 쓰인다.
			<header></header>	문서의 헤더를 정의한다.
			<footer></footer>	문서의 밑 부분을 정의한다.

	- 자주쓰이는 속성들
		- id	태그의 고유명 -  중복은 불가능 하다
		  name	태그의 이름 - 중복은 가능하다.
		  class 태그가 속한 그룹 - 중복되는 태그에 권장
		  href  클릭을 할 경우 이동할 주소 또는 이동할 위치의 태그 ID를 지정해주는 부분  
		  src 	이미지나 비디오 등의 주소를 알려주는 속성 
		  style 태그의 디자인을 추가해주는 설정 CSS보다 적용 우선순위가 높다. 사용을 권장하지 않는다.
		  width 태그의 넓이, 대신 CSS의 사용을 권장한다.
		  heigth 태그의 높이, 대신 CSS의 사용을 권장한다.

태그가 태그를 포함할때 
	- 태그의 주어진 공간 내에 배치가 된다. (소속관계)


CSS의 기본 

문서의 대상을 지정
	- id 	: 앞에 #를 붙여서 구분한다.(#example)
	- class	: 앞에 .를 붙여서 구분한다.(.example)
	- tag 	: 태그는 추가로 붙지 않는다.

각 지정된 요소들 별 적용 우선 순위
	- id(적용 우선순위가 높다) > class > tag
	- 동일 설정이 있을 경우 적용 우선 순위가 높은쪽으로 적용이 된다.
		#example {
			background-color: red;
		}
소속된 태그를 지정하기
	- 여러 태그들에게 공통 속성 부여하기 
		table, thead,tbody ,tr, td{
			line-height: 1;
		}

	- 연달아서 소속 주소(태그 클래스 아이디)를 나열
		tbody tr{
			border-top: 1px #c8c8c8 solid;
		}
	- 짝수 태그 지정하기 (<> odd)
		tbody tr:nth-child(even) {
			background-color: #f8f8f8;	
		}
	- 숫자 지정하기 
		li:nth-child( 3n+2 ) {
				color: red;
			}
		li:nth-last-child( 3n+2 ) {
			color: red;
		}
	
	- 태그 상태 지정하기
		a:hover{
			color: red;
		}



미디어 쿼리 
	- 주어진 조건에맞는 설정만 적용하게 해주는 기능
		@media screen and (max-width: 992px) {
		  body {
		    background-color: blue;
		  }
		}

		/* On screens that are 600px or less, set the background color to olive */
		@media screen and (max-width: 600px) {
		  body {
		    background-color: olive;
		  }
		}
		@media screen and (min-width: 600px) {
		  body {
		    background-color: black;
		  }
		}

애니메이션 - keyframes
	- 에니메이션을 넣을 수 있도록 해주는 기능

		@keyframes example {
		  from {background-color: red;}
		  to {background-color: yellow;}
		}
		div {
		  width: 100px;
		  height: 100px;
		  background-color: red;
		  -webkit-animation-name: example; /* Safari 4.0 - 8.0 */
		  -webkit-animation-duration: 4s; /* Safari 4.0 - 8.0 */
		  animation-name: example;
		  animation-duration: 4s;
		}

JS의 기본 

Jquery의 사용



-------------------참고 문헌
https://www.w3schools.com/tags/ref_byfunc.asp
https://www.w3schools.com/html/html_attributes.asp
https://www.w3schools.com/css/css3_mediaqueries_ex.asp
https://www.w3schools.com/css/css3_animations.asp

