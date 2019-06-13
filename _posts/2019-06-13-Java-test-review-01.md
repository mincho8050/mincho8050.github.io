---
title:  "Java test review 01"
date:   2019-06-13 16:23:00
categories: 
- JavaReview
tags:
- review
- java
author: "mincho8050"
---

# Java test review 01



### 1) 3+5=8출력

```java
int z=3,x=5
char op='+';

System.out.println(z+op+x+"="+(z+x));	//51=8
				//3+'+'+5+"="+(8)
				//3+43+5	char형 +가 아스키코드값이 있음 > 43
				//51=8
System.out.println(z+""+op+""+x+"="+(z+x)); //3+5=8
```





### 2) 1년 : 365일 ?시간 ?분 ?초 구하기.

> 1분:60초
>
> 1시간:60*60초=3600초
>
> 1일:24시간*3600초=86400초
>
> 1년:365.2425*86400=31556952초

```java
doble year=365.2425;	//1년
int total=(int)(year*86400);	//int로 강제 형변환 및 1년이 몇초인지 구하는거.
int day,hour,min,sec;	//미리 값을 안넣고 먼저 선언해도 된다.

day=total/86400;	//1일이 86400초니까 그걸로 나눠서 몇 일 나오는지 구하기 ,몫=일수
total=total%86400;	//total에 일수 구하고 남은 초들을 대입해준다. 나머지=일 구하고 남은 초 > 20952
hour=total/3600;	//1시간이 3600초니까 나눠서 몇시간인지 구하기
total=total/3600;	//여기도 동일하게 몇시간 구하고 남은 초들을 total에 대입 > 2952

min=total/60;
sec=total%60;	//분으로 나누고 남은 시간은 초이기 때문이다.

System.out.println(day+"일 "+hour+"시 "+min+"분 "+sec+"초");
```





### 3) 임의의 문자가 알파벳 대문자인지 확인하시오.

> - 대문자 확인법
>   - 소문자 대문자는 +-32차이.
>   - 소문자 다음 대문자. 

```java
char ch='t';
System.out.println(ch>='A'&&ch<='Z');
				  //'t'>='A'&&'t'<='Z'
                  //	true	false
				  //	false
```





### 4) 세개의 수 중에서 최댓값을 구하시오

> 맨 처음 두개를 비교한 후 큰거랑 마지막 하나랑 비교하기
>
> 삼항연산자 이용. (조건)?참:거짓 > 조건은 boolean형

```java
int a=5,b=7,c=9;

int max=(a<b)?b:a;	//맨 처음 두개를 먼저 비교한다.
	   //5<7  ->7	//a<b를 비교해서 참이면 b를 max에 대입, 거짓이면 a를 max에 대입
max=(r<max)?max:r;
    //9<7	->9	->max=9
//누적시키는 과정.
//이렇게 해서 출력하면 max는 9다. 
```





### 5) 대소문자를 서로 바꿔서 출력하시오.

> 대소문자 차이는 -+32차이
>
> 소문자가 숫자가 더 크다. 
>
> 대문자 A==65 , 소문자 a==97

```java
char ch='t';
char alp=((ch>='A'&&ch<='Z')?(char)(ch+32):(char)(ch-32));
		 //ch가 대문자라면      소문자로 바꾸기   대문자로 바꾸기	
```





### 6) 증감연산자 문제

```java
int a=2,b=6;
int f=++a*b--;

System.out.println(s);	//++a기 때문에 먼저 증감. 3
System.out.println(d);	//나중감소 b--여도 단독이면 상관없다. 5
System.out.println(f);	//2++이기 때문에 3, 나중감소 6--기때문에 6 > 18
```





### 7) 평균 점수에 따라서 A,B,C,D,F 학점을 출력하기

> switch ~case문 이용
>
> int는 정수기 때문에 10으로 나누면 정수로 나온다.

```java
int kor=95,eng=90;mat=80;
int aver=(kor+eng+mat)/3;

switch(aver/10){
    case 10:	//System.out.println("A");break; 이렇게 적어도 되지만
        		//어차피 10과9는 A기 때문에 흘러가게 냅두면 9에서 멈추고 그 값이 나온다.
   	case 9:System.out.println("A");break;
    case 8:System.out.println("B");break;
    case 7:System.out.println("C");break;
    case 6:System.out.println("D");break;
    default:System.out.println("F");
}
```





### 8) 운행거리에 따라 택시 요금을 계산하는 프로그램

> 2000m까지는 기본요금 900원, 초과시 200m 마다 기본요금에 100원씩 가산하여 요금 계산
>
> 예 ) 운행거리:3200m

```java
int distance=3200;	//운행거리
int fee=0;
int overFee=0;

if(distance<=2000){
    fee=900;	//기본요금
}else{
    int exceed=distance-2000;	//초과거리
    overFee=(exceed/200)*100;	//초과거리당 계산되는 추가요금
    fee=900+overFee;			//기본요금+초과요금=총요금
}
```





### 9) 1~10사이 수 중에서 3의 배수의 갯수 구하기

> 갯수파악의 변수는 0부터 시작, 등수는 1부터 시작.

```java
int count=0;

for(int e=3;e<=10;e++){
    if(e%3==0){
        count=count+1;	
        //count+=1; 또는 count++; 또는 ++count; 다 같은 의미.
    }
}
System.out.println(count+"개");	//3개
```





### 10) 1~10사이 중에서 3의 배수의 갯수의 합 구하기.

> 합 누적은 초기값이 0 , 곱하기 누적은 1부터 시작!

```java
int sum=0;
for(int a=1;a<=10;a++){
    if(a%3==0){
        sum=sum+a;
        //sum+=a; 라고 해도 됨.
    }
}
System.out.println(hap);
```





### 11) 4팩토리얼값을 구하시오

>  4 * 3 * 2 * 1의 값이 팩토리얼 값
>
> 곱하기 누적은 1부터 시작.

```java
int fac=1;	//곱하기는 누적곱의 수가 크니까 long형으로 해야 좋을것같다.
for(int a=4;a>=1;a--){
    fac=fac*a;
}
```





### 12) 1~100중에서 2의 배수이면서, 3의 배수의 갯수를 구하시오

> 2의배수 그리고 3의 배수를 각각 구하는 방법이나, 6의 배수를 구하는 방법이 있다.

```java
int count=0;
for(int a=1;a<=100;a++){
    if(a%6==0){
        count++;
    }
}
```





### 13) 소문자 중에서 모음의 갯수를 구하시오

> 모음 aeiou > 5개

```java
int count=0;
for(char ch='a';ch<='z';ch++){
    if(ch=='a'||ch=='e'||ch=='i'||ch=='o'||ch=='u'){
        count++;
    }
}
```

> switch 방법
>
> switch는 break를 걸지 않으면 계속 내려간다.

```java
for(char ch='a';ch<='z';ch++){
    switch(ch){
        case 'a':
        case 'e':
        case 'i':
        case 'o':
        case 'u': count++;
    }//switch
}//for
```





### 14) 대문자 알파벳을 한 줄에 5개씩 출력하시오.

> 카운트 변수가 5의 배수여야 5번에 한번씩
>
> 5의 배수가 나오면 줄바꿈.

```java
for(char alp='A';alp<='Z';alp++){
    //System.out.println(alp);
    //여기에 작성하게 되면 이렇게 A만 따로 써지는데,
    //이유는 A가 65라서 5로 나눠지기 때문에 한칸이 띄어진다.
    //왜냐하면 순서상 먼저 출력 후에 if문을 돌리기 때문에 A를 출력후 한칸이 띄어진다.
    /*출력
    A
    BCDEF
    GHIJK
    LMNOP
    QRSTU
    VWXYZ
    */
    if(alp%5==0){
        System.out.println();
    }
    System.out.println(alp);
    //여기에 작성하게되면
    //잘나오는것 같지만 위에 공백이 하나 생긴다.
    /*출력
	
    ABCDE
    FGHIJ
    KLMNO
    PQRST
    UVWXY
    */
}//for
```

> 다른 방법
>
> 윗줄에 공백이 생기지 않음.

```java
int upper=0;
for(char alp='A';alp<='Z';alp++){
    upper++;	//알파벳의 갯수를 카운트 한다.
    System.out.print(alp);
    if(upper%5==0){//그 카운트 된 갯수의 나머지를 구해 0과 비교한다.
        System.out.println();
    }
}//for
//윗줄에 공백이 생기지 않고 출력된다.
    /*출력
    ABCDE
    FGHIJ
    KLMNO
    PQRST
    UVWXY
    */
```





### 15) 1-2+3-4 .... +99-100=? 의 결과식을 구하시오

>-+-+-+ 엇갈리게 나온다.

```java
//홀수는 홀수끼리 더하고, 짝순는 -로 시작하니까 -로 계속 더한다. 
//그리고 나중에 짝,홀수를 더한다.
int even=0;	//짝수
int odd=0;	//홀수
for(int a=1;c<=100;c++){
    if(a%2!=0){
        odd+=a;
    }else if(a%2==0){
        even-=a;
        //계속 음수의 상태로 더해지는중.
    }
}//for
System.out.println(ocount+ecount);
```

> 다른 방법

```java
boolean flag=false;
int sum=0;
for(int a=1;a<=100;a++){
 if(flag){	//if의 조건이 flag인데 flag=false이고,
     		//if는 true일때 조건이 발생되기 때문에 else의 조건을 실행한다.
     sum-=a;
     flag=false;
 }else{
     sum+=a;
     flag=true;
     //else의 조건을 실행하고 나면 flag=true가 되고
     //그 다음 반복문에서 if의 조건이 실행된다.
 }
}
System.out.println(sum);
```
