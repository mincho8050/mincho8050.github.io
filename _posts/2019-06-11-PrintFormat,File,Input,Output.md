---
title:  "PrintFormat,File class,Input,Output"
date:   2019-06-11
categories: Java
tags: 
- basicJava
author: "mincho8050"
---


# PrintFormat

- 출력서식
  - 콘솔창 위주

- 큰따옴표 출력 > ( \ "  )
  - 기호로 나타내려면 역슬래쉬 이용. \\
- 역슬래쉬 출력 > ( \\ \\ )
  - \\ 출력 시 2번 씩
  - 경로구분할때 역슬래쉬를 하는데 이때 두번씩한다.
  - 웹에서는 / 가 경로구분인데 자바도 허용이라 / 써도 된다.

```java
System.out.println("d:\\java0514\\workspace");
System.out.println("d:/java0514/workspace");
```

- 줄바꿈 > (\\n)
- tap키 > (\\t)
- (\\r) , (\\b) > 잘안씀



- 10진 정수형 > %d
- 실수형 > %f
- 문자형 > %c
- 문자열형 > %s
  - 콘솔과 메모장 기준

```java
System.out.printf("출력포맷",값);
System.out.printf("%d %d %d\n",1,3,5);
System.out.printf("%d, %d, %d\n",1,3,5);
System.out.printf("%d \t %d \t %d\n",1,3,5);

System.out.printf("나이:%d\n",20);

//#은 칸수확인용
System.out.printf("#%5d#\n",2);
System.out.printf("#%-5d#\n",2);
        /*
		 * 출력값
		 *  1 3 5
			1, 3, 5
			1 	 3 	 5
			나이:20
			#    2#
			#2    #
		 */
```







------







# File 클래스

- 파일 관련 정보를 알 수 있다.



> try~catch문에서 작성하자!

- 경로명+파일명
- 경로명에서는 \\\\ 두번해야 문자로 인식하지만, 웹에서 사용하는 / 사용해도 자바는 허용

```java
String pathname="D:/java0514/setup/jdk-8u211-windows-x64.exe";
File file=new File(pathname);
```

- .exists() 파일이 존재하는지 유무 > boolean타입.
- 이걸 이용해 if문으로 파일이 존재하면(treu) 파일을 가져와 가공

```java
if(file.exists()){
    System.out.println("파일 가져오기 성공");
    long filesize=file.length(); //return형이 long!

    //파일크기. byte로 나오기때문에 필요한 단위는 가공.
    System.out.println("파일크기:"+filesize+"Byte"); //자바는 여기까지만
    System.out.println("파일크기:"+(filesize/1024)+"KB");
    /*출력값
     * 	파일크기:225748832Byte
        파일크기:220457KB
     */
```

- 파일명.확장명

```java
String filename=file.getName();
System.out.println("파일명.확장명:"+filename);
//출력값 jdk-8u211-windows-x64.exe
```

- 경로명+파일명

```java
//출력값 : D:\java0514\setup\jdk-8u211-windows-x64.exe
System.out.println(file.getPath());
//출력값 : D:\java0514\setup
System.out.println(file.getParent());
```

> 파일명과 확장명을 분리해라
>
> > .을 기준으로 나누다보면 파일명에 . 이 있을 수 있다.
> >
> > > .lastIndexOf(); 를 사용한다.
> >
> > 맨 마지막 . 을 기준으로 나눌 수 있게 한다.
>
> 만약 파일명 분리 후 파일명이 같으면
>
> 파일명(1) , 파일명(2) , 파일명(3) 이런식으로 파일명 변경하도록 하는 프로그래밍.

```java
int lastdot=filename.lastIndexOf("."); //마지막 .을 찾는것.
String name=filename.substring(0,lastdot);
String ext=filename.substring(lastdot+1);
System.out.println("파일명:"+name);
System.out.println("확장자:"+ext);

/*
 * 출력값
 * 	파일명:jdk-8u211-windows-x64
    확장자:exe
 */
```

- 파일삭제
- boolean형

```java
file.delete();
```

- 만약 파일이 없다면. 

```java
}else{
	System.out.println("파일 없어요!!");
}//if
```







------







# Input

- 파일 내용 읽기



> try~catch문으로 작성
>
> 예시 파일경로임.
>
> > D:/java0514/workspace/basicJava/src/oop0610/Test04_BuyerTest.java

**1. 파일 가져오기**

- 내용을 가져오는 것은 File클래스 사용해도 되지만 내용을 보려면 FileReader클래스 이용

```java
String fileName="D:/java0514/workspace/basicJava/src/oop0610/Test04_BuyerTest.java";
FileReader in=new FileReader(fileName);
```

**2. 파일 내용 읽기**

- reader계열이니까 BufferedReader클래스 이용

```java
BufferedReader br=new BufferedReader(in);
```

**3. while 무한루프로 출력하기, 엔터(\\n), 줄의 끝(\\r) 기준으로 한줄 씩 가져오기**

```java
while(true){ //무한루프
 
        String line=br.readLine();
        if(line==null){ //줄의 마지막은 null값이 나옴
            break;
        }//if

        System.out.println(line); //이렇게 출력해도 되고

}//while
```

**4. 자원 반납**

- 순서 주의!!!

```java
br.close();	//나중에 연거
in.close(); //맨처음 연겨
```





### InputStream기반과 Reader기반 비교



**1. FileInputStream**

- 1바이트 기반
- 한글깨짐

> try~catch문으로 작성

```java
String fileName="D:/java0514/workspace/basicJava/src/oop0610/Test04_BuyerTest.java";

FileInputStream fis=new FileInputStream(fileName);
    int data=0;

    while(true){//무한루프
        data=fis.read(); //1바이트씩 읽기.
        if(data==-1){ //파일의 끝(End Of File)인지?
            break;
        }//if
        System.out.print((char)data);
        //그냥 data를 하면 int값이 나오니까 형변환
        //근데 1바이트씩 읽는거라 한글은 깨짐

}//while

fis.close();//자원반납!
```

**2. FileReader**

- char(2바이트)기반
- 한글 안깨짐

> try~catch문으로 작성

```java
String fileName="D:/java0514/workspace/basicJava/src/oop0610/Test04_BuyerTest.java";
FileReader fr=new FileReader(fileName);
    int data=0;
    while(true){
        data=fr.read();
        if(data==-1){
            break;
        }//if
    System.out.print((char)data);

}//while

fr.close();
```







------







# Output

- 파일출력
- 입출력
  - 표준입출력:모니터(console) , 키보드
  - 파일입출력



- 출력파일(sungjuk.txt)
  - 없으면 생성된다.(creat)
  - 있으면 추가(append) 또는 덮어쓰기(overwrite)
  - 추가할려면 true , 덮어쓰기는 false

> try~catch문으로 작성

```java
String filenName="D:/java0514/testFile/sungjuk.txt"; 
FileWriter fw=new FileWriter(filenName,false);	//false파일이 있으면 덮어쓰기. 
												//true는 추가
```

- autoFlush
  - true 버퍼 클리어

```java
PrintWriter out=new PrintWriter(fw,true);
out.println("무궁화,95,90,100");
out.println("홍길동,30,55,40");
out.println("라일락,60,95,75");
out.println("진달래,20,85,65");
out.println("봉선화,100,35,100");

out.close();
fw.close();

System.out.println("sungjuk.txt 데이터 파일 완성!");
```
