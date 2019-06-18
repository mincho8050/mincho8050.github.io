---
title:  "[08]Super,Polymorphism"
date:   2019-06-04
categories: Java
tags: 
- Java
author: "mincho8050"
---

# Super

- 부모 , 조상 : super
- super : 자식클래스에서 부모클래스 멤버변수에 접근할 때
- super() : 자식클래스의 생성자함수가 부모클래스의 생성자함수를 호출할 때
- this : 멤버변수(field)와 지역변수 구분하기 위해
- this() : 자신의 생성자 함수를 호출할 때
- 부모클래스 : superclass
- 자식클래스 : subclass



```java
class School{
	String name="학교";
	public School(){
		System.out.println("School()...");
	}//school
}//school class , 부모클래스


class MiddleSchool extends School{
	
	String name="중학교";
	public MiddleSchool(){
		// 부모생성자함수를 호출하는 명령어 
		super(); // 생략가능.
	System.out.println("MiddleSchool()...");
     }
  }//middleschool class , 자식클래스


class HighSchool extends School{ //부모는 School , 자식이 HighSchool
	String name="고등학교";	//field 멤버변수 , class 내에서만 사용.
	public HighSchool() {
		
		super(); //생략가능
	}

	public void disp(){//method 멤버함수
		/*
		 * 아무것도 정의하지않으면 부모 것을 사용하지만
		 * 나의 것을 정의해도 부모것도 쓸 수 있다. 
		 * 옵션이 하나 추가된 느낌
		 */
		String name="종로고등학교"; // 지역변수, disp() 안에서만 사용가능.
						// 지역변수, 멤버변수(this) 부모변수 3개 사용가능.
						// 아무것도 지정안하면 내것이 가장 우선순위 높음
		System.out.println(name);	//지역변수 > 종로고등학교
		System.out.println(this.name);	//멤버변수 > 고등학교
		System.out.println(super.name);	//부모변수 > 학교
	}
	
	
}//highschool class

```

```java
public static void main(String[] args) {
    
    // 상속관계에서 생성자함수 호출순서
    // 부모 > 자신
    // School() > MiddleSchool()
    MiddleSchool ms=new MiddleSchool();
    	/*출력값
		 *  School()...
			MiddleSchool()...
			부모 > 자식 순으로 출력
		 */
    System.out.println(ms.name);  	//중학교
    
    
}//main
```



> super 예시

```java
class Parent{
	int one;
	int two;
	public Parent(){}
	public Parent(int one, int two) {
		this.one = one;
		this.two = two;
	}
	
}//parent class , 부모클래스


class Child extends Parent{
	int three; //물려받은 one, two를 그대로 쓰고 싶을 때 그냥 둔다. 여기에 다시 쓰면 재정의되기 때문이다.
	public Child(){
		//super(); 생략되어 있음
	}
	public Child(int a,int b,int c){
		super(a,b); //부모로부터 물려받은 멤버변수(field)에 초기값 전달.
		//super.one=a;
		//super.two=b;도 같은의미
		three=c;
	}
	
}//child class
```

- 부모클래스 super

```java
public static void main(String[] args) {

		Child child=new Child(10,20,30);
		System.out.println(child.one);		//10
		System.out.println(child.two);		//20
		System.out.println(child.three);	//30
		
}//main
```





------





# Polymorphism 다형

- 상속관계에서의 다형성 (형태가 다양함)
- 부모클래스 입장에서 형태가 여러가지
- 클래스들간의 형변환을 위해서 필요함.



```java
class Father extends Object{ //extends Object 생략가능
	
	public String name="아버지";
	public String addr="주소";
	
	public Father() {} //기본생성자함수
	
	public Father(String name, String addr) {
//		super(); 생략가능
		this.name = name;
		this.addr = addr;
	}
	
	public void disp(){
		System.out.println(this.name);
		System.out.println(this.addr);
	}
	
	
}//father class , 부모클래스


class Son extends Father{
	//name,addr 살아있음
	public Son(){}
	public Son(String n,String a){
		super.name=n; //부모한테서 물려받은 거니까 super 사용.
		super.addr=a;
	}
}//son class


class Daugther extends Father{
	//name,addr 살아있음
	String friend="절친"; //멤버변수가 추가되었지만 부모가 물려준게 아니라 형변환이 되지 않는다.
							// > 클래스에 새로 추가된 애들은 다형성의 대상이 아니다.
	public Daugther(){}
	public Daugther(String name,String addr){
		super(name,addr);
		//부모로 부터 물려받은 name, addr 사용 > super 사용
	}
	
}//daugther class

```

**1. 일반적인 방식의 객체 생성**

- new 연산자를 사용
- POJO(Plain Old Java Object) 방식

```java
Father f=new Father();
		f.disp();
		
Son s=new Son("손흥민","영국");
s.disp();

Daugther d=new Daugther("김연아","캐나다");
d.disp();


		/*
		 * 출력값
		 
		    아버지
			주소
			손흥민
			영국
			김연아
			캐나다
		 */
```

**2. 다형성을 이용한 객체를 생성**

- 자식클래스가 부모클래스에 대입 가능하다.
- 자식클래스의 모양으로 부모클래스가 형태를 변경한다.
- 가장 일반적인 다향성

```java
Father fa=new Son("개나리","관철동");
		fa.disp();
		/*
		 * 출력값
		 * 	개나리
			관철동
		 */

		fa=new Daugther("진달래","인사동");
		fa.disp();
		/*
		 * 	진달래
			인사동
		 */
		System.out.println(fa.name);
		System.out.println(fa.addr);


		//자식클래스에 추가된 멤버는 다형성의 대상이 아니다.
//		System.out.println(fa.friend); 에러
		/*
		 * 	진달래
			인사동
		 */
```

- 부모클래스도 자식클래스에 대입 가능하다.
  - 단, 자식클래스의 모양으로 변환 > 선행조건
  - 자주 쓰이지는 않음.

```java
Father fa2=new Father();
    Son son2=new Son();
    Daugther daugh2=new Daugther();

    fa2=son2;	// 다형성 , 자식 > 부모에 대입
    son2=(Son) fa2;	//  son2=fa2; 에러, 부모 > 자식에 대입
                    // 자식클래스 모양으로 변환해야 한다. son2=(Son) fa2;

    //Exception 발생
    daugh2=(Daugther) fa2;
```

- 모든 자바 클래스의 최고 조상 : Object
- 자바의 모든 클래스는 Object 클래스의 후손.
- 자바의 모든 클래스는 Object 클래스에 대입 가능 (다형성)

```java
Object inte=new Integer(3); // 다형성
Object fa3=new Father(); // 다형성
```

