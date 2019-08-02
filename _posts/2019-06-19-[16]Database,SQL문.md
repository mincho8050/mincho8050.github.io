---
title:  "[16]Database,SQL문"
date:   2019-06-19
categories: 
- Database
tags: 
- Database
author: "mincho8050"
---

# Database

- 다수의 인원, 시스템 또는 프로그램이 사용할 목적으로 통합하여 관리되는 데이터의 집합

### 데이터베이스의 개념

- DataBase Management System
- 데이터의 중복을 막을 수 있고 자료의 일관성을 유지할 수 있다.
- 데이터를 쉽게 검색 할 수 있고 데이터의 통합이 쉽다.
- 여러 사람이 함께 자료를 열람 할 수 있고 보안을 적용 하기가 쉽다.
- 대용량의 기억공간를 제공

### DBMS의 종류 및 규모

- My-SQL : 중소 기업, PHP, JAVA.
- SQLite : 스마트폰 및 임베디드 기기의 내장 데이터베이스로 사용, Android, iPhone.
- Oracle : 중대규모 기업의 데이터베이스로 사용, JAVA.
- MS-SQL : 중소 기업, MS기반 운영체제만 사용 가능, ASP.NET.
- 오피스 : ACCESS(.mdb)
- Google Cloud : Java, Perl

###### 데이터베이스 종류

1. 파일시스템
2. 관계형 데이터베이스 관리시스템(RDBMS)
   - 가장 보편화된 데이터베이스 관리시스템이다.
   - SQL문이 필요
   - Oracle DB , MySQL , Maria DB
3. NoSQL 데이터베이스
   - SQL문이 없다.
   - Mongo DB

### 데이터베이스 관리시스템(DBMS) 특징

- 데이터 무결성
- 데이터 일관성
- 데이터 회복성
- 데이터 보안성
- 데이터 효율성



------



# CSV 변환

- CSV는 , 로 구분기호가 되어있는 파일이다. 이걸로 데이터 저장을 많이 한다.
- 만약 받은 데이터가 txt파일이라면
  - 엑셀 ->  열기 -> 데이터.txt -> 구분기호 및 텍스트 선택 해주고 엑셀파일로 변환 -> 다시 csv 파일로 변환하여 저장하고 사용한다.

**임포트툴 사용**

sqldeveloper -> 접속 -> myOracle11g(DB명) -> 테이블(우클릭) -> 데이터임포트

**직접 cmd를 통한 데이터 입력**

> 일단 테이블구조를 넣은 테이블명.ctl 파일을 생성한다.
>
> 그리고 이 작업을 할때는 왠만하면 하기 편하도록 csv 파일과 ctl 파일을 같은 경로에 위치시킨다.

```
LOAD DATA INFILE '저장할데이터자료.csv'
INSERT
INTO TABLE 테이블명
FIELDS TERMINATED BY ','		--구분기호
TRAILING NULLCOLS
(칼럼1,칼럼2,칼럼3,칼럼4,칼럼5,칼럼6)
```

- cmb > 파일을 저장한 경로로 이동. 
  - -> sqlldr(컨트롤파일 실행할때 명령어) 아이디/비번 control="파일명.확장자"

------







# SQL문

1. **DCL명령어 Data Control Language 제어어**    -  사용자가 별로 쓸일 없음. 호스팅업체가 주로 사용    - grant (사용자 접근 권한부여)    - revoke (사용자 접근 권한 취소)    - deny (특정사용자만 접근차단)
2. **DDL명령어 Data Definition Language 정의어**    -  DB및 TABLE에 대한 정의    - Create (생성)    - Drop (삭제)    - Alter (수정)
3. **DML명령어 Data Manipulation Language 조작어**    -  레코드작업    - Select (조회 및 검색)    - insert (삽입)    - update (수정)    - delete (삭제)



- sungjuk 테이블 삭제

```
drop table sungjuk;
```

- sungjuk 테이블 생성

> 한글은 2byte라 5글자 들어감. 한글 인코딩에 따라 3byte도 들어감. 영문은 10글자.
>
> not null	:	빈값 허용 하지 않음 , 이걸쓰지않으면 빈값허용.

```
create table sungjuk(
	uname varchar(20) not null,
	kor int not null,
    eng int not null,
    mat int not null,
    aver int
);
```

- sungjuk 테이블 구조 확인

```
desc sungjuk;
```

- sungjuk 테이블 구조 수정

> insert 한 table에 대해 수정하는건 update
>
> create한 table에 대해 수정하는 건 alter
>
> > alter를 많이쓰는건 애초부터 구조를 잘 못 했기때문에 좋은건 아님.
>
> alter table 테이블명 rename column 원래칼럼명 to 바꿀컬럼명; -> 컬럼이름변경

```
alter table sungjuk rename column kor to korea;
```

- 칼럼 삭제

> alter table 테이블명 drop(칼럼명);

```
alter table sungjuk drop(korea);
```

- 칼럼 추가

> alter table 테이블명 add (칼럼명 데이터타입)

```
alter table sungjuk add (total int);
```

###### 연습

- sungjuk 테이블을 아래와 같이 수정하시오.

1. uname 칼럼의 글자수를 50개로 수정

> alter table 테이블명 modify (수정할 사항);

```
alter table sungjuk modify (uname varchar(50));
```

2. uname 칼럼을 null 조건으로 수정

```
alter table sungjuk modify (uname varchar(50) null);
```



------



# Transaction

- 데이터 파일의 내용에 영향을 미치는 모든 거래
- insert , update , delete 쿼리가 사용되는 경우 Transaction 상태가 된다.
- 데이터 변형되면 상황에 따라 복구되어야 하는 상태가 필요한 경우 명령어를 이용하여 최초 상태로 데이터를 돌릴 수 있다.
- commit work(commit)

동시에 다수의 작업을 독립적으로 안전하게 처리하기 위한 상호작용 단위

- 일 처리 단위
- 분할할 수 없는 최소단위

목적

- 안전한 거래를 보장

특징

- 원자성 : 트랜잭션은 한꺼번에 완료되거나 한꺼번에 취소 되어야 함
- 일관성
- 고립성
- 지속성

TCL 명령어

- COMMIT : 거래 내역을 확정함
- ROLLBACK : 거래 내역을 취소함
- CHECKPOINT : ROLLBACK할 위치를 지정함

> 명령완료
>
> 변경된 데이터 확인 후 데이터 영역에 적용

```
commit;
```

- rollback work(rollback)

> 명령 취소
>
> 변경된 데이터를 취소하고 원래대로 복구한다.
>
> sqlplus는 자동커밋된다.

```
rollback;
```



## 실습

cmd > sqlplus에서 테스트

시작 -> cmd -> sqlplus 아이디/비번



**테이블 생성**

```
CREATE TABLE dep(
    id  VARCHAR2(20)    PRIMARY KEY
    ,name   VARCHAR2(15)    NOT NULL
    ,location   VARCHAR2(50)
);
```

**테이블 구조 확인**

```
DESC dep;
```

**행추가**

```
INSERT INTO dep VALUES('10','영업부','서울 강남구');
INSERT INTO dep VALUES('20','개발부','부산 동래구');
INSERT INTO dep VALUES('30','회계부','인천 계양구');
```



#### 명령어 한꺼번에 모두 다 취소 (ROLLBACK)

> 행 갯수 카운트 하면 0이 됨.
>
> 방금전에 했던, 15분 이전까지 했던 모든 작업들을 취소하는 것
>
> 명령어 한꺼번에 취소

```
ROLLBACK;
```



#### 명령어 완료 및 확정 (COMMIT)

> COMMIT 하지 않고 그냥 cmd창을 빠져나오면 다시 행갯수가 0이 된다.
>
> 이클립스나 외부 프로그램을 연결해서 쓸때는 자동커밋이 되는데. 이렇게 직접 cmd창으로 할 때는 자동저장 되지 않기 때문에 COMMIT을 해줘야 한다.
>
> sqlplus에서는 그냥 창닫기를 하면 자동 ROLLBACK되므로 주의!

```
COMMIT;
```



#### 롤백시점 지정(CHECKPOINT)

```
INSERT INTO dep VALUES('40','영업부','서울 종로구');

SAVEPOINT a; --ROLLBACK TO a;  하면 여기 이후부터 취소

INSERT INTO dep VALUES('50','개발부','서울 중구');

SAVEPOINT b;

INSERT INTO dep VALUES('60','관리부','서울 마포구');
```

> 이렇게 하고 롤백취소

```
ROLLBACK TO a;
```





## COMMIT한 자료의 복구 방법

```
SQL> SHOW PARAMETER UNDO;
```

- undo_retention
- DELETE , UPDATE후에 커밋 할 경우 속성값의 시간(초)까지는 오라클에서 임시로 저장한 데이터로 복구할 수 있다.
- DEFAULT 속성값은 900초(15분 정도->변경가능 -> COMMIT을 했을 경우 15분간만 값 저장)
- 시간을 1500초(25분)으로 늘릴려면

```
ALTER SYSTEM SET undo_retention=1500;
```



### undo_retention 기능을 이용한 데이터 복구 방법



**15분 이내에 dep테이블에서 커밋된 데이터 출력**

> 일시적이고 실시간적임

```
SELECT *
FROM dep
AS OF TIMESTAMP(SYSTIMESTAMP-INTERVAL '15' minute);
```

**삭제하고 커밋한지 15분 안의 데이터를 복구**

```
INSERT INTO dep SELECT * FROM dep
AS OF TIMESTAMP(SYSTIMESTAMP-INTERVAL '15' minute);
```





------





# Table

- Schema(스키마)
- 물리적 스키마(테이블)
  - create , insert 등을 직접한 값들
- 논리적 스키마(테이블)
  - 조인 , 뷰 등을 통해 만들어진 값들
- 데이터베이스 저장 단위
- CRUD
  - Creat , Read , Update , Delete



> 예시

```
성적	-> 테이블명
-------------------------
번호 이름 국어 영어 수학 평균
-------------------------
1 	무궁화 90  80  70  80	-> 줄row , record,행,Tuple > 한사람에대한모든정보 > 2줄 있음
2 	개나리 90  80  70  80
칼럼column, 필드, 열,  Attribute > 6개있음
```



###### 테이블 생성

```
create table 테이블명(
	칼럼1 자료형 제약조건,
	칼럼2 자료형 제약조건,
	칼럼3 자료형 제약조건
);
```

> 예시
>
> > -- 표시는 주석

```
create table sungjuk(
	snum int,			--번호 , 일단 제약조건은 제외 , 숫자형
	uname varchar(10),  --이름,10글자 이내만 허용하겠다. 문자열형 ' '
	kor int,			--국어
	eng int,			--영어
	mat int,			--수학
	aver int			--평균
);
```



###### 테이블 삭제

```
drop table 테이블명;
```



###### 행 추가 - **Create**

```
insert into 테이블명(칼럼1, 칼럼2 ~)	--칼럼명은 생략가능, 만약 생략하면 칼럼 순서대로 값 입력.
values(값);
```

> 예시
>
> > 평균은 만들어진값 칼럼을 이용해 만들 수 있다.

```
insert into sungjuk(snum,uname,kor,eng,mat)
values(1,'무궁화',80,90,70);
insert into sungjuk(snum,uname,kor,eng,mat)
values(2,'개나리',80,90,70);
insert into sungjuk(snum,uname,kor,eng,mat)
values(3,'라일락',100,90,75);
```

> 칼럼명이 생략되면 values() 값은 테이블 설계순으로 입력
>
> > 자바에서는 이렇게 쓰면 유지보수때 힘듦.

```
insert into sungjuk
values('이강인',90,65,50,70);
```

> 복수 레코드 행 추가

```
INSERT INTO sungjuk (snum,uname,kor,eng,mat)
VALUES('아이린',90,95,30,80),
VALUES('강슬기',95,65,50,60),
VALUES('박수영',100,65,50,70);
```



###### 조회 및 목록 - **Read**

```
select 칼럼명 from 테이블명;
```

> 예시

```
select snum,uname from sungjuk;
select snum,uname,kor,eng,mat from sungjuk; --칼럼추가
select * from sungjuk; --전체 모든 칼럼 조회
```



###### 행 수정 - **Update**

> 백업이 없으면 영구히 수정

```
update 테이블명 set 칼럼1=값1, 칼럼2=값2, ~~;
```

> 예시

```
update sungjuk set aver=(kor+eng+mat)/3;	--데이터베이스에서도 자바랑 똑같은 연산자
```



###### 행삭제 - **Delete**

> 백업이 없으면 영구히 삭제

```
delete from 테이블명;
```

> 예시
>
> > select count(*) from 테이블명; -- 남아있는 행 갯수

```
delete from sungjuk;
```



###### 연습

- address 테이블 생성.

1. 테이블 생성

```
create table address(
	Address1 varchar(255),
	Address2 varchar(255),
	Address3 varchar(255),
	Postal_Code varchar(255),
	Client_ID varchar(255)
);
```

1. 행 추가

```
insert into address(Address1,Address2,Address3,Postal_Code,Client_ID)
values('서울특별시','성동구 XXehd','대신아파트 1동 101호','09100','121');
```

1. 조회 및 목록

```
select * from address;
```

> as (생략가능)

- 특정 칼럼명, 테이블명을 일시적으로 리네임

```
select kor as korea, eng as english,mat from sungjuk;
[sungjuk테이블 샘플데이터]

insert into sungjuk(uname,kor,eng,mat)
values ('홍길동',50,60,30);

insert into sungjuk(uname,kor,eng,mat)
values ('무궁화',30,30,40);

insert into sungjuk(uname,kor,eng,mat)
values ('진달래',90,90,20);

insert into sungjuk(uname,kor,eng,mat)
values ('개나리',100,60,30);

insert into sungjuk(uname,kor,eng,mat)
values ('라일락',30,80,40);

insert into sungjuk(uname,kor,eng,mat)
values ('봉선화',80,80,20);

insert into sungjuk(uname,kor,eng,mat)
values ('대한민국',10,65,35);

insert into sungjuk(uname,kor,eng,mat)
values ('해바라기',30,80,40);

insert into sungjuk(uname,kor,eng,mat)
values ('나팔꽃',30,80,20);

insert into sungjuk(uname,kor,eng,mat)
values ('대한민국',100,100,100);
```





------



# count() 함수

> 레코드 갯수(행갯수)
>
> null 값은 카운트 대상이 아니다.

```
select count(uname) from sungjuk;
select count(uname) as cnt from sungjuk;
select count(uname) cnt from sungjuk;
```

> 국어점수 합산 , 그 중 최고점 , 최저점 조회 및 평균

```
select sum(kor) as hap,
max(kor) as maximum,
min(kor) as minimum,
avg(kor) as average 
from sungjuk;
```

> 국영수 각 과목의 평균점수 조회하기

```
select avg(kor) as avg_kor,
avg(eng) as avg_eng,
avg(mat) as avg_mat
from sungjuk;
```





# 정렬 (sort)

> 오름차순 ASCending	ASC	--기본값
>
> 내림차순 DESCending	DESC
>
> java로 DB를 가져올 땐 정렬해서 가져오기
>
> order by 칼럼1,칼럼2,칼럼3,~~
>
> > 칼럼 1을 기준으로 내림차순 정렬하고 , 칼럼1값이 동일하다면 칼럼2를 기준으로 오름차순 정렬하고 칼럼2값도 동일하다면 칼럼3을 기준으로 오름차순으로 정렬.
> >
> > > order by 칼럼1 desc,
> > >
> > > ​	칼럼2 asc,
> > >
> > > ​	칼럼3, ~~~	--아무것도 안쓰여져 있으면 기본값 오름차순

```
select uname
from sungjuk
order by uname;		--오름차순 , asc 생략가능, 기본값

select uname,kor
from sungjuk
order by kor desc;	--내림차순
```

- 2차 정렬

> 국어 점수가 동일하다면 이름으로 정렬

```
select kor,uname
from sungjuk
order by kor,uname;
```

- 3차 정렬

```
select kor,eng,mat,uname
from sungjuk
order by kor,eng,mat;
```

- 맨 위 1건만 조회

  ```
  SELECT * FROM salaries
  ORDER BY from_date DESC LIMIT 1;
  ```

  

  

  ------

  

  

# 조건절

- **where 조건절**
  - 조건에 만족하는 레코드만 대상
- **having 조건절**
  - group by 절과 같이 사용
- **on 조건절**



###### where 조건절

- 특정한 레코드를 수정, 삭제, 조회하기 위해 사용
- 산술연산자	:	+ - * /
- 비교연산자 : < <= > >= !=(<>) =(같다,대입연산자)
- 논리연산자 : and or not

> dual 테이블
>
> 결과값을 일시적으로 출력할 때 유용한 임시 테이블

```
select 5+3 from dual;
select 5-3 from dual;
select 5*3 from dual;
select 5/3 from dual;
select mod(5,3) from dual;			--나머지함수
```



### 조건절 연습

- 국어점수 90이상 조회

```
select kor,uname
from sungjuk
where kor>=90;
```

- 수학점수 50점 미만 조회

```
select mat,uname
from sungjuk
where mat<50;
```

- 이름이 '무궁화' 조회

```
select uname,kor,eng,mat
from sungjuk
where uname='무궁화';
```

- 이름이 '대한민국' 행의 평균을 구하시오

```
update sungjuk
set aver=(kor+eng+mat)/3
where uname='대한민국';
```

- 비어있는 aver 칼럼을 조회하시오

> where aver=null; 이렇게 하는 것은 틀린방법

```
select uname,aver
from sungjuk
where aver is null;
```

- 비어있는 aver 칼럼의 갯수를 구하시오

```
select count(*)
from sungjuk
where aver is null;
```

- 비어있지 않은 aver 칼럼의 갯수

```
select count(*)
from sungjuk
where aver is not null;
```

- 국영수 모두 100점의 행을 평균을 구하시오

```
update sungjuk
set aver=(kor+eng+mat)/3
where kor=100 and eng=100 and mat=100;
```

- 국영수 모두 50미만인 행의 평균을 구하시오

```
update sungjuk
set aver=(kor+eng+mat)/3
where kor<50 and eng<50 and mat<50;
```

- 비어있는 평균값을 모두 채우시오

```
update sungjuk
set aver=(kor+eng+mat)/3;
where aver is null;
```

- 평균 60~69점 사이의 평균, 이름 조회

```
select aver,uname
from sungjuk
where aver>=60 and aver<=69;
```

###### between A and B

```
select aver,uname
from sungjuk
where aver between 60 and 69;
```

- 국영수 중에서 한과목이라도 40점 미만 조회(과락)

```
select *
from sungjuk
where kor<40 or eng<40 or mat<40;
```

- 이름 '진달래' , '개나리' , '라일락' 조회

```
select *
from sungjuk
where uname='진달래' or uname='개나리' or uname='라일락';
```

###### in 연산자

> 일치하는 값만 찾음

```
select *
from sungjuk
where uname in('개나리','진달래','라일락');

select *
from sungjuk
where kor in(80,90,100);
```

###### like 연산자

- 문자열에서 비슷한 유형을 찾을 때 사용
- 문자열에서 만능문자(% _)를 이용해 비슷한 유형을 조회

```
select uname
from sungjuk
where uname like'홍%';	--홍으로 시작하는 문자열
						-- '%화' > 화로 끝나는 문자열
						-- '%나%' > 나 문자열이 포함되어 있는지
						--'_화' > 두글자중에서 화로 끝나는 문자열 / 글자수제한할때 _ 사용
						--'__화'> 세글자 중에서 화로 끝나는 문자열
						--'_나_' > 세글자 중 두번째 글자가 나 인 문자열
```



#### 문) 자유게시판 검색 시 제목+내용을 선택하고 검색어 happy를 입력했을 때 레코드를 조회

> 제목 또는 내용 둘 중에 하나라도 들어가면 검색되도록
>
> 제목은 칼럼명 subject , 내용은 content
>
> 나중에는 happy를 변수처리해야함.

```
where subject like 'happy'		--5글자가 happy인 것을 찾는것.
where subject like 'happy%'		--앞글자가 happy인 것.
where subject like '%happy'		--끝나는글자가 happy인것.
where subject like '%happy%'	--제목 중간에 happy가 들어가있는것.	
where subject like '%happy%' or content like '%happy%' --제목,내용 둘 중 하나라도!
```



