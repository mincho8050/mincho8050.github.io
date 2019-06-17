---
title:  "Exception,getter , setter 함수"
date:   2019-06-06
categories: Java
tags: 
- basicJava
author: "mincho8050"
---

# Exception 예외처리

- 자바 클래스 실행(run)시 발생하는 에러
- try ~ catch 문
- throws 문
- finally문



###### 

### try~catch 문



###### Exception 처리하지 않는 경우

```java
System.out.println(1);		// 출력값 1
System.out.println(2/0);	// ArithmeticException 발생. 여기서 프로그램이 멈춤 (exception미처리)
System.out.println("end");		// "end"가 출력되지 않음.

// 출력값
//1
//Exception in thread "main" java.lang.ArithmeticException: / by zero
//	at oop0604.Test06_Exception.main(Test06_Exception.java:16)
```



###### Exception 처리한 경우

```java
try{
			//예외가 발생할 예상이 되는 코드를 작성.
			//주로 여기다 코딩한다고 함.
			System.out.println(1);
			System.out.println(2/0); //exception이 발생하면 더 이상 진행되지 않고.
			System.out.println(3);
}catch(ArithmeticException e){ //여기로 와서 e변수에 저장되고
			
			//예외가 발생되면 처리할 코드를 작성.
			System.out.println(e); //콘솔창에 표시된다.
}//try end


//		출력값
// 		1
//		java.lang.ArithmeticException: / by zero
//		END
```



###### 다중 catch문 

```java
try{
			int a=Integer.parseInt("happy");
			int b=3/0;
			int[] su=new int[3];
			su[3]=5;
}catch(ArithmeticException e){
			System.out.println(e);
}catch(ArrayIndexOutOfBoundsException e){
			System.out.println(e);
}catch(NumberFormatException e){
			System.out.println(e);
}catch(NullPointerException e){
			System.out.println(e);
}
		System.out.println("end");


		// 출력값 > java.lang.NumberFormatException: For input string: "happy"
```



###### 다중 catch문 간략하게 작성하기

```java
try{
			int a=Integer.parseInt("happy");
			int b=3/0;
			int[] su=new int[3];
			su[3]=5;
}catch(Exception e){ // Exception 클래스 : 모든 Exception의 조상 > 다형성
			System.out.println(e);
}//try
		
		System.out.println("end");
		
//		출력값
//		java.lang.NumberFormatException: For input string: "happy"

```





------







### finally 문

- 예외가 발생하거나, 발생하지 않거나 > 무조건 실행
- 비정상적으로 오픈을 했을때도 문 닫는 실행을 하기 위해 finally 사용.
- 외부 프로그램을 연결하 때 사용
  - 자바에서 외부 프로그램에 연결하려면 일단 오픈을 하고 닫고 나와야 나중에 다른 서버와 혼동이 되지 않는다. 이때 비정상적인 연결이라도 나올땐 닫고 나와야 한다. 
  - 이럴때 Exception 상황과 상관없이 작동하는 finally를 이용해 close를 해야한다. 

```java
try{
			System.out.println("open");
			System.out.println(1);
			System.out.println(2/0);
			System.out.println(3);
}catch(Exception e){
			System.out.println(e);
}finally{
			System.out.println("close"); 
}
		System.out.println("end");


//		출력값 > 정상적일때
//		open
//		1
//		2
//		3
//		close		> 예외가 발생 되지 않아도 finall 찍음
//		end


//		출력값 > 값에 문제가 있을때
//		open
//		1
//		java.lang.ArithmeticException: / by zero	> 예외
//		close 										> 예외가 있어도 finally를 찍음.
//		end
```







### throws 문

- 메소드 호출 시 예외처리를 한꺼번에 모아서 처리

```java
try{
			Test test=new Test();
			test.view();
			test.disp();
}catch(Exception e){
			System.out.println(e);
}//try
		

//		출력값
//		java.lang.ArithmeticException: / by zero
```







------







# getter , setter 함수

- 클래스를 설계할 때 필드를 private로 했다고 해도 연결시킬 수 있다.
- set : 보내는거. 전달.
- get : 가져오는거. 받는거.



```java
class Member{
	
	private String id;
	private String password;
	
	public void setId(String id){
		this.id=id; //setter함수
					//값을 줄때 set
	}
    
	public String getId(){ //getter함수
							//값을 가져올때 get
		return this.id;
	}
	
	public void setPassword(String password){
		this.password=password;
	}
    
	public String getPassword(){
		return this.password;
	}
	
    
    
}//member class
```

```java
public static void main(String[] args) {
    
   		Member mem=new Member();
		mem.setId("soldesk");		//값 전달해주기.
		mem.setPassword("1234");	//값 전달해주기. 
		
		System.out.println(mem.getId());		//값 가져오기
		System.out.println(mem.getPassword());	//값 가져오기
    
}//main
```



