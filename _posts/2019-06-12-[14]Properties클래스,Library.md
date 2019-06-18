---
title:  "[14]Properties클래스,Library"
date:   2019-06-12
categories: Java
tags: 
- Java
author: "mincho8050"
---

# Properties 클래스

- Key 와 Value 를 구분해서 저장할 수 있다.
- Key 와 Value의 구분기호는 = 또는  : 추천
- HashMap이랑 비슷하게 쓰임
  - 토크나이저보다 좀 더 쓰기 편함
- 데이터를 저장할 때 확장명을 .properties로 저장한다!
  - 온전한 데이터만 저장한다.

> try~catch문 

- 파일 경로
  - 한글이 없는 파일이라 1바이트 단위로 읽음
  - FileReader 가 2바이트 단위

```java
String fileName="D:/java0514/workspace/basicJava/src/oop0612/command.properties";
FileInputStream fis=new FileInputStream(fileName);
```

- 파일 가져오기

```java
Properties pr=new Properties();
pr.load(fis);
```

- pr이 가지고 있는 것을 set으로 형변환 시킨 후 iterator로 변환

```java
Iterator iter=pr.keySet().iterator();
```

- 어디에도 =이란 말이 없지만 명령어를 쓰면 자동인식된다.
  - 그래서 전제조건을 완벽하게 해서 데이터저장을 해야한다.

```java
while(iter.hasNext()){//커서가 없을떄 까지 돌림
    String key=(String) iter.next(); //= 앞의 문자열을 가지고 오면 key값으로 인식
    String value=pr.getProperty(key);//= 뒤의 문자열을 value값으로 인식
    System.out.println(value);
}//while

/*
 * 출력값
 * 	1234
    java0514
    jdbc:oracle:thin:@localhost:1521:xe
    Oracle.jdbc.driver.OracleDriver
 */
```









------







# Library 라이브러리 생성 및 실행





### JAR (파일포맷)

- JAR(Java Archive, 자바 아카이브)

- 자바 소스 프로그램 .java > 컴파일(번역)  > 자바 목적 프로그램 .class
- .class 파일들 압축(이클립스에서 가능) > .jar > 이것을 라이브러리라고 함.
- 배포할 때 .jar로 배포.



### 

### 실행 가능한 JAR 파일 만들기

1. 해당 프로젝트 우클릭
2.  Export... 
3. Java -> Runnable JAR file -> Next
4. Launch configuration에서 main()함수가 있는 대상 클래스 선택
5. Export destination -> Browse -> jar파일 이름과 위치 지정
6. Library handling
   - Extract required libraries into generated JAR
           사용하는 라이브러리들의 class파일들이 추출되어 생성될 jar파일에 포함된
   -  Package required libraries into generated JAR
           사용하는 라이브러리들이 jar포맷 그대로 생성될 jar파일에 포함됨
   - Copy required libraries into a sub-folder next to the generated JAR
           사용하는 라이브러리 파일들이 새로 만들어진 서브 폴더에 복사
7. Finish





### jar 실행

1. 시작->cmd 명령 프롬프트
2. java -jar 파일명.jar





### 관련 포맷

- WAR (파일 포맷)
  - 웹 애플리케이션을 구성하는 자바 클래스와, 자바 서버 페이지, 관련 XML 파일 등을 묶은 압축 파일 포맷
- RAR (resource adapter archive)
  - J2EE 커넥터 아키텍처(JCA) 애플리케이션을 묶는데 사용되는 압축 포맷이다.
- EAR (enterprise archive)
  - 자바 엔터프라이스 애플리케이션에서 이용되는 압축 포맷으로, 애플리케이션 클래스 및 관련 JAR, WAR, RAR 압축 파일들을 묶는 용도로 사용된다.
- APK (Android Application Package)
  - 안드로이드 애플리케이션에서 사용되는 자바 압축 포맷의 일종이다.
