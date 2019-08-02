---
title:  "[28]제약조건,DISTINCT,GROUP_BY"
date:   2019-07-20
categories: 
- Database
tags: 
- Database
author: "mincho8050"
---









# 제약조건(constraint)

정의 

- 테이블의 해당 칼럼에 원치않는 데이터가 입력/변경/삭제되는 것을 방지하기 위해 테이블 생성 또는 변경 시 설정하는 조건

종류

- PRIMARY KEY : 기본키, 유일성 NULL값을 인정하지 않음 -> 테이블당 1개만 가능
- FOREIGN KEY : 외래키, 자식테이블이 부모테이블 칼럼을 참조 -> REFERENCES  부모테이블(칼럼명) 
- UNIQUE : 중복을 허용하지 않음 -> 테이블에 1개 이상 가능
- CHECK : 특정 데이터만 입력가능
- NOT NULL : 빈값을 허용하지 않음

예시

- 주민번호는 PK / 이메일,핸드폰번호는 UNIQUE





## 제약조건을 반영한 테이블 생성

제약조건은 중복사용 가능

> CONSTRAINT 제약조건명 은 생략할 수 있음 
>
> > 예시) PRIMARY KET 만 쓸 수 있음

```
CREATE TABLE c_emp(
    id NUMBER(5) CONSTRAINT c_emp_id_pk PRIMARY KEY
    ,name VARCHAR2(25) CONSTRAINT c_emp_name_nn NOT NULL
    ,salary NUMBER(7,2) DEFAULT 0 --제약조건은 아닌데, 데이터가 입력되지 않으면 dafault 값에 0을 넣으라는것. /SYSDATE도 넣을 수 있음
    ,phone VARCHAR2(15) CONSTRAINT c_emp_phone_ck CHECK(phone LIKE '1234-%')       --()안에 입력데이터를 지정할 수 있음. 기본적인 sql문을 작성할 수 있음
                                                                                                                                         --앞글자가 무조건 1234가 와야한다는 조건임!
    ,dept_id NUMBER(7) CONSTRAINT c_emp_dept_id_fk REFERENCES dept(deptno)
);
```



### 1-1) 제약조건 이름 검색하기

제약조건 목록 확인(데이터사전)

> 테이블 네임으로 검색할때는 대문자!

```
SELECT * FROM user_constraints;
SELECT * FROM user_constraints WHERE table_name='C_EMP(테이블명)';
SELECT * FROM user_constraints WHERE table_name='DEPT(테이블명)';
```



### 1-2) 제약조건 삭제

제약조건은 수정할 수 없고 삭제만 가능

```
ALTER TABLE c_emp(테이블명) DROP CONSTRAINT c_emp_name_nn(제약조건이름);
```



### 1-3) 제약조건 추가

```
ALTER TABLE c_emp(테이블명) ADD CONSTRAINT c_emp_name_un(제약조건이름) UNIQUE(name);
```



### 1-4) NOT NULL 제약조건

NOT NULL 제약조건은 ADD로 할 수 없고 MODIFY로 가능

```
ALTER TABLE c_emp(테이블명) 
MODIFY (name(칼럼명) VARCHAR2(25) CONSTRAINT c_emp_name_nn(제약조건이름) NOT NULL);
```





### 2-1) id 칼럼에 PK제약 조건 추가하기

```
ALTER TABLE c_emp(테이블명) ADD CONSTRAINT c_emp_id_pk(제약조건이름) PRIMARY KEY(id(칼럼));
```



### 2-2) CHECK 제약조건

> CHECK 제약조건에 걸렸을 때의 오류 메시지
>
> ORA-02290: check constraint (JAVA0514.C_EMP_CK) violated

```
CREATE TABLE 테이블명(
    ,칼럼명 NUMBER(7,2) CONSTRAINT 제약조건이름 CHECK(칼럼명 BETWEEN 100 AND 1000)
);
```



### 2-3) UNIQUE 제약조건

> 오류메시지
>
> ORA-00001: unique constraint (JAVA0514.C_EMP_UN) violated

```
CREATE TABLE 테이블명(
    ,칼럼명 VARCHAR2(25)(자료형) CONSTRAINT 제약조건이름 UNIQUE 
);
```



### 2-4) FOREIGN KEY 제약조건

> 부모테이블에 없는 데이터가 들어가려고 하면 오류남.
>
> ORA-02291: integrity constraint (JAVA0514.C_EMP_DEPT_ID_FK) violated - parent key not found

```
CREATE TABLE 자식테이블(
    ,칼럼명 NUMBER(7) CONSTRAINT 제약조건이름 REFERENCES 부모테이블 (부모테이블칼럼명)
);
```



------



# DISTINCT

칼럼에 중복내용이 있으면 대표값 1개만 출력

- 형식

```
DISTINCT(칼럼명)
```

- 예시

```
SELECT DISTINCT(칼럼) FROM 테이블명;
```





------





# GROUP BY 절

칼럼에 동일 내용끼리 그룹화를 시킨다. 

대표값 1개만 출력

GROUP BY에 의한 결과값이 오로지 1개만 존재하는 값만 조회할 수 있어서 집계함수와 많이 쓰인다

- 형식

```
GROUP BY 칼럼명1, 칼럼명2, ~
```

- 집계함수

```
SELECT
    COUNT(*)    --레코드 갯수
    ,SUM(칼럼)   --칼럼 합계
    ,AVG(칼럼)   --칼럼 평균
    ,MAX(칼럼)   --칼럼 최고점
    ,MIN(칼럼)   --칼럼 최저점
FROM 테이블명;
```



## 성적테이블(연습용)로 GROUP BY절 연습하기

- 성적 테이블 구조

```
CREATE TABLE sungjuk (
    sno NUMBER NOT NULL PRIMARY KEY
    ,uname VARCHAR(50) NOT NULL
    ,kor NUMBER(3) NOT NULL CHECK(kor BETWEEN 0 AND 100)
    ,eng NUMBER(3) NOT NULL CHECK(eng BETWEEN 0 AND 100)
    ,mat NUMBER(3) NOT NULL CHECK(mat BETWEEN 0 AND 100)
    ,tot NUMBER(3) DEFAULT 0
    ,aver NUMBER(5,2) DEFAULT 0
    ,addr VARCHAR(30) CHECK(addr IN ('Seoul','Jeju','Suwon','Busan'))
    ,wdate DATE DEFAULT SYSDATE
);
```

- 성적 데이터 입력(행 삽입)

```
insert into sungjuk(sno,uname,kor,eng,mat,addr)
values(sungjuk_seq.nextval,'무궁화',40,50,20,'Seoul');

insert into sungjuk(sno,uname,kor,eng,mat,addr)
values(sungjuk_seq.nextval,'진달래',90,50,90,'Jeju');

insert into sungjuk(sno,uname,kor,eng,mat,addr)
values(sungjuk_seq.nextval,'개나리',20,50,20,'Jeju');

insert into sungjuk(sno,uname,kor,eng,mat,addr)
values(sungjuk_seq.nextval,'봉선화',90,90,90,'Seoul');

insert into sungjuk(sno,uname,kor,eng,mat,addr)
values(sungjuk_seq.nextval,'나팔꽃',50,50,90,'Suwon');

insert into sungjuk(sno,uname,kor,eng,mat,addr)
values(sungjuk_seq.nextval,'선인장',70,50,20,'Seoul');

insert into sungjuk(sno,uname,kor,eng,mat,addr)
values(sungjuk_seq.nextval,'소나무',90,60,90,'Busan');

insert into sungjuk(sno,uname,kor,eng,mat,addr)
values(sungjuk_seq.nextval,'참나무',20,20,20,'Jeju');

insert into sungjuk(sno,uname,kor,eng,mat,addr)
values(sungjuk_seq.nextval,'홍길동',90,90,90,'Suwon');

insert into sungjuk(sno,uname,kor,eng,mat,addr)
values(sungjuk_seq.nextval,'무궁화',80,80,90,'Suwon');
```



### 문) 각 주소별 인원수

```
SELECT addr,COUNT(*)
FROM sungjuk
GROUP BY addr;
```

#### 주소별 인원수를 구한 후 주소순으로 정렬

```
SELECT addr,COUNT(*)
FROM sungjuk
GROUP BY addr
ORDER BY addr;
```

#### 주소별 인원수를 구한 후 인원수 순으로 내림차순 정렬

```
SELECT addr,COUNT(*)
FROM sungjuk
GROUP BY addr
ORDER BY COUNT(*) DESC;
```

#### 주소별 국어점수 평균을 구한 후 국어점수 평균순으로 내림차순 정렬

```
SELECT addr,ROUND(AVG(kor),2)
FROM sungjuk
GROUP BY addr
ORDER BY (AVG(kor) DESC;
```

#### 지역별 국,영,수 최고점을 지역별 순으로 정렬해서 조회

```
SELECT addr
	   ,MAX(kor)
	   ,MAX(eng)
	   ,MAX(mat)
FROM sungjuk
GROUP BY addr
ORDER BY addr;
```



### 문) 지역별로 그룹핑을 하고, 만일 지역이 같다면 2차 그룹으로 수학 점수별 그룹핑 하기

1차 값이 동일하다면 그 그룹내에서 2차 그룹이 가능하다!!

```
SELECT addr,mat,COUNT(*)
FROM sungjuk
GROUP BY addr,mat;
```



### 문) aver 칼럼값 구하고 aver칼럼값이 50인 이상 레코드 대상으로 지역별 국영수 평균을 반올림 소수점 한자리까지 구한 후 조회



#### 평균구하기

```
UPDATE sungjuk
SET aver=(kor+eng+mat)/3;
```

#### aver칼럼 50 이상 조회

```
SELECT addr,aver
FROM sungjuk
WHERE aver>=50;
```

#### 지역별 국영수 평균 반올림 소수점 한자리 조회

```
SELECT addr
	   ,ROUND(AVG(kor),1)
       ,ROUND(AVG(eng),1)
       ,ROUND(AVG(mat),1)
FROM sungjuk
WHERE aver>=50
GROUP BY addr;
```



## GROUP BY 와 쓰는 조건절

HAVING 조건절

- 그 외에 WHERE조건절과 ON 조건절 (테이블 조인할때!) 사용하는 조건절 들이 있다.

```
SELECT addr,COUNT(*)
FROM sungjuk
GROUP BY addr
HAVING COUNT(*)=3
```



### 문) 지역별 국어점수 평균을 구한 후 그 평균이 80점 이상인 지역만 조회

```
SELECT addr,ROUND(AVG(kor),0)
FROM sungjuk
GROUP BY addr
HAVING AVG(kor)>=80;
```

#### 국어평균 60~79점 사이만 조회

```
SELECT addr,ROUND(AVG(kor),0)
FROM sungjuk
GROUP BY addr
HAVING AVG(kor) BETWEEN 60 AND 79;
```




