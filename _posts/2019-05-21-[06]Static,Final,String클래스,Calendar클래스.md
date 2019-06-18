---
title:  "[06]Static,final,Calendar클래스"
date:   2019-05-21
categories: Java
tags:
- Java
author: "mincho8050"
---

# Static

- 변수 , 함수
- 정적변수 , 정적함수
- 메모리 생성도 1번, 소멸도 1번
- new 연산자를 이용해서 별도의 객체생성 없이 사용가능하다.





### static 특징을 적용한 경우

- static 변수와 함수는 이미 static 메모리에 값이 상주해 있기 때문에 별도의 객체생성을 하지 않고도 직접 접근할 수 있다.
- static 접근 방법
  - 클래스명.static변수
  - 클래스명.static함수()





------





# Final

- 변수 : 변수의 상수화(절대 변하지 않는 값). 주로 static과 같이 사용한다.

- 클래스 : 누군가의 부모 클래스가 될 수 없다. 더 이상 파생시킬 수 없다.
- 함수 : 더 이상 override(재정의)를 할 수 없다.







------



# String 클래스

- String
- StringBuffer
- StringBuilder
- StringTokenizer : java.util 패키지
- 나머지는 java.lang 패키지
- 문자열 가공하는 경우 속도(String이 제일 느림)
  - String < StringBuffer < StringBuilder



예시.

```java
String str="Gone With The Wind";
```



**1. 클래스명.charAt()**

- 어떤 글자가 들어오든 0번째는 고정
- char형으로 출력

```java
str.charAt(0) // 'G'
str.charAt(str.length()-1) 
    //마지막 글자는 가변적이니까 이렇게 구하자.
```



**2. 클래스명.indexOf()**

- 문자열의 순서값(index) 구하기
- 중복되는 문자열이면 처음 문자열의 인덱스 값이 나옴.
- 없는 문자열이면 -1 출력.

```java
str.indexOf("G") // 0
```



**3. 클래스명.toLowerCase()**

- 전부 소문자로 출력



**4. 클래스명.toUpperCase()**

- 전부 대문자로 출력



**5. 클래스명.replace(' ',' ')**

- 글자 치환하여 출력.
- 사용자가 엔터치면 화면상에 드러나는 글자는 아님. 이때 사용.

	> '엔터' > ' < br >' 로 바꿀 때 사용하기도함

```java
str.replace('n','N') // 출력값 GoNe With The WiNd
```



**6. 클래스명.isEmpty()**

- 리턴값이 boolean형
- 클래스명.length 가 0일때 true , 아니면 false



**7. 클래스명.startsWith()**

- 앞에서부터 문자열을 비교해서 boolean형으로 나타냄

```java
str.startsWith("Gone") // true
```

**8. 클래스명.endsWith()**

- 뒤에서부터 문자열 비교해서 boolean형으로 나타냄



**9. 문자열 내용비교**

- "sky"가 "hi"인지 문자열 내용비교 > boolean형

```java
"sky".equals("hi") // false
```





------





# 날짜 관련 클래스

- Data
- Calendar  >  java.util 패키지
  - new를 통해 객체생성을 못함.
- GregorianCalendar
  - Calendar의 후손 클래스





### Calendar 클래스



- 현재 시스템의 날짜정보 가져오기

```java
Calendar now=Calendar.getInstance();
```

> get이 붙는 함수는 어디에서 가져온다는 것.
>
> get으로 존재하는 함수는 리턴값이 있음.
>
> 반댓말은 set

```java
System.out.println(now.get(1)); //출력값 2019 > 현재 연도
System.out.println(now.get(2)); //출력값 5 > 현재 달.
                                //근데 0부터 카운트가 되어서 +1해줘야 됨.
System.out.println(now.get(2)+1);//출력값 6 > +1해줘야 현재 달이 나옴.
System.out.println(now.get(5)); //출력값 9 > 현재 날짜
```

```java
System.out.println(now.get(Calendar.YEAR));
System.out.println(now.get(Calendar.MONTH)+1);
System.out.println(now.get(Calendar.DATE));

System.out.println(now.get(Calendar.HOUR));  //현재 시 , 
System.out.println(now.get(Calendar.MINUTE)); //현재 분 
System.out.println(now.get(Calendar.SECOND));//현재 초
```



- 24시간을 기준으로

```java
System.out.println(now.get(Calendar.HOUR_OF_DAY)); 
```

- 요일

```java
System.out.println(now.get(Calendar.DAY_OF_WEEK)); //1
```

> - 요일은 정수형으로 나옴.
> - 1 > 일 , 2 > 월 , 3 > 화 , 4 > 수 , 5 > 목 , 6 > 금 , 7 > 토



