---
title:  "Java test review 02"
date:   2019-06-13 17:20:00
categories: 
- JavaReview
tags: 
- review 
- java
author: "mincho8050"
---

# Java test review 02









### 1)  두 수 사이의 합을 구하시오

> 두 수의 값이 바뀌어도 동일하게 나오게 하기.
>
> 예) 2와5 > 2+3+4+5=?

```java
int start=5,end=2;
int sum=0;
if(start>end){
    for(int a=end;a<=start;a++){
        sum+=a;
    }
}else if(start<end){
    for(int a=start;a<=end;a++){
        sum+=a;
    }
}
System.out.println(a);
```

>선생님이 하신 방법
>
>swap : 변수값을 서로 교환.

```java
int start=5,end=2;
int sum=0;
if(start>end){ //만약 시작값이 마지막값보다 크다면
    int tmp=start;	//가상의 변수에 시작값을 저장하고
    start=end;		//시작값에 마지막값을 적어준다(두 수중 적은값)
    end=tmp;		//그리고 가상의 변수 값을 마지막 값에 넣는다.
    //이렇게 되면 두 수가 서로 바뀌었다.
}
for(int a=start;a<=end;a++){
    sum+=a;
}
System.out.println(sum);
```









### 2) (1/2)-(2/3)+(3/4)-(4/5)+ ... (99/100) 의 결과를 예측하시오

> 나누기 연산자 수일치 처럼 실행하면 된다. 
>
> 나누기 때문에 애초에 변수설정을 할 때 int형이 아니라 double형을 사용한다.
>
> > int형을 사용하면 쓰레기값 나옴

```java
double sum=0.0;
boolean flag=false;
for(int a=1;a<100;a++){
    if(flag){
        sum=sum-(a/(double)(a+1));
        flag=false;
    }else{
        sum=sum+(a/(double)(a+1));
        flag=true;
    }
}
```

> 분석

```
a=1	flag=false
	else식 수행	sum=0.0+(1/2.0)=1/2.0	flag=true
a=2	flag=true
	if식 수행		sum=1/2.0-(2/3.0)=7/6.0 flag=false
a=3	flag=false
	else식 수행	sum=7.0/6.0+(3/4.0)=23.0/12.0 flag=true
```









### 3) x값이 10으로부터 x를 여러 번 뺸 후 결과가 음수가 되면 x를 몇번 뺐는가를 구하시오.

> 음수가 되면 break를 사용

```java
int x=3;
int count=0;
int num=10;

while(true){	//무한반복
    count++;	//카운트+1씩
    num-=x;		//10-3씩 빼는 중. 
    if(num<0){	//num이 음수가되면
        break;	//반복문 탈출.
    }
}
System.out.println(count);
```









### 4) 어느 달팽이가 낮에는 3cm올라가고 밤에는 2.5cm내려온다고 할 때, 13cm의 나무꼭대기에 올라가려면 며칠이 걸리는지 구하시오

> 13cm의 나무 올라가면 break 사용
>
> 하루에 0.5cm씩 올라감
>
> 누적하는 변수 설정

```java
int day=0;
double snail=0.0;

while(true){	//무한루프
    day++;		//일 수 
    snail+=3.0;	//달팽이가 낮에 올라가는 3.0cm 더하기
    if(snail>=13.0){//달팽이가 13cm에 도달하면 break;
        break;
    }else{			//13.0에 도달하지 못했다면 밤에 내려가는 길이인 2.5cm 빼주기.
        snail-=2.5;
    }
}
System.out.println(day+"일");
```









### 5) 사각형 만들기

> 등수 구할 때 회전수 참조

```java
for(int a=0;a<4;a++){
    for(int b=0;b<4;b++){
        System.out.print("#");
    }
    System.out.println();
}
/*출력값
    ####
    ####
    ####
    ####
*/
```









### 6) 삼각형 만들기

> 삼각형 만들기 , 회전수 증가
>
> 예 ) 로또번호

```java
for(int a=0;a<4;a++){
    for(int b=0;b<4;b++){
        System.out.print("@");
    }
    System.out.println();
}
/*출력
@
@@
@@@
@@@@
*/
```









### 7) 역삼각형 만들기

> 회전수 감소
>
> 예) 정렬

```java
for(int a=4;a>0;a++){
    for(int b=0;b<a;b++){
        System.out.print("$");
    }
    System.out.println();
}
```









### 8) 도형을 출력하시오 01

> @★★★
> ★@★★
> ★★@★
> ★★★@

```java
for(int a=0;a<4;a++){
    for(int b=0;b<4;b++){
        if(b==a){
            System.out.print("@");
        }else{
            System.out.print("★");
        }
    }
    System.out.println();
}
```









### 9) 도형을 출력하시오 02

> ★★★★
>   ★★ ★
>      ★★
> 		★
>
> 안 보이는 곳을 공백으로 설정

```java
for(int a=0;a<4;a++){
    for(int b=0;b<4;b++){
        if(b<=a-1){
            System.out.print(" ");
        }else{
            System.out.print("★");
        }
    }
    System.out.println();
}
```









### 10) 숫자를 정렬하시오

> 12345
> 23456
> 34567
> 45678
> 56789

```java
for(int a=1;a<=5;a++){	//1부터 시작
    for(int b=a;b<a+5;b++){
        System.out.print(b);	//1부터 출력해야하니까 1부터 시작해야 한다.
    }
    System.out.println();
}
```

> 다른 방식

```java
for(int a=0;a<5;a++){
    for(int b=a+1;b<=a+1;b++){	//1부터 출력해야 하니까 +1
        System.out.print(b);
    }
    System.out.println();
}
```









### 11) 2g , 3g , 5g 짜리 추가 각각 5개씩 있을 때, 세가지의 추의 조합으로 무게가 40~45g 사이일 때, 각 추의 갯수와 무게를 출력하는 프로그램

> 출력결과
>
> 2g 3g 5g  total
> 1   5  5   42
> 2   4  5   41
> 2   5  5   44

```java
System.out.println("2g 3g 5g total");
int sum=0;
for(int a=1;a<=5;a++){	//2g 추
    for(int b=1;b<=5;b++){	//3g 추
        for(int c=1;c<=5;c++){	//4g 추
            sum=(a*2)+(b*3)+(c*5);	//각 추의 합
            if(sum>=40&&sum<=45){
                System.out.println(a+" "+b+" "+c+" "+sum);
            }
        }
        
    }
}
```









### 12) 대문자와 소문자를 서로 바꿔서 출력하시오

> 출력값
>
> sOLDESK

```java
char[] ch={'S','o','l','d','e','s','k'};
int size=ch.length;

for(int a=0;a<size;a++){
    if(ch[a]>='A'&&ch[a]<'Z'){
        ch[a]=(char)(ch[a]+32);	//강제 형변환
    }else{
        ch[a]=(char)(ch[a]-32);	//강제 형변환
    }
    System.out.print(ch[idx]);
}
```









### 13) num배열 요소의 전체 합을 구하시오

```java
int[] num={-3,7,0,8,-6};
int size=su.length;
int sum=0;

for(int a=0;a<size;a++){
    sum=sum+num[a];
}
System.out.println("합:"+sum);
```









### 14) num 배열의 등수 구하기

> 점수가 높은 것을 1등 
>
> 등수 하나 밀려나면 +1

```java
int[] num={-3,7,0,8,-6};
int size=su.length;
int[] rank={1,1,1,1,1};	//num의 요소들 순서를 1로 초기화 한다.

for(int a=0;a<size;a++){
    for(int b=0;b<size;b++){
        if(num[a]<num[b]){	//만약 rank[a]의 수가 적으면
            rank[a]++;	//rank[a]의 순위가 1칸씩 올라감
        }
    }
    System.out.println(su[a]+"의 등수:"+rank[a]+"등");
}
```









### 15) 최댓값, 최솟값을 각각 구하기

> 만약 모든 수가 음의 정수일 때도 고려하기.

```java
int[] num={-3,7,0,8,-6};
int size=su.length;
int max=num[0];	//최댓값
int min=num[0];	//최솟값
//단순히 0 집어넣지 않게 주의하자.

for(int a=0;a<size;a++){
    if(max<num[a]){
        max=num[a];
    }
    if(min>num[a]){
        min=num[a];
    }
}
System.out.println("최댓값:"+max);
System.out.println("최솟값:"+min);
```

