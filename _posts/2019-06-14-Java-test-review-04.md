---
title:  "Java test review 04"
date:   2019-06-14 05:50:00
categories: 
- JavaReview
tags:
- review
- java
author: "mincho8050"
---

# Java test review 04







### 1) 누적의 합을 구하시오

>1+....+10=55
>1+....+20=210
>1+....+30=
>..
>
>1+....+90=
>1+....+100=5050
>
>맨 처음 10번, 20번, 30번.. 회전수 10번씩 증가
>
>마지막 값도 10씩 증가. 
>
>반복문 삼각형

```java
//int sum=0; 밖에 쓰면 전체 누적의 수가 나온다.
for(int a=1;a<=10;a++){	//다른 조건은
    					//int a=10;a<=10;a=a+1
    int sum=0;
    //여기에 위치하면 한번 b반복문을 끝날 때 sum변수는 초기화. > 누적되지 않음
    for(int b=1;b<=10*a;b++){//다른 조건은
        					//int b=1;b<=a;b++
        sum+=b;
    }
    System.out.println("1+....+"+a*10+"="+sum);
}
```





### 2) 1~1000까지의 합 중 그합이 10000이 넘을 때 결과 출력

>1+2+3+4+5+6+...
>
>break

```java
int sum=0;
for(int a=1;a<=1000;a++){
    sum+=a;
    if(sum>=10000){
        //a가 141되면 break
        break;
    }
}
/*
출력값
10011
*/
```





### 3) 3의 배수의 누적 합이 100이 넘어가려면 3부터 어디까지 더해야 하는지 구하시오

> 3+6+9+...
>
> break

```java
int sum=0;

for(int a=1;sum<100;a++){//;; 이면 무한루트  
    					//혹은 변수 a로 조건을 만들지 않아도됨
    					//sum<100은 합이 100보다 작을때까지 반복시킴
    if(a%3==0){
        sum+=a;
        System.out.print(a3);
        if(sum<100){//이 조건을 안걸면 마지막까지 +가 나온다
            		//100 미만일때만 +기호가 나올 수 있도록
            System.out.print("+"); 
        }
        if(sum>=100){
            System.out.println('='+" "+sum3);
            break;
        }
    }
}
```

> 다른 방식

```java
int sum=0;
int num=0;
String result=""; //정수는 0에서 많이 시작하지만
				  //문자열은 빈문자열로 시작할 수 있다.
				  //공백은 빈문자열이 아님

while(true){ //무한루프
    num+=3;
    sum+=num;
    result=result+num+"+"; //문자열에서 더하기 연산자 쓰면 끝에 계속 붙음.
    if(sum>100){
        break; //반복문 탈출
    }
}
System.out.println(result + "...="+hap);
```





### 4)  로또번호 1~45 사이 중에서 서로 겹치지 않게 6개 발생

> 1차원 배열에다가 저장

```java
int[] lotto=new int[6];
/*
lotto[0]=0;
lotto[1]=0;
lotto[2]=0;
lotto[3]=0;
lotto[4]=0;
lotto[5]=0;
*/
for(int a=0;;){//a++을 증가조건으로 걸면 무조건 증가만 하기 때문에
    			//증가 컨트로를 위해 무한루프로 돌린다
    			//첫번째 for문은 정말 큰 조건
  
    int n=(int)(Math.random()*45+1); //랜던 번호 발생시키기.
    boolean check=true;
    
    //전체적으로 확인하는 부분
    for(int b=0;b<6;b++){//중복체크하는 for문
        				 //여기서 자세히 조건을 걸면 조절할 수 있다.
        if(lotto[b]==n){//랜덤숫자를 넣고 있는데 만약 겹치게 된다면 
            check=false;//boolean형이 false로 바뀌고
            			//아래에 있는 if문을 통과할 수 없어 
            			//lotto[]배열에 숫자를 대입할 수 없으므로 
            			//숫자가 겹치지 않을 때까지 무한루프를 돌린다.
        }
    }
    if(check){//겹치지 않으면 true면 발생한다.
        lotto[a++]=n;//랜덤번호 n을 넣어준다. 그리고 a변수를 하나씩 증가한다
        System.out.print(n+" ");
    }
    if(a==6){
        break;//만약 랜덤숫자n이 6개 다 겹치지 않으면 break
    }
    
}//for
```

> 다른 방법
>
> 로또는 if(자신==){random() 다시발생}

```java
int size=lotto.length;

for(int a=0;a<size;a++){
    lotto[a]=(int)(Math.random()*45)+1;
    for(int b=0;b<a;b++){
        if(lotto[a]==lotto[b]){
            a--;
            break;//lotto[a]==lotto[b] 같으면 -1 상태로 두번째 for문을 빠져나옴
            	  //뒤로 한 번 후퇴했다가 다시 오라는 소리
            	  // 만약 a=4일때  같다면 -1 즉, a=3일때 반복문을 다시 돌려야함
        }
    }
    
}//for

//Array 클래스를 이용하여 정렬
Array.sort(lotto);
for(int c=0lc<size;c++){
    System.out.print("번호"+lotto[c]+" ");
}//for
```





### 5) 대각선 방향의 각 요소의 합을 구하시오

> 대각선 방향의 합(4+9+7) , (2+9+6)

```java
int[][] num={
    {4,3,2},
    {5,9,1},
    {6,8,7}
};
int sum=0;

for(int a=0;a<3;a++){
    sum+=num[a][a]; //(4+9+7)
}
System.out.println("대각선 방향의 합1 : "+sum5);

for(int b=0;b<3;b++){
    sum+=num[a][2-0];// (2+9+6)
}
System.out.println("대각선 방향의 합2 : "+sum5);
```

###### 위의 num배열에서 행과 열을 바꿔서 출력하시오.

> 456
>
> 398
>
> 217

```java
for(int c=0;c<3;c++){
    for(int d=0;d<3;d++){
        System.out.print(num[d][c]);
    }
}
```





### 6) 행렬 각각의 합, 차를 구하시오

```java
int[][] aa={
        {4,3},
        {5,9}
};
int[][] bb={
        {1,2},
        {6,7}
};
int[][] cc={
	{0,0},{0,0}
}; //합
int[][] dd={
	{0,0},{0,0}
}; //차

for(int a=0;a<2;a++){
    for(int b=0;b<2;b++){
        cc[a][b]=aa[a][b]+aa[a][b]; //합
        dd[a][b]=aa[a][b]-aa[a][b]; //차
    }
}
//출력
for(int i=0; i<2; i++) {
    for(int j=0; j<2; j++) {
        System.out.print(cc[i][j] + " ");
    }
    System.out.println();
}//for end

for(int i=0; i<2; i++) {
    for(int j=0; j<2; j++) {
        System.out.print(dd[i][j] + " ");
    }
    System.out.println();
}//for end  
```





### 7) 10진수 값을 2진수 값으로 변환 후 출력하시오

```java
int num=5; //출력값 101
int[] binary=new int[8];
int count=0;

while(true){//무한루프
    binary[count]=num%2;
    count++;
    num=num/2;
    if(num==1){
        binary[count]=num;
        break;
    }
}//while

for(int a=count;a>=0;a--){ //2진수는 마지막부터 값을 세가니까 거꾸로 하는 조건문을 쓴다.
    						//2,1,0 순으로
    System.out.print(binary[idx7]);
}
```

> 분석

```
count==0
	binary[0]=5%2 > 1
	count++;> 1 
	num=5/2 > 2
	if(false)
	반복문 다시 한번더
count==1	
	binary[1]=2%2 > 0
	count++; 2
	num=2/2 > 1
	if(true)
	binary[2]=1;
	break 멈춤
	
binary[0]=1
binary[1]=0
binary[2]=1

```





### 8) selection sort 알고리즘을 이용해서 오름차순으로 정렬하시오

```java
int[] num={9,7,5,3,1};

for(int a=0;a<num.length-1;a++){ //num.length-1 안하게되면 
    							 //a=4일때 b=5가 되어서 조건에 맞지 않기 때문이다.
    							 //어차피 앞의 수와 비교기때문에 -1을 해준다.
    for(int b=a+1;b<num.length;b++){
        if(num[a]>num[b]){//앞의 수가 뒤에 수보다 크다면
            int tmp=num[a];//가상의 변수에 앞의 수를 저장하고
            num[a]=num[b];//앞의 수 배열에 뒤의 수를 넣는다.
            num[b]=tmp;
        }
    }
}//for
for(int c=0;c<num.length;c++){
    System.out.print(su[c]+" ");
}//for 출력값:1 3 5 7 9
```





### 9) bubble sort 알고리즘을 이용해서 내림차순으로 정렬 후 출력하시오

```java
int[] num={1,2,5,7,9};

for(int a=3;a>=0;a--){ //구하려는 갯수보다 한번 더 적게 단계를 거치니까 a=3
    for(int b=0;b<=a;b++){
        if(num[b]<num[b+1]){
            int tmp=num[b+1];
            num[b+1]=num[b];
            num[b]=tmp;
        }
    }
}//for
for(int idx=0;idx<su.length;idx++){
    System.out.print(su[idx]+" ");
}//for 출력값 : 9 7 5 3 1
```

> 분석

```
a=3
	b=0	
	num[0]<num[1] > 1<2 
    2 1 5 7 9
	b=1
	num[1]<num[2] > 1<5
	2 5 1 7 9
	b=2
	num[2]<num[3] > 1<7
	2 5 7 1 9
	b=3
	num[3]<num[4] > 1<9
	2 5 7 9 1
a=2
	b=0
	num[0]<num[1] > 2<5
	5 2 7 9 1
	b=1
	num[1]<num[2] > 2<7
	5 7 2 9 1
	b=2
	num[2]<num[3] > 2<9
	2 5 9 2 1
...
```

> Arrays 클래스를 이용해서 정렬하기

```java
Arrays.sort(num);
for(int idx=0;idx<su.length;idx++){
    System.out.print(su[idx]+" ");

}//for 출력값 1 3 5 7 9
```







### 10) 이메일 주소 @ 기준으로 분리하기

> @ 없다면 "이메일 주소 틀림" 출력

```java
String email="webmaster@soldesk.com";
int num=email.indexOf("@");	//"@"이 없으면 -1값이 나옴
String[] eword=email.split("@");

if(num==-1){
    System.out.println("이메일주소 틀림");
}else{
    for(int a=0;a<eword.length;a++){
        System.out.println(eword[a]);
    }
}
```







### 11) 주민번호를 이용해 생년월일,성별,나이,전체합을 구하시오

> 주민번호를 이용해서 아래와 같이 출력하시오 > 1512304123456
>
> 생년월일 : 2015년 12월 30일
>
> 성별 : 여자
>
> 나이 : 4

```java
String jumin="1512304123456";
	    	
String year=jumin.substring(0, 2);
String month=jumin.substring(2,4);
String date=jumin.substring(4,6);
String sex=jumin.substring(6,7);

if(sex.equals("1")||sex.equals("2")){
    System.out.println("생년월일 : "+1900+Integer.parseInt(year)+"년 "+month+"월 "+date+"일");
    System.out.println("나이 : "+((2019-1900)-Integer.parseInt(year)));
}else{
    System.out.println("생년월일 : "+20+Integer.parseInt(year)+"년 "+month+"월 "+date+"일");
    System.out.println("나이 : "+((2019-2000)-Integer.parseInt(year)));
}

if(sex.equals("2")||sex.equals("4")){
    System.out.println("성별 : 여자");
}else{
    System.out.println("성별 : 남자");
}
```

> 주민번호 각 숫자의 전체 합 : 1+5+1+2+3+0+4+1+2+3+4+5+6=37

```java
String[] Ijumin=jumin.spilt("");
int sum=0;

for(int a=0;a<Ijumin.length;a++){
    sum+=Integer.parseInt(Ijumin[idx2]); //String형>int형 
}
System.out.println("주민번호 각 숫자의 전체 합 : "+sum2);
```







### 12) 파일이 이미지 파일인지 확인하시오

> 이미지 파일 : gif , jpg , png
>
> 맞으면 "파일이 전송되었습니다."
>
> 아니면 "파일을 다시 선택해주세요."
>
> .lastIndexOf() 뒤에서 부터 찾아옴.
>
> .indexOf() 는 앞에서 부터 찾아옴

```java
String filename="d:java0514/workspace/sky.png";

System.out.println("파일명 : "+filename.substring(filename.lastIndexOf("/")+1));
String check=filename.substring(filename.length()-3);

if(check.equals("png")||check.equals("jpg")||check.equals("gif")){
    System.out.println("파일이 전송되었습니다.");
}else{
    System.out.println("파일을 다시 선택해주세요.");
}
```





### 13) 파일명 ,  확장명을 분리해서 출력하시오

> 파일명 : 2019.05.30.sky
>
> 확장명 : png
>
> 내가 잘라오고 싶은 index에 +1만 해주면 된다.

```java
String filename4="d:/java0514/workspace/2019.05.30.sky.png";

System.out.println("파일명:"+filename4.substring(filename4.lastIndexOf('/')+1));
System.out.println("확장명:"+filename4.substring(filename4.lastIndexOf('.')+1));
```





### 14) 대소문자를 서로 바꿔서 출력하시오

> 클래스를 이용하자
>
> 출력값 gONE wITH tHE wIND

```java
String str="Gone With The Wind";

for(int a=0;a<str.length;a++){
    char ch=str.charAt(a); int형을 char로 변환한 것을 char ch에 저장
    if(Character.isUpperCase(ch)){//만약 대문자라면 > true
        System.out.print(Character.toLowerCase(ch)); //소문자로 변환
    }else if(Character.isLowerCase(ch)){//만약 소문자라면
        System.out.print(Character.toUpperCase(ch));//대문자로출력
    }else{//대,소문자도 아니라면
        System.out.print(ch); //그냥출력
    }//if
}
```





### 15) 주민번호 각 숫자의 누적의 합을 구하시오

> 클래스를 이용

```java
String jumin="8912304123654";
int sum=0;

for(int a=0;a<jumin.length();a++){
    char ch=jumin.charAt(idx3);
    sum+=Character.getNumericValue(ch);
    //'8' > 8 / char > int형으로 변환 
};//출력값 48
System.out.println(sum);
```







### 16) StringTokenizer 이용해 분리된 문자열 가져오기

> 토큰기호로 분리된 문자열 가져오기

```java
String str="Seoul,Jeju,Busan";
StringTokenizer word2=new StringTokenizer(str, ","); //, 기호로 분리하기

while (word2.hasMoreElements()) {//토큰기호가 있는지? / 있을때까지 반복
      //토큰기호로 분리된 문자열 가져오기.
      //토큰기호로 분리된 문자열이 있을때까지 분리한다.
    System.out.println(word2.nextToken());

}//while
/*출력값
* 	Seoul
    Jeju
    Busan
*/
```
