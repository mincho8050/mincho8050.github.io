---
title:  "[31]View,서브쿼리,Index,CaseWhen"
date:   2019-07-23
categories: 
- Database
tags: 
- Database
author: "mincho8050"
---









# View

물리적 테이블 : 사용자가 CREATE에 의해 생성된 실제 존재하는 테이블

논리적 테이블 : 사용자가 SQL문에 의해 가공된 테이블



### 1) 정의

- 테이블에 대한 가상의 테이블로써 테이블처럼 직접 데이터를 소유하지 않고 검색시에 이용할 수 있도록 정보를 담고 있는 객체
- 테이블 정보의 부분 집합



### 2) 사용 목적

- 테이블에 대한 보안 기능을 설정해야 하는 경우
- 복잡하며 자주 사용하는 질의 SQL문을 보다 쉽고 간단하게 사용해야 하는 경우



### 3) 뷰 생성 권한 부여

- cmd -> sqlplus / as sysdba 입력
- SQL> grant create view to 아이디;



### 4) 뷰 생성 형식

> REPLACE : 이미 존재하는 뷰의 내용을 수정함

```
CREATE OR REPLACE VIEW 뷰이름
AS [SQL문]
```



### 5) 뷰 생성하기

> CREATE VIEW 뷰이름

```
CREATE VIEW test_view
AS SELECT * FROM DUAL;
```



### 6) 뷰 삭제하기

> DROP VIEW 뷰이름

```
DROP VIEW test_view;
```



### 7) 테이블,뷰 목록 확인

```
SELECT * FROM tab;
```



### 8) 뷰생성(두번째부터는 수정)

> CREATE OR REPLACE VIEW 뷰이름
>
> AS 실제로실행할SQL명령어

```
CREATE OR REPLACE VIEW test_view
AS
    SELECT empno,ename,job,sal,deptno
    FROM DUAL;
--뷰에 등록된 SQL문 실행   
--생성된 뷰는 테이블처럼 사용가능
SELECT * 
FROM test_view;
```



### 9) 뷰의 세부 정보 확인(데이터사전)

```
SELECT * FROM user_views;
```





------







# 서브쿼리



### 1) 월급을  가장 많이 받는 직원정보 조회



#### 1-1) 최대급여

```
SELECT MAX(sal) FROM DUAL;
```

#### 1-2) 서브쿼리

```
SELECT *
FROM DUAL
WHERE sal=(SELECT MAX(sal) FROM DUAL);
```



### 2) 평균 급여보다 많은 급여를 받는 직원의 이름, 부서명, 급여 조회



#### 2-1) 평균급여

```
SELECT AVG(sal) FROM DUAL;
```

#### 2-2) 서브쿼리

```
SELECT ename,deptno,sal
FROM DUAL
WHERE sal>(SELECT AVG(sal) FROM DUAL);
```





### 3) 부서코드 10의 최고급여보다 더 많은 급여를 받는 직원목록 조회



#### 3-1) 부서코드 10의 최고급여

```
SELECT MAX(sal)
FROM DUAL
WHERE deptno=10;
```

#### 3-2) 서브쿼리

```
SELECT *
FROM DUAL
WHERE sal>(
    SELECT MAX(sal)
    FROM DUAL
    WHERE deptno=10
);
```



### 4) 손흥민과 같은 입사일에 입사한 직원들중에 박지성보다 급여를 적게 받는 사람의 이름, 급여, 입사일 조회



#### 4-1) 손흥민의 입사일

```
SELECT hiredate
FROM DUAL
WHERE ename='손흥민';
```

#### 4-2) 박지성의 급여

```
SELECT sal
FROM DUAL
WHERE ename='박지성';
```

#### 4-3) 서브쿼리

```
SELECT ename
	   ,sal
	   ,hiredate
FROM DUAL
WHERE hiredate=(
    SELECT hiredate
    FROM DUAL
    WHERE ename='손흥민'
) AND
sal<(
    SELECT sal
    FROM DUAL
    WHERE ename='박지성'
);
```





------



# Index(인덱스)

- 데이터를 빠르게 찾을 수 있는 수단
- 테이블에 대한 조회 속도를 높여 주는 자료속도
- PK칼럼은 자동을 인덱스 생성된다.
- 인덱스 생성
  - CREATE INDEX 인덱스이름
- 인덱스 삭제
  - DROP INDEX 인덱스이름
- 인덱스 수정
  - ALTER INDEX 인덱스 이름
- 인덱스 방식
  - 1) full scan
    - 처음부터 끝까지 일일이 검사하는 방식 (전수조사)
  - 2) index range scan
    - 이름이 여러개인 경우 목차를 찾아서 페이지를 찾아감, 훨씬 빠름. 별도의 메모리가 있어야 한다.
  - 3) index unique scan
    - 학번은 1개만 존재함. 유일한 값
- 실행계획보기 F10 (커서위치 중요)
- PRIMARY KEY, UNIQUE 제약조건을 만들면 해당 인덱스 페이지가 자동으로 생성





## 인덱스 테스트

테스트용 레코드 100만건 입력

- PL/SQL(Procedural Language) 프로시저
  - 절차적인 데이터베이스 프로그래밍 언어

```
CREATE TABLE emp3(
    no      NUMBER
    ,name VARCHAR2(10)
    ,sal       NUMBER
);
```



### 기본골격

```
DECLARE --프로시저 선언문
    --필요한 변수 선언
    i NUMBER := 1; -- i 변수에 1 대입(대입연산자 :=)
    name VARCHAR(20) :='kim';
    sal NUMBER :=0;
BEGIN
    -- T-SQL문 (제어문-조건문,반복문 등)
    WHILE i<=1000000 LOOP 
        IF i MOD 2 = 0 THEN --(MOD : 나머지연산자 , 같다연산자 = 하나만!)
            name := 'kim' || TO_CHAR (i);
            sal := 300;
        ELSIF i MOD 3 = 0 THEN --ELSE IF는 이렇게 작성
            name :='park' || TO_CHAR(i);
            sal := 400;
        ELSIF i MOD 5 = 0 THEN 
            name :='hong' || TO_CHAR(i);
            sal := 500;
        ELSE
            name :='shin' || TO_CHAR(i);
            sal := 250;
        END IF; --IF문 닫는 태그
        
        INSERT INTO emp3(no,name,sal)
        VALUES (i ,name ,sal);
        i := i+1;        
    END LOOP; --LOOP ~ END LOOP 마치 블라켓같은 느낌
END;
/
```

> 프로시저 전에는 0 출력, 후에는 1000000

```
SELECT COUNT(*) FROM emp3;
```



### 모조칼럼

ROWID : 행의 주소값 / ROWNUM : 행번호

```
SELECT * 
FROM emp3
WHERE ROWNUM >=1 AND ROWNUM<=10;
```



### 인덱스 사용하지 않은 경우

> 실행계획 F10 > full scan, cost 894

```
SELECT *
FROM emp3
WHERE name='kim466';
```



### name 칼럼을 기준으로 인덱스 생성하기

name 칼럼에서 조회하고 F10 계획 결과 확인하기

```
CREATE INDEX emp3_name_index(인덱스명)
ON emp3(name);
```

인덱스 사용한 경우

> 실행계획 F10 > range scan, cost 3

```
SELECT *
FROM emp3
WHERE name='kim466';
```



### 인덱스 목록

```
SELECT * FROM user_indexes;
```

> 결과값
>
> EMP3_NAME_INDEX	EMP3	NONUNIQUE   > 중복될수 있음
>
> SYS_C007084	EMP4	UNIQUE                      > PRIMARY KEY 를 설정했음 그래서 UNIQUE

```
SELECT INDEX_NAME, TABLE_NAME, UNIQUENESS
FROM user_indexes
WHERE TABLE_NAME IN ('EMP3','EMP4'); --테이블 이름 여기서는 대문자로 ! '' 안에!!
```

> name 인덱스 삭제 후 테스트

```
DROP INDEX emp3_name_index;
```



### 이름과 급여를 기준으로 인덱스 생성

```
CREATE INDEX emp3_name_sal_index
ON emp3(name,sal);
```

> full scan , cost 894

```
SELECT * 
FROM emp3
WHERE no=466;
```

> range scan , cost 17

```
SELECT * 
FROM emp3
WHERE name='kim466';
```

> full scan , cost 895

```
SELECT * 
FROM emp3
WHERE sal > 200;
```

> range scan , cost 3

```
SELECT *
FROM emp3
WHERE name='kim466' AND sal>200;
```





------





# CASE WHEN ~ THEN END 구문

형식

```
CASE WHEN 조건1 THEN 조건만족시 값1
	 WHEN 조건2 THEN 조건만족시 값2
	 WHEN 조건3 THEN 조건만족시 값3
	 ~
	 ELSE 값
END 결과 칼럼명
```





### 1) 국어점수에 따라 A, B, C, D, F학점 구하기

```
SELECT uname
            ,kor
            ,CASE WHEN kor>=90 THEN 'A학점'
                       WHEN kor>=80 THEN 'B학점'
                       WHEN kor>=70 THEN 'C학점'
                        WHEN kor>=60 THEN 'D학점'
                       ELSE 'F학점'
            END AS grade
FROM sungjuk; 
```

> DECODE() 함수 사용
>
> - 값을 비교하여 해당하는 값을 돌려주는 함수
> - 단, 비교시에 정확히 같은 값(=)만 비교가 가능함.
> - DECODE(A, B, 같을때의 값, 다를때의 값)

```
SELECT uname
            ,TRUNC(((kor+eng+mat)/3)/10)
            ,DECODE(
                TRUNC(((kor+eng+mat)/3)/10),10, 'A'
                                            ,9, 'A'
                                            ,8, 'B'
                                            ,7, 'C'
                                            ,6, 'D'
                                            ,'F'
            ) grade
FROM sungjuk;
```



### 2) addr(주소)칼럼의 주소를 한글로 조회

```
SELECT uname
            ,addr
            ,CASE   WHEN addr='Seoul' THEN '서울'
                        WHEN addr='Jeju' THEN '제주'
                        WHEN addr='Busan' THEN '부산'
                        ELSE '수원'
                        --WHEN addr='Suwon' THEN '수원' 이렇게 작성해도됨
            END AS kor_name
FROM sungjuk;
```



### 3) 부서코드 10 경리팀, 20 연구팀, 30 총무팀, 40 전산팀

```
SELECT ename
            ,deptno
            ,CASE   WHEN deptno=10 THEN '경리팀'
                        WHEN deptno=20 THEN '연구팀'
                        WHEN deptno=30 THEN '총무팀'
                        WHEN deptno=40 THEN '전산팀'
            END AS deptno_name
FROM emp;
```

> DECODE() 함수

```
--DECODE(A, B, 같을때의 값, 다를때의 값)
SELECT ename
            ,deptno
            ,DECODE(
                deptno, 10,'경리팀'
                            ,20,'연구팀'
                            ,30,'총무팀'
            ) 부서
FROM emp;
```



### 4) 커미션 5이상 '5%', 4이상 '4%', 3이상 '3%', 2이상 '2%' 나머지 '없음'

```
SELECT ename
            ,comm
            ,CASE   WHEN NVL(comm,0)>=5 THEN '5%'
                        WHEN NVL(comm,0)>=4 THEN '4%'
                        WHEN NVL(comm,0)>=3 THEN '3%'
                        WHEN NVL(comm,0)>=2 THEN '2%'
                        ELSE '없음'
            END AS bonus
FROM emp;
```

