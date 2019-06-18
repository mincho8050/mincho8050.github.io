---
title:  "[16]Server/Client,Socket"
date:   2019-06-18
categories: 
- Java
tags: 
- Network
author: "mincho8050"
---

# Server/Client

- Server
  - 요청을 받고 응답 할 수 있는 시스템
  - Web Server : 웹브라우저를 통해 웹서비스를 제공
    - 종류 : IIS (MS기반 언어-ASP , 닷넷) , Tomcat (JSP언어 , 무료) , JBoss/Resin (JSP언어 , 유료) , Linux 운영체제 기반 (PHP언어, 무료) 
  - Database Server
    - 대용량 저장 장소
    - 관계형 데이터베이스
      - 종류 : Oracle DB , MySQL  DB , Maria DB , MS-SQL DB , Orient DB , SQLite
    - No SQL 데이터베이스
      - 종류 : Mongo DB , 카산드라 
  - Mail Server
- Client
  - 요청하는 시스템





------





# 소켓(Socket)

- 네트워크 프로그래밍을 위한 인터페이스
- 물리적 소켓
  - UDB 소켓등
- 논리적 소켓
  - 서버소켓
    - 서비스를 제공하는 소켓
  - 클라이언트소켓
    - 서비스에 접속하는 소켓



### 소켓(Socket)을 이용한 네트워크 프로그래밍

1. 서버는 서비스 제공을 위한 서버소켓을 생성하고 클라이언트의 접속을 기다린다. 
2. 클라이언트가 서버에 접속한다
3. 서버에서 연결을 수락하면 데이터 송수신을 위한 새로운 회선이 생성된다.



###### 1) 서버가 클라이언트측에 메세지 전송

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



###### 2) 클라이언트가 서버측에 메세지 전송

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

