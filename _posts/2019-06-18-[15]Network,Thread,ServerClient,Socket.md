---
title:  "[15]Network,Thread,ServerClient,Socket"
date:   2019-06-18
categories: 
- Java
tags: 
- Network
author: "mincho8050"
---

# Network

- 2개 이상의 PC간 서로 접속
  - LAN : Local Area Network

### 주요 네트워크 관련 명령어

1. ipconfig
   - 내 컴퓨터의 IP 주소를 확인하는 명령어
   - ipconfig > 좀 더 자세한 정보
2. ping
   - 서버의 IP정보도 확인하고 통신이 원활한지 파악하기 위해 사용
3. nslookup

- 네임서버(도메인주소로 매핑)를 조회하는 명령어

1. netstat
   - NETwork STATus , 네트워크 상태 정보
   - netstat -n > 좀 더 자세한 정보
   - TCP 프로토콜
     - 인터넷에서 컴퓨터와 컴퓨터가 연결된 상태에서 서로 데이터를 주고 받는 통신 방법
   - UDP 프로토콜
     - 연결이 끊어진 상태에서도 일방적으로 데이터를 주고 받는 방식
2. tracert
   - 서비스 경로 추적, 서버까지 가기 위한 중간 경로를 보여줌
3. arp
   - IP address와 Mac Address 같이 조회

### IP and Port의 이해

- IP Address. . . . . : 172.16.203.31
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

- TELNET : 텍스트 기반의 원격접속 서비스
- putty.exe
- FTP(File Transfer Protocol) - 파일전송
  - 알드라이브 , FileZilla
- TCP(Transmission Protocol)
- UDP(User Datagram Protocol) - 방송국
- SMTP(Simple Mail Transfer Protocol) - 이메일
- POP3(Post Office Protocol) - 이메일
- DHCP(Dynamic Host Control Protocol) - 유동IP
- ARP(Address Resolution Protocol) - IP 주소를 물리적 주소로 변환
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





------





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





# Ticket 예매 프로그램

- Thread 클래스를 이용한 티켓예매프로그램.



### 1) Data 클래스 생성

> synchronized(동기화) 함수
>
> > 여러 쓰레드에서 공유데이터를 접근하여 사용하고 있을 때, 한 개의 쓰레드에서 공유데이터를 사용하는 중에 쓰레드의 교체가 일어나서 공유데이터가 온전하게 관리되지 못하는 문제가 발생하는데, OS가 개입하여 문제가 발생하지 않도록 조정하는 기법.

```java
class Data{
	
	private int movieTicket=0;	//좌석번호
	public Data(){}
    
	public synchronized String ticket(){
		//좌석번호 생성
		String str="";	//결과값
        
		if(movieTicket<10){
			movieTicket=movieTicket+1;
			str="영화 좌석번호"+movieTicket;
		}//if
		return str;
	}//ticket()
    
	public synchronized int getMovieTicket(){
		//좌석번호 리턴
		return this.movieTicket;
	}//getMovieTicket()

}
```

### 2) Thread 클래스 상속받기

```java
public class Test04_Ticket extends Thread{
	
	String where; //현장구매, 모바일, 인터넷
	Data data;
	public Test04_Ticket(){}
	public Test04_Ticket(String where, Data data) {
		this.where = where;
		this.data = data;
	}

	@Override
	public void run() {
		while(true){
			if(data.getMovieTicket()>=10){
				//티켓이 10장만 있음
				break;
			}//if
			System.out.println(where+"-"+data.ticket());
		}//while
	}//run()


	public static void main(String[] args) {

		Data data=new Data();
		
		Test04_Ticket ticket1=new Test04_Ticket("현장구매",data);
		ticket1.start();
		Test04_Ticket ticket2=new Test04_Ticket("모바일",data);
		ticket2.start();
		Test04_Ticket ticket3=new Test04_Ticket("인터넷",data);
		ticket3.start();
		
		/*
		 * 출력값
		 * 	모바일-영화 좌석번호2
			인터넷-영화 좌석번호3
			현장구매-영화 좌석번호1
			인터넷-영화 좌석번호5
			모바일-영화 좌석번호4
			인터넷-영화 좌석번호7
			현장구매-영화 좌석번호6
			인터넷-영화 좌석번호9
			모바일-영화 좌석번호8
			현장구매-영화 좌석번호10
		 */

		
	}//main

}//class
```





------





# Server/Client

- Server
  - 요청을 받고 응답 할 수 있는 시스템
  - Web Server : 웹브라우저를 통해 웹서비스를 제공
    - 종류 : IIS (MS기반 언어-ASP , 닷넷) , Tomcat (JSP언어 , 무료) , JBoss/Resin (JSP언어 , 유료) , Linux 운영체제 기반 (PHP언어, 무료)
  - Database Server
    - 대용량 저장 장소
    - 관계형 데이터베이스
      - 종류 : Oracle DB , MySQL DB , Maria DB , MS-SQL DB , Orient DB , SQLite
    - No SQL 데이터베이스
      - 종류 : Mongo DB , 카산드라
  - Mail Server
- Client
  - 요청하는 시스템





------



# Socket

- 네트워크 프로그래밍을 위한 인터페이스
- 물리적 소켓
  - UDB 소켓등
- 논리적 소켓
  - 서버소켓
    - 서비스를 제공하는 소켓
  - 클라이언트소켓
    - 서비스에 접속하는 소켓
- ServerSocket
  - 클라이언트보다 먼저 실행되어 클라이언트의 접속 요청을 기다리며, 클라이언트가 접속하면 양방향 통신을 할 수 있는 Socket 객체를 생성한다.
- Socket
  - 다른 Socket과 데이터를 송수신 한다.





------





# Network 프로그램의 운영순서

1. Server : ServerSocket 생성
2. Server : 포트감시 시작, Client의 접속을 기다림
3. Client : Socket 생성 시에 인자 값으로 서버의 IP , PORT를 지정, 서버에 접속 요구
4. Server : Client의 요구를 받아 Socket 객체 생성
5. Server : 생성된 Socket 객체를 이용해 Client에게 데이터를 보냄
6. Client : Socket객체로 데이터를 받고 필요한 데이터를 다시 서버로 전송함





------







# 소켓(Socket)을 이용한 네트워크 프로그래밍

1. 서버는 서비스 제공을 위한 서버소켓을 생성하고 클라이언트의 접속을 기다린다.
2. 클라이언트가 서버에 접속한다
3. 서버에서 연결을 수락하면 데이터 송수신을 위한 새로운 회선이 생성된다.

### 1) 서버가 클라이언트측에 메세지 전송

> 서버구축 (Server)
>
> - ServerSocket
>   - 클라이언트보다 먼저 실행되어 클라이언트의 접속 요청을 기다리며, 클라이언트가 접속하면 양방향 통신을 할 수 있는 Socket 객체를 생성한다.
> - dos창에서 실행할 거라 패키지에 만들지 않았음(명령프롬프트에서 테스트)
> - 내가만든 Server1에 접근한다는 것이다.
>   - port 는 전용문 , 0~65535(2byte)
>   - Server1 클래스만 사용하는 포트번호를 설정한다 > 2019
>   - 클라이언트가 접속할때는 2019를 통해 접속해야한다.
>
> > 이클립스로 하고 run을 하면 cmd에서 한글창에서 깨지니까 따로 메모장에 저장해서 run하자. 이때 경로는 다르게 하고 실행시키기.

```java
import java.io.BufferedWriter;
import java.io.OutputStreamWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class Server1 {
	
	public static void main(String[] args) {

		ServerSocket server=null;
		try{
            server=new ServerSocket(2019);
			
			while(true){//언제들어올지 모르니까 무한루트 , 무조건대기
                
				//서버를 run하면 띄우는 메세지
				System.out.println("\n클라이언트 접속 대기중...");
               	//다른 Socket과 데이터를 송수신 합니다.
				Socket socket=server.accept();
                System.out.println("[접속]접속IP: "+socket.getInetAddress().getHostAddress());
                
                //요청한 사용자 pc에 메세지 출력하기.
				BufferedWriter writer=new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
				writer.write("[본인이름]/1.토이스토리/2.알라딘/3.기생충");
				writer.newLine();	//줄바꿈
				writer.flush();		//client에 전송
                
				socket.close();//접속자 연결 종료
                
			}//while
			
		}catch(Exception e){
			System.out.println("SverError"+e);
		}finally{
			//finally문에도 try~catch문이 올 수 있다.
			try{
				server.close();//자원반납
			}catch(Exception e){
				System.out.println("SverClose"+e);
			}//try
			
		}//try

	}//main

}//class
```

> 클라이언트 구축 (Client)
>
> > 명령프롬프트에서 실행하기
> >
> > 1. javac Client1.java
> > 2. java Client1 요청서버IP
> >
> > - 메인함수가 받음. String[] args

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.Socket;

public class Client1 {

	public static void main(String[] args) {
		Socket socket=null;
		try{
			
			//>java Client1 172.16.83.100 > 선생님 IP
			socket=new Socket(args[0],2019);
			//접속하면 Server의 ip가 보임.
			System.out.println("[접속]서버IP: "+socket.getInetAddress().getHostAddress())		
			BufferedReader reader=new BufferedReader(new InputStreamReader(socket.getInputStream()));
			String line=reader.readLine();
			String[] movie=line.split("/");
            
			System.out.println("----------------------------------");
            
			for(int idx=0;idx<movie.length;idx++){
				System.out.println(movie[idx]);
			}//for
            
			System.out.println("----------------------------------");
			
		}catch(Exception e){
			System.out.println("ClientError"+e);
		}finally{
			
			try{
				socket.close();//자원반납
			}catch(Exception e){
				System.out.println("ClientClose"+e);
			}//try
			
		}//try
		
	}//main

}//class
```

### 2) 클라이언트가 서버측에 메세지 전송

- 서버가 클라이언트측에 보내는 것과 반대로 하면 됨.

> 서버 구축

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.ServerSocket;
import java.net.Socket;


public class Server2 {

	public static void main(String[] args) {
        
		ServerSocket server=null;
		try{
			
			server=new ServerSocket(2020);
			System.out.println("\n클라이언트 접속 대기중...");
			
			while(true){
				
				Socket socket=server.accept();
				System.out.println("[접속]접속IP: "+socket.getInetAddress().getHostAddress());//요청한 사람의 ip가 나오는것.
				
				BufferedReader reader=new BufferedReader(new InputStreamReader(socket.getInputStream()));
				String line=reader.readLine();
				String[] str=line.split("/");
				System.out.println("----------------------------------");
				for(int idx=0;idx<str.length;idx++){
					System.out.println(str[idx]);
				}//for
				System.out.println("----------------------------------");
				
				try{
					socket.close(); //접속자 연결 종료
				}catch(Exception e){}
				
				
			}//while
			
			
		}catch(Exception e){
			System.out.println("ServerError"+e);
		}finally{
			//finally문에도 try~catch문이 올 수 있다.
			try{
				server.close();//자원반납
			}catch(Exception e){
				System.out.println("ServerClose"+e);
			}//try
			
		}//try

		
	}//main

}//class
```

> 클라이언트 구축

```java
import java.io.BufferedWriter;
import java.io.OutputStreamWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class Client2 {

	public static void main(String[] args) {
        
		Socket socket=null;
		try{
			socket=new Socket(args[0],2020);
			System.out.println("[접속]접속IP: "+socket.getInetAddress().getHostAddress());//요청한 사람의 ip가 나오는것.
			BufferedWriter writer=new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
			writer.write("[색상]/1.빨간색/2.검정색/3.노란색");
			writer.newLine();	
			writer.flush();		
		
		}catch(Exception e){
			System.out.println("ClientError"+e);
		}finally{
			//finally문에도 try~catch문이 올 수 있다.
			try{
				socket.close();//자원반납
			}catch(Exception e){
				System.out.println("ClientClose"+e);
			}//try
			
		}//try
		
	}//main

}//class
```





------





# 1:1 채팅 프로그램 구축하기



### 1) Server 구축하기

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.ServerSocket;
import java.net.Socket;


class ChatServerRead extends Thread{	//데이터 읽어오기
										//Server2.java 참조
	private Socket socket;
	private String line;
	public ChatServerRead() {}
	public ChatServerRead(Socket socket) {
		this.socket = socket;
	}
	
	@Override
		public void run() {
			
		try{
			
			BufferedReader reader=new BufferedReader(new InputStreamReader(socket.getInputStream()));
			while(true){
				line=reader.readLine();
				if(line==null){break;}//
				System.out.println("\n"+line);
				System.out.print("[서버]");
			}//while
			
		}catch(Exception e){
			System.out.println("데이터 전송 실패 !!"+e);
		}finally{
			try{
				socket.close();
			}catch(IOException e){
				e.printStackTrace();
			}
		}

		}//run()
	
}//class

class ChatServerSend extends Thread{	//데이터 보내기
										//Server1.java 참조
	private Socket socket;
	private BufferedWriter writer;
	private BufferedReader in;
	private String str="";
	
	public ChatServerSend() {}
	public ChatServerSend(Socket socket) {
		this.socket = socket;
	}
	
	@Override
		public void run() {
			
			try{
				writer=new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
				//키보드에서 입력
				in=new BufferedReader(new InputStreamReader(System.in));
				
				while(true){
					System.out.print("[서버]");
					str=in.readLine();	//키보드로부터 입력 대기
					if(str.equals("exit"))break;
					
					writer.write(" [서버] "+str);
					writer.newLine();//줄바꿈기호가 있어야 BufferedReader의 readLine()이 인식함
					writer.flush();  //client로 전송
				}//while
				
				
			}catch(Exception e){
				System.out.println("데이터 전송 실패 !!"+e);
			}finally{
				try{
					socket.close();
				}catch(IOException e){
					e.printStackTrace();
				}
			}

		}//run()
	

}//class


public class ChatServerThread {
	
	private ServerSocket server=null;
	private Socket socket=null;
	
	public void serverStart(){
		System.out.println("접속자를 기다리는 중...");
		try{
			
			server=new ServerSocket(6901);
			socket=server.accept();
			
			//socket으로 부터 데이터를 읽어오는 쓰레드
			//ChatServerRead 클래스 : Server2.java 참조
			ChatServerRead read=new ChatServerRead(socket);
			read.start(); //run() 호출
			
			//키보드에서 입력받아 socket으로 데이터를 보내는 쓰레드
			//ChatServerSend 클래스 : Server1.java 참조
			ChatServerSend send=new ChatServerSend(socket);
			send.start();
					
		}catch(Exception e){
			System.out.println("연결이 되어 있지 않습니다."+e);
		}finally{
			try{
				server.close();
			}catch(IOException e){
				e.printStackTrace();
			}
		}
		
	}//serverStart()

	public static void main(String[] args) {

		ChatServerThread cs=new ChatServerThread();
		cs.serverStart();

	}//main

}//class
```

### 2) Client 구축하기

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.Socket;
import org.xml.sax.InputSource;

class ChatClientRead extends Thread{
	private Socket socket;
	private String line;
	
	public ChatClientRead() {}
	public ChatClientRead(Socket socket) {
		this.socket = socket;
	}
	
	@Override
	public void run() {
		
		try{
			
			BufferedReader reader=new BufferedReader(new InputStreamReader(socket.getInputStream()));
			while(true){
				line=reader.readLine();	//네트워크로부터 전송 받음
				if(line==null) break;
				System.out.println("\n"+line);
				System.out.println("[내이름]");
			}//while
			
			
		}catch(Exception e){
			System.out.println("연결이 되어 있지 않습니다"+e);
		}finally{
			try{
				socket.close();
			}catch(IOException e){
				e.printStackTrace();
			}
		}
		
		
	}//run()
	

}//class

class ChatClientSend extends Thread{
	private Socket socket;
	private BufferedWriter writer;
	private BufferedReader in;
	private String str;
	
	public ChatClientSend() {}
	public ChatClientSend(Socket socket) {
		this.socket = socket;
	}
	
	@Override
	public void run() {
		try{
			writer=new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
			in=new BufferedReader(new InputStreamReader(System.in));	//키보드 입력
			
			while(true){
				System.out.println("[본인이름]");
				str=in.readLine();
				if(str.equals("exit"))break;
				writer.write(" [본인이름] "+str);
				writer.newLine();
				writer.flush();
			}//while
			
		}catch(Exception e){
			System.out.println("데이터 전송 실패"+e);
		}finally{
			try{
				socket.close();
			}catch(IOException e){
				e.printStackTrace();
			}
		}	
		
	}//run()
	
}//class

public class ChatClientThread {
	
	private Socket socket=null;
	
	public void clientStart(String ip,int port){
		System.out.println("클라이언트 프로그램 작동...");
		
		try{
			
			socket=new Socket(ip,port);
			
			//socket으로 부터 데이터를 읽어오는 쓰레드
			//ChatClientRead 클래스 : Client2.java 참조
			ChatClientRead read=new ChatClientRead(socket);
			read.start();//run() 호출
			
			//키보드에서 입력받아 socket으로 데이터를 보내는 쓰레드
			//ChatClientSend 클래스 : Client1.java 참조
			ChatClientSend send=new ChatClientSend(socket);
			send.start();
			
		}catch(Exception e){
			System.out.println("서버와 연결 실패: "+e);
		}finally{
			try{
				if(socket==null)socket.close();
			}catch(IOException e){
				e.printStackTrace();
			}
		}
		
	}//clientStart()

	public static void main(String[] args) {
		
		ChatClientThread cc=new ChatClientThread();
		cc.clientStart(args[0], 6901);

	}//main

}//class
```

