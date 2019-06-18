---
title:  "[15]Thread,Network"
date:   2019-06-17
categories: 
- Java
tags:
- Network
author: "mincho8050"
---

# Thread 클래스

- 하나의 점유 프로그램이나 하나의 메소드가 CPU 자원을 전부 점유하는 것을 막을 수 있다.
  - ex) 채팅 프로그램.



### 1. 쓰레드를 사용하지 않는 경우.

> class

```java
class MyThread1{
	
	private int num;
	private String name;
	
	public MyThread1() {} //기본생성자함수
	public MyThread1(int num, String name) {
		this.num = num;
		this.name = name;
	}
	
	public void run(){
		for(int a=0;a<num;a++){
			System.out.println(name+":"+a);
		}
	}//run
	public void start(){
		run(); //start함수에는 run();함수만 넣는다. 
	}//start
	
	
}//class
```

> main
>
> t1 이 끝나야 t2,t3가 순서대로 나온다. 

```java
public static void main(String[] args) {
    
MyThread1 t1=new MyThread1(100, "★");
MyThread1 t2=new MyThread1(100, "★★");
MyThread1 t3=new MyThread1(100, "★★★");

t1.start();
t2.start();
t3.start();
/*
출력값(일부)
★:0~★:99
★★:0~★★:99
★★★:0~★★★:99
*/
    
}//main
```



### 2. 쓰레드를 사용하는 경우

- Thread클래스와 인터페이스Runnable 있다.
- Thread클래스를 받으면 단일상속 밖에 안되기 때문에, 보통 인터페이스Runnable이 활용도가 더 높다.
  - 인터페이스는 다중상속이 가능하다.
- JVM이 쓰레드 관리자에 등록하고 start()메소드가 run()을 호출한다.

###### Thread 클래스를 상속받는 경우

> class
>
> > Thread 클래스를 상속받음
> >
> > > 클래스가 클래스 상속받는 경우는 단일 상속만 가능
> >
> > start() , run() 함수가 있음
> >
> > > 오버라이드 해야함. 대신 run() 함수만.

```java
class MyThread2 extends Thread{
	private int num;
	private String name;
	public MyThread2() {} //기본생성자함수
	public MyThread2(int num, String name) {
		this.num = num;
		this.name = name;
	}
    
    //Thread 클래스에서 물려받은 함수를 오버라이드 하기.
	@Override
		public void run() {
			for(int a=0;a<num;a++){
				System.out.println(name+":"+a);
			}
		}//run
	//start함수는 실제적으로 run()을 실행하는것이기 때문에 굳이 오버라이드 안해도 된다.
	
}//class
```

> main
>
> CPU의 빈공간을 찾아서 들어간다. 그래서 출력값이 순서대로 나오지 않음

```java
public static void main(String[] args) {
    
MyThread2 t1=new MyThread2(1000, "★");
MyThread2 t2=new MyThread2(1000, "★★");
MyThread2 t3=new MyThread2(1000, "★★★");

t1.start();
t2.start();
t3.start();

/*
 * 출력값(일부)
 * 	★:0
    ★★★:0
    ★★:0
    ★★★:1
    ★:1
    ★★★:2
    ★★:1
    ★★★:3
 */  
    
}//main
```

###### Runnable 인터페이스를 이용한 경우

> class
>
> 다중상속 가능

```java
class MyThread3 implements Runnable{ //구현
	private int num;
	private String name;
	public MyThread3() {} //기본생성자함수
	public MyThread3(int num, String name) {
		this.num = num;
		this.name = name;
	}
	
	@Override
		public void run() {
			for(int a=0;a<num;a++){
				System.out.println(name+":"+a);
			}
		}//run
	
}//class
```

> main

```java
public static void main(String[] args) {
        //다형성
		//Runnable target=new Thread();
		Thread t1=new Thread(new MyThread3(1000,"★"));
		Thread t2=new Thread(new MyThread3(1000,"★★"));
		Thread t3=new Thread(new MyThread3(1000,"★★★"));

		t1.start();
		t2.start();
		t3.start();
        /*
		 출력값(일부)
		  	★:0
			★:1
			★★★:0
			★★:0
			★★★:1
		 */
    
}//main
```







------







# Network

- 2개 이상의 PC간 서로 접속
  - LAN : Local Area Network



### 주요 네트워크 관련 명령어

1. ipconfig
   - 내 컴퓨터의 IP 주소를 확인하는 명령어
   - ipconfig > 좀 더 자세한 정보
2. ping
   - 서버의 IP정보도 확인하고 통신이 원활한지 파악하기 위해 사용
3.  nslookup
   - 네임서버(도메인주소로 매핑)를 조회하는 명령어
4. netstat
   - NETwork STATus , 네트워크 상태 정보
   - netstat -n > 좀 더 자세한 정보
   - TCP 프로토콜
     - 인터넷에서 컴퓨터와 컴퓨터가 연결된 상태에서 서로 데이터를 주고 받는 통신 방법
   - UDP 프로토콜
     - 연결이 끊어진 상태에서도 일방적으로 데이터를 주고 받는 방식
5. tracert
   - 서비스 경로 추적, 서버까지 가기 위한 중간 경로를 보여줌
6. arp
   - IP address와 Mac Address 같이 조회





### IP and Port의 이해

- IP Address. . . .  . : 172.16.203.31 
- 접속 위치를 나타내는 프로토콜 , 위치 정보에 해당하며 컴퓨터를 구분하는 용도
- 네트워크로 접속되는 지점. 
- 전산에서는 하나의 IP에 할당된 여러개의 네트워크 프로그램을 구분하는 용도
- 하나의 포트는 하나의 프로그램과 매핑(연결)됨
- IP 하나당 사용 가능한 포트 : 0 ~ 65535 (2Byte)

```
 알려진 사용할 수 없는 포트 
     20, 21 : FTP, 파일 전송 
     22 : Secure Shell 접속 
     23 : Telnet, 원격 접속 
     25 : SMTP, 메일 전송 
     80 : HTTP, Apache, IIS등 웹서버, 인터넷 웹 페이지 서비스 
     3306 : MySQL 기본 포트, DBMS 
     1521 : Oracle 기본 포트, DBMS 
     8080 : Apache, 기타 웹 서버 
     1433 : MS-SQL 기본 포트, DBMS

-1500번 이하는 시스템이 사용하는 포트가 많음으로 1500번 이상 사용을 
 권장합니다.
```



### 프로토콜(Protocol)

- 서로 다른 컴퓨터 간의 의사소통을 위한 통신 규약
- 운영체제도 다를 수 있고, 모바일 장비와 PC, 다양한 하드웨어와 다양한 운영체제 간에 서로 주고 받으려면 미리 약속을 해야 함
- 컴퓨터끼리 정보를 주고 받을때의 통신방법에 대한 약속



###### 프로토콜의 종류

-  TELNET : 텍스트 기반의 원격접속 서비스
  - putty.exe
- FTP(File Transfer Protocol) - 파일전송
  - 알드라이브 , FileZilla
- TCP(Transmission Protocol)
- UDP(User Datagram Protocol) - 방송국
- SMTP(Simple Mail Transfer Protocol) - 이메일
- POP3(Post Office Protocol)  - 이메일
- DHCP(Dynamic Host Control Protocol) - 유동IP
- ARP(Address Resolution Protocol)    - IP 주소를 물리적 주소로 변환
- HTTP(Hyper Text Transfer Protocol)
  - 웹 서비스. 웹브라우저에서 hyper text 문서를 교환하기 위한 프로토콜
  - 인터넷에서 하이퍼텍스트(hyper text) 문서를 교환하기 위하여 사용되는 통신규약.
  - 하이퍼텍스트는 문서 중간중간에 특정 키워드를 두고 문자나 그림을 상호 유기적으로 결합하여 연결시킴으로써, 서로 다른 문서라 할지라도 하나의 문서인 것처럼 보이면서 참조하기 쉽도록 하는 방식을 의미한다. Server에 저장되어 있는 데이터를 사용자가 요청하면 그때마다 데이터를 보여주기 위해 사용되는 Protocol이다.
- HTTPS(HyperText Transfer Protocol over Secure Socket Layer)
  - 월드 와이드 웹 통신 프로토콜인 HTTP의 보안이 강화된 버전이다.





### 네트워크 관련된 클래스

- HTTP Protocol과 관련된 클래스
- java.net

> try~catch문

```java
InetAddress ip=InetAddress.getByName("www.soldesk.com");
System.out.println(ip.getHostName()); //www.soldesk.com
System.out.println(ip.getHostAddress()); //211.40.118.45 , ip확인
```

###### HTML 문서 내용 가져오기

> try~catch문

```java
String address="http://www.soldesk.com";
URL url=new URL(address);
BufferedReader br=new BufferedReader(new InputStreamReader(url.openStream()));

//엔터를 기준으로 한줄씩 불러오기
while(true){
    String line=br.readLine();
    if(line==null){
        break;
    }//if
    System.out.println(line);
}//while
```

###### 웹페이지문서 저장하기

> try~catch문

```java
String address="http://www.soldesk.com";
URL url=new URL(address);
InputStream in=url.openStream();
FileOutputStream out=new FileOutputStream("d:/java0514/workspace/basicNetwork/soldesk.html");
//여기 경로에 저장됨.

while(true){
    int data=in.read();
    if(data==-1){
        break;
    }//if
    out.write(data);
}//while

in.close();
out.close();
```

