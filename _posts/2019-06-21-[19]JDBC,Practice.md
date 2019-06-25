---
title:  "[19]JDBC,Practice"
date:   2019-06-21
categories: 
- Database
tags: 
- Database
author: "mincho8050"
---

# JDBC

- JDBC ( Java Database Connection)
- JDBC(Java Database Connection)
- 자바와 데이터베이스를 연동
- 자바와 오라클DB 를 연결하기 위한 클래스들이 필요하다.
  - 위의 클래스들을 압축해 놓은 라이브러리를 오라클 사에서 제공(.jar)
  - 또는 오라클 설치 폴더에서 복사해서 사용 (ojdbc6.jar)
  - 경로 > C:\oraclexe\app\oracle\product\11.2.0\server\jdbc\lib



### 자바와 데이터베이스를 연동 (DB연결)

>try~catch문에서 작성



###### 0) 자원반납해야 하는 것.

> try~catch문 밖에 작성한다.

```java
Connection con=null;
PreparedStatement pstmt=null;
ResultSet rs=null;	//select를 사용할때 사용.
```



###### 1) 오라클DB 연결 관련 정보

> try~catch문에서 밖에 작성해도 됨.

```java
String url="jdbc:oracle:thin:@172.16.83.22:1521:xe";	//오라클 서버 어디에 있는지.
			//172.16.83.22 이 부분은 고정아이피니까 본인거 들어갈땐 loclahost라고 해도 됨.
String user="java0514";
String password="1234";	//이게 제일 많이 바뀜
String driver="oracle.jdbc.driver.OracleDriver";
//패키지와 패키지 사이는 . 으로 구분 , .class는 생략
```



###### 2) 드라이버 로딩

```java
Class.forName(driver);
```



###### 3) 오라클 DB 서버 연결

```java
con=DriverManager.getConnection(url, user, password);
System.out.println("오라클DB 서버 연결 성공!!");
```

>**에러 메세지**
>
>- java.lang.ClassNotFoundException: oracle.jdbc.driver.OracleDriver
>  - oracle.jdbc.driver.OracleDriver 이 클래스가 없다.
>  - ojdbc6.jar를 연결해줘야한다. 
>    - 프로젝트 우클릭 >Build Path > Configure Build Path > Libraries에서 넣기
>    - JSP에서는 넣는 폴더가 따로 있음
>- java.sql.SQLException: Listener refused the connection with the following error: ORA-12505, TNS:listener does not currently know of SID given in connect descriptor
>  - Oracle 서버가 꺼져있을때 뜨는 오류 메세지.





### 행 추가



###### 4) SQL문 작성

> 여기 중요!
>
> 대소문자 구분은 없지만 대문자로 작성함.
>
> > sql+=" INSERT INTO sungjuk "; 식으로 칼럼명 생략 가능하지만 자바에선 헷갈리고 기억이 나지 않을 수 있기 때문에 칼럼명을 다 적어준다.
>
> DB에서는 문자열 구분기호 '' 임!!! 
>
> sql+=" INSERT INTO sungjuk()";
>
> sql+="VALUES()"; 
>
> > 뒤에 공백을 주지 않으면 한줄로 이어져서 나온다
> >
> > > " INSERT INTO sungjuk()VALUES"; 
> >
> > 그래서 앞, 뒤에 공백을 준다.
> >
> > > sql+=" VALUES() ";
>
> 종결 세미콜론 찍지 않기
>
> > EX) sql+=" VALUES(); ";
>
> **여기까지는 String이기 때문에 그냥 String 값이다!!**

```java
String sql="";
sql+=" INSERT INTO sungjuk(uname,kor,eng,mat)"; 
sql+=" VALUES('박지성',90,50,60) "; 
```

> String 클래스에 연산자로 계속 하면 속도가 현저히 느려지기 때문에, StringBuilder 나 StringBuffer 클래스를 사용한다.
>
> **변환하기 전까지 StringBuilder**

```java
StringBuilder sql=new StringBuilder();
sql.append(" INSERT INTO sungjuk(uname,kor,eng,mat)");
sql.append(" VALUES('김연아',95,90,100) ");
```



###### 5) SQL형식 변환

> 오라클 DB연결 성공은 DriverManager.getConnection 이게 성공했다는 의미.
>
> > 리턴값이 있다. 변수에 담아야함.
>
> DriverManager.getConnection(url, user, password); 적지말고,
>
> Connection con=DriverManager.getConnection(url, user, password);
>
> > 변수에 저장하기
> >
> > DB 연결했다는 값을 con이 가지고 있다.
> >
> > 3번 '오라클 DB 서버 연결' 에서 변수에 저장 시킴.
>
> SQL문 변환한 값을 가지고 있는 자료형. 리턴형이 PreparedStatement. 
>
> 이제 pstmt 를 가지고 실행해야 함.

```java
pstmt=con.prepareStatement(sql);
```

> StringBuilder 클래스 사용시

```java
pstmt=con.prepareStatement(sql.toString());
```



###### 6) SQL문 실행

> executeUpdate()의 리턴형은 int , 변수에 저장한다

```java
int result=pstmt.executeUpdate();
```

> 출력 > 실행결과1
>
> insert된 행의 갯수가 나옴. 행 추가가 된건지 확인이 가능하다.
>
> 만약 0이 나온다면 추가되지 않은것임.

```java
System.out.println("실행결과:"+result);
```



**여기까지가 JDBC 기본이다.**

------



### 행 수정

> DB연결 후 (1~3번 까지는 고정)



###### 4) SQL문 작성

```java
StringBuilder sql=new StringBuilder();
sql.append(" UPDATE sungjuk ");
sql.append(" SET aver=(kor+eng+mat)/3 ");
sql.append(" WHERE uname IN ('박지성','김연아') ");
```



###### 5) SQL문 변환

```java
pstmt=con.prepareStatement(sql.toString());

int result=pstmt.executeUpdate();
if(result==0){
    System.out.println("행추가실패!!");
}else{
    System.out.println("행추가 성공!!");
}
```





### 행 삭제



###### 4) SQL문 작성

```java
StringBuilder sql=new StringBuilder();
sql.append(" DELETE FROM sungjuk ");
sql.append(" WHERE uname ='박지성' ");
```



###### 5) SQL문 변환

```java
prtmt=con.prepareStatement(sql.toString());
														
int result=prtmt.executeUpdate();
if(result==0){
    System.out.println("행삭제 실패!!");
}else{
    System.out.println("행삭제 성공!!");
}
```



###### 6) 자원반납

- 순서주의하기!!
- 맨 처음에 열었던 걸 나중에 닫기.

```java
try{
    //DB 연결 및 SQL문 작성 및 변환.
}catch(Exception e){
			System.out.println("실패!"+e);
}finally{

    try{
        if(pstmt!=null){pstmt.close();}	//맨 마지막에 열었던 것.
    }catch(Exception e){}

    try{
        if(con!=null){con.close();}	// 맨 처음 열었던거
    }catch(Exception e){}


}//try
```





### 행 읽기 1 (Select One)



###### 4) SQL문 작성

```java
StringBuilder sql=new StringBuilder();
sql.append(" SELECT uname,kor,eng,mat,aver");
sql.append(" FROM sungjuk ");
sql.append("WHERE uname='라일락' ");
```



###### 5) SQL문 변환

```java
pstmt=con.prepareStatement(sql.toString());
```

> int result=pstmt.executeUpdate(); 에서
>
> executeUpdate()는 insert문,update문,delete문 사용할 때 사용. 
>
> Select문에서는 pstmt.executeQuery() 사용한다.
>
> > 리턴값이 ResultSet 

```java
rs=pstmt.executeQuery();
```

**5-1) 칼럼 순서로 출력**

> 순서가 중요하다!!!
>
> cursor : 가리키는 값. 이동할 수 있다.

```java
if(rs.next()){//cursor가 존재하는지?
    System.out.println("자료있음");
    //sql.append(" SELECT uname,kor,eng,mat,aver");
    // > 여기에 쓴 순서대로 출력하기 때문에 자료형 맞춰주기.
    System.out.print(rs.getString(1)+" ");//uname
    System.out.print(rs.getInt(2)+" ");//kor
    System.out.print(rs.getInt(3)+" ");//eng
    System.out.print(rs.getInt(4)+" ");//mat
    System.out.print(rs.getInt(5)+" ");//aver
    System.out.println();
    //출력값 라일락 30 80 40 50 

}else{
    System.out.println("자료없음");
    //sql.append("WHERE uname='라일' "); 없는 데이터 입력시
    //자료없음 출력됨.
}//if
```

**5-2) 칼럼명으로 접근**

> 자바니까 "" 안에 적어야 한다.

```java
if(rs.next()){//cursor가 존재하는지?
    System.out.println("자료있음");

    System.out.print(rs.getString("uname")+" ");//uname
    System.out.print(rs.getInt("kor")+" ");//kor
    System.out.print(rs.getInt("eng")+" ");//eng
    System.out.print(rs.getInt("mat")+" ");//mat
    System.out.print(rs.getInt("aver")+" ");//aver
    System.out.println();
    //출력값 라일락 30 80 40 50
    
}else{
    System.out.println("자료없음");
    //sql.append("WHERE uname='라일' "); 없는 데이터 입력시
    //자료없음 출력됨.
}//if
```



###### 6) 자원반납

```java
try{
    //DB 연결 및 SQL문 작성 및 변환.
}catch(Exception e){
			System.out.println("실패!"+e);
}finally{
    
    try{
        if(rs!=null){rs.close();}
    }catch(Exception e){}				//맨 마지막

    try{
        if(pstmt!=null){pstmt.close();}	//두번째
    }catch(Exception e){}

    try{
        if(con!=null){con.close();}		// 맨 처음 열었던거
    }catch(Exception e){}

}//try
```





### 행 읽기 2 (Select All)



###### 4) SQL문 작성

```java
StringBuilder sql=new StringBuilder();
sql.append(" SELECT uname,kor,eng,mat,aver");
sql.append(" FROM sungjuk ");
sql.append(" ORDER BY uname ");	//오름차순으로 정렬해서 보기
```



###### 5) SQL문 변환

```java
pstmt=con.prepareStatement(sql.toString());
rs=pstmt.executeQuery();

if(rs.next()){//cursor가 존재하는지?
    System.out.println("자료있음");
    do{
        System.out.print(rs.getString("uname")+" ");//uname
        System.out.print(rs.getInt("kor")+" ");//kor
        System.out.print(rs.getInt("eng")+" ");//eng
        System.out.print(rs.getInt("mat")+" ");//mat
        System.out.print(rs.getInt("aver")+" ");//aver
        System.out.println();
    }while(rs.next()); //while문 true면 do가 실행
}else{
    System.out.println("자료없음");
}//if
```



###### 6) 자원반납은 역순으로!





------



# 연습



### 오라클 DB연결관련 정보 및 자원반납 해야하는것 설정

> try~catch문 밖에서 작성

```java
String url="jdbc:oracle:thin:@localhost:1521:xe";
String user="java0514";
String password="1234";
String driver="oracle.jdbc.driver.OracleDriver";

//자원반납해야하는것.
Connection con=null;
PreparedStatement pstmt=null;
ResultSet rs=null; //여기선 있어도 되고 없어도 되는거. select문 사용할 시 사용.
```



### 드라이버 로딩 및 오라클 DB 서버 연결

> 여기서부터는 try~catch문에서 작성!!
>
> 여기까지 해야 오라클 DB연결됨

```java
Class.forName(driver);
con=DriverManager.getConnection(url, user, password);
System.out.println("오라클DB 서버 연결 성공!!");
```



### SQL문 작성 및 변환



###### 연습1-행추가) '이강인'의 국영수 점수와 평균 점수를 수정.

```java
String uname="이강인";
int kor=95,eng=40,mat=60;
int aver=(kor+eng+mat)/3;

StringBuilder sql=new StringBuilder();
sql.append(" UPDATE sungjuk ");
sql.append(" SET kor=?,eng=?,mat=?,aver=? ");//?는 변수처리
sql.append(" WHERE uname=? ");//'?'라고 하면 문자열 ? 이기 떄문에 따옴표 삭제 후 ?

pstmt=con.prepareStatement(sql.toString());
// ? 순서와 ?에 들어갈 자료형 주의 -> ?의 갯수와 자료형이 일치되어야한다.
pstmt.setInt(1, kor);//1-첫번째 ?, kor칼럼
pstmt.setInt(2, eng);//2->두번째 ?, eng칼럼
pstmt.setInt(3, mat);//3->세번째 ?, mat칼럼
pstmt.setInt(4, aver);//4->네번째 ?, aver칼럼
pstmt.setString(5, uname);//5 -> 다섯번째 ?, "" uname칼럼

int count=pstmt.executeUpdate();
if(count==0){
    System.out.println("행수정 실패");
}else{
    System.out.println("행수정 성공");
}
```



###### 연습2-행 삭제) 이름 '이강인' 행을 삭제하시오

```java
StringBuilder sql=new StringBuilder();
sql.append(" DELETE FROM sungjuk ");
sql.append(" WHERE uname=? ");

pstmt=con.prepareStatement(sql.toString());
pstmt.setString(1, "이강인");

int count=pstmt.executeUpdate();
if(count==0){
    System.out.println("행삭제 실패");
}else{
    System.out.println("행삭제 성공");
}
```



### 자원반납

```java
try{
    //DB 연결 및 SQL문 작성 및 변환.
}catch(Exception e){
			System.out.println("실패!"+e);
}finally{
    
    try{
        if(rs!=null){rs.close();}
    }catch(Exception e){}				//맨 마지막

    try{
        if(pstmt!=null){pstmt.close();}	//두번째
    }catch(Exception e){}

    try{
        if(con!=null){con.close();}		// 맨 처음 열었던거
    }catch(Exception e){}

}//try
```