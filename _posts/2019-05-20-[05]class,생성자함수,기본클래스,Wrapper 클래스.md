---
title:  "[05]class,생성자함수,기본클래스,Wrapper 클래스"
date:   2019-05-20
categories: Java
tags:
- Java
author: "mincho8050"
---

# Class 



### Class 클래스



- C언어 
  - 구조체(struct) , 공용체(union)
  - 변수로 구성되어 있다.
- JAVA : class
  - 변수와 함수로 구성되어 있다.
- 자바 클래스의 구성
  - 멤버변수(field) + 멤버함수(method)



> 클래스 명은 식별자와 같지만 무조건 첫글자는 대문자로 준다.



**접근허용범위 Access Modifier**

접근지정, 접근제어, 접근수정

- private : 비공개
- package : 생략가능, 같은 폴더 내에서만 접근가능
- protected : 다른 패키지라도  상속관계에서는 접근가능
- public : 공개

```java
class Sungjuk{ //package 생략

	 // 멤버변수(field) 선언 : column, property 속성
	String name; //접근허용범위 안쓰면 package
	public int kor,eng,mat; //공개
	private int aver; //Sungjuk 클래스 내에서만 접근가능.
	
	// 멤버함수(method) 선언
	void calc(){ //접근제어 package 생략
		aver=(kor+eng+mat)/3;
	}//calc
	
	public void disp(){
		System.out.println(name);
		System.out.println(kor);
		System.out.println(eng);
		System.out.println(mat);
		System.out.println(aver);
		
	}//disp

	
}//Sungjuk class
```



- 클래스는 메모리를 할당을 한 후 사용한다.
- new 연산자 : 메모리할당 연산자
- new를 하면 heap공간에 메모리를 할당.
- new 클래스명();
  - 객체(Object) : 함수가 있으면 객체, 함수가 없으면 변수
  - new 연산자를 이용해서 메모리 할당을 함

> 예시
>
> kim > #50(예시)번지를 참조하는 변수
>
> >  Sungjuk kim=new Sungjuk();
>
> lee > #70(예시)번지를 참조하는 변수
>
> > Sungjuk lee=new Sungjuk();



**객체의 주소값**

- 주소값 확인하는 hashCode

> kim.hashCode()
>
> lee.hashCode()

```java
public class Test01_Class {

	public static void main(String[] args) {
            Sungjuk kim=new Sungjuk();
            // .연산자를 이용해서 멤버에 접근한다.
            // .연산자로 접근한다. 일반변수와 다른점.
            kim.name="손흥민"; 
            kim.kor=95;
            kim.eng=70;
            kim.mat=55;
            //kim.aver=70; 에러. private 속성은 접근 불가.
        
        kim.calc(); //평균은 private속성이지만 평균값을 계산하는 calc함수는 package기 때문에 가져올 수 있음.
		kim.disp(); // 그 값이 다 들어있는 disp함수를 가져오면 
        			/*
		            * 출력값
		            * 손흥민
						95
						70
						55
						73  > 평균값도 같이 나옴.
		            */
		
        
    }
}
```





------





# 생성자 함수(constructor)

- 클래스명과 동일한 함수
- 기본생성자(default constructor) : 매개변수가 없다.
  - 기본생성자 함수는 자동코딩
    - 이클립스 상단메뉴 Source >  Generate Constructors from Superclass...  
  - 매개변수가 있는 생성자 함수 자동추가
    - 이클립스 상단 메뉴 Source > Generate Constructors using fields...
  - 대부분 기본생성자함수는 필요가 없어도 수동으로 작성한다.
    - 단, 기본생성자함수는 생성자 함수가 오버로딩이 되면 자동추가 되지 않는다. 강제적으로 선언하는 것을 강추.

```java
public School(){}  //생성자 함수 예시
```

- 생성자 함수는 리턴형이 존재하지 않는다!

  - public void School(){}  > 가능

- 생성자 함수도 오버로딩(overloading)이 가능하다.

  ​	생성자 함수가 많은게 좋다. 할 일이 줄어들기 때문이다.





### This

**this**

- 자신의 클래스 멤버를 가리킴
- 일반변수와 멤버변수를 구분하기 위함.

**this()**

- 생성자 함수간의 호출
- 생성자 함수가 생성자 함수를 호출할 수 있다.
- 생상자 함수를 호출하려 할때는 첫줄에서 한다.
- 일반 함수에서는 this를 사용할 수 없다.(생성자 함수 호출 불가능)

```java
class Sungjuk{
	private String name;
	private int kor,eng,mat;
	private int aver;
	
	public Sungjuk(){
		//생성자 함수가 생성자 함수를 호출할 수 있다.
		//Sungjuk("손흥민"); > 이렇게 부르면 에러.
		this("손흥민"); //이렇게 불러야 에러가 안남. 
	}//sungjuk > 매개변수가 없는 생성자함수는 default constructor > 이거 안적어도 되고 edit가면 자동생성
	

	public Sungjuk(String name){
		this.name=name; //여기서 this.name은 매개변수 name, 여기에 위에서 private name한 것을 대입.
	}
	
	public Sungjuk(int kor,int eng,int mat){
		this(); //생성자 함수가 자신의 생성자 함수를 호출할 수 있다. 
		this.kor=kor;
		this.eng=eng;
		this.mat=mat;
	}
	
	public Sungjuk(int aver){
		this(10,20,30); //첫줄. 여기에 적으면 에러아님
		this.aver=aver;
	  //this(10,20,30); > 에러, 생성자 함수를 호출하려 할때는 첫줄에서 한다.
		disp(); //생성자 함수에서는 일반함수 호출 가능.
	}
	
	public void disp(){
		//this(85); > 에러, 일반함수에서는 this를 사용할 수 없다. (생성자함수 호출 불가능)
	}//disp
	
}//Sungjuk
```





------





# 기본 클래스

```java
import java.lang.*; 
// java.lang은 생략가능하지만 다른 패키지를 사용할 시에는 적어준다.
```



###### Integer 클래스

```java
Integer a=new Integer(5);
Integer b=new Integer("7");

System.out.println(a.toString());   //toString의 리턴값이 String이기 떄문에 > "5"
System.out.println(b.intValue());   //intValue의 리턴값이 int기 떄문에 > 7
                                    //결국 둘 다 형변환 시킨것.
```

- Integer.parseInt(); : 문자열 안에 있는 숫자기호를 정수형으로 변환



###### String 클래스

- String
- StringBuffer
- StringBuilder
- StringTokenizer : java.util

```java
String name=new String("HAPPY"); //참조형
String str="HAPPY"; //기본형
```

- 문자열의 내용을 비교하는 경우 같다 연산자(==)를 사용하지 말고 equals() 함수를 사용.

```java
if(name.equals(str)){
			System.out.println("같다");
		}else{
			System.out.println("다르다");
}
```







------





# Wrapper 클래스

- 클래스 이름이 Wrapper가 아니라 통칭해서 부름.
- 기본 자료형을 참조형화 해놓은 클래스들
- java.lang 패키지에 선언되어 있음
- 기본형
  - boolean, byte, int, long, float, double, char
- 참조형
  - Boolean, Byte, Integer, Long, Float, Double, Character
- 참조형이 된다는 것은 . 이 된다는것 > 다양한 기능을 사용할 수 있음

```java
Integer inte1=new Integer(5);  //정수형 
Integer inte2=new Integer("7"); //String형
System.out.println(inte1);
System.out.println(inte2.doubleValue()); //int > double 결과값 7.0 형변환

System.out.println(Integer.toBinaryString(9)); //9라는 10진수를 2진수로 변환해 String값으로 하는것.
                                          	  //결과값 "1001"
System.out.println(Integer.toOctalString(9)); //8진수로 변환해서 String값 > "11"
System.out.println(Integer.toHexString(9));  //16진수로 변환 String값 > "9"
System.out.println(Integer.parseInt("4"));  //String값을 정수로 변환

System.out.println(Integer.max(6, 8)); //최댓값
System.out.println(Integer.min(6, 8)); //최솟값
```

```java
Long l1=new Long(4);
Long l2=new Long("6");
System.out.println(l1);
System.out.println(l2.longValue()); // String > long

System.out.println(Long.parseLong("8")); // String > long
```

```java
Double d1=new Double(3.4);
Double d2=new Double("5.6");

// 문1) d1과 d2의 합을 구하시오
int hap=d1.intValue()+d2.intValue(); //double , String > int
//hap변수가 int니까 형변환 시켜야함 .intValue() int로 형변환 시켜줌.
System.out.println(hap);
```

```java
char c='R';
Character ch=new Character('T');
System.out.println(c);
System.out.println(ch);

System.out.println(Character.isAlphabetic(6));//int값을 주면 boolean으로 리턴값을 준다.
//int값이 알파벳값인지 알려줌.
//is로 묻는것은 대부분이 리턴형이 boolean
//to로 시작하는것은 뒤에걸로 뭐 하겠다는 거.
// > ex)toString to뒤에 나와있는 자료형으로 돌려주겠다는 의미. 대부분!

System.out.println(Character.isAlphabetic('가'));//출력값 true         
System.out.println(Character.isAlphabetic('a')); //true
System.out.println(Character.isAlphabetic('!')); //false   
System.out.println(Character.isWhitespace(' ')); //공백인 경우 true
```

