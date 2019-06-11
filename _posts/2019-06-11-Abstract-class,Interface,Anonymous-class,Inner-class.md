---
title:  "Abstract class,Interface,Anonymous class,Inner class"
date:   2019-06-11 08:00:00
categories: Java
tags: basic
author: "mincho8050"
---

# Abstract class 추상클래스

- 추상클래스는 객체를 생성할 수 없다.
  - 직접 new 연산자를 사용할 수 없다.
  - 추상 클래스 : 일반메소드 + 추상메소드 
    - 불완전한 클래스
    - 추상메소드가 1개라도 있어야 추상화 시킴
- 추상메소드
  - 메소드의 body{}가 없음
  - 메소드의 머릿말만 존재. 
    - 하는 기능이 없음
  - 형식 : 리턴형 함수명(); > 불완전한 함수
- 인터페이스
  - 추상메소드만 선언 가능하다.
  - 활용도가 더 높음.





### 추상클래스



- 추상클래스는 누군가의 부모 역할만 한다. (extends)
- 추상클래스를 상속받은 자식클래스는 반드시 추상메소드를 완성해야 한다.(override)

```java
abstract class Animal{ 	//추상메소드가 1개 있으니까 일반클래스가 아니라
						//추상클래스 > abstract를 붙여준다.
						
	String name;
	void view(){}			//일반메소드
	abstract void disp();	//{}가 없음 메소드가 불완전. 추상메소드.
							//void disp(); 하면 불완전해서 에러뜸
							//abstract void disp(); 해야 추상메소드의 형태.
}//animal class
```

- 추상클래스는 부모역할만 하고 자식이 가져다 변형해서 사용한다.
- 추상클래스는 확장시킬 때 사용

```java
class Elephant extends Animal{ 	// 추상클래스도 상속 가능
								// 추상클래스는 부모역할만 하고 자식이 가져다 변형해서 사용한다.
								// 추상클래스는 확장시킬 때 사용
	@Override
	void disp() {
		System.out.println("점보...");
	}
	
}//elephant class


class Tuna extends Animal{
	@Override
	void disp() {
		System.out.println("니모...");
	}
	
}//tuna class
```

- 추상클래스는 new연산자로 객체생성을 할 수 없다.

```java
public static void main(String[] args) {
    
//		Animal ani=new Animal();	> 에러
			
		Elephant jumbo=new Elephant();
		jumbo.disp();	//점보...
		
		Tuna nimo=new Tuna();
		nimo.disp();	//니모...   
}
```

- 추상클래스의 다형성

```java
public static void main(String[] args) {
    
    	/*
		 * Animal 에는 disp(); 불완전한 함수가 있는데
		 * 자식클래스 Elephant 에서는 disp();를 
		 * 무조건 오버라이드 하게 되어있어서
		 * 모양이 비슷해 다형성이 더 잘됨.
		 * 추상클래스는 이렇게 쓰기위해서 함.
		 */
    
    	Animal ani=new Elephant(); //이게 더 잘됨
		ani.disp();//점보...
    
        ani=new Tuna();
		ani.disp(); //니모...
}
```





### 추상클래스의 다형성

- 다형성으로만 해야한다. 
- 추상클래스는 자신의 클래스 이름으로 객체생성 불가.

```java
abstract class Travel{ 	//추상메소드가 하나라도 있으면 추상클래스 > abstract 붙인다.
	 void view(){} 		//일반메소드 , 추상클래스에는 일반메소드를 섞어 쓸 수 있다.
	 abstract String travelWhere();	//추상메소드 > 	abstract 붙인다
}//travel class

class TypeA extends Travel{ //부모클래스에 있던 추상메소드를 오버라이드를 해서 완성시켜야 오류가 나지 않는다.

	@Override
	String travelWhere() {
		return "여의도 불꽃축제";
	}

} //typea


class TypeB extends Travel{
	@Override
	String travelWhere() {
		return "지리산 둘레길";
	}
} //typeb


class TypeC extends Travel{
	@Override
	String travelWhere() {
		return "남산공원";
	}
} //typec
```

- 추상클래스는 자신의 클래스로 객체 생성 불가

```java
public static void main(String[] args) {
    //		Travel tour=new Travel(); 에러
    
    Travel tour1=new TypeA();
    System.out.println(tour1.travelWhere());

    Travel tour2=new TypeB();
    System.out.println(tour2.travelWhere());

    Travel tour3=new TypeC();
    System.out.println(tour3.travelWhere());
    
        /*
		 * 출력값
		 	여의도 불꽃축제
			지리산 둘레길
			남산공원
		 */
}
```





------





### 인터페이스 Interface

- 추상메소드만 구성되어 있다.
  - 일반메소드와 섞여 있으면 추상클래스.
- 추상클래스보다 더 추상화

> 클래스 입장에서 부모가 클래스일 경우
>
> > extends 확장
>
> 부모가 인터페이스일 경우
>
> > implements 구현

```java
interface Parent{ //interface는 추상메소드만 올 수 있다.
	
	// void disp();{} > 에러, 일반메소드 사용불가
	
	abstract void kind();	//추상메소드
	void breathe();			//interface자체는 추상메소드만 
    						//올 수 있기 때문에
							//abstract를 생략 가능.
}//interface 


class Son implements Parent{	// 구현 
								// 클래스 입장에서 부모가 인터페이스
    							//implements 사용.
	@Override
	public void kind() {
		System.out.println("아들");
	}
@Override
	public void breathe() {	
	System.out.println("허파 숨쉬기 1");
	}
}//son class



class Daughter implements Parent{	// 구현
	@Override
	public void kind() {
		System.out.println("딸");
	}
	@Override
	public void breathe() {
		System.out.println("허파 숨쉬기 2");
	}
}//daughter

```

- 인터페이스 자신으로 직접 객체 생성 불가.

```java
public static void main(String[] args) {
    //		Parent parent=new Parent(); 에러
    
    Son son=new Son();
    son.kind();
    son.breathe();
		/*출력값
		 아들
		 허파 숨쉬기 1
		 */
    
    Daughter daug=new Daughter();
    daug.kind();
    daug.breathe();
		/*출력값
		 딸
		 허파 숨쉬기 2
		 */
    
}
```

- 인터페이스에서의 다형성
  - 다형성은 인터페이스에서 더 잘된다. (추상클래스보다.)

```java
public static void main(String[] args) {
    
    Parent parent=new Son();
    parent.kind();
    parent.breathe();
		/*
		 * 출력값
		 아들
		 허파 숨쉬기 1
		 */
    
        parent=new Daughter();
		parent.kind();
		parent.breathe();
		/*
		 * 출력값
		 딸
		 허파 숨쉬기 2
		 */
}
```



- 인터페이스를 활용해서 개발 프로젝트에서 발생하는 여러 페이지들을 표준화, 구조화 할 수 있다.

```java
interface Icalc{		// 산술연산 + - * / %
	public int add();
	public int sub();
	public int mul();
	public int div();
	public int mod();
}//icalc


class Calclmp implements Icalc{
	private int x,y;
	
	public Calclmp(){}
	public Calclmp(int x,int y){
		this.x=x;
		this.y=y;
	}

	@Override
	public int add() {
		return x+y;
	}

	@Override
	public int sub() {
		return x-y;
	}

	@Override
	public int mul() {
		return x*y;
	}

	@Override
	public int div() {
		return x/y;
	}

	@Override
	public int mod() {
		return x%y;
	}
	
}//calclmp
```

- 다형성

```java
public static void main(String[] args) {
    
    Icalc calc=new Calclmp(5,3);
    System.out.println(calc.add());
    System.out.println(calc.sub());
    System.out.println(calc.mul());
    System.out.println(calc.div());
    System.out.println(calc.mod());
    
}
```



### 클래스와 인터페이스간의 상속 및 구현

- 인터페이스는 다중상속이 가능.
  - 클래스간에는 단일상속만 가능

```java
class Unit{
	int currentHP;	//유닛의 체력
	int x,y;		//유닛의 x,y좌표
}//unit class

interface Movable{
	void move(int x,int y);
}//movable interface

interface Attackable{
	void attack(Unit u);
}//attackable interface
```

- 인터페이스가 인터페이스를 상속받을때는 extends (클래스가 클래스 상속받을 때도.)
- 클래스가 인터페이스를 상속받을때는 implement

```java
interface Fightable extends Movable,Attackable{
							//인터페이스는 다중상속이 가능하다.
							//클래스간에는 단일상속만 가능하다.
}//fightable

class Fight extends Unit implements Fightable{

	@Override
	public void attack(Unit u) {
	}

	@Override
	public void move(int x, int y) {
	
}//fight 
```





------





### 익명 내부 클래스 Anonymous class

- 클래스 이름이 아니라 이름없는 것들을 통칭.

```java
interface Imessage{
	public void msgPrint();
}//imessage

class Message implements Imessage{
	//인터페이스는 new 할 수 없어서.
	//자손을 만들고 써야 실행이 됨.
	//왜? {}가 없는 불완전상태니까!
	@Override
	public void msgPrint() {
		System.out.println("Message class");	
	}
}//class
```

**1. 구현클래스**

```java
public static void main(String[] args) {
    Message msg=new Message();
    msg.msgPrint();
}
```

**2. 다형성**

```java
public static void main(String[] args) {
		Imessage msg2=new Message();
		msg2.msgPrint();
}
```

**3. 익명클래스**

- 필요한 곳에서 일시적으로 실행
- 이벤트가 발생할 때만 실행
  - 안드로이드 자바, JavaScript,jQuery 에서 많이 활용

```java
public static void main(String[] args) {
    Imessage mess=new Imessage(){
			@Override
			public void msgPrint() {
			System.out.println("Anonymous 익명 내부 클래스");	
			}
	};
		//클래스가 아닌데 new가 됨.
    
    mess.msgPrint();
}
```





------





### 내부 클래스 inner class

- 클래스 내부에서 선언된 클래스

```java
class WebProgram {
    String title="Java Program";
    
    class Language{ //inner class
        String basic="JAVA, HTML, CSS, JavaScript";
        void display(){
            System.out.println("기초수업:" + basic);
        }
    }//class end , 내부클래스

    
    class Smart{    //inner class 
        String basic="Objective-C, Java OOP, C#";
        void display(){
            System.out.println("기초수업:" + basic);
        }
    }//class end , 내부클래스
    
    void print(){
        Language lang=new Language();
        lang.display();
        
        Smart sm=new Smart();
        sm.display();
    }    
    
}//class end 



// 안드로이드 기반 자바
class R{
	static class id{
		static String btn="버튼";
	}
}//r class
```

- 내부 클래스는 외부에서 단계적으로 접근할 수 있다.

```java
public static void main(String[] args) {
    
    WebProgram web=new WebProgram();
    web.print();
    /*
    출력값
    기초수업:JAVA, HTML, CSS, JavaScript
    기초수업:Objective-C, Java OOP, C#
    */
    
    
    	
//    	Language lang=new Language();
//    	Smart sm=new Smart();
//    		> 둘다 에러남. 
//    		> 내부 클래스는 직접 접근할 수 없다.
    
    
    
    //내부 클래스는 외부에서 단계적으로 접근할 수 있다.
    Language lang=new WebProgram().new Language();
    Smart sm=new WebProgram().new Smart();

    lang.display();
    sm.display();   
    /*
    출력값
    기초수업:JAVA, HTML, CSS, JavaScript
    기초수업:Objective-C, Java OOP, C#
    */
    
 
    
    System.out.println(R.id.btn);
    /*
    출력값
    버튼
    */
}
```

