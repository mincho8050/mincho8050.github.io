---
title:  "상속,Null,CallBy,GregorianCalendar클래스"
date:   2019-06-09 10:22:00
categories: Java
tags:basic
author: "mincho8050"
---

# 상속



### 상속(inheritance)

- 클래스의 재활용
- 부모 , 조상 , super class
- 자식 , 파생 , sub class
- 클래스는 ,로 연속으로 올 수 없다. 단일상속만 가능하다.
- 부모클래스에서 상속받은 것과 다르게 나만의 함수를 만들 수 있다.

> 형식 ) class 자식 extends 부모클래스{}

```java
 // final class AA{} 클래스라면 종단클래스는 상속시킬 수 없다.


class AA{
	private void zero(){} //상속안됨.
	public void one(){
		System.out.println("one()...");
	}
	public void two(){
		System.out.println("two()...");
	}
}//aa

class BB extends AA{//부모클래스 AA
	                //자식클래스 BB
	public void three(){
		System.out.println("three()...");
	}
	
}//bb

class CC extends BB{
	public void four(){
		System.out.println("four()...");
	}
}//cc
```

- AA가 BB에게 물려준거 + BB가 가진것을 CC에게 물려줌.



### 클래스 상속관계에서의 생성자 호출 순서

- 부모생성자()	> 	자신의 생성자() 호출된다.



### 함수의 재정의 (Method Override)

- 상속관계에서 메소드를 다시 수정하는 것.(리폼)
- Method Override는 함수명 중복 (실제로는 개별함수)



- 오버라이드 할 때
- ctrl+space 또는
- Source->Override/Implements Methods...사용



- 자바의 기본 패키지
  - java.lang
- 자바의 최고 조상 클래스
  - Object 클래스(java.lang.Object)
- 자바의 모든 클래스는 생성할 때 무조건 Object 클래스를 상속받는다. 
- 자바의 모든 클래스는 Object의 후손이다.
  - Object 클래스 점검할 필요가 있다.

```java
class Korea {  //extends Object 가 생략되어있다. 
	           //> 모든 클래스는 Object 클래스를 상속받아야 한다.
	           //> java.lang.Object
	String name="대한민국"; 
	
	final void view(){
		System.out.println("view()...");
	} //final함수 상속불가.
		void disp(){
			System.out.println("Korea..."+this.name);
		}

}//korea

class Seoul extends Korea{ //korea 랑 seoul은 부모자식관계.
	String name="서울특별시";
	
	@Override //< annotation(주석)
			  //< 재정의.
		void disp() {
			System.out.println("Seoul..."+this.name);
    }//리폼의 패턴.
```



```java
class Suwon{//extends Object 생략가능
	
	private String id="webmaster";
	private String pw="1234";
	@Override
		public String toString() {
		  return "Suwon [id="+id+",pw="+pw+"]";
		}
	
}//suwon
```

- toString()은 private 변수값을 리턴하는 용도로 주로 사용.(에러값을 잡기 위해)

```java
Suwon su=new Suwon();
System.out.println(su.toString());	//Suwon [id=webmaster,pw=1234]
System.out.println(su);				//Suwon [id=webmaster,pw=1234]
```





------







# Null



### null값

- 레퍼런스 변수를 선언만 해 놓은 상태
- 빈값
- true : 참 , false : 거짓



- 빈값. 그냥 str 변수만 선언상태

```java
String str=null;
//NullPointerException 발생
//System.out.println(str.length()); //데이터가 없기 때문에 글자갯수가 존재하지 않음
```

- 빈 문자열. 메모리를 잡는다.

```java
String name="";
System.out.println(name.length()); //출력값 0 나옴
```





------





# CallBy



### 메소드 호출 방식

- Call By Value
  - 값에 의한 호출방식
- Call By Reference
  - 참에 의한 호출방식
  - 주소값. 객체나 클래스



### 함수명 작성 시 고려사항

- is ~
  - 맞고, 틀리다. 대부분 return 값이 boolean형
- to ~
- get ~
  - 주고 받기. 대부분 return 값이 있음
- set ~
  - 대부분 주고 끝나서 return 값 없음. 대부분 void





------





# GregorianCalendar 클래스



- 현재 시스템 날짜 가져오기

```java
GregorianCalendar now=new GregorianCalendar();
```

- 년,월,일 구하기

```java
System.out.println(now.get(Calendar.YEAR));		//2019
System.out.println(now.get(Calendar.MONTH));	//5 >달은 0부터 시작. 6월로 읽어야함
System.out.println(now.get(Calendar.DATE));		//9
```

- 윤년 구하기. 
  - 윤년이면 true

```java
System.out.println(now.isLeapYear(2020));
```



### 날짜 데이터의 연산



- now 날짜에 3년 더하기.

```java
now.add(Calendar.YEAR, 3); //2022
```

- now 날짜에 2달 빼기

```java
now.add(Calendar.MONTH, -2); // 2022-3-9 //3이지만 4월.
```

- now 날짜에 10일 더하기

```java
now.add(Calendar.DATE,10); // 2022-3-19
```

