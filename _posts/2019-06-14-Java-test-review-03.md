---
title:  "Java test review 03"
date:   2019-06-14 02:29:00
categories: 
- JavaReview
tags:
- review
- Java
author: "mincho8050"
---

# Java test review 03







### 1) 성적 프로그램 완성하기

```java
String[] name={"아이린","웬디","슬기","조이","예리"};
int[] kor={100,20,85,55,95};
int[] eng={100,40,80,65,95};
int[] mat={100,35,75,70,35};
int[] aver=new int[5];
int[] rank={1,1,1,1,1};
int size=name.length;
```

###### 평균 구하기

```java
for(int a=0;a<size;a++){
    aver[a]=(kor[a]+eng[a]+mat[a])/3;
}
```

###### 등수 구하기 

> 평균값을 기준으로

```java
for(int a=0;a<size;a++){
    for(int b=0;b<size;b++){
        if(aver[a]<aver[b]){
            rank[a]++;
        }
    }
}
```

###### 과락 , 합격 , 재시험 ,  불합격 표시하기

> 평균 70점 미만 불합격, 70점 이상이어도 각 과목 40미만 재시험 모든 조건 만족하면 합격

```java
for(int a=0;a<size;a++){
    if(aver[a]>=70){
        if(kor[a]<40||eng[idx]<40||mat[idx]<40){
            System.out.print(" 재시험 ");
        }else{
            System.out.print(" 합격 ");
        }
            
    }else{
        System.out.print(" 불합격 ");
    }
}
```

###### 평균 10점당 ★표시

```java
for(int a=0;a<size;a++){
    for(int b=0;b<(aver[a]/10);b++){
        System.out.print("★");
    }
}
```

###### 평균 95점 이상은 장학생 표시

```java
for(int a=0;a<size;a++){
    if(aver[a]>=95){
        System.out.print(" 장학생");
    }
}
```







### 2) 2차원 배열로 모음의 갯수를 구하시오

> aeiou

```java
char[][] ch={
        {'H','a','p','p','y'},	
        {'A','p','p','l','e'},
        {'S','o','l','d','e','s','k'}
};
int row=ch.length;	//행의 갯수
int count=0;

for(int a=0;a<row;a++){
    int col=ch[a].length;	//각 행의 열의 갯수
    for(int b=0;b<col;b++){
        if(ch[x][y]=='a'||ch[x][y]=='e'||
           ch[x][y]=='i'||ch[x][y]=='o'||
           ch[x][y]=='u'||ch[x][y]=='A'||
           ch[x][y]=='E'||ch[x][y]=='I'||
           ch[x][y]=='O'||ch[x][y]=='U'){
            count++;
        }
    }
}
System.out.println("모음의 갯수 : "+count+"개");
```

> 다른 방법

```java
int mo=0;
char[][] ch={
    {'H','a','p','p','y'},	
    {'A','p','p','l','e'},
    {'S','o','l','d','e','s','k'}
};
int count=0;
int row=ch.length; //행의 갯수.

for(int a=0;a<row;a++){
    int col=ch[a].lenghth;
    for(int b=0;b<col;b++){
        char c=ch[a][b];	//각 요소의 값 가져오기
        if(c>='A'&&c<='Z'){	//대문자인지 묻기
            c=(char)(c+32)	//대문자라면 소문자로 변환
        }
        switch(c){ //그다음 switch를 사용해서 갯수 구하기.
        case 'a':
        case 'e':
        case 'i':
        case 'o':
        case 'u':mo++;
    }
}
System.out.println(mo);
```









### 3) 요일 구하는 프로그램

> 서기 1년~서기 2019년=총날수 > 365*2019=736935
>
> 총날수%7 > 나머지 1은 월, 2는 화, 3-수,4-목,5-금,6-토,7-일
>
> 1) 서기 1년~서기 2018년 (2019년도 까지하면 2019.12.31일 까지 계산해야 된다.)
>
> 그러니까 구하려는 년도보다 하나 낮아야 한다.
>
> 윤년+=366
>
> 평년+=355
>
> 
>
> 2) month는 4가지 형태
>
> 31,30,29,28일
>
> 1~4월(만약 구하려는 달이 5월이면 5월말일까지 계산되기 때문에 구하려는 월보다 전월로 구해야 한다.)
>
> 1,3,5,7,8,10,12 +=31
>
> 4,6,9,11 +=30
>
> 윤년 2월 +=29
>
> 평년 2월 +=28
>
> 
>
> 3)+=22일(구하려는 일수)

###### 총 날수 구하기

> 평년과 윤년 구별하기

```java
int year=2019,month=5,date=22;	//2019.5.22일 요일 구하기
int sum=0;

for(int y=1;y<year;y++){
    if(y%4==0&&y%100!=0||y%400==0){	//윤년공식.
        sum+=366;	//윤년일때 
    }else{
        sum+=365;	//평년일때
    }
}//sum 결과값 737059
```

###### month 구하기

> 배열 사용

```java
int[] mon={0,31,28,31,30,31,30,31,31,30,31,30,31}; 
//평년 기준으로 1~12월. 
//앞에 0이란 값을 추가해서 1~4로 해서 값을 구한다. 알아보기 쉬우니까.
//예) 5월 > sum+=mon[1]~mon[4]

if(cyear%4==0&&cyear%100!=0||cyear%400==0){//여기서는 윤년물어보는 연도를
    										//구하려는 년도로 물어봐야함.
    mon[2]=29;
}
for(int m=1;m<month;m++){	//구하려는 달수가 5월이면 1~4월까지 구함.
    sum=sum+mon[m];
}
```

###### 일수 더하기

```java
sum+=date;	//결과값 737201
```

###### 요일 구하기

> 7로 나눠서 나머지로 구하기

```java
switch(hap%7){
case 0:System.out.println(cyear+"년 "+cmonth+"월 "+cdate+"일은 "+"일요일");break;
case 1:System.out.println(cyear+"년 "+cmonth+"월 "+cdate+"일은 "+"월요일");break;
case 2:System.out.println(cyear+"년 "+cmonth+"월 "+cdate+"일은 "+"화요일");break;
case 3:System.out.println(cyear+"년 "+cmonth+"월 "+cdate+"일은 "+"수요일");break;
case 4:System.out.println(cyear+"년 "+cmonth+"월 "+cdate+"일은 "+"목요일");break;
case 5:System.out.println(cyear+"년 "+cmonth+"월 "+cdate+"일은 "+"금요일");break;
case 6:System.out.println(cyear+"년 "+cmonth+"월 "+cdate+"일은 "+"토요일");break;
}
```







### 4) 메소드를 이용한 문제풀이



###### 1.절대값 출력하기

```java
public static void abs(int a){
    if(a<0){
        System.out.println(a+"의 절대값:"+(-a));
    }else{
        System.out.println(a+"의 절대값"+a);
    }
}

public static void main(String[] args) {
    abs(-3);
}
```



###### 2. 윤년, 평년을 구분해서 출력하시오

```java
public static void leap(int y){
    if(y%4==0&&y%100!=0||y%400==0){
        System.out.println(y+"년은 "+"윤년");
    }else{
        System.out.println(y+"년은 "+"평년");
    }
}//leap

public static void main(String[] args) {
    leap(2019);
}
```



###### 3. 팩토리얼값을 출력하시오

```java
public static void fact(int f){
    long count=1; //값이 클 수도 있으니 int형 보다는 long형으로 하자.
    for(int a=1;a<=f;a++){
        count=count*a;
    }
    System.out.println(f+"!:"+count);
}//fact

public static void main(String[] args) {
    fact(4);
}
```



###### 4. 전달한 수만큼 문자를 출력하시오

```java
public static void grahp(int a,char b){

    for(int c=1;c<=a;c++){
        System.out.print(b);
    }
    System.out.println();
}//grahp

public static void main(String[] args) {
    grahp(10,'#');
}
```



###### 5. 두 수 사이의 누적의 합을 구하시오

```java
public static void hap(int a,int b){

    int start=a,end=b; 
    int hap=0;
    if(start>end){ //만약 시작값이 마지막값보다 크다면
        int tmp=start; //가상의 변수에 시작값을 잠시 두고
        start=end; //시작값을 마지막값에 둔다.
        end=tmp; //그리고 가상의 변수의 값을 마지막값에 넣는다.
                //이렇게 되면 시작값과 마지막값이 바뀌면서 시작값<마지막값. 이렇게 된다.
    }
    for(int c=start;c<=end;c++){//시작값<마지막값(원래부터 이렇던가 아니면 위의 식에서 바뀌었든가)
        hap=hap+c;
    }
    System.out.println(a+"와"+b+"의 두 수 사이의 합은 "+hap);
}//hap

public static void main(String[] args) {
    hap(2,5);
}
```
