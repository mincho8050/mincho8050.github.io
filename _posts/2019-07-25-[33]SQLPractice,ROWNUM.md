---
title:  "[33]SQLPractice,ROWNUM"
date:   2019-07-25
categories: 
- Database
tags: 
- Database
author: "mincho8050"
---





# 문제 풀기 [학사관리 테이블]

학생테이블, 과목테이블, 수강테이블 3개의 테이블 조인

```
SELECT su.hakno
       ,stu.uname
       ,su.gcode
       ,gw.gname
       ,gw.ghakjum
FROM tb_sugang su INNER JOIN tb_student stu
ON su.hakno=stu.hakno INNER JOIN tb_gwamok gw
ON su.gcode=gw.gcode; --INNER 생략가능
```

![01](https://user-images.githubusercontent.com/49340180/62268076-9f60d580-b469-11e9-9fec-325ca0f9c499.PNG)





### 문1) 학번별 수강신청 과목의 총 학점 조회

```
SELECT su.hakno
       ,stu.uname
       ,SUM(ghakjum)
FROM tb_sugang su INNER JOIN tb_student stu
ON su.hakno=stu.hakno INNER JOIN tb_gwamok gw
ON su.gcode=gw.gcode
GROUP BY su.hakno, stu.uname
ORDER BY hakno;
```

> 다른 방법

```
SELECT aa.hakno
       ,aa.총학점
       ,stu.uname
FROM(
    SELECT su.hakno
           ,SUM(gw.ghakjum) AS 총학점
    FROM tb_sugang su JOIN tb_gwamok gw
    ON su.gcode=gw.gcode
    GROUP BY su.hakno
) aa JOIN tb_student stu
ON aa.hakno=stu.hakno
ORDER BY aa.hakno;
```

![01](https://user-images.githubusercontent.com/49340180/62268150-d33bfb00-b469-11e9-885a-c301cede5261.PNG)



### 문2) 디자인 교과목 대상으로 학번별 수강신청 과목의 총학점 조회

```
SELECT stu.hakno
       ,stu.uname
       ,SUM(gw.ghakjum)
FROM tb_sugang su INNER JOIN tb_student stu
ON su.hakno=stu.hakno INNER JOIN tb_gwamok gw
ON su.gcode=gw.gcode
WHERE gw.gcode LIKE 'd%'
GROUP BY stu.hakno,stu.uname;
```

> 선생님 방식

```
SELECT bb.hakno
       ,bb.총학점
       ,stu.uname
FROM( 
    SELECT aa.hakno
                ,SUM(aa.ghakjum) 총학점
    FROM(
         SELECT su.hakno
                ,su.gcode
                ,gw.ghakjum
        FROM tb_sugang su JOIN tb_student stu
        ON su.hakno=stu.hakno JOIN tb_gwamok gw
        ON su.gcode=gw.gcode
        WHERE su.gcode LIKE 'd%'
     ) aa 
     GROUP BY aa.hakno
 )bb JOIN tb_student stu
 ON bb.hakno=stu.hakno;
```

![01](https://user-images.githubusercontent.com/49340180/62268209-15653c80-b46a-11e9-8fea-45c779b5ce16.PNG)





### 문3) 과목코드 p001을 신청한 학생들의 명단 조회

```
SELECT gw.gcode
       ,stu.hakno
       ,stu.uname
       ,gw.gname
FROM tb_sugang su JOIN tb_student stu
ON su.hakno=stu.hakno JOIN tb_gwamok gw
ON su.gcode=gw.gcode
WHERE gw.gcode='p001'
GROUP BY stu.hakno,stu.uname,gw.gname,gw.gcode;
```

> 선생님 방식

```
SELECT gw.gcode
       ,stu.hakno
       ,stu.uname
       ,gw.gname
FROM tb_sugang su JOIN tb_student stu
ON su.hakno=stu.hakno JOIN tb_gwamok gw
ON su.gcode=gw.gcode
WHERE gw.gcode='p001';
```

![01](https://user-images.githubusercontent.com/49340180/62268255-3a59af80-b46a-11e9-9443-e6ca41de26fa.PNG)





### 문4) 프로그램 교과목중에서 학점이 제일 많은 과목을 신청한 학생들 명단 조회

```
SELECT aa.gcode
       ,aa.hakno
       ,stu.uname
FROM (
    SELECT tb_sugang.gcode
           ,tb_sugang.hakno
    FROM tb_sugang
    WHERE gcode IN (SELECT gcode
                    FROM tb_gwamok
                    WHERE ghakjum=(SELECT MAX(ghakjum)
                                    FROM tb_gwamok
                                    WHERE gcode LIKE 'p%') 
                    AND gcode LIKE 'p%')
) aa JOIN tb_student stu
ON aa.hakno=stu.hakno;
```

![01](https://user-images.githubusercontent.com/49340180/62268451-e4393c00-b46a-11e9-853f-894d32642a68.PNG)





### 문5) 수강신청을 하지 않은 학생들의 명단 조회

수강신청을 한 학생들

```
SELECT hakno
       ,uname
FROM tb_student
WHERE hakno IN (SELECT hakno
                FROM tb_sugang
                GROUP BY hakno);
```

수강신청 하지 않은 학생들

```
SELECT hakno
       ,uname
FROM tb_student
WHERE hakno NOT IN (SELECT hakno
                    FROM tb_sugang
                    GROUP BY hakno);
```





### 문6) 수강신청 하지 않은 과목들 조회



**수강신청한 과목**

```
SELECT gcode
FROM tb_sugang
GROUP BY gcode;
```

![01](https://user-images.githubusercontent.com/49340180/62269117-0633be00-b46d-11e9-8057-9fb6537a151e.PNG)

**수강신청하지 않은 과목**

```
SELECT gcode
       ,gname
FROM tb_gwamok
WHERE gcode NOT IN (SELECT gcode
                    FROM tb_sugang
                    GROUP BY gcode);
```

![02](https://user-images.githubusercontent.com/49340180/62269124-0b910880-b46d-11e9-8332-e83489bb579d.PNG)

**LEFT JOIN 사용**

```
SELECT gw.gcode
       ,gw.gname
       ,gw.ghakjum
       ,su.gcode
FROM tb_gwamok gw LEFT JOIN tb_sugang su
ON gw.gcode=su.gcode
WHERE su.gcode IS NULL;
```

![03](https://user-images.githubusercontent.com/49340180/62269139-10ee5300-b46d-11e9-8647-751756951f86.PNG)

**RIGHT JOIN 사용**

```
SELECT su.gcode
       ,gw.gcode
       ,gw.gname
       ,gw.ghakjum
FROM tb_sugang su RIGHT JOIN tb_gwamok gw
ON su.gcode=gw.gcode
WHERE su.gcode IS NULL;
```

![04](https://user-images.githubusercontent.com/49340180/62269146-151a7080-b46d-11e9-9145-2711714d4027.PNG)

**WHERE 조건절 사용방법**

```
SELECT stu.hakno
       ,stu.uname
       ,su.gcode
FROM tb_student stu , tb_sugang su
WHERE stu.hakno=su.hakno(+);
```

![05](https://user-images.githubusercontent.com/49340180/62269161-1b105180-b46d-11e9-9245-33563e437891.PNG)







------





# ROWNUM

ROWNUM : 행번호

ROWID : 행의 주소값



**줄번호**

```
SELECT ROWID
       ,ROWNUM
       ,hakno
       ,uname
FROM tb_student;
```

![01](https://user-images.githubusercontent.com/49340180/62269414-f8326d00-b46d-11e9-8e20-2126e8d4ace4.PNG)

> 에러

```
SELECT address
       ,ROWNUM
FROM tb_student
GROUP BY address;
```

**줄 번호 1~3사이 조회**

```
SELECT ROWNUM
       ,hakno
       ,uname
FROM tb_student
WHERE ROWNUM>=1 AND ROWNUM<=3;
```

![02](https://user-images.githubusercontent.com/49340180/62269420-fe284e00-b46d-11e9-85b6-08798b48b756.PNG)

**줄번호 4~6사이 조회**

> 이렇게 하면 에러

```
SELECT ROWNUM
       ,hakno
       ,uname
FROM tb_student
WHERE ROWNUM>=4 AND ROWNUM<=6;
```

> 셀프 조인후 행번호 추가
>
> 모조칼럼 ROWNUM을 실제칼럼으로 인식시킨 후 
>
> 다른 명령어와 병행해서 사용한다(셀프조인 후 사용할 것)

```
SELECT rnum
       ,hakno
       ,uname
       ,address
FROM(
    SELECT ROWNUM rnum
           ,hakno
           ,uname
           ,address
    FROM (
        SELECT hakno
               ,uname
               ,address
        FROM tb_student
        ORDER BY hakno
    )
)
WHERE rnum>=4 AND rnum<=6;
```

![03](https://user-images.githubusercontent.com/49340180/62269429-02ed0200-b46e-11e9-916c-5b67e2725df8.PNG)









### 문1) 학번별 수강신청 총 학점을 구하고 총 학점 순으로 정렬 후 위에서 부터 3건만 조회하기

학번, 이름, 총학점 조회

**과목 코드별 학점 가져오기**

```
SELECT su.hakno
       ,su.gcode
       ,gw.ghakjum
FROM tb_sugang su JOIN tb_gwamok gw
ON su.gcode=gw.gcode;
```

![01](https://user-images.githubusercontent.com/49340180/62269605-945c7400-b46e-11e9-96a8-638e54ac4ccc.PNG)

**학번별로 총학점 구하기**

```
SELECT su.hakno
       ,SUM(gw.ghakjum) AS 총학점
FROM tb_sugang su JOIN tb_gwamok gw
ON su.gcode=gw.gcode
GROUP BY su.hakno;
```

![02](https://user-images.githubusercontent.com/49340180/62269613-97effb00-b46e-11e9-9725-0333b2acd6e8.PNG)

**총학점 순 정렬**

```
SELECT su.hakno
       ,SUM(gw.ghakjum) AS 총학점
FROM tb_sugang su JOIN tb_gwamok gw
ON su.gcode=gw.gcode
GROUP BY su.hakno
ORDER BY SUM(gw.ghakjum) DESC;
```

![03](https://user-images.githubusercontent.com/49340180/62269624-9d4d4580-b46e-11e9-821e-4caee9fa14e0.PNG)

**학생이름 가져오고, 행번호 출력**

```
SELECT aa.hakno
       ,aa.총학점
       ,stu.uname
       ,ROWNUM
FROM(
    SELECT su.hakno
                ,SUM(gw.ghakjum) AS 총학점
    FROM tb_sugang su JOIN tb_gwamok gw
    ON su.gcode=gw.gcode
    GROUP BY su.hakno
    ORDER BY SUM(gw.ghakjum) DESC
) aa JOIN tb_student stu
ON aa.hakno=stu.hakno
ORDER BY aa.hakno;
```

![04](https://user-images.githubusercontent.com/49340180/62269631-a1796300-b46e-11e9-841d-a542cd579578.PNG)

**위에서부터 3건 출력**

```
SELECT aa.hakno
       ,aa.총학점
       ,stu.uname
       ,ROWNUM
FROM(
    SELECT su.hakno
           ,SUM(gw.ghakjum) AS 총학점
    FROM tb_sugang su JOIN tb_gwamok gw
    ON su.gcode=gw.gcode
    GROUP BY su.hakno
    ORDER BY SUM(gw.ghakjum) DESC
) aa JOIN tb_student stu
ON aa.hakno=stu.hakno
WHERE ROWNUM>=1 AND ROWNUM<=3;
```

![05](https://user-images.githubusercontent.com/49340180/62269640-a63e1700-b46e-11e9-84aa-1af0d6cec53e.PNG)

**행번호 4~6번 출력하기**

> ROWNUM은 모조 칼럼이기 때문에 조건절에 직접 사용하지 말고 실제 칼럼으로 인식한 후 사용할 것을 추천

```
SELECT hakno
            ,총학점
            ,uname
            ,rnum
FROM (
    SELECT aa.hakno
           ,aa.총학점
           ,stu.uname
           ,ROWNUM AS rnum
    FROM(
        SELECT su.hakno
               ,SUM(gw.ghakjum) AS 총학점
        FROM tb_sugang su JOIN tb_gwamok gw
        ON su.gcode=gw.gcode
        GROUP BY su.hakno
        ORDER BY SUM(gw.ghakjum) DESC
    ) aa JOIN tb_student stu
    ON aa.hakno=stu.hakno
) bb 
WHERE bb.rnum>=4 AND bb.rnum<=6;
```

![01](https://user-images.githubusercontent.com/49340180/62269718-e1d8e100-b46e-11e9-9303-cc0d80f4c2fc.PNG)





### 문2) 학번별 수강신청한 총학점 조회

단, 수강신청하지 않은 학생의 총학점도 0으로 표시

> 예시)
>
> g1001 홍길동 8
>
> g1002 무궁화 6
>
> g1003 진달래 0

```
SELECT stu.hakno
       ,stu.uname
       ,SUM(NVL(aa.ghakjum,0)) AS 총학점
FROM (
    SELECT *
    FROM tb_sugang su JOIN tb_gwamok gw
    ON su.gcode=gw.gcode
) aa RIGHT JOIN tb_student stu
ON aa.hakno=stu.hakno
GROUP BY stu.hakno , stu.uname
ORDER BY stu.hakno;
```

![01](https://user-images.githubusercontent.com/49340180/62269882-63c90a00-b46f-11e9-9967-6d4b091e1e03.PNG)





### 문3) 학생테이블에서 학번순으로 정렬 후 행번호를 아래와 같이 붙여서 조회하시오.

> 예시) 
>
>     8  g1001	홍길동	hong1@naver.com	  서울	111-5588	19/07/26
>     7  g1002	홍길동	hong2@soldesk.com 제주	331-2223	19/07/26
>     6  g1003	개나리	user1@daum.net	  서울	111-2224	19/07/26
>     5  g1004	홍길동	hong3@naver.com	  부산	222-2255	19/07/26
>     4  g1005	진달래	user2@soldesk.com 서울	445-2277	19/07/26
>     3  g1006	개나리	user3@naver.com	  제주	578-5588	19/07/26
>     2  g1007	김연아	user7@naver.com	  제주	578-5588	19/07/30
>     1 g1008	박지성	user8@naver.com	제주	578-5588	19/07/30

```
SELECT ROW_NUMBER() OVER ( ORDER BY hakno DESC) AS rnum
       ,hakno
       ,uname
       ,email
       ,address 
       ,phone
       ,regdt
FROM tb_student
ORDER BY rnum DESC;
```

> 선생님 방식

```
SELECT hakno
       ,uname
       ,rnum
FROM (
    SELECT hakno
           ,uname
           ,ROWNUM AS rnum
    FROM (
        SELECT hakno
               ,uname
        FROM tb_student
        ORDER BY hakno DESC
    )
)
ORDER BY rnum DESC;
```

![02](https://user-images.githubusercontent.com/49340180/62269896-6a578180-b46f-11e9-8887-720d19add2f1.PNG)









