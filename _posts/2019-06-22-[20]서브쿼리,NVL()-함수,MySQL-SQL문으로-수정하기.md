---
title:  "[20]서브쿼리,NVL() 함수,MySQL SQL문으로 수정하기"
date:   2019-06-22
categories: 
- Database
tags: 
- Database
author: "mincho8050"
---

# 서브쿼리

- 쿼리문 안에 쿼리문



### 오라클 DB > cmd 창에서 실행

- 국어점수의 평균보다 못한 레코드를 조회

> where kor<국어점수평균

```
SELECT uname,kor
FROM sungjuk
WHERE kor<(SELECT avg(kor) FROM sungjuk);
```

- 국어점수의 평균보다 잘한 레코드를 조회

```
SELECT uname,kor
FROM sungjuk
WHERE kor>(SELECT avg(kor) FROM sungjuk);
```

- 제주지역의 영어점수 평균보다 잘한 레코드를 조회

> 먼저 제주지역의 영어점수 평균을 구한 뒤, 잘한 레코드를 조회

```
SELECT avg(eng)
FROM sungjuk
WHERE addr='Jeju';	--제주지역 영어평균

SELECT uname,eng,addr
FROM sungjuk
WHERE eng>(SELECT avg(eng) FROM sungjuk WHERE addr='Jeju');
```

- 수학점수의 최고점이 국어점수에도 있는지 조회

```
SELECT MAX(mat)
FROM sungjuk;	--수학점수 최고점

SELECT uname,kor
FROM sungjuk
WHERE kor=(SELECT MAX(mat)
			FROM sungjuk);
```

- 칼럼추가 : tot

```
ALTER TABLE sungjuk ADD (tot number);
```





------



# NVL() 함수

>null값을 다른 값으로 치환

```
SELECT NVL(MAX(tot),0)+1
FROM sungjuk;
```

**NVL() 함수 쓰기 전**

![01](https://user-images.githubusercontent.com/49340180/60243497-55795280-98f3-11e9-83fa-7e73f3d558a5.PNG)

**NVL() 함수로 null값을 0으로 치환한 후**

![02](https://user-images.githubusercontent.com/49340180/60243535-67f38c00-98f3-11e9-91b7-856f4bdbf452.PNG)



```
SELECT NVL(MAX(tot),0)+1
FROM sungjuk;
```

![03](https://user-images.githubusercontent.com/49340180/60243642-a5581980-98f3-11e9-8135-b19ae7eb4910.PNG)





### MAX()를 이용해서 일련번호 발생

```
SELECT MAX(sno) FROM sungjuk;
SELECT MAX(sno)+1 FROM sungjuk;
```

![04](https://user-images.githubusercontent.com/49340180/60244112-b0f81000-98f4-11e9-8646-5f9fb7e9e73f.PNG)

```
SELECT MAX(tot)+1 FROM sungjuk;	--tot엔 아직 값이 없음
```

![05](https://user-images.githubusercontent.com/49340180/60244217-e56bcc00-98f4-11e9-9208-88ba2188e300.PNG)

> 이럴땐 tot의 값이 null이기 때문에 NVL()를 이용해서 값을 넣어준뒤 +1을 해라.

```
SELECT NVL(MAX(tot),5)+1 FROM sungjuk;
```

![06](https://user-images.githubusercontent.com/49340180/60244327-249a1d00-98f5-11e9-9937-2dd13dff1c72.PNG)



### MAX()함수를 이용해서 일련번호 발생한 후 행추가 하기

```
INSERT INTO sungjuk(sno,uname,kor,eng,mat,addr,wdate)
VALUES((SELECT NVL(MAX(tot),5)+1 FROM sungjuk)
,'김연아',60,80,55,'Jeju',sysdate);

SELECT sno,uname 
FROM sungjuk
ORDER BY sno DESC;
```

![07](https://user-images.githubusercontent.com/49340180/60244998-99218b80-98f6-11e9-8890-cd7c1a46fd92.PNG)





------



# MySQL SQL문으로 수정하기

- Oracle INSERT 문을 MySQL INSERT 문으로 수장하기

> INSERT INTO sungjuk(sno,uname,kor,eng,mat,addr,wdate)
> VALUES((SELECT NVL(MAX(tot),5)+1 FROM sungjuk)
> ,'김연아',60,80,55,'Jeju',sysdate);

> MySQL 에서는 IFNULL()이라는 함수가 있다.

```
INSERT INTO sungjuk(sno,uname,kor,eng,mat,regdt)
VALUES((SELECT IFNULL(MAX(sno),0)+1 FROM sungjuk AS TB)
,'김연아',90,80,70,NOW());
```

![01](https://user-images.githubusercontent.com/49340180/60247222-3c749f80-98fb-11e9-801c-f3b34305f1f3.PNG)

> COALESCE() 함수를 이용

```
INSERT INTO sungjuk(sno,uname,kor,eng,mat,regdt)
VALUES((SELECT COALESCE(MAX(sno),0)+1 FROM sungjuk AS TB)
,'박지성',80,60,75,NOW());
```

![02](https://user-images.githubusercontent.com/49340180/60247542-d2a8c580-98fb-11e9-99a3-94a5e45483b7.PNG)