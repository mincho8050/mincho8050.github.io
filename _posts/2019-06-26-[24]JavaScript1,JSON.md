---
title:  "[24]JavaScript1,JSON"
date:   2019-06-26
categories: 
- Frontend
tags: 
- JavaScript
author: "mincho8050"
---

# JavaScript

- interpreter 방식의 스크립트 언어
- Server Side Interpreter 실행
- on Server 실행 : JSP , ASP , PHP
- 대소문자를 구분한다
- 종결문자 ; (필수는 아니다)
- 변수의 자료형이 없다
  - 변수 선언 형식 > var 변수명
  - 대입되는 값의 형태에 따라 자동으로 변수의 자료형이 결정된다
  - 변수 선언하지 않아도 변수를 사용할 수 있다.
- 함수 선언 형식 > function 함수명(){}
  - 함수의 리턴형이 없다 (리턴값은 존재)
  - 이벤트를 발생시켜 함수를 호출하는 방식으로 대부분 실행한다.

```
<script></script> 스크립트 안에서 작성한다. 위치는 보통 head안에서 작성
```

- 주로 사용자가 입력한 자료에 대한 유효성 검사시 사용
  - 유효성 검사를 할때는 한계가 있다.
  - 예를 들어 주민번호와 실명확인 등의 유효성 검사는 하지 못한다. 다만, 회원가입이라면 아이디가 빈값인지? 이메일 입력을 제대로 했는지? 등의 유효성 검사는 자바스크립트가 그 역할을 해준다. 
- 자바문법과 거의 동일
- 자바스크립트는 서버에서 실행되는 언어가 아니라 클라이언트에서 실행되는 스크립트 언어



------



## 자바스크립트의 기본문법



#### 1) 자바스크립트는 Object(객체)로 구성되어 있다.

- Object = property(field) + method로 구성되어 있다
- 자바스크립트의 최상위 객체는 window
  - window 객체명은 생략 가능하다
- property , field , 멤버변수 : window.location
- method , 멤버함수 : window.alert()

- 경고창 실행

> window.alert()
>
> > window는 생략가능

```javascript
alert();
```

- 확인 , 취소 버튼이 나옴

> window.confirm();
>
> > window 생략가능
> >
> > 확인 > true , 취소  > false 값을 반환한다

```javascript
confirm("영구히 삭제됩니다.\n지울까요?");
```





#### 2) 변수의 자료형은 var만 있다

- 값이 변수에 대입이 되면서 자료형이 결정된다
- 변수선언 하지 않아도 사용 가능하다

```javascript
var name="홍길동";
var age=25;
heigh=178.5;	//var 생략가능
var now=new Date();	//날짜형
var cars=[];	//배열선언
```





#### 3) document 객체

- 웹페이지의 본문 body를 가리키는 객체
- HTML tag 사용시  "" 안에서 사용

```javascript
	document.write(123);
    document.write("대한민국");
    document.write(456);
    
    document.write("<hr>");
    document.write("무궁화<br>라일락<br>");
    document.write("<strong>솔데스크</strong>");
    document.write("<img src='../images/apeach.png'>");
```

![01](https://user-images.githubusercontent.com/49340180/61308362-158af880-a82b-11e9-9ebc-03113bdbe864.PNG)

- 본문 body에 있는 id 속성 접근

> JavaScript 

```javascript
document.getElementById("아이디명").innerHTML="안녕";
```

> JavaScript로 body에 표 출력하기

```javascript
<!DOCTYPE html>
<html lang="ko">
<head>
	<meta charset="UTF-8">
	<title>표출력</title>
	<style>
	</style>
</head>
<body>
	<div id="demo"></div>

	<script>
		var name="홍길동";
		var age=25;
		var height=178.5;
		var table="";
        
		table +="";
		table +="<table border='1'>";
		table +="<tr>";
		table +="	<td>이름</td>";
		table +="	<td>"+name+"</td>";
		table +="</tr>";
		table +="<tr>";
		table +="	<td>나이</td>";
		table +="	<td>"+age+"</td>";
		table +="</tr>";
		table +="<tr>";
		table +="	<td>키</td>";
		table +="	<td>"+height+"</td>";
		table +="</tr>";
		table +="</table>";
        
		document.getElementById("demo").innerHTML=table;		
	</script>

</body>
</html>

```

![01](https://user-images.githubusercontent.com/49340180/61309145-9f879100-a82c-11e9-95aa-a77a999ceff9.PNG)





------



## 연산자

- Java 와 동일하다

```
+ - * / % < <= > >= == !=
```

- 논리연산자

> 예시) 윤년인지 확인하시오

```javascript
var y=2019;

if(y%4==0&&y%100!=0||y%400==0){
    document.write("윤년");
}else{
    document.write("평년");
}; // 평년
document.write(y%4==0&&y%100!=0||y%400==0); //false
```

- 삼항연산자

> 예) 세개의 수 중에서 가장 큰 값 구하기

```javascript
var a=3,b=5,c=7;
var max=(a<b)?b:a;
max=(c<max)?max:c;
```

- 1 증감 연산자

```javascript
var x=10,y=10;
var p=++x;
var q=y--;

document.write(x);	//11
document.write(y);	//9
document.write(p);	//11
document.write(q);	//10
```



#### 형변환



###### 1) The Number() Method

- 문자를 숫자로 변형

> NaN : 에러메세지로 이렇게 뜰 때 쓰레기값이라는 의미(자바스크립트)

```javascript
Number(true) 		//1
Number(false)		//0
Number("10")		//10
Number(" 10")		//10
Number("10 ")		//10
Number(" 10 ")		//10
Number("10.33")		//10.33
Number("10,33")		//NaN
Number("10 33")		//NaN
Number("John")		//NaN
```



###### 2)  The parseInt() Method

```javascript
parseInt("10")			//10
parseInt("10.33")		//10
parseInt("10 20 30")	//10
parseInt("10 years")	//10
parseInt("years 10")	//NaN
```



###### 3)  The parseFloat() Method

```javascript
parseFloat("10")		//10
parseFloat("10.33")		//10.33
parseFloat("10 20 30")	//10
parseFloat("10 years")	//10
parseFloat("years 10")	//NaN
```



###### 4) The toString() Method

> 문자형 변환

```javascript
var x=123;
x.toString()		//"123"
(123).toString()	//"123"
(100+23).toString()	//"123"
```



###### 5) The valueOf() Method

> 참조형(Integer) -> 기본형(int)

```javascript
var x=123;
x.valueOf()			//123
(123).valueOf()		//123
(100+23).valueOf()	//123
```



###### 6) isNaN()

- 값이 숫자형태로 되어져있는지 물어보는것
- 숫자면 false , 숫자가 아니면 true

> undefined , NaN : 쓰레기값

```javascript
isNaN(123)			//false
isNaN(-1.23)		//false
isNaN(5-2)			//false
isNaN(0)			//false
isNaN('123')		//false
isNaN('Hello')		//true
isNaN('2005/12/12')	//true
isNaN('')			//false
isNaN(true)			//false
isNaN(undefined)	//true
isNaN('NaN')		//true
isNaN(NaN)			//true
isNaN(0 / 0)		//true
```



###### 7) eval()

> "x * y" , "2 + 2" , "x + 17" 는 문자열로 구성되어있는데 계산해주는 함수
>
> 원래 변수명은 a1에서 a와 1을 분리해서 생각할 수 없는데 eval()를 사용하면 숫자를 반복문 처리해서 할 수 있음(따로 볼 수 있음)
>
> > eval(a+"1") -> a1 변수로 인식함

```javascript
var x=10,y=20;
eval("x * y")		//200
eval("2 + 2")		//4
eval("x + 17")		//27
```



------





## 제어문

- 조건문 
  - if문 , switch~case문
- 반복문
  - for문 , while문 , do~while문
- break문 , continue문



#### 예시) 성적프로그램



###### 1) 학생정보

```javascript
var uname="홍길동";
var kor=100,eng=95,mat=100;
var total=kor+eng+mat;
var aver=parseInt(total/3);
```

###### 2) 국어점수의 학점 구하기

```javascript
var grade="";
if(kor>90){
    grede="국어:A학점";
}else if(kor>80){
    grede="국어:B학점";
}else if(kor>70){
    grede="국어:C학점";
}else if(kor>60){
    grede="국어:D학점";
}else{
    grede="국어:F학점";
}
```

###### 3) 과락 구하기

> 평균을 기준으로 합격, 재시험, 불합격

```javascript
var result="";
if(aver>=70){
    if(kor<40||eng<40||mat<40){
        result="재시험";
    }else{
        result="합격";
    }
}else{
    result="불합격";
}
```

###### 4) 평균 95점 이상 학생은 장학생

```javascript
var special="";
if(aver>=95){
    special="장학생";
}
```

###### 5) 위의 데이터값을 문자열 변수에 모두 모아서 테이블로 작성한 후 id=demo에 출력하기

```javascript
var table="";
table +="<table>";
table +="<tr>";
table +="	<td colspan='2'>성적표</td>";
table +="</tr>";
table +="<tr>";
table +="	<td>이름</td>";
table +="	<td>"+uname.toString()+"</td>";
table +="</tr>";
table +="<tr>";
table +="	<td>국어</td>";
table +="	<td>"+kor.toString()+"점"+"</td>";
table +="</tr>";
table +="<tr>";
table +="	<td>영어</td>";
table +="	<td>"+eng.toString()+"점"+"</td>";
table +="</tr>";
table +="<tr>";
table +="	<td>수학</td>";
table +="	<td>"+mat.toString()+"점"+"</td>";
table +="</tr>";
table +="<tr>";
table +="	<td>총점</td>";
table +="	<td>"+total.toString()+"점"+"</td>";
table +="</tr>";
table +="<tr>";
table +="	<td>평균</td>";
table +="	<td>"+aver.toString()+"점"+"</td>";
table +="</tr>";
table +="<tr>";
table +="	<td>과락</td>";
table +="	<td>"+result.toString()+"</td>";
table +="</tr>";
table +="<tr>";
table +="	<td>별점</td>";
table +="	<td>"+star.toString()+"</td>";
table +="</tr>";
table +="<tr>";
table +="	<td>장학여부</td>";
table +="	<td>"+special.toString()+"</td>";
table +="</tr>";
table +="<tr>";
table +="	<td>국어 학점</td>";
table +="	<td>"+grade.toString()+"</td>";
table +="</tr>";
table +="</table>";
document.getElementById("demo").innerHTML=table;
```

![01](https://user-images.githubusercontent.com/49340180/61312017-87b30b80-a832-11e9-9915-8e9994f775ed.PNG)





------





## 배열

- 배열 Array , 요소 element , 순서 index
- 배열 선언할 때

> old version (bad)
>
> > var points = new Array();

```javascript
var points=new Array(40,100,1,5,25,10);
```

> new version (good)
>
> > var points = [];

```javascript
var points=[40,100,1,5,25,10];
```

- 배열 선언

```javascript
var cars=[];
cars[0]="Saab";
cars[1]="Volvo";
cars[2]="BMW";
document.write(cars+"<hr>");	//배열요소 전체출력
```

- 자바와 다르게 자바스크립트는 배열 순서에 상관없이 값을 넣어도 된다.

```javascript
var fruits=["Banana", "Orange", "Apple", "Mango"];
fruits[6]="Lemon";
document.write(fruits.length+"<hr>");	//7개 나옴
document.write(fruits[6]+"<hr>");		//"Lemon"
document.write(fruits[4]+"<hr>");		//undefined
document.write(fruits[5]+"<hr>");		//undefined
```



#### 배열관련 메소드

```javascript
var fruits=["Banana", "Orange", "Apple", "Mango"];
```

###### 1) 배열 출력하기

> Banana,Orange,Apple,Mango

```javascript
document.write(fruits);
document.write(fruits.toString());
```

###### 2) 요소 사이에 * 넣어서 출력하기

> Banana*Orange*Apple*Mango

```javascript
document.write(fruits.join("*"));
```

###### 3) 배열 마지막 요소 제거

> Banana,Orange,Apple

```javascript
fruits.pop();
```

###### 4) 배열 마지막 요소 추가

> Banana,Orange,Apple,Kiwi

```javascript
fruits.push("Kiwi");
```

###### 5) 첫번째 배열 요소 제거 후 다른 모든 요소를 낮은 인덱스로 이동

> Orange,Apple,Kiwi

```javascript
fruits.shift();
```

###### 6) 첫번째 요소 추가

> Lemon,Orange,Apple,Kiwi

```javascript
fruits.unshift("Lemon");
```

###### 7) 0,1번째 요소 제거

> Orange,Apple,Kiwi

```javascript
fruits.splice(0,1);
```

###### 8) concat() 메소드는 기존 배열을 병합(연결)하여 새 배열을 만든다

- 여러개의 배열 인수를 사용할 수 있다.

> Cecilie,Lone,Emil,Tobias,Linus

```javascript
var myGirls = ["Cecilie", "Lone"];
var myBoys = ["Emil", "Tobias", "Linus"];
var myChildren = myGirls.concat(myBoys);
```

###### 9) 배열의 일부를 새 배열로 분열

> fruits.slice(1, 3);
>
> > Orange,Lemon 출력 ( 마지막 지정한 자릿수는 포함되지 않음. 실제 1~2)
>
> Orange,Lemon,Apple,Mango

```javascript
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus=fruits.slice(1); //실제는 0번째 자릿수 출력
```

###### 

#### 배열정렬



##### Array Sort

- 문자열 배열

###### 1) 오름차순

> Apple,Banana,Lemon,Mango,Orange

```javascript
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
fruits.sort();
```

###### 2) 내림차순

> Mango,Apple,Lemon,Orange,Banana

```javascript
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
fruits.reverse();
```



##### Numberic Sort

- 숫자 배열

###### 1) 오름차순

> 1,5,10,25,40,100

```javascript
var points = [40, 100, 1, 5, 25, 10];
points.sort(function(a, b){return a - b});
```

###### 2) 내림차순

> 100,40,25,10,5,1

```javascript
var points = [40, 100, 1, 5, 25, 10];
points.sort(function(a, b){return b - a});
```





------



# JSON

- JavaScript Object Notation : JSON

- 자바스크립트 객체 표기법

- 배열을 객체화

- 속성(name) : 값(value) 쌍으로 데이터가 구성

- AJAX , NoSQL 등에서 주로 사용

- 비동기식 통신(문자단위 통신)에서 XML를 대체하는 주요 데이터 포맷

  - XML 문법
    - custom tag(사용자 정의 태그 )기반
    - 환경설정, 안드로이드 기반의 앱개발 시 View단 구성

  ```javascript
  <person>
      <firstName>John</firstName>
  	<lastName>Doe</lastName>
  	<age>46</age>
  </person>   
  ```



- name(key)과 value 한 쌍으로 구성
- name과 value는 : 기호로 구분
- name 구성 시 "" 기호를 생략 가능하다

- {} 안에 데이터를 저장한다

> 형식) {name:value,"name":value,name:value}
>
> > 예) {"firstName":"John","lastName":"Doe","age":46}

```javascript
var person={"firstName":"John","lastName":"Doe","age":46};
```

- 1번째 방법

> "John" , "Doe" , 46

```javascript
person["firstName"]
person["lastName"]
person["age"]
```

- 2번째 방법

> "John" , "Doe" , 46

```javascript
person.firstName
person.lastName
person.age
```



#### 다양한 값 넣어보기

```javascript
var persons=[
    {"firstName":"John", "lastName":"Doe1", "age":46},//0번째 요소
    {"firstName":"Tom", "lastName":"Doe2", "age":26},//1번째 요소
    {"firstName":"Sam", "lastName":"Doe3", "age":36}//3번째 요소 마지막에 , 안들어가도록 주의
];
```

> [object Object] 출력
>
> > 값이 제대로 주고 받고가 잘 되고 있다는 뜻

```javascript
persons[0]	
persons[1]	
persons[2]	
```

> 출력값
>
> John Doe1 46
>
> Tom Doe2 26
>
> Sam Doe3 36

```javascript
for(idx=0;idx<person.length;idx++){
    document.write(person[idx].firstName+"\n");
    document.write(person[idx].lastName+"\n");
    document.write(person[idx].age+"\n");
}
```



#### JSON.parse()

- String형태의 JSON문법을 분리할 때
- JSON 문법으로 변환, JSON문법이라는 가정하에!!

```javascript
var txt='{"name":"John","age":30,"city":"New York"}';
var obj=JSON.parse(txt);
```



#### JSON.stringfy()

- JSON값을 일반 문자열로 변환

```javascript
var obj={name:"John",age:30,city:"New York"};
var myJSON=JSON.stringfy(obj);
```

> '{"name":"John","age":30,"city":"New York"}'

```javascript
document.write(myJSON);
```

> 길이 : 42

```javascript
document.write(myJSON.length)
```



#### JSON Array

```javascript
var myObj,x;
myObj={
  "name":"John",
   "age":30,
   "cars":["Ford","BMW","Fiat"] 
};
x=myObj.cars[0];	//"Ford"
```
