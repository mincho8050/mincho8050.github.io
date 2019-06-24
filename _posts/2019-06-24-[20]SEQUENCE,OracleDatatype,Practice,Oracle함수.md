---
title:  "[20]SEQUENCE,OracleDatatype,Practice,Oracle함수"
date:   2019-06-24
categories: 
- Database
tags: 
- Database
author: "mincho8050"
---

# SEQUENCE 시퀀스

- 오라클에서 자동으로 일련번호를 부여
- 기술방법
  - 시퀀스명
  - INCREMENT BY 증가값 : 시작 값에서 증가할 값을 기술
  - START WITH 시작값 : 시퀀스의 초기값
  - MAXVALUE 최댓값 : 시퀀스의 최종값
  - MINVALUE 최솟값 : 시퀀스의 최솟값
  - CYCLE 반복횟수 : 반복횟수
  - CHACHE : 캐시 메모리에 저장한다
  - ORDER : 요청 순서대로 생성
- 시퀀스 생성

> 옵션지정하지 않으면 자동으로 1부터 시작해서 1씩 증가

```
CREATE SEQUENCE 시퀀스명;
```

>옵션 예시

```
CREATE SEQUENCE 시퀀스명
INCREMENT BY  증가값
START WITH 시작값;
```

- 시퀀스에서 일련번호 발생

```
시퀀스명.NEXTVAL
```

- 시퀀스 삭제

```
DROP SEQUENCE 시퀀스명;
```



# Oracle DB 자료형

1. 숫자형
   - INT 			 표준형
   - NUMBER    정수형
   - NUMBER()  실수형
     - ex ) NUMBER(5,2) -> 최대 5자리, 소수점2자리까지 표현 -> (999.99) 까지 표현가능
2. 문자형
   - CHAR               표준형 , 고정형
     - ex) CHAR(5) : 5글자를 잡음 -> 'SKY  ' : 2칸의 공백이 있음.
     - ex) 우편번호
     - 속도는 고정형이 빠름
   - VARCHAR        표준형 , 가변형
     - ex) VARCHAR(5) : 5글자 잡음 -> 'SKY' : 남은 2칸은 제외됨. 가변형!
     - ex) 아이디, 비밀번호, 이름, 주소 ~~ (쓰이는 곳이 많음)
   - VARCHAR2      가변형
     - 4000 까지 가능
3. 날짜형 (년월일시분초)
   - DATE
     - 현재 시스템 날짜값 함수  :  SYSDATE



# 시퀀스 및 자료형 연습

- 시퀀스 생성

```
CREATE SEQUENCE sungjuk_seq;
```

- sungjuk 테이블 생성

> not null	:	빈 값을 허용하지 않음.

```
CREATE TABLE sungjuk(
sno NUMBER NOT NULL 			--일련번호
,uname VARCHAR2(255) not null	--이름
,kor NUMBER NOT NULL			--국어
,eng NUMBER NOT NULL			--영어
,mat NUMBER NOT NULL			--수학
,aver NUMBER					--평균
,addr VARCHAR2(255)				--주소
,wdate DATE						--작성일
);
```

- 행 추가	(Sample Data)

> 칼럼 생략할 수 있지만 알아보기 어려우니 생략하지 말자.
>
> VALUES순서가 중요함
>
> 시퀀스의 일련번호로 순서를 정해주기 위함.	>	
>
> ​	sno 칼럼에 sungjuk_seq.NEXTVAL 순서를 넣어준다. 

```
INSERT INTO sungjuk(sno,uname,kor,eng,mat,addr,wdate)
VALUES(sungjuk_seq.NEXTVAL
	   ,'무궁화',100,95,80,'Seoul',SYSDATE);
INSERT INTO sungjuk(sno,uname,kor,eng,mat,addr,wdate)
VALUES(sungjuk_seq.NEXTVAL,'진달래',90,50,90,'Jeju',SYSDATE);

INSERT INTO sungjuk(sno,uname,kor,eng,mat,addr,wdate)
VALUES(sungjuk_seq.NEXTVAL,'개나리',20,50,20,'Jeju',SYSDATE);

INSERT INTO sungjuk(sno,uname,kor,eng,mat,addr,wdate)
VALUES(sungjuk_seq.NEXTVAL,'봉선화',90,90,90,'Seoul',SYSDATE);

INSERT INTO sungjuk(sno,uname,kor,eng,mat,addr,wdate)
VALUES(sungjuk_seq.NEXTVAL,'나팔꽃',50,50,90,'Suwon',SYSDATE);

INSERT INTO sungjuk(sno,uname,kor,eng,mat,addr,wdate)
VALUES(sungjuk_seq.NEXTVAL,'선인장',70,50,20,'Seoul',SYSDATE);

INSERT INTO sungjuk(sno,uname,kor,eng,mat,addr,wdate)
VALUES(sungjuk_seq.NEXTVAL,'소나무',90,60,90,'Busan',SYSDATE);

INSERT INTO sungjuk(sno,uname,kor,eng,mat,addr,wdate)
VALUES(sungjuk_seq.NEXTVAL,'참나무',20,20,20,'Jeju',SYSDATE);

INSERT INTO sungjuk(sno,uname,kor,eng,mat,addr,wdate)
VALUES(sungjuk_seq.NEXTVAL,'홍길동',90,90,90,'Suwon',SYSDATE);

INSERT INTO sungjuk(sno,uname,kor,eng,mat,addr,wdate)
VALUES(sungjuk_seq.NEXTVAL,'무궁화',80,80,90,'Suwon',SYSDATE);
```

> 레코드 갯수 확인

```
SELECT COUNT(*) FROM sungjuk;
```

- 이름 '무궁화'를 조회하시오

```
SELECT *
FROM sungjuk
WHERE uname='무궁화';
```

- 국어점수 50점 이하 조회하시오

```
SELECT *
FROM sungjuk
WHERE kor<=50;
```

- 국영수 모두 90점 이상 조회하시오

```
SELECT *
FROM sungjuk
WHERE kor>=90 and eng>=90 and mat>=90;
```

- 이름 개나리, 진달래, 무궁화 조회하시오

```
SELECT *
FROM sungjuk
WHERE uname in('개나리','진달래','무궁화');
```

- 주소가 서울인 레코드의 평균을 구하시오

```
UPDATE sungjuk
SET aver=(kor+eng+mat)/3
WHERE addr='Seoul';

SELECT *
FROM sungjuk
WHERE addr='Seoul';
```

- 수학점수 50~59점 사이 레코드를 조회하시오

```
SELECT *
FROM sungjuk
WHERE mat>=50 and mat<=59;
--또는 WHERE mat 	BETWEEN 50 AND 59;
```

- 비어있는 평균값을 모두 구하시오

```
UPDATE sungjuk
SET aver=(kor+eng+mat)/3
WHERE aver is null;

SELECT *
FROM sungjuk;
```

- 평균 70점이상 레코드를 이름순으로 정렬해서 조회하시오

> ex) 게시판테이블에서 가장 최근에 작성한 글 순서로 출력
>
> > order by wdate desc --내림차순
>
> 연결문자 ||
>
> > MySQL -> concat() 함수
> >
> > select '#'|| addr ||'#'
> >
> > from sungjuk;

```
SELECT *
FROM sungjuk
WHERE aver>=70
ORDER BY uname;
```

- aver 칼럼의 데이터타입 number(6,2)로 수정하시오

> aver 값이 있으면 안된다. null 초기화 시킨 후 구조를 바꾸고 UPDATE를 한다.
>
> 값이 있으면 구조변화가 안된다.
>
> > UPDATE sungjuk SET aver=null;

```
ALTER TABLE sungjuk
MODIFY(aver NUMBER(6,2));
```

- 수학점수 50점 미만 학생들에게 수학점수 5점씩 추가하시오

```
UPDATE sungjuk
SET mat=mat+5
WHERE mat<50;
```

- 이름에 '화' 문자가 들어가 있는 레코드만 검색

> LIKE 연산자는 문자열에 사용!!

```
SELECT *
FROM sungjuk
WHERE uname LIKE '%화%';
```

- 주소가 'Seoul' , 'Jeju'이면서 이름에 '나' 글자가 포함되어 있는 레코드 조회

```
SELECT *
FROM sungjuk
WHERE addr IN('Seoul','Jeju') AND uname LIKE '%나%';
```

- 국영수 과목의 각 평균을 구하시오

> AS 	:	특정 칼럼명, 테이블명을 일시적으로 리네임

```
SELECT AVG(kor) AS avg_kor,
AVG(eng) AS avg_eng,
AVG(mat) AS avg_mat
FROM sungjuk;
```





# Oracle 함수



### 문자열 함수

- LOWER 	:	해당 칼럼의 값을 소문자로 변환

> dual은 임시 테이블

```
SELECT LOWER('Hello World') FROM dual;
```

- UPPER	:	대문자로 변환

```
SELECT UPPER('Hello World') FROM dual;
```

- CONCAT	:	두개의 문자열을 연결

```
SELECT CONCAT('Hello','World') FROM dual;
```

- SUBSTR	:	문자열 추출

> 6번째 자리부터 전체를 추출

```
SELECT SUBSTR('HelloWorld',6) FROM dual;
```

> 1~5번째까지 추출

```
SELECT SUBSTR('HelloWorld',1,5) FROM dual;
```

- INSTR	:	문자열에서 특정문자의 위치를 세는 함수, 특정문자가 없으면 0

```
SELECT INSTR('HelloWorld', 'W') FROM dual;
```

- TRIM	:	공백을 제거

```
SELECT TRIM(' SQLPLUS') FROM dual;
```

- LTRIM	:	좌측부터 문자열에서 해당 문자를 제거

> 우측에 제거하는 기호가 몇개가 있어도 해당된다면 모두 다 제거

```
SELECT LTRIM('*SQLPLUS', '*') FROM dual;
```

- RTRIM	:	우측부터 문자열에서 해당 문자 제거

```
SELECT RTRIM('SQLPLUS*', '*') FROM dual;
```

- REPLACE	:	문자열에서 해당 문자를 다른 문자로 바꾸어 줌

```
SELECT REPLACE('SEVLTL', 'L', 'EN') FROM dual;
```





### 숫자함수

- ABS	:	절대값

```
SELECT ABS(-7) FROM dual;
```

- MOD	:	나머지 반환

> 100

```
SELECT MOD(1500, 200) FROM dual;
```

- CEIL

>2

```
SELECT CEIL(1.123) FROM dual;
```

> -1
>
> 소수점 첫째자리에서 해당 값을 올림 처리한 정수를 반환하고 해당 값보다는 크지만 가장 근접하는 최소값을 구하는 함수

```
SELECT CEIL(-1.623) FROM dual;
```

- FLOOR

> 1

```
SELECT FLOOR(1.123) FROM dual;
```

> -2
>
> 소수점 첫째자리에서 해당 값을 내림 처리한 정수를 반환하고 해당 값보다는 작지만 가장 근접하는 최대값을 구하는 함수

```
SELECT FLOOR(-1.123) FROM dual;
```

- ROUND(n,m)	:	해당 숫자 n에m자리까지 반올림

```
SELECT ROUND(17.825, 2) FROM dual;		--17.83
SELECT ROUND(17.825, 1) FROM dual;		--17.8
SELECT ROUND(17.825, 0) FROM dual;		--18
SELECT ROUND(17.825, -1) FROM dual;		--20
SELECT ROUND(17.825, -2) FROM dual;		--0
```

- TRUNC(n,m)	:	해당 숫자 n에서 m자리 까지 버림

```
SELECT TRUNC(17.825, 2) FROM dual;		--17.82
SELECT TRUNC(17.825, 1) FROM dual;		--17.8
SELECT TRUNC(17.825, 0) FROM dual;		--17
SELECT TRUNC(17.825, -1) FROM dual;		--10
SELECT TRUNC(17.825, -2) FROM dual;		--0
```



### 함수 연습

- 국영수 과목의 각 평균을 반올림해서 소수점 두 자리까지 출력 하시오.

```
SELECT
ROUND(AVG(kor),2) AS avg_kor,
ROUND(AVG(eng),2) AS avg_eng,
ROUND(AVG(mat),2) AS avg_mat
FROM sungjuk
```



### 연습

![](/_image/0624image01.PNG)

- 테이블 구조

```
CREATE TABLE emp(
empno INT 				NOT NULL
,ename VARCHAR(10) 		NOT NULL
,job VARCHAR(10) 		NOT NULL
,mrg CHAR(4) 			NOT NULL
,hiredate DATE 			NOT NULL
,sal INT				NOT NULL
,comm INT
,deptno INT	 			NOT NULL
);
```

- 행 삽입

```
INSERT INTO emp(empno,ename,job,mrg,hiredate,sal,comm,deptno)
VALUES(7369,'smith','clerk','7902','1980-12-17',800,null,20);
INSERT INTO emp(empno,ename,job,mrg,hiredate,sal,comm,deptno)
VALUES(7499,'allen','salesman','7698','1981-02-20',1600,300,30);
INSERT INTO emp(empno,ename,job,mrg,hiredate,sal,comm,deptno)
VALUES(7521,'ward','salesman','7698','1981-02-22',1200,500,30);
```

![](/_image/0624image02.PNG)

