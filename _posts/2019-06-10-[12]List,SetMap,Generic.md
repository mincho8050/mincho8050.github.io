---
title:  "[12]List,SetMap,Generic"
date:   2019-06-10
categories: Java
tags: 
- Java
author: "mincho8050"
---

# List 계열



> **Java Collection Freamework**
>
> - 자료를 모아서 저장할 수 있는 클래스들
> - E : Element , 요소
> - List : 순서(index)가 있다. 0부터 ~
>   - List , Vector , ArrayList , Collection
> - Set : 순서가 없다.
>   - Set , HashSet
> - Map : 순서가 없다.
>   - Map , HashMap , HashTable



- 주로 쓰이는 Vector()

```java
//		List list=new Vector();		//다형성
									//자식이 부모집에 들어감.

Vector vec=new Vector();
vec.add(new Object()); 		//가장 기본형
vec.add(new Integer(3));
vec.add(new Double(1.2));
vec.add(new String("HAPPY"));	//계속 vec에 자료가 쌓여가고 있음. 
                                //자료를 한번에 넘길 때 좋음

vec.add(5);						//autoboxing > jdk버전 영향을 받음
                                //기본형을 클래스가 받으면 object형으로 변함.
                                //Integer a=5; 이렇게 해도 참조형으로 변함
                                //원래는 Integer a=new Integer(5); 이렇게 해야함
vec.add(6.7);								
vec.add("Apple");	    
```

- 요소의 갯수

```java
System.out.println(vec.size());		//출력값 : 7
//element , 요소의 갯수를 출력 > 7
```

- 3번째 요소 가져오기 (0부터 시작)

```java
Object obj=vec.get(3);		//원형
System.out.println(obj);	//출력값 : "HAPPY"

String str=(String) obj;	//다형성
                            //부모도 자식에 들어갈 수 있지만 제약조건이 있음.
System.out.println(str);	//"HAPPY"
```

- 5번째 요소 가져오기

```java
Double doub=(Double) vec.get(5);	//가져오면서 동시에 형변환
System.out.println(doub);			//출력값 : 6.7
```

- 전체요소 출력

```java
for(int idx=0;idx<vec.size();idx++){
        System.out.println(vec.get(idx));
}
		/*
		 * 출력값
		 *  java.lang.Object@15db9742
			3
			1.2
			HAPPY
			5
			6.7
			Apple
		 */
```

- 0번째 요소 제거

```java
vec.remove(0);
		for(int idx=0;idx<vec.size();idx++){
        System.out.println(vec.get(idx));
}
		/*
		 * 출력값
		 * 	3
			1.2
			HAPPY
			5
			6.7
			Apple
		 */
```

- 모든 요소 전부 삭제

```java
vec.removeAllElements();
		System.out.println(vec.size());		//출력값 : 0
		
		
		if(vec.isEmpty()){ //비어있는지 묻는것. boolean형
			System.out.println("요소없다");
		}else{
			System.out.println("요소있다.");
		}//if
		//출력값 : 요소없다	
```

- remove()를 이용해서 요소 전부 삭제
- 아래에서 위로 삭제하는 방향으로 해야함

```java
for(int idx=vec.size()-1;idx>=0;idx--){
			//요소 길이는 7지만 맨 마지막은 6이기 때문에 -1 해준다.
			vec.remove(idx);
}
    System.out.println(vec.size());
```

- 다형성을 이용한 ArrayList()

```java
List list=new ArrayList();
		list.add(3);
		list.add(1.2);
		list.add("SOLDESK");
		list.add(new Integer(5));

		for(int idx=0;idx<list.size();idx++){
			System.out.println(list.get(idx));
}
		/*
		 * 출력값
		 * 	3
			1.2
			SOLDESK
			5
		 */
```









------











# Set계열

- 순서 없음.



```java
Set set=new HashSet();
set.add(3);
set.add(1.2);
set.add("HAPPY");
set.add(new Integer(5));

System.out.println(set.size()); 	// 요소의 사이즈 > 4
```

- cursor가 가리키는 요소를 가져올 수 있음 (C언어는 pointer)
- 가리켰는데 있으면 true , 없으면 false
- next는 그 다음으로 넘기는 거
- previous는 이전.



- Interface Iterator
  - cursor를 이용해서 요소를 접근하는 경우

```java
Iterator iter=set.iterator();
		while(iter.hasNext()){//.hasNext() cursor유무 판단
			//cursor가 가리키는 요소 가져오기
			Object obj=iter.next();
			System.out.println(obj);
        }

		/*
		 * 출력값
		 *  1.2
			3
			HAPPY
			5
			
			값이 없으면 빠져나옴
			순서 없음.
		 */
```









------









# Map 계열

- 순서 없음.
- Key : 이름 , 중복 불가능
- Value : 값 , 중복 가능
- 값을 이름을 붙여 모아놓을 수 있음



```java
Map map=new HashMap();
		map.put("one", "손흥민"); //key , value
		map.put("two", 3);
		map.put("three", new Double(5.6));
//		map.put("one", "박지성");		//이건 허용 안된것.
									//key는 중복허용 안된다.
		map.put("four", "손흥민"); //허용 . value는 중복가능

		System.out.println(map.size()); 	// 4
		System.out.println(map.get("one"));	//손흥민
		System.out.println(map.get("three"));	//5.6
```



> key 값으로 "read.do" 호출하면 value 값으로 "net.bbs.Read" 출력하기.
>
> - 문자 기준으로 문자열 분리해서 앞의 문자열은 key , 뒤는 value로 map에 저장



- set에 저장되어있다.

```java
HashSet command=new HashSet();
    command.add("list.do=net.bbs.List");
    command.add("read.do=net.bbs.Read");
    command.add("write.do=net.bbs.Write");
```

- map으로 옮겨야한다. 단 나눠서!

```java
HashMap map=new HashMap();
		
		
    Iterator iter=command.iterator();
    while(iter.hasNext()){
        String str=(String)iter.next();
        //cursor가 가리키는 요소를 String값에 저장하기.
        
        int pos=str.indexOf("=");
        //"="의 index값 추출
        
        String key=str.substring(0,pos);
        String value=str.substring(pos+1);
        //spilt나 StringTokenizer 사용가능.
        //다만 내용이 좀 바뀔뿐
        
        map.put(key, value);
        //값을 map에 저장
    }//while
		
		
    System.out.println(map.get("read.do")); //net.bbs.Read
                                            //read.do 는 key 값
    System.out.println(map.get("list.do")); //net.bbs.List
    System.out.println(map.get("write.do")); //net.bbs.Write
```







------







# Generic 

- 데이터를 수집하는 경우 자료형을 제한할 수 있다.
- < E > Element : <참조자료형>
- < T >
- < ? >



- Vector();

```java
    Vector<String> vec=new Vector<String>(); //참조자료형(클래스)만 모으겠다는 표시.
    vec.add("손흥민");
//		vec.add(3); 에러
    vec.add(new String("박지성"));
//		vec.add(new Integer(5)); 에러
    vec.add("김연아");
    vec.add("이강인");

    for(int idx=0;idx<vec.size();idx++){
        String str=vec.get(idx);
        System.out.println(str);
    }
		
		/*출력값
		 * 	손흥민
			박지성
			김연아
			이강인
		 */
```

- ArrayList();

```java
ArrayList<Integer> list=new ArrayList<Integer>();
        list.add(3);
        list.add(new Integer(5));
//		list.add(""); 문자열 에러
//		list.add(' '); char형 에러
//		list.add(new Double(2.4)); 에러
```

- HashSet();

```java
HashSet<String> set=new HashSet<String>();
set.add("개나리");
set.add("진달래");
set.add("라일락");
```

- HashMap();

```java
HashMap<String, Integer> map=new HashMap<String, Integer>();
        map.put("무궁화", 3);	//아직까지 한글은 key값이 아니라 value값에 쓰자.
        map.put("", new Integer(5));
//		map.put("", "홍길동"); 에러
```



```java
class Mountain{
	String name;
	int height;
	public Mountain(){};
	public Mountain(String name,int height){
		this.name=name;
		this.height=height;
	};
	
}//mountain
```

```java
public static void main(String[] args) {
    
		Mountain one=new Mountain("한라산", 1950);
		Mountain two=new Mountain("관악산", 1500);
		Mountain three=new Mountain("북한산", 1000);
		
    ArrayList<Mountain> list=new ArrayList<Mountain>();
		list02.add(one);
		list02.add(two);
		list02.add(three);
//		list02.add("솔데스크"); 에러
		
		for(int idx=0;idx<list.size();idx++){
			Mountain dto=list.get(idx);
			System.out.println(dto.name+" "+dto.height);
		}//for
	
		/*출력값
		 *  한라산 1950
			관악산 1500
			북한산 1000
		 */
}    
```
