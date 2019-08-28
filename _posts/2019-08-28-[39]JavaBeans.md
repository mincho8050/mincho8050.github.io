---
title:  "[39]JavaBeans"
date:   2019-08-28
categories: 
- JSP
tags: 
- JSP
author: "mincho8050"
---

# Java Beans 개요





## 1) 자바 웹페이지의 개발방식의 흐름

servlet(.java)  -> jsp  -> servlet(.java)



## 2) 서블릿의 이해



**서블릿을 이용한 구성**

- 자바로 개발된 .class 파일

- JSP에 비하여 컴파일 단계가 생략되기 때문에 속도가 빠르다.

- 서블릿의 단점 : 디자인 부분과 혼합하여 작업하기가 매우 어렵다.


**JSP(Java Server Page)를 이용한 구성**

- .jsp 파일

- Servlet의 불편한 디자인 부분을 좀더 간결하게 제작하도록 개선된 서버 스크립트

- 하나의 JSP페이지에는 HTML, CSS, JavaScript, Java Code등이 혼합되어 코딩이 이루어 진다.

- 유지보수 및 보안 문제, 기능 확장이 어려워 개발시 JSP와 Beans로 분리한다.


**. JSP + Beans 를 이용한 구성**

- .jsp + .java 파일로 구성
- BEANS는 콤포넌트 자바 개발자가 Business Logic(Process Logic)를 구현하는 목적으로 사용된다.
- DB관련해서 INSERT, UPDATE, DELETE, SELECT등 구현시 주로 사용한다.





## 3) Beans의 이해

- jsp페이지에 나열되는 자바 처리로직은 디자인 코드와 함께 매우 복잡한 코드를 구성한다.
    이로인해 디자인 변경시 자바코드가 영향을 받아 오류가 자주 발생되며, 
    코드 수정시 코드를 알아볼 수 없어 유지보수가 어렵다.
    또한 작업 도중 자바 코드가 JSP 페이지에 반복적으로 사용될 수 있으며,
    코드의 노출로 저작권등 보안에 심각한 문제가 발생된다.
    이러한 반복되는 자바 코드들을 JAVA파일안에 저장하여 사용하는 형태를 빈즈라고 한다.



- 데이터를 출력하는 레이어와 데이터를 처리하는 레이어를 분리(Manager, Process class)하여
    유지보수 시간을 단축 시키 수 있으며 코드의 가독성을 높일 수 있다. 


- 확장자는 *.java 이며 컴파일해서 .class형태로 배포한다.

- 빈즈는 dll과 같은 원리를 가지고 있다.

- 빈즈의 종류

   DTO(Data Transfer Object) class 
   Manager(Process) class
   DAO(Data Access Object) class







------





# Beans 테스트1 - 계산기





## 계산기(computerForm.jsp)



```html
<body>
	<h1>* 계산기 *</h1>
	<!-- 1) JSP 메소드를 통한 계산기 실습
		<form action="computerok1.jsp">
	 -->
	 
	 <!-- 2) new 연산자
	<form action="computerok2.jsp">
	 -->
	 
    <!-- 3) beans 이용-->
	 <form action="computerok3.jsp">
	<table border="1">
		<tr>
			<th>숫자1</th>
			<td><input type="number" name="num1" size="5" autofocus></td>
		</tr>
		<tr>
			<th>숫자2</th>
			<td><input type="number" name="num2" size="5"></td>
		</tr>
		<tr>
			<td colspan="2" class="subbtn">
				<input type="submit" value="계산">
			</td>
		</tr>
	</table>
	</form>

</body>
```







## 1) JSP 메소드를 이용 (computerok1.jsp)



```java
<body>
	<h1>* 계산결과 *</h1>
	<h2>1) JSP 메소드 호출</h2>
	
	<%
	int num1=Integer.parseInt(request.getParameter("num1").trim());
	int num2=Integer.parseInt(request.getParameter("num2").trim());
	
	out.print(num1+"+"+num2+"="+add(num1,num2));
	out.print("<hr>");
	out.print(num1+"-"+num2+"="+sub(num1,num2));
	out.print("<hr>");
	out.print(num1+"*"+num2+"="+mul(num1,num2));
	out.print("<hr>");
	out.print(num1+"/"+num2+"="+div(num1,num2));
	out.print("<hr>");
	out.print(num1+"%"+num2+"="+mod(num1,num2));
	out.print("<hr>");
	%>
	
	
	<%!
	//JSP 메소드 작성 영역--------------------------------------
	public int add(int a, int b){return a+b;}
	public int sub(int a, int b){return a-b;}
	public int mul(int a, int b){return a*b;}
	public int div(int a, int b){return a/b;}
	public int mod(int a, int b){return a%b;}
	%>

</body>
```







## 2) new 연산자 (computerok2.jsp)

Compute.java 클래스를 만든다

```java
package net.testbean;
public class Compute {
	
	public int add(int a, int b){return a+b;}
	public int sub(int a, int b){return a-b;}
	public int mul(int a, int b){return a*b;}
	public int div(int a, int b){return a/b;}
	public int mod(int a, int b){return a%b;}
	
}
```

new 연산자를 이용해 불러온다.

```java
<%@ page contentType="text/html; charset=UTF-8"%>
<%@ page import="net.testbean.*" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>computerok2.jsp</title>
</head>
<body>
	<h1>* 계산 결과 *</h1>
	<h2>2) new연산자를 이용해서 객체 생성 후 메소드 호출</h2>
	
	<%
	int num1=Integer.parseInt(request.getParameter("num1").trim());
	int num2=Integer.parseInt(request.getParameter("num2").trim());
	
	/*
	객체 new Compute()
	->RAM의 heap영역에 메모리가 할당되고 주소값이 발생
	참조변수 comp
	*/
	
	Compute comp=new Compute();
	out.print(num1+"+"+num2+"="+comp.add(num1,num2));
	out.print("<hr>");
	out.print(num1+"-"+num2+"="+comp.sub(num1,num2));
	out.print("<hr>");
	out.print(num1+"*"+num2+"="+comp.mul(num1,num2));
	out.print("<hr>");
	out.print(num1+"/"+num2+"="+comp.div(num1,num2));
	out.print("<hr>");
	out.print(num1+"%"+num2+"="+comp.mod(num1,num2));
	out.print("<hr>");
	%>

</body>
</html>
```







## 3) beans 이용 (computerok3.jsp)

위와 동일하게 Compute.java 클래스를 만들어 불러온다.

**Java Bean(객체) 생성**

```jsp
<jsp:useBean id="객체명" class="팩키지명.클래스명" scope="page">    
```

**Java Bean(id)의 유효범위**

- scope="page" : 현재페이지에서만 유효, 생략 시 기본값
- scope="request" : 다른페이지에서 공유
- scope="session" : 다른페이지에서 공유
- scope="application" : 다른페이지에서 공유

**[주의사항]**

Java Bean(id)가 제대로 생성 안될 수도 있다.

-> 해당프로젝트를 Refresh , Validate, Project -> Clean 해주기

```java
<%@ page contentType="text/html; charset=UTF-8"%>
<%@ page import="net.testbean.*"%>
<jsp:useBean id="comp" class="net.testbean.Compute" scope="page"></jsp:useBean>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>computerok3.jsp</title>
</head>
<body>	
	<h1>* 계산 결과 *</h1>
	<h2>3) Java Bean을 이용해서 객체 생성 후 메소드 호출</h2>

	
	<%
	int num1=Integer.parseInt(request.getParameter("num1").trim());
	int num2=Integer.parseInt(request.getParameter("num2").trim());

	out.print(num1+"+"+num2+"="+comp.add(num1,num2));
	out.print("<hr>");
	out.print(num1+"-"+num2+"="+comp.sub(num1,num2));
	out.print("<hr>");
	out.print(num1+"*"+num2+"="+comp.mul(num1,num2));
	out.print("<hr>");
	out.print(num1+"/"+num2+"="+comp.div(num1,num2));
	out.print("<hr>");
	out.print(num1+"%"+num2+"="+comp.mod(num1,num2));
	out.print("<hr>");
	%>

</body>
</html>
```







------







# Beans 테스트2 - 절대값, 팩토리얼





## 절대값,팩토리얼 입력(calcForm.jsp)



```html
<body>
	<h1>* 절대값/팩토리얼 *</h1>
	
	<form action="calcok.jsp">
	<table border="1">
		<tr>
			<th>숫자</th>
			<td><input type="number" name="num1" size="5" autofocus></td>
		</tr>
		<tr>
			<td colspan="2" class="subbtn">
				<input type="submit" value="계산">
			</td>
		</tr>
	</table>
	</form>

</body>
```







## 결과 창 (calcok.jsp)

CalcBean.java 클래스를 만든다.

> package째 복사해서 src에 붙여넣으면 자동으로 생성

```java
package net.testbean;

public class CalcBean {

  public int abs(int su){
    // 절대값 구하기(무조건 양수 출력)
    if(su<0)
      return -su;
    else
      return su;

}

  public int fact(int su){
    // 팩토리얼값 구하기 4*3*2*1
    return (su==0) ? 1 : su*fact(su-1);    
  }

}
```

만든 CalcBean.java 클래스를 불러온다.

```java
<%@ page contentType="text/html; charset=UTF-8"%>
<%@ page import="net.testbean.*" %>
<jsp:useBean id="calc" class="net.testbean.CalcBean" scope="page"></jsp:useBean>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>calcok.jsp</title>
</head>
<body>
	<h1>* Java Bean 이용한 계산 결과 *</h1>
	
	<%
	int su=Integer.parseInt(request.getParameter("num1"));
	out.print(su+" 절대값: "+calc.abs(su));
	out.print("<hr>");
	out.print(su+" 팩토리얼: "+calc.fact(su));
	%>
</body>
</html>
```









------





# Beans를 이용한 성적테이블





## 성적 입력폼(sungjukForm.jsp)



```html
<body>

	<h1>*성적 입력 폼*</h1>
	
	<p><a href="sungjukList.jsp">[성적목록]</a></p>
	
	<form method="post" action="sungjukIns.jsp">
		<table border="1">
			<tr>
				<th>이름</th>
				<td>
					<input type="text" name="uname" maxlength="20" required autofocus>
				</td>
			</tr>
			<tr>
				<th>국어</th>
				<td>
					<input type="number" name="kor" size="5" min="0" max="100" placeholder="숫자입력">
				</td>
			</tr>
			<tr>
				<th>영어</th>
				<td>
					<input type="number" name="eng" size="5" min="0" max="100" placeholder="숫자입력">
				</td>
			</tr>
			<tr>
				<th>수학</th>
				<td>
					<input type="number" name="mat" size="5" min="0" max="100" placeholder="숫자입력">
				</td>
			</tr>
			<tr>
				<th>주소</th>
				<td>
					<select name="addr">
						<option value="Seoul">서울</option>
						<option value="Jeju">제주</option>
						<option value="Busan">부산</option>
						<option value="Suwon">수원</option>
					</select>
				</td>
			</tr>
			<tr>
				<td colspan="2" align="center">
					<input type="submit" value="전송">
					<input type="reset" value="취소">
				</td>
			</tr>
		</table>
	</form>

</body>
```





## 데이터베이스 클래스

보안을 위해 따로 데이터베이스를 불러오는 코드는 클래스로 만든다.

DBOpen.java 와 DBClose.java 클래스를 만든다.(이번은 DBOpen.java만 사용)

**DBOpen.java**

```java
package net.utility;

import java.sql.Connection;
import java.sql.DriverManager;

public class DBOpen {
	public static Connection getConnection() throws Exception{
		Class.forName("oracle.jdbc.OracleDriver");
		Connection con=DriverManager.getConnection(
				"jdbc:oracle:thin:@//localhost:1521/xe"
				,"java0514"
				,"1234");
		return con;
	}//getConnection
	
	
	
}//
```





## 성적 결과 폼(sungjukIns.jsp)

SungjukDAO.java , SungjukDTO.java 클래스를 만든다.



**DAO (Data Access Object)**

- Insert, delete, update, select등 레코드 처리를 주 목적으로 한다.
- DTO 객체를 데이터베이스 서버에 저장한다.
- 데이터베이스 서버로 부터 레코드를 SELECT한 후, DTO 객체로 변경해 가져온다.
- SQL을 직접 실행하는 기능을 가지고 있다.
- JSP에서 데이터베이스 접속 로직을 Beans로 작성하여 DB정보 변경시 쉽게 변경사항을 구현할 수 있다.
- 데이터 접근을 목적하는 객체이다.
- 커넥션 같은 것을 하나만 두고 여러 사용자가 DAO의 인터페이스를 사용하여 필요한 자료에 접근 하도록 하는 것
- DB를 사용해 데이터를 조회하거나 조작하는 기능을 전담하도록 만든 오브젝트를 말한다.

```java
package net.sungjuk;

import java.sql.Connection;
import java.sql.PreparedStatement;

import net.utility.*; //DB

public class SungjukDAO {
	//Data Access Object 데이터베이스 관련 작업
	public int insert(String uname, int kor, int eng, int mat, int aver, String addr){
		
		int cnt=0;
		
		try{
			//DB연결
            //클래스를 불러와서 사용
			Connection con=DBOpen.getConnection();
			
			StringBuilder sql=new StringBuilder();
			sql.append(" INSERT INTO sungjuk(sno,uname,kor,eng,mat,aver,addr,wdate) ");
			sql.append(" VALUES((SELECT NVL(MAX(sno),0)+1 FROM sungjuk) ");
			sql.append(" ,?,?,?,?,?,?,SYSDATE)");
			
			//4)SQL문 변환
			PreparedStatement pstmt=null;
			pstmt=con.prepareStatement(sql.toString());
			pstmt.setString(1, uname);
			pstmt.setInt(2, kor);
			pstmt.setInt(3, eng);
			pstmt.setInt(4, mat);
			pstmt.setInt(5, aver);
			pstmt.setString(6, addr);
			
			//5)SQL문 실행
			cnt=pstmt.executeUpdate();
			
		
		}catch(Exception e){
			System.out.println("행추가 실패"+e);
		}//try
		
		return cnt;
		
	}//insert
	
	public int insert(SungjukDTO dto){
		int cnt=0;
		try{
			Connection con=DBOpen.getConnection();
			StringBuilder sql=new StringBuilder();
			sql.append(" INSERT INTO sungjuk(sno,uname,kor,eng,mat,aver,addr,wdate) ");
			sql.append(" VALUES((SELECT NVL(MAX(sno),0)+1 FROM sungjuk) ");
			sql.append(" ,?,?,?,?,?,?,SYSDATE)");
			
			//4)SQL문 변환
			PreparedStatement pstmt=null;
			pstmt=con.prepareStatement(sql.toString());
			pstmt.setString(1, dto.getUname());
			pstmt.setInt(2, dto.getKor());
			pstmt.setInt(3, dto.getEng());
			pstmt.setInt(4, dto.getMat());
			pstmt.setInt(5, dto.getAver());
			pstmt.setString(6, dto.getAddr());
			
			
			//5)SQL문 실행
			cnt=pstmt.executeUpdate();
			
			
		}catch(Exception e){
			System.out.println("행추가 실패"+e);
		}//try
		return cnt;
	}//insert

}//
```



**DTO (Data Transfer Object) 데이터 전송 객체**

- 사용자가 입력한 값을 객체형태로 받을수 있게 클래스를 구성한다
- 폼에서 입력된 데이터들은 하나의 DTO 객체로 변환 될 수 있다.
- 하나의 데이터베이스 레코드를 저장하며 레코드와 같은 구조를 가지고 있다.
- 레코드가 50개이면 DTO 객체도 50개가 만들어 진다. 
- 한개의 레코드는 빈즈 클래스 객체 하나로 매핑된다.
- 한개의 컬럼은 멤버 변수로 매핑된다.
- 데이터가 포함된 객체를 한 시스템에서 다른 시스템으로 전달하는 작업을 처리하는 객체이다.
- DTO는 VO로 바꿔 말할 수 있는데 계층간 데이터 교환을 위한 자바빈즈를 말한다.
  - SungjukVO (Value Object) , SungjukDataBean 라고도 쓰임
- 테이블의 칼럼과 1:1 매칭되는 field를 private 접근제어로 선언하고 getter, setter함수를 작성한다

```java
package net.sungjuk;

public class SungjukDTO {

	private int sno;
	private String uname;
	private int kor;
	private int eng;
	private int mat;
	private int aver;
	private String addr;
	private String wdate;
	
	//default constructor
	public SungjukDTO(){}//SungjukDTO
	
	//각 field에 접근할 수 있는 getter, setter함수
	//>Source -> Generate Getters and Setters... -> 자동추가
	public int getSno() {
		return sno;
	}

	public void setSno(int sno) {
		this.sno = sno;
	}

	public String getUname() {
		return uname;
	}

	public void setUname(String uname) {
		this.uname = uname;
	}

	public int getKor() {
		return kor;
	}

	public void setKor(int kor) {
		this.kor = kor;
	}

	public int getEng() {
		return eng;
	}

	public void setEng(int eng) {
		this.eng = eng;
	}

	public int getMat() {
		return mat;
	}

	public void setMat(int mat) {
		this.mat = mat;
	}

	public int getAver() {
		return aver;
	}

	public void setAver(int aver) {
		this.aver = aver;
	}

	public String getAddr() {
		return addr;
	}

	public void setAddr(String addr) {
		this.addr = addr;
	}

	public String getWdate() {
		return wdate;
	}

	public void setWdate(String wdate) {
		this.wdate = wdate;
	}

	//개발자들이 에러 확인용으로 주로 사용
	//Source -> Generate toSting()...
	@Override
	public String toString() {
		return "SungjukDTO [sno=" + sno + ", uname=" + uname + ", kor=" + kor
				+ ", eng=" + eng + ", mat=" + mat + ", aver=" + aver
				+ ", addr=" + addr + ", wdate=" + wdate + "]";
	}
	
}//
```



