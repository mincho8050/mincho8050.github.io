---
title:  "[34]Kotlin Basic"
date:   2019-08-14
categories: 
- Kotlin
tags: 
- Kotlin
author: "mincho8050"
---

# 0) Kotlin

코틀린은 인텔리제이를 만든 젯브레인이 만든 언어이다. 구글에서도 코틀린을 자바에 이어 안드로이드 공식 언어로 선언했다. 

코틀린을 사용하면 안드로이드 버전에 관계 없이 현대 언어의 장점을 사용할 수 있다. 안드로이드 개발에서 코틀린을 사용하면 다음과 같은 이점이 있다.

1. **호환성** : 코틀린은 JDK 6과 완벽하게 호환되어 구형 안드로이드 기기에서도 완벽하게 실행된다. 그리고 코틀린 개발 도구는 안드로이드 스튜디오에서 완벽히 지원되므로 안드로이드 빌드 시스템과도 완벽히 호환된다.
2. **성능** : 코틀린은 자바만큼 빠르거나 종종 람다로 실행되는 코드는 종종 자바보다 훨씬 바르게 작동한다.
3. **상호 운용성** : 코틀린은 자바와 100% 상호 운용이 가능하여 기존의 모든 안드로이드 라이브러리를 사용할 수 있다. 
4. **학습 곡선** : 자바 개발자가 코틀린을 배우기 쉽다. 안드로이드 스튜디오는 자바 코드를 코틀린으로 자동으로 변환해주는 도구를 제공한다





------





# 1) Kotlin 기본구문

가장 자주 등장하는 변수 선언 및 사용법, 함수의 생김새.



## 1-1) 변수와 상수

코틀린에서 변수는 var로 상수는 val로 선언.

```kotlin
var a:Int=10	//var 변수명:자료형=값
val b:Int=20	//val 상수명:자료형=값
```

코틀린은 자료형을 지정하지 않아도 추론하는 형추론을 지원해 자료형 생략 가능

```kotlin
var a=10
val b=20
```

변수는 값을 변경할 수 있고, 상수는 값을 변경할 수 없다. val은 자바에서의 final 키워드에 대응





## 1-2) 함수

함수(Function)는 일정 동작을 수행하는 특정 형식의 코드 뭉치. 함수는 자바의 메서드에 해당

함수를 선언하는 방법은 다음과 같다

```
fun 함수명(인수1:자료형, 인수2:자료형):반환자료형
```

다음은 하나의 문자열을 인수로 받고 println() 메서드로 출력하는 greet() 함수를 작성하고 사용하는 예시이다. 

코틀린에서는 반환값이 없을 때 Unit형을 사용한다. Unit은 자바의 void에 대응

```kotlin
fun greet(str:String):Unit{
    println(str)
}
greet("안녕")
```

반환값이 Unit일 경우는 생략가능

```kotlin
fun greet(str:String){
    println(str)
}
```

**메서드 VS 함수**

```
메서드와 함수는 혼동되는 용어이다. 메서드는 객체와 관련된 함수이다. 함수는 좀 더 일반적인 용어이며 모든 메서드는 함수이다.
```







------







# 2) Kotlin 기본 자료형

코틀린의 기본 자료형은 모두 객체이다. 자바가 프리미티브 자료형과 객체 자료형으로 분류되는 것과 다르다.





## 2-1) 숫자형

코틀린에서 숫자를 표현하는 자료형은 다음과 같다.

- **Double** : 64비트 부동소수점
- **Float** : 32비트 부동소수점
- **Long** : 64비트 정수
- **Int** : 32비트 정수
- **Short** : 16비트 정수
- **Byte** : 8비트 정수

리터럴이란 자료형을 알 수 있는 표기 형식을 말한다. 리터럴에 따라 코틀린 컴파일러는 자료형을 추론한다.

```kotlin
val a=10	//Int
val b=10L	//Long
val c=10.0	//Double
val d=10.0f	//Float
```





## 2-2) 문자형

코틀린에서 문자를 나타내는 자료형은 두 가지이다. Char가 숫자형이 아니라는 점이 자바와 다르다.

- **String** : 문자열
- **Char** : 하나의 문자

문자열의 리터럴은 큰따옴표 \"", 한 문자는 작은따옴표 \''로 자바와 같다



### 2-2-1) 여러 줄의 문자열 표현

여러 줄에 걸쳐 문자를 표현할 때는 큰따옴표 3개를 리터럴로 사용한다. 여러 줄에 걸친 문자열을 작성할 때 편리하다.

```kotlin
val str="""오늘은
		날씨가 좋습니다.
		빨래를 합시다.
"""
```



### 2-2-2) 문자열 비교

문자열 비교는 ==을 사용한다. 자바의 equals() 메서드와 대응

```kotlin
val str="hello"
if (str=="hello"){
    println("안녕하세요")
} else {
    println("안녕 못해요")
}
```

자바에서는 ==는 오브젝트 비교 시에 사용하는데 코틀린에서 오브젝트 비교 시에는 ===를 사용한다.



### 2-2-3) 문자열 템플릿

자바와 같이 + 기호로 문자열을 연결할 수 있다.

$기호를 사용하면 문자열 리터럴 내부에 변수를 쉽게 포함할 수 있다. 변수와 글자를 붙여야 할 때는 중괄호\{\}로 변수명을 감싸면 된다.

```kotlin
val str="안녕"
println(str+"하세요")	//자바 > 안녕하세요

//코틀린
println("$str 하세요")	//안녕 하세요
println("${str}하세요")//안녕하세요
```







## 2-3) 배열

배열은 Array라는 별도의 타입으로 표현한다. arrayOf() 메서드를 사용하여 배열의 생성과 초기화를 함께 수행한다. 컴파일러가 자료형을 유추할 수 있을때는 이를 생략한다. 배열의 요소에 접근하는것은 대괄호\[] 안에 요소 번호를 지정하는 것으로 자바와 같다.

```kotlin
val numbers: Array<Int>=arrayOf(1,2,3,4,5)

val number2=arrayOf(1,2,3,4,5)	//자료형 생략가능

numbers[0]=5	//index는 0부터 시작
```







------







# 3) 제어문

제어문은 크게 if, when, for, while의 4가지로 나뉜다. when문을 제외하고는 자바와 거의 같다.





## 3-1) if

실행할 문장이 한 줄이면 블록을 생략할 수 있다.

```kotlin
val a=10
val b=20

//일반적인 방법
var max=a
if(a<b)max=b
```

if-else문의 사용법도 자바와 거의 같다.

```kotlin
if(a>b){
    max=a
}else{
    max=b
}
```

다음과 같이 if문을 식처럼 사용할 수 있다.

```kotlin
val max=if(a>b) a else b
```





## 3-2) when

when문은 자바의 switch문에 대응한다. when문을 사용하는 다양한 방법은 다음과 같다. 값이 하나인 경우는 물론 콤마나 in 연산자로 값의 범위를 자유롭게 지정하는 것이 특징이다. 그 밖의 경우에는 else를 사용하여 나머지에 대한 경우를 처리. 코드를 작성할 때 블록으로 감쌀 수 있다.

```kotlin
val x=1

when(x){
    1 -> println("x==1")						//값 하나
    2,3 -> println("x==2 or x==3")				//여러 값은 콤마로
    in 4..7 -> println("4부터 7사이")			 //in 연산자로 범위 지정
    !in 8..10 -> println("8부터 10 사이가 아님")
    else -> {									//나머지
        print("x는 1이나 2가 아님")
    }
}
```

when문 역시 if문과 마찬가지로 식처럼 사용할 수 있다.

```kotlin
val numberStr=1

val numStr=when(numberStr%2){
    0 -> "짝"
    else -> "홀수"
}
```

when문의 결과를 함수의 반환값으로 사용할 수 있다.

```kotlin
val number=1

fun isEven(num:Int)=when(num%2){
    0 -> "짝"
    else -> "홀"
}
println(isEven(number))
```





## 3-3) for

in 키워드를 사용하여 모든 요소를 num 변수로 가져온다

```kotlin
val numbers=arrayOf(1,2,3,4,5)

for(num in numbers){
    print(num)	//12345
}
```

그 밖에도 다음과 같이 다양한 사용 방법이 있다. 증가 범위는 .. 연산자를 사용. 감소 범위는 downTo 키워드를 사용하며 step 키워드로 증감의 간격을 조절할 수 있다.

```kotlin
//1~3까지 출력
for(i in 1..3){
    println(i)	//1,2,3
}
//0~10까지 2씩 증가하며 출력
for(i in 0..10 step 2){
    println(i)	//0,2,4,6,8,10
}
//10부터 0까지 2씩 감소하며 출력
for(i in 10 downTo 0 step 2){
    println(i)	//10,8,6,4,2,0
}
```





## 3-4) while

while문은 주어진 조건이 참일 때 반복하는 문법이다. while문의 변형으로는 무조건 한 번은 실행되는 do while문이 있다. 자바와 완전히 동일하다.

```kotlin
//while
var x=10
println(x)
while(x>0){
    x--
    println(x)//10,9,8,7,6,5,4,3,2,1,0
}

//do while
var x=10
do{
    x--
    println(x)	//9,8,7,6,5,4,3,2,1,0
} while(x>0)
```







------





# 4) 클래스

클래스는 붕어빵 틀에 비유할 수 있고 인스턴스는 클래스로 생성한 객체의 실체인 붕어빵에 비유할 수 있다. 코틀린에서의 클래스는 자바와 역할은 유사하지만 더 간결하다.





## 4-1) 클래스 선언

다음은 클래스를 선언하고, 생성한 클래스로 인스턴스를 생성하는 방법을 나타낸다.

자바에서는 new 키워드로 객체를 생성하지만 코틀린에서는 new 키워드를 사용하지 않는다

```kotlin
//클래스 선언
class Person{
    
}
//인스턴스 생성
val person=Person()
```



## 4-2) 생성자

생성자를 가지는 클래스를 다음과 같이 표현할 수 있다. 이 코드는 name 파라미터(매개변수)를 받는 생성자를 가지는 클래스다.

```kotlin
class Person(var name:String){
}
```

생성자에서 초기화 코드를 작성하려면 다음과 같이 constructor로 생성자를 표현하고 블록에 코드를 작성한다. 이 생성자는 name을 출력한다.

```kotlin
class Person{
    constructor(name:String){
        println(name)
    }
}
```

코틀린에서는 생성자 이외에도 init 블록에 작성한 코드가 클래스를 인스턴스화할 때 가장 먼저 초기화 된다. 즉 위 코드는 아래 코드처럼 작성할 수 있다.

```kotlin
class Person(name:String){
    init{
        println(name)
    }
}
```





## 4-3) 프로퍼티

클래스의 속성을 사용할 때는 멤버에 직접 접근하며 이를 프로퍼티라고 한다. 다음 코드에서 Person 클래스는 name프로퍼티를 가지고 있다. 프로퍼티에 값을 쓰려면 = 기호로 값을 대입한다. 값을 읽을 때는 프로퍼티를 참조한다.

코틀린에서는 다음과 같이 사용한다. 속성에 값을 설정하거나 얻으려면 게터(getter)/세터(setter) 메서드 없이 바로 점을 찍고 name 프로퍼티에 접근할 수 있다.

> 코틀린에서의 클래스

```kotlin
//클래스 선언
class Person(var name:String){
    
}

//인스턴스 생성
val person=Person("멋쟁이")
person.name="키다리"	//쓰기
println(person.name)  //읽기
```

자바로 작성된 클래스의 게터/세터 메서드는 코틀린에서 사용할 때 기존의 게터/세터를 사용할 수도 있고 프로퍼티로 사용할 수도 있다.

자바에서는 private 접근지정자로 은닉화된 멤버 변수에 게터/세터 메서드를 사용해서 접근하는 방식이 일반적이다. 코틀린은 프로퍼티가 게터/세터를 대체한다. 자바의 경우 다음 코드가 일반적이다.

> 자바에서의 클래스

```java
class Person{
    private String name;
    
    public Person(String name){
        this.name=name;
    }
    
    public String getName(){
        return name;
    }
    
    public void setName(String name){
        this.name=name;
    }
    
}
```





## 4-4) 접근 제한자

접근 제한자란 변수나 함수를 공개하는 데 사용하는 키워드다.

- **public** : (생략가능) 전체 공개이다. 아무것도 안 쓰면 기본적으로 public
- **private** : 현재 파일 내부에서만 사용할 수 있다.
- **internal** : 같은 모듈 내에서만 사용할 수 있다.
- **protected** : 상속받은 클래스에서 사용할 수 있다. 

```kotlin
class A{
    val a=1	//public
    private val b=2
    protected val c=3
    internal val d=4
}
```

> 안드로이드 스튜디오의 프로젝트는 app 모듈을 기본 제공해 여기서 앱을 개발한다. 같은 프로젝트에 스마트폰용, 시계용, TV용 안드로이드 앱을 만든다면 모듈 3개를 생성한다. internal은 이 모듈 간 접근을 제한하는 키워드이다.





## 4-5) 클래스의 상속

코틀린에서의 클래스는 기본적으로 상속이 금지된다. 상속이 가능하게 하려면 open 키워드를 클래스 선언 앞에 추가한다. 

Animal 클래스를 상속받는 Dog 클래스를 나타낸다.

```kotlin
open class Animal{}

class Dog:Animal(){}
```

만약 상속받을 클래스가 생성자를 가지고 있다면 다음과 같이 상속받을 수 있다.

```kotlin
open class Animal(val name:String){}

class Dog(name:String):Animal(name){}
```





## 4-6) 내부 클래스

내부 클래스 선언에는 inner를 사용한다. 내부 클래스는 외부 클래스에 대한 참조를 가지고 있다. 

아래 코드에서 inner키워드가 없다면 a를 20으로 변경할 수 없다.

```kotlin
class OuterClass{
    var a=10
    
    //내부 클래스
    inner class OuterClass2{
        fun something(){
            a=20	//접근 가능
        }
    }
}
```







## 4-7) 추상 클래스

추상 클래스는 미구현 메서드가 포함된 클래스를 말한다. 클래스와 미구현 메서드 앞에 abstract 키워드를 붙인다. 추상 클래스는 직접 인스턴스화할 수 없고 다른 클래스가 상속하여 미구현 메서드를 구현해야 한다. 기본적으로 자바와 동일한 특성.

```kotlin
abstract class A{
    abstract fun func()
    
    fun func2(){
        
    }
}

class B:A(){
    override fun func(){
        println("hello")
    }
}

val a=A()	//에러
val b=B()	//OK
```







------







# 5) 인터페이스

인터페이스는 미구현 메서드를 포함하여 클래스에서 이를 구현한다. 추상 클래스와 비슷하지만 클래스가 단일 상속만 되는 반면 인터페이스는 다중 구현이 가능하다. 주로 클래스에 동일한 속성을 부여해 같은 메서드라도 다른 행동을 할 수 있게 하는데 사용한다. 코틀린의 인터페이스는 자바와 거의 사용법이 같다.





## 5-1) 인터페이스의 선언

다음과 같이 인터페이스에 추상 메서드를 포함할 수 있다. 원래 추상 클래스에서 추상 메서드는 abstract 키워드가 필요하지만 인터페이스는 생략 가능하다.

```kotlin
interface Runnable{
    fun run()
}
```

인터페이스는 구현이 없는 메서드뿐만 아니라 구현된 메서드를 포함할 수 있다. 이는 자바 8의 default 메서드에 대응한다.

```kotlin
interface Runnable{
    fun run()
    
    fun fastRun()=println("빨리 달린다")
}
```





## 5-2) 인터페이스의 구현

인터페이스를 구현할 때는 인터페이스 이름을 콜론: 뒤에 나열한다. 그리고 미구현 메서드를 작성하는데 이때 override 키워드를 메서드 앞에 추가한다. run 함수를 오버라이드 한다고 말한다.

```kotlin
class Human : Runnable{
    override fun run(){
        println("달린다")
    }
}
```





## 5-3) 상속과 인터페이스를 함께 구현

상속과 인터페이스를 함께 구현할 수 있다. 상속은 하나의 클래스만 상속하는 반면 인터페이스는 콤마로 구분하여 여러 인터페이스를 동시에 구현할 수 있다.

```kotlin
open class Animal{}

interface Runnable{
    fun run()
    
    fun fastRun()=println("빨리 달린다.")
}

interface Eatable{
    fun eat()
}

class Dog: Animal(), Runnable, Eatable{
    override fun eat(){
        println("먹는다.")
    }
    
    override fun run(){
        println("달린다")
    }
}
```





------





# 6) null 가능성

기본적으로 객체를 불변으로 보고 null값을 허용하지 않는다. null값을 허용하려면 별도의 연산자가 필요하고 null을 허용한 자료형을 사용할 때도 별도의 연산자들을 사용하여 안전하게 호출해야 한다.





## 6-1) null 허용?

코틀린에서는 기본적으로 null값을 허용하지 않는다. 따라서 모든 객체는 생성과 동시에 값을 대입하여 초기화 해야 한다.

다음 코드는 초기화를 하지 않아 에러가 발생한다.

```kotlin
val a:String	//에러: 초기화를 반드시 해야함!!
```

다음 코드는 null값으로 초기화해서 에러 발생

```kotlin
val a:String=null	//에러: 코틀린은 기본적으로 null을 허용하지 않음
```

코틀린에서 null값을 허용하려면 자료형의 오른쪽에 ? 기호를 붙여준다.

```kotlin
val a:String?=null	//OK
```

> 자바에서는 int, long, double과 같은 프리미티브 자료형은 null값을 허용하지 않지만, 그 외 모든 클래스형 변수는 null값을 허용한다!!





## 6-2) lateinit 키워드로 늦은 초기화

안드로이드를 개발할 때는 초기화를 나중에 할 경우가 있다. 이때는 lateinit 키워드를 변수 선언 앞에 추가하면 된다. 안드로이드에서는 특정 타이밍에 객체를 초기화할 때 사용한다. 초기화를 잊는다면 잘못된 null값을 참조하여 앱이 종료될 수 있으니 주의한다.

```kotlin
lateinit var a:String	//OK

a="hello"
println(a)
```

lateinit은 다음 조건에서만 사용할 수 있다.

- var 변수에서만 사용 가능
- null값으로 초기화할 수 없다.
- 초기화 전에는 변수를 사용할 수 없다.
- Int, Long, Double, Float에서는 사용할 수 없다.





## 6-3) lazy로 늦은 초기화

lateinit이 var로 선언한 변수의 늦은 초기화라면 lazy는 값을 변경할 수 없는 val을 사용할 수 있다. val 선언 뒤에 by lazy 블록에 초기화에 필요한 코드를 작성한다. 마지막 줄에는 초기화할 값을 작성한다.

str이 처음 호출될 때 초기화 블록의 코드가 실행된다. println() 메서드로 두 번 호출하면 처음에만 "초기화"가 출력된다.

```kotlin
val str:String by lazy{
    println("초기화")
    "hello"
}

println(str)		//초기화, hello
println(str)		//hello
```

lazy로 늦은 초기화를 하면 앱이 시작될 때 연산을 분산시킬 수 있어 빠른 실행에 도움이 된다.

lazy는 다음 조건에서만 사용 가능하다.

- val에서만 사용 가능하다.

조건이 적기 때문에 상대적으로 lateinit보다 편하게 사용할 수 있다.





## 6-4) null값이 아님을 보증(!!)

변수 뒤에 !!를 추가하면 null값이 아님을 보증하게 된다. 

다음과 같이 null값이 허용되는 name 변수의 경우 String? 타입이기 때문에 String 타입으로 변환하려면 !!를 붙여서 null값이 아님을 보증해야 한다.

```kotlin
val name:String?="키다리"

val name2:String=name	//에러
val name3:String?=name	//OK

val name4:String=name!!	//OK
```





## 6-5) 안전한 호출(?.)

메서드 호출 시 점 연산자 대신 ?. 연산자를 사용하면 null값이 아닌 경우에만 호출된다. 

다음 코드는 str 변수의 값이 null 값이 아니라면 대문자로 변경하고, null값이라면 null을 반환한다.

```kotlin
val str:String?=null

var upperCase=if(str!=null) str else null
upperCase=str?.toUpperCase()
```





## 6-6) 엘비스 연산자(?:)

안전한 호출 시 null이 아닌 기본값을 반환하고 싶을 때는 엘비스 연산자를 함께 사용한다.

마지막 코드는 이제 null이 아닌 "초기화하시오"라는 문자열을 반환한다.

```kotlin
val str:String?=null

var upperCase=if(str!=null) str else null	//null
upperCase=str?.toUpperCase()?:"초기화하시오"	//초기화하시오
```







------





# 7) 컬렉션

개발에 유용한 자료구조이다. 안드로이드 개발에서도 리스트나 맵은 자주 사용되는 자료구조이다.







## 7-1) 리스트

배열처럼 같은 자료형의 데이터들을 순서대로 가지고 있는 자료구조이다. 중복된 아이템을 가질 수 있고 추가, 삭제, 교체 등이 쉽다.

요소를 변경할 수 없는 읽기 전용 리스트는 listOf() 메서드로 작성

```kotlin
val foods:List<String>=listOf("라면","갈비","밥")
```

형추론으로 자료형 생략 가능

```kotlin
val foods=listOf("라면","갈비","밥")
```

요소를 변경하는 리스트는 mutableListOf() 메서드를 사용한다. 자바와 다른 점은 특정 요소에 접근할 때 대괄호 안에 요소 번호로 접근 가능

```kotlin
val foods=mutableListOf("라면","갈비","밥")

foods.add("초밥")			//초밥을 맨뒤에 추가
foods.removeAt(0)		 //맨 앞에 아이템 삭제
foods[1]="부대찌개"		  //foods.set(1,"부대찌개") > 1번째 아이템을 부대찌개로 변경

println(foods)			//[갈비, 부대찌개, 초밥]
println(foods[0])		//갈비
println(foods[1])		//부대찌개
println(foods[2])		//초밥
```







## 7-2) 맵

맵은 키(key)와 값(value)의 쌍으로 이루어진 키가 중복될 수 없는 자료구조이다. 리스트와 마찬가지로 mapOf() 메서드로 읽기 전용 맵을 만들 수 있고, mutableMapOf() 메서드로 수정이 가능한 맵을 만들 수 있다. 맵의 요소에 접근할 때는 대괄호 안에 키를 요소명으로 작성하여 접근한다.

```kotlin
//읽기 전용 맵
val map=mapOf("a" to 1, "b" to 2, "c" to 3)

//변경 가능한 맵
val citiesMap=mutableMapOf("한국" to "서울",
                          "일본" to "동경",
                          "중국" to "북경")

//요소에 덮어쓰기
citiesMap["한국"]="서울특별시"
//추가
citiesMap["미국"]="워싱턴"
```

맵 전체의 키와 값을 탐색할 때는 다음과 같이 간단히 탐색할 수 있다.

```kotlin
//맵의 키와 값을 탐색
for((k,v) in citiesMap){
    println("$k -> $v")
}
/*
한국 -> 서울특별시
일본 -> 동경
중국 -> 북경
미국 -> 워싱턴
*/
```





## 7-3) 집합

집합(set)은 중복되지 않는 요소들로 구성된 자료구조이다. setOf() 메서드로 읽기 전용 집합을, mutableSetOf() 메서드로 수정 가능한 집합을 생성한다.

```kotlin
//읽기 전용 집합
val citySet=setOf("서울", "수원", "부산")

//수정 가능한 집합
val citySet2=mutableSetOf("서울", "수원", "부산")
citySet2.add("안양")		//[서울, 수원, 부산, 안양]
citySet2.remove("수원")	//[서울, 부산, 안양]

//집합의 크기
println(citySet2.size)				//3
//"서울"이 집합에 포함되어 있는지
println(citySet2.contains("서울"))	//true
```







------







# 8) 람다식

코틀린은 자바 8과 같이 람다식을 지원한다. 람다식은 하나의 함수를 표현하는 방법으로 익명 클래스나 익명 함수를 간결하게 표현할 수 있어서 매우 유용하다. 

람다식은 코드를 간결하게 해주는 장점이 있지만 디버깅이 어렵고 남발할 경우 오히려 코드 가독성이 떨어져 주의하여 사용해야 한다.

먼저 두 수를 인수로 받아서 더해주는 add() 메서드이다.

```kotlin
fun add(x:Int, y:Int):Int{
    return x+y
}
```

이 코드를 다음과 같이 표현할 수 있다. 반환 자료형을 생략하고 블록\{\}과 retrun을 생략할 수 있다.

```kotlin
fun add(x:Int, y:Int)=x+y
```

또한 다음과 같이 표현할 수 있다. 코틀린의 람다식은 다음과 같이 항상 줄괄호로 둘러 싸여 있다. 내용으로는 인수 목록을 나열하고 -> 이후에 본문이 위치한다. 람다식을 변수에 저장할 수 있고 이러한 변수는 일반 함수처럼 사용할 수 있다.

```kotlin
//{인수1:타입1, 인수2:타입2 -> 본문}
var add={x:Int, y:Int -> x+y}

println(add(2,5))	//7
```







## 8-1) SAM 변환

코틀린에서는 추상 메서드 하나를 인수로 사용할 때는 함수를 인수로 전달하면 편하다. 자바로 작성된 메서드가 하나인 인터페이스를 구현할 때는 대신 함수를 작성할 수 있다. 이를 SAM(Single Abstract Method) 변환이라고 한다.

SAM 변환의 예를 안드로이드에서 든다면, 안드로이드에서는 버튼의 클릭 이벤트를 구현할 때 onClick() 추상 메서드만을 가지는 View.OnClickListener 인터페이스를 구현한다.

다음은 안드로이드에서 버튼에 클릭 이벤트 리스너를 구현하는 코드를 일반적인 익명 클래스를 작성하듯 작성한 코드다. 여기서 View.OnClickListener 인터페이스에는 onClick() 추상 메서드가 하나 있기 때문에 onClick() 메서드를 오버라이드 하고 있다.

```kotlin
button.setOnClickListener(object : View.OnClickListener){
    override fun onClick(v: View?){
        //클릭 시 이벤트 처리
    }
}
```

구현하는 인터페이스에 구현해야 할 메서드가 하나뿐일 때는 이를 람다식으로 변경할 수 있다. 

다음 코드는 람다식으로 변경되어 코드가 줄었지만 괄호도 중첩되어 있고 기호도 많고 뭔가 코드는 복잡해 보인다.

```kotlin
button.setOnClickListener({v: View? ->
    //클릭 시 이벤트 처리                       
    
})
```

메서드 호출 시 맨 뒤에 전달되는 인수가 람다식인 경우에는 람다식을 괄호 밖으로 뺄 수 있다. 위 코드는 하나의 인수만 있고 람다식이 전달되었기 때문에 마지막 인수라고 볼 수 있다.

```kotlin
button.setOnClickListener(){v: View? ->
    //클릭 시 처리
}
```

그리고 람다가 어떤 메서드의 유일한 인수인 경우에는 메서드의 괄호를 생략할 수 있다.

```kotlin
button.setOnClickListener{v: View? ->
    //클릭 시 처리
}
```

컴파일러가 자료형을 추론하는 경우에는 자료형을 생략할 수 있다.

```kotlin
button.setOnClickListener{ v ->
    //클릭 시 처리
}
```

만약 클릭 시 처리에 어떤 코드를 작성했는데 v 인수를 사용하지 않는다면 v라는 이름은 _ 기호로 대치할 수 있다. 인수가 많은 경우에 꼭 사용하는 인수 이외에는 _ 기호로 변경하여 애초에 잘못 사용하는 것을 방지할 수도 있다. 이러한 방식은 다른 함수형 언어에서도 적용되는 함수형 언어의 특징 중 하나이다.

```kotlin
button.setOnClickListener{ _ ->
    //클릭 시 처리
}
```

그리고 람다식에서 인수가 하나인 경우에는 이를 아예 생략하고 람다 블록 내에서 인수를 it로 접근할 수 있다. 다음 코드에서 it는 View? 타입의 v 인수를 가리킨다.

```kotlin
button.setOnClickListener{
    it.visibility=View.GONE
}
```

위 7가지 형태는 모두 같은 결과를 내지만 마지막 코드가 가장 읽기 쉽다. 

중요한 것은 SAM 변환은 자바에서 작성한 인터페이스일 때만 동작한다. 코틀린에서는 인터페이스 대신에 함수를 사용하는 것이 좋다.





------







# 9) 기타 기능

다음과 같은 기타 유용한 기능에 대해 간단히 살펴본다.

- **확장 함수** : 원래 있던 클래스에 기능을 추가하는 함수
- **형변환** : 숫자형 자료형끼리 쉽게 형변환 가능
- **형 체크** : 변수의 형이 무엇인지 검사하는 기능
- **고차 함수** : 인자로 함수를 전달하는 기능
- **동반 객체** : 클래스의 인스턴스 생성 없이 사용할 수 있는 객체
- **let() 함수** : 블록에 자기 자신을 전달하고 수행된 결과를 반환하는 함수
- **with() 함수** : 인자로 객체를 받고 블록에서 수행된 결과를 반환하는 함수
- **apply() 함수** : 블록에 자기 자신을 전달하고 이 객체를 반환하는 함수
- **run() 함수** : 익명함수처럼 사용하거나, 블록에 자기 자신을 전달하고 수행된 결과를 반환하는 함수





## 9-1) 확장 함수

코틀린은 확장 함수 기능을 사용하여 쉽게 기존 클래스에 함수를 추가할 수 있다. 확장 함수를 추가할 클래스에 점을 찍고 함수 이름을 작성한다. 확장 함수 내부에서는 이 객체를 this로 접근할 수 있고 이러한 객체를 리시버 객체라고 한다. 

다음은 Int 자료형에 짝수인지 아닌지를 알 수 있도록 isEven() 확장 함수를 추가한 예시다.

```kotlin
fun Int.isEven() = this%2 == 0

val a=5
val b=6

println(a.isEven())	//false
println(b.isEven())	//true
```

> 자바에서는 기본 자료형에 기능을 추가하려면 상속을 받고 추가 메서드를 작성해야 했다. String 클래스의 경우는 final로 상속이 막혀 있어 이 마저도 불가능했다.







## 9-2) 형변환

숫자형 자료형끼리는 to자료형() 메서드를 사용하여 형변환이 가능하다

```kotlin
val a=10L
val b=20

val c=a.toInt()		//Long을 Int
val d=b.toDouble()	//Int -> Double
val e=a.toString()	//Long -> String
```

숫자 형태의 문자열을 숫자로 바꿀 때는 자바와 마찬가지로 Integer.parseInt() 메서드를 사용한다.

```kotlin
val intStr="10"
val str=Integer.parseInt(intStr)
```

일반 클래스 간에 형변환을 하려면 as 키워드를 사용해야 한다.

```kotlin
open class Animal

class Dog:Animal()

val dog=Dog()

val animal=dog as Animal	//dog를 Animal형으로 변환
```







## 9-3) 형 체크

is 키워드를 사용하여 형을 체크할 수 있다. 자바의 instanceOf에 대응한다.

```kotlin
val str="hello"

if (str is String){	//str이 String형이라면
    println(str.toUpperCase())
}
```







## 9-4) 고차 함수

코틀린에서는 함수의 인수로 함수를 전달하거나 함수를 반환할 수 있다. 이렇게 다른 함수를 인수로 받거나 반환하는 함수를 고차 함수(higher-order function) 라고 한다.

add 함수는 x, y, callback 세 개의 인수를 받는다. 내용은 callback에 x와 y의 합을 전달한다. 여기서 callback은 하나의 숫자를 받고 반환이 없는 함수다. 자바에서는 주로 인터페이스를 사용하는데 코틀린은 함수를 활용하는 점이 다르다.

```kotlin
//인수:숫자, 숫자, 하나의 숫자를 인수로 하는 반환값이 없는 함수
fun add(x:Int, y:Int, callback:(sum:Int) -> Unit){
    callback(x+y)
}
//함수는 {}로 감싸고 내부에서는 반환값을 it로 접근할 수 있다.
add(5, 3, {println(it)})	//8
```





## 9-5) 동반 객체

프래그먼트는 특수한 제약 때문에 팩토리 메서드를 정의하여 인스턴스를 생성해야 한다. 팩토리 메서드는 생성자가 아닌 메서드를 사용해 객체를 생성하는 코딩 패턴을 말하는데 클래스와 별개로 보며 포함 관계도 아니다. 코틀린에서는 자바의 static과 같은 정적인 메서드를 만들 수 있는 키워드를 제공하지 않는다. 대신 동반 객체(companion object)로 이를 구현한다.

다음 코드는 newInstance() 정적 메서드를 사용해서 Fragment 객체를 생성하는 팩토리 패턴을 구현 및 사용 예시다.

```kotlin
class Fragment{
    companion object{
        fun newInstance():Fragment{
            println("생성됨")
        }
    }
}

val fragment=Fragment.newInstance()
```

여기서 동반 객체 내부의 메서드는 Fragment 클래스와 아무 관계 없는 정적인 존재이다.





## 9-5) let() 함수

코틀린 기본 라이브러리는 몇 가지 유용한 함수를 제공한다. let() 함수는 블록에 자기 자신을 인수로 전달하고 수행된 결과를 반환한다. 인수로 전달한 객체는 it으로 참조한다. let() 함수는 안전한 호출 연산자(?.)와 함께 사용하면 null값이 아닐 때만 실행하는 코드를 다음과 같이 나타낼 수 있다.

```kotlin
//fun<T,R>T.let(block: (T) -> R) : R
val result=str?.let{	//Int
    Integer.parseInt(it)
}
```

이 코드는 str이 null이 아닐 때만 정수로 변경하여 출력하는 코드다. 복잡한 if문을 대체할 수 있다.





## 9-7) with() 함수

with() 함수는 인수로 객체를 받고 블록에 리시버 객체로 전달한다. 그리고 수행된 결과를 반환한다. 리시버 객체로 전달된 객체는 this로 접근할 수 있다. 

this는 생략이 가능하므로 다음과 같이 작성할 수 있다. 안전한 호출이 불가능하여 str이 null값이 아닌 경우에만 사용한다.

```kotlin
//fun<T,R>with(receiver: T, block T.() -> R):R
with(str) {
    println(toUpperCase())
}
```







## 9-8) apply() 함수

apply() 함수는 블록에 객체 자신이 리시버 객체로 전달되고 이 객체가 반환한다. 객체의 상태를 변화시키고 그 객체를 다시 반환할 때 주로 사용한다.

```kotlin
//fun<T> T.apply(block:T.() -> Unit):T
val result = car?.apply{
    car.setColor(Color.RED)
    car.setPrice(1000)
}
```





## 9-9) run() 함수

run() 함수는 익명 함수처럼 사용하는 방법과, 객체에서 호출하는 방법을 모두 제공한다. 익명 함수처럼 사용할 때는 블록의 결과를 반환한다. 블록안에 선언된 변수는 모두 임시로 사용되는 변수이다. 이렇게 복잡한 계산에 임시변수가 많이 필요할 때 유용하다.

```kotlin
//fun <R> run(block: () -> R):R
val avg=run{
    val korean=100
    val english=80
    val math=50
    
    (korean+english+math)/3.0
}
```

객체에서 호출하는 방법은 객체를 블록의 리시버 객체로 전달하고 블록의 결과를 반환한다. 안전한 호출을 사용할 수 있어서 with() 함수보다 더 유용하다.

```kotlin
//fun <T,R> T.run(block:T.() -> R):R
str?.run{
    println(toUpperCase())
}
```