---
title:  "[21]MongoDB,MongoDB 문법,MongoDBJDBC"
date:   2019-06-23
categories: 
- Database
tags: 
- Database
author: "mincho8050"
---



# MongoDB

- 빅데이터의 저장기술 NoSQL

### 개론

- NoSQL (Not Only SQL)
- 전통적인 관계형 데이터베이스 관리 시스템(DBMS)과는 다르게 설계된 비관계형 DBMS로 대규모 데이터를 유연하게 처리할 수 있다는 장점을 있다.
- RDBMS의 "관계형" 특징을 제거하고 만들어진 다른 모든 DBMS를 말한다

### NoSQL DB 종류

- HBase
- MongoDB
- 카산드라 DB
- CouchDB

### 쿼리

1. MongoDB - Set of DBs : DB들의 모음 DB - Set of Collections : 콜렉션들의 모음 Collection - Set of Documents : 도큐먼트들의 모음 Document - JSON-style objects {field: value, …} : 하나의 JSON-style인 데이터 라고 생각하시면됩니다.
2. CRUD

 C (Create) - 데이터를 생성 (명령어 : insert)  R (Read) - 데이터 읽기 (명령어 : find)  U (Update) - 데이터 수정 (명령어 : update)  D (Delete) - 데이터 삭제 (명령어 : remove)

1. MySQL과 비교

```
     -----------------------------------------------------------
     MySQL 용어   Mongo 용어 
     ----------------------------------------------------------
     database     				database 
     table        				collection 
     index        				index
     row          				BSON document
     column       				BSON field
     join         				embedding and linking
     튜플/로우     				도큐먼트(Document)
     기본키(Primary Key)        기본키(Primary Key - 몽고DB는 자체적으로 _id라는 디폴트 키를 제공
     테이블 조인                 Embedded Documents
     ------------------------------------------------------------
```



### mongoDB 설치

- [https://www.mongodb.com](https://www.mongodb.com/) -> Download -> Community Server -> mongodb-win32-x86_64-2008plus-ssl-3.2.9-signed.msi 다운후 설치



### 작업폴더

1. 폴더생성 c:\data\db
2. 몽고DB 서버실행 cmd cd C:\Program Files\MongoDB\Server\3.2\bin>mongod
3. 몽고DB 실행 cmd cd C:\Program Files\MongoDB\Server\3.2\bin>mongo
4. 에러상황 mongod.exe - 시스템 오류

\- windows server 2012 r2 64bit 에서 bin폴더에서 서비스 등록시 아래와 같은 에러 발생

```
\- 구글 검색시 이것은 Visual C++ Redistributable for Visual Studio 2015 재배포패키지가 윈도우에
     기본적으로 없다는 얘기가 있어서 재배포패키시 (64bit)설치
     
     https://www.microsoft.com/en-us/download/details.aspx?id=48145
     
     재배포패키지 설치후 서비스 실행을 확인함
```





------



# MongoDB 문법

- show dbs --데이터베이스 목록 조회(새로 생성한 데이터베이스 확인 가능)
- use mycustomers --데이터베이스 생성. 이미 만들어진 경우라면 기존의 데이터베이스를 반환한다. mycustomers라는 이름의 db생성
- show collections -- 컬렉션 목록
- db --현재 사용하는 데이터베이스 조회

### INSERT

```
>db.customers.insert({first_name:"John", last_name:"Doe"});
>db.customers.find();			--값 출력하기
>db.customers.insert([{first_name:"Steven", last_name:"Smith"},{first_name:"Joan", last_name:"Johnson",gender:"female"}]);
>db.customers.find();
>db.customers.find().pretty();	--정렬해서 보여주기
```

[![result](https://user-images.githubusercontent.com/49340180/60311278-72fcf980-9991-11e9-80fc-ab5a82db9998.PNG)](https://user-images.githubusercontent.com/49340180/60311278-72fcf980-9991-11e9-80fc-ab5a82db9998.PNG)

> "_id" 유일성 칼럼.

### SQL과 NoSQL

[![01](https://user-images.githubusercontent.com/49340180/60311597-dc313c80-9992-11e9-8f48-d317ca13c517.PNG)](https://user-images.githubusercontent.com/49340180/60311597-dc313c80-9992-11e9-8f48-d317ca13c517.PNG)







------





# MongoDBJDBC



```java
package jdbc0628;
import org.bson.Document;

import com.mongodb.MongoClient;
import com.mongodb.client.FindIterable;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoCursor;
import com.mongodb.client.MongoDatabase;
public class MongoDBJDBC {

	public static void main(String[] args) {
		//mongod.exe 실행하면 27017 보임
        //로컬호스트에 있는 몽고DB 서버에 접속. 포트 27017
		/*
		 * 선생님이 하신버전
		 * 	- MongoDB 드라이버 다운
			  -> https://repo1.maven.org/maven2/org/mongodb/mongo-java-driver/
			  -> mongo-java-driver-3.2.2.jar
		 */
        MongoClient mongoClient = new MongoClient("localhost", 27017); 
        
        MongoDatabase db = mongoClient.getDatabase("mycustomers");
        System.out.println("데이터베이스명: "+db.getName());

        MongoCollection<Document> collections = db.getCollection("customers");
        
        FindIterable<Document> iterate = collections.find();
        MongoCursor<Document>  cursor = iterate.iterator();
        
        while(cursor.hasNext()) {
          Document document = cursor.next();
          String JsonResult = document.toJson();
          System.out.println(JsonResult);
        }

	}

}
```



### MongoDB연결하기

- mycustomers 데이터베이스에 있는 customers 도큐먼트 내용을 출력하기

```java
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.DBCursor;
import com.mongodb.MongoClient;
import com.mongodb.WriteConcern;



public class Test01_DBOpen {

	public static void main(String[] args) {

		MongoClient mongoClient=null;

		try{
			
			mongoClient=new MongoClient("localhost",27017);
			System.out.println("접속 성공");
			
/*			//쓰기권한 부여
			WriteConcern w=new WriteConcern(1,2000);
			//쓰기 락 갯수, 연결 시간 2000 //쓰레드 쓰게되면 2개 동시에 쓸 경우도 생기니까
*/			
			//데이터베이스 연결
			DB db=mongoClient.getDB("mycustomers");
			//컬렉션 가져오기
			DBCollection coll=db.getCollection("customers");
			
			//customers의 모든 데이터 가져오기
			DBCursor cursor=coll.find();
			while(cursor.hasNext()){
				//있는 값을 반복문을 이용하여 모두 콘솔에 출력하기.
				System.out.println(cursor.next());

			}//

			/*
			 * 출력값
			 * -> 빨간글자로 나오는것은 부연설명
			 * 	{"_id": {"$oid": "5d156ff9e7e5402f3c308315"}, "first_name": "John", "last_name": "Doe"}
				{"_id": {"$oid": "5d15700fe7e5402f3c308316"}, "first_name": "Steven", "last_name": "Smith"}
				{"_id": {"$oid": "5d15700fe7e5402f3c308317"}, "first_name": "Joan", "last_name": "Johnson", "gender": "female"}
			 */
			
			
		}catch(Exception e){
			System.out.println(e);
			
		}
		
	}//main

}//class
```

