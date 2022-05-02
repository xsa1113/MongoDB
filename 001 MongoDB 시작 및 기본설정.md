## NOSQL

- 초고용량 데이터 처리, 성능에 특화된 목적을 위한, 비관계형 데이터 저장소
- 데이터 저장하기 위한 분산 저장 시스템
- 매우 큰 데이터 볼륨을 바탕으로 어떤 DB를 활용할지 생각해보기

## MongoDB

- 데이터를 json 2진 버전이다.
- 웹 인터넷 기반 서비스를 위함
- `스크립트형`기반으로도 가능하다
- `비정형`데이터 기반도 다룰 수 있다
- `키` 와 `value`값으로 이루어져 있ㅇ므

## 설치

[Node.js MongoDB Create Database](https://www.w3schools.com/nodejs/nodejs_mongodb_create_db.asp)

- 리소스 다운로드

[MongoDB Community Download](https://www.mongodb.com/try/download/community)

- 환경설정 (path설정)
- mongo db / bin 파일 path 설정

## 사용법

`use db`

- db라는 이름으로 사용하겠다

`show dbs`

- 전체 데이터 베이스 확인

`show collections`, `show tables`

- 전체 테이블 확인

`db.getCollectionsName()`

- 데이터베이스 이름 getCollectionsName → 배열 형태로 테이블 확인 가능

`db.getCollectionsName("만들테이블이름")`

- 테이블 만들기

## Create

```java
// colleciton 생성
db.user.insertOne(
	//field : value 
	{
		name : "sue",
		age : 26
	}

)

// 삽입

db.books.insert([
... {title:"HTML", author:"mary", price:150},
... {title:"CSS3", author:"Celin", price:200},
... {title:"C++", author:"smith", price:300}
... ])

// 결과
BulkWriteResult({
        "writeErrors" : [ ],
        "writeConcernErrors" : [ ],
        "nInserted" : 3,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
})
```

## Read

```java

// 전체 데이터를 읽겠다
db.users.find(

)

//특수 연산자
&eq ==> 같음 (==)
&gt ==> 초과 (>)
&gte ==> 이상 (>=)
&in ==> 전달한 배열 요소중 하나
&lt ==> 미만 (<)
&lte ==> 이하 (<=)
&ne ==> 같지 않음 (!=)
&nin ==> 전달한 배열 요소중에 없거나 필드가 존재하지 않을 때 조회
```

## Update

```java
db.users.updateMany(

)
```

## Delete

```java
db.users.deleteMany(

)
```

## 예제 실습

```java
db.test.insert({name: "홍길동"})
WriteResult({ "nInserted" : 1 })
db.test.find()
{ "_id" : ObjectId("626f2a4dfa39e44867b7c959"), "name" : "홍길동" }

db.test.insert({name:"김만복",age:26})
WriteResult({ "nInserted" : 1 })

몽고디비는 **schema less** 하므로 동일한 **collection** 내에 있더라도 doucument level의 다른 스키마를 가질 수 있따

//전체검색
db.test.find()

{ "_id" : ObjectId("626f2a4dfa39e44867b7c959"), "name" : "홍길동" }
{ "_id" : ObjectId("626f2aa8fa39e44867b7c95a"), "name" : "김만복", "age" : 26 }

//전체검색
db.test.find().pretty()

{ "_id" : ObjectId("626f2a4dfa39e44867b7c959"), "name" : "홍길동" }
{ "_id" : ObjectId("626f2aa8fa39e44867b7c95a"), "name" : "김만복", "age" : 26 }

//검색 -> 내가 보고자하는 컬럼만 보기 가능 
db.books.find({},{_id:0, title:1})

{ "title" : "datastructure" }
{ "title" : "HTML" }
{ "title" : "CSS3" }
{ "title" : "C++" }

//0 -> 확인하지않겠다
//1 -> 확인하겠다

db.books.find({},{_id: 0,title:1,author:1})
{ "title" : "datastructure", "author" : "james" }
{ "title" : "HTML", "author" : "mary" }
{ "title" : "CSS3", "author" : "Celin" }
{ "title" : "C++", "author" : "smith" }

//gt -> 초과, lte -> 이하
db.books.find({price:{$gt:200, $lte:300}})
{ "_id" : ObjectId("626f2944fa39e44867b7c958"), "title" : "C++", "author" : "smith", "price" : 300 }

// sql LIke 연산문과 비슷함
db.books.find({title:/^C/})
{ "_id" : ObjectId("626f2944fa39e44867b7c957"), "title" : "CSS3", "author" : "Celin", "price" : 200 }
{ "_id" : ObjectId("626f2944fa39e44867b7c958"), "title" : "C++", "author" : "smith", "price" : 300 }
```

```java
&eq ==> 같음 (==)
&gt ==> 초과 (>)
&gte ==> 이상 (>=)
&in ==> 전달한 배열 요소중 하나
&lt ==> 미만 (<)
&lte ==> 이하 (<=)
&ne ==> 같지 않음 (!=)
&nin ==> 전달한 배열 요소중에 없거나 필드가 존재하지 않을 때 조회
```

## MongoDB 연결

---

## Find

**기본연결** 로직 

```java
//값 한개
import java.util.Iterator;

import com.mongodb.MongoClient;
import com.mongodb.client.FindIterable;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;

public class Find {
	public static void main(String[] args) {
		try {
			// mogodb에 접속
			MongoClient mongoClient = new MongoClient("localhost", 27017);
			// 데이터베이스 연결 
			MongoDatabase mongoDatabase = mongoClient.getDatabase("mydb");
			// 컬렉션 연결
			MongoCollection collection = mongoDatabase.getCollection("books");			
			// 다큐멘트 조회
			FindIterable iterDoc = collection.find();
			int i=1;
			Iterator it = iterDoc.iterator();
			while(it.hasNext()) {
				System.out.println(it.next());
				i++;
			}
		}catch(Exception e) {
			System.out.println(e.getClass().getName() + e.getMessage());
			
		}
	}

}
```

## Insert

```java
import java.util.Iterator;

import org.bson.Document;

import com.mongodb.MongoClient;
import com.mongodb.client.FindIterable;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;

public class Insert {
	public static void main(String[] args) {
		
		try {
			MongoClient mongoClient = new MongoClient("localhost", 27017);
			MongoDatabase mongoDatabase = mongoClient.getDatabase("mydb");
			MongoCollection collection = mongoDatabase.getCollection("test");
			//basicDbObject 클래스 이용하여 삽입
			Document docu = new Document("title", "MongoDB")
					.append("author","kim" )
					.append("price", 20);
			collection.insertOne(docu);

//collection find 결과값 출력
			FindIterable iterDoc = collection.find();
			int i=1;
			Iterator it = iterDoc.iterator();
			while(it.hasNext()) {
				System.out.println(it.next());
				i++;
			}
		} catch (Exception e) {
			System.out.println(e.getClass().getName() + e.getMessage());
		}
	}

}
```

```java
//값 여러개 
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

import org.bson.Document;

import com.mongodb.MongoClient;
import com.mongodb.client.FindIterable;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;

public class Insert {
	public static void main(String[] args) {
		
		try {
			MongoClient mongoClient = new MongoClient("localhost", 27017);
			MongoDatabase mongoDatabase = mongoClient.getDatabase("mydb");
			MongoCollection collection = mongoDatabase.getCollection("test");
			//basicDbObject 클래스 이용하여 삽입
			Document docu1 = new Document("title", "javascript")
					.append("author","park" )
					.append("price", 300);
			Document docu2 = new Document("title", "HTML5")
					.append("author","sung" )
					.append("price", 400);
			
			// 리스트 하나 씩 만들어주기
			
			List<Document> list = new ArrayList<>();
			
			list.add(docu1);
			list.add(docu2);
			// 굳이 배열 돌 필요없다
//			for(Document x : list) {
//				collection.insertOne(x);
//			}
			
//			listMany를 통해서 가능
			collection.insertMany(list);
			
			FindIterable iterDoc = collection.find();
			int i=1;
			Iterator it = iterDoc.iterator();
			while(it.hasNext()) {
				System.out.println(it.next());
				i++;
			}
		} catch (Exception e) {
			System.out.println(e.getClass().getName() + e.getMessage());
		}
	}

}
```

## Update

```java
import java.util.Iterator;

import org.bson.Document;

import com.mongodb.MongoClient;
import com.mongodb.client.FindIterable;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.model.Filters;
import com.mongodb.client.model.Updates;

public class Update {
	public static void main(String[] args) {
		try {
			MongoClient mongoClient = new MongoClient("localhost", 27017);
			MongoDatabase mongoDatabase = mongoClient.getDatabase("mydb");
			MongoCollection mongoCollection = mongoDatabase.getCollection("test");
			mongoCollection.updateOne(Filters.eq("title","javascript"), Updates.set("author","song"));
			
			FindIterable iterDoc = mongoCollection.find();
			int i=1;
			Iterator it = iterDoc.iterator();
			while(it.hasNext()) {
				System.out.println(it.next());
				i++;
			}
			
			
		} catch (Exception e) {
			System.out.println(e.getClass().getName() + e.getMessage());
		}
	}

}
```

## Delete

```java
import java.util.Iterator;

import org.bson.Document;

import com.mongodb.MongoClient;
import com.mongodb.client.FindIterable;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;

public class delete {
	public static void main(String[] args) {
		
		
		try {
			MongoClient mongoClient = new MongoClient("localhost", 27017);
			MongoDatabase mongoDatabase = mongoClient.getDatabase("mydb");
			MongoCollection collection = mongoDatabase.getCollection("test");
			Document query = new Document("title", "javascript");
			collection.deleteOne(query);
			
			FindIterable iterDoc = collection.find();
			int i=1;
			Iterator it = iterDoc.iterator();
			while(it.hasNext()) {
				System.out.println(it.next());
				i++;
			}
			
			
		} catch (Exception e) {
			System.out.println(e.getClass().getName() + e.getMessage());
		}
		
		
	}

}
```
