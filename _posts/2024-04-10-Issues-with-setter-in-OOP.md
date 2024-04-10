---
title: Issues with Setters in Object-Oriented Programming (OOP)
permalink: /posts/2024/04/Issues-with-setter-in-OOP/
date: 2024-04-10
tags:
  - OOP
  - setter
  - Mutability
  - side effect
  - Thread safety
  - Temporal Coupling
  - Identity Mutability
  - 객체 지향 프로그래밍
---

<style>
@font-face {
    font-family: 'NanumSquareNeo-Variable';
    src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_11-01@1.0/NanumSquareNeo-Variable.woff2') format('woff2');
    font-weight: normal;
    font-style: normal;
}
body {
   font-family: 'NanumSquareNeo-Variable', sans-serif;
   font-weight: normal; /* 볼드를 더 낮게 설정 */
   font-style: normal; /* 기울임체 해제 */
}
</style>

setter를 왜 사용하면 안되는지, 그리고 setter를 사용하지 않고 얻을 수 있는 이점들을 소개하려고 합니다.
여기서 setter는 꽤 흥미로운 주제를 가지고 있는데 가변성이라고 불리는 꽤 큰 문제에 대한 문을 열어주기 때문입니다.

# OOP (Object-Oriented-Programing)의 문제 setter편

## 가변성이란? 무엇인가? (Mutability)

**[type A]**
```java
public class Book {  
    private String title;  
    Book(String t) {this.title = t;}  
  
    void setTitle(String t) {  
        this.title = t;  
    }  
  
    String getTitle() {  
        return this.title;  
    }  
  
    public void test() {  
        Book b = new Book("This is a book");  
        b.setTitle("This is another book");  
    }  
}
```

**[type B]**
```java
public class Book{  
    private final String title;  
    Book(String t) { title = t;}  
    Book withTitle(String t) {  
        return new Book(t);  
    }  
  
    String getTitle() {  
        return this.title;  
    }  
  
    public void test() {  
        Book b1 = new Book("This is a book");  
        Book b2 = b1.withTitle("This is another book");  
    }  
}
```

여기 typeA로 작성되어있는 코드를 보자. 우리가 흔하게 볼 수 있는 일반적인 디자인 접근 방식입니다. 이 method는 set이라는 수식어가 붙어있고, title값을 객체에 직접 설정하기 때문에 setter라고 합니다.<br>

typeB를 보게 되면 거의 동일하게 구성되어있지만 메서드가 setTitle이라고 되어있지 않습니다.  withTitle()이라고 되어있습니다. 차이 점은 객체 내부의 값을 변경하는 것이 아닌 새 객체를 만듭니다.<br>
그리고 필드에 final 수식어가 붙어있습니다.  따라서 title값을 변경할 수 없으며, 새 객체만 만들 수 있습니다.<br>

그래서 코드에서 보게 되면 객체를 만들고, 새로운 책이 필요한 경우에는 객체를 생성합니다. 필요한 경우에 따라 계속 늘려가면서 사용하게 됩니다.<br>
``` java
    public void test() {  
        Book b1 = new Book("This is a book");  
        Book b2 = b1.withTitle("This is another book");  
    }  
```

따라서 원본을 절대 건드리지 않고, 변경하는 방법은 새로운 객체를 생성하는 것입니다.<br>
그래서 typeB는 불변(immutable) 객체라고 할 수 있습니다.<br>

typeA는 변경이 가능하고 typeB는 변경이 불가능합니다.<br>

보기에는 typeB가 불편해 보일 수 있습니다. 왜냐면 값이 필요할 때마다 새로운 객체를 생성하고 있고, typeA의 방식보다 객체를 많이 생성하기 때문입니다. 그리고 성능 적인 측면에서 바라 보았을 때에도 불변 객체를 사용하는 것이 set을 사용하는 것보다 느립니다. 그러나 유지 보수, 관리성, 코드 가독성, 코드 품질등 다른 많은 요소들이 향상될 것입니다.<br>

<img src="/images/Performance_issues.png" alt="Performance_issues.png">

그래서 우리는 항상 타협점에 서있습니다. 코드의 품질, 유지 관리 가능성, 코드의 아름다움은 항상 성능, 속도와 충돌하게 됩니다. 따라서 코드가 빠르게 작동하도록 하려면 코드의 품질과 유지 관리성을 타협 해야 합니다.<br>

코드 빠르게 작동하기 위한 예시로 static method가 있습니다. 작업을 빠르게 하려면 정적 메서드를 사용해야 합니다. 정적 method가 아니라면 객체와 객체 내부의 method는 항상 메모리와의 상호 작용이 필요합니다. 메모리에 객체를 할당 해야하고, 객체에 가상 테이블을 넣어야 하며, method에 접근하여 작업을 수행하려면 동적 디스패치 프로세스를 거쳐야 합니다.<br>

<img src="/images/Static_method.png" alt="Static_method.png">

그러나 static method의 경우 메모리를 복잡하게 만들지 않고, 코드로 바로 접근하여 필요한 작업을 수행하게 됩니다. 따라서 속도가 중요한 경우에는 객체 지향 프로그래밍과 타협하여 정적 method를 사용하면 됩니다. 가끔은 사용해도 되지만, 항상 static method를 사용하게 되면 큰 문제가 생길 것입니다. 따라서 특정 코드 부분에서 성능이 필요한 경우에만 가끔 수행하는 것이 좋습니다.

다시 돌아와서 불변성은 간단하게 변하지 않는다는 것을 의미하지 않습니다.<br>

> 엥...? 이게 무슨소리인가요?

불변성에 대해 조금 더 자세하게 코드와 함께 설명드리겠습니다.<br>

``` java
public class Book{  
    private final String title;  
    Book(String t) { title = t;}  
    String title(){  
        return this.title;  
    }  
}
```

여기 내가 만든 필드에 final 넣으면 아무도 그 값을 바꿀 수 없습니다. 따라서 method title()을 호출하게 되면 몇 번 호출하더라도 정확히 동일한 결과가 반환됩니다.<br>

```java
public class BookDemo {
    public static void main(String[] args) {
        Book book1 = new Book("The Alchemist");
        System.out.println(book1.title());  
	    // The Alchemist

        Book book2 = new Book("1984");
        System.out.println(book2.title());  
        // 1984

        System.out.println(book1.title());  
        // The Alchemist
    }
}
```

그렇다면 final 코드를 가져다가 사용할 때 아래와 같이 사용하게 된다면 이건 상수가 아니게 됩니다.

```java
public class Book{  
    private final String title;  
    Book(String t) { title = t;}  
  
    String title(){  
        return String.format("/%s / %s", title,new Date());  
    }  
  
    public static void main(String[] args) {  
        Book b1 = new Book("Object Thinking");  
        String t1 = b1.title();  
        String t2 = b1.title();  
        System.out.println(t1);  
        System.out.println(t2);  
    }  
}
```

여기서 title() method를 호출할 때마다 새로운 Date 객체를 생성하여 문자열 단위로 포맷팅하여 반환하므로, 각 method를 호출하면 다른 값이 출력되게 됩니다. 반환하는 값이 변하긴 하지만 그래도 불변객체이긴 합니다. 따라서 불변이 상수를 의미하지 않는다는 점을 기억하시면 좋을 것 같습니다. 위 코드와 같이 불변일 수 있지만 동작에 따라 일정하지 않습니다.

> 상태는 불변이지만, 동작은 실제로 변경이 가능하다.

자 이제 그럼 가변성이 왜 나쁜 것인지 설명드리겠습니다.<br>
가변성의 문제점과 그 이유는 무엇일까요?<br>

## 가변성의 문제점

#### 1. side effect (부작용)
**[typeA]**
```java
public String post (Request request) {
	request.setMethod("POST");
	return request.fetch();
}

Request r = new Request("http://...");
r.setMethod("GET");
String first = this.post(r);
String second = r.fetch();
```

**[typeB]**
```java
public String post(Request request){
	return request
	.withMethod("POST")
	.fetch()
}
Request r = new Request("http://...")
	.withMEthod("GET");
String first = this.post(r);
String second = r.fetch();
```

typeA 코드를 보게되면, 여기서 post method는 Request 객체의 HTTP 메소드를 "POST"로 설정하고, fetch method를 호출하여 결과를 반환합니다. fetch method는 주어진 설정으로 HTTP 요청을 수행하고 그 결과를 문자열로 반환하는 것입니다.

새 Request 객체를 생성하고, HTTP 메소드를 "GET"으로 설정합니다. 그리고 post 메서드를 호출하여 첫 번째 HTTP 요청을 수행하고 그 결과를 first 변수에 저장합니다.

그러나 주의해야 할 점은 post 메서드 내에서 Request 객체의 메소드를 "POST"로 변경했습니다. 따라서, post 메서드를 호출한 후에는 r 객체의 HTTP 메소드가 "POST"가 됩니다.<br>
마지막으로 fetch 메서드를 다시 호출하여 두 번째 HTTP 요청을 수행하고 그 결과를 second 변수에 저장합니다. 이 시점에서의 요청은 "POST" 메소드를 사용하게 됩니다.<br>

따라서 first는 "POST" 메소드를 사용한 요청의 결과를, secont는 같은 "POST" 메소드를 사용한 또 다른 요청의 결과를 저장하게 됩니다. 처음에 "GET" 메소드를 설정했지만, 실제로 수행되는 모든 요청은 "POST" 메소드를 사용하게 됩니다.

post 메소드를 호출할 때 이 메소드가 post 요청을 수행하는 것 이외에 작업을 수행 할 것이라고 기대하지 않았습니다. 그런데 post를 사용해서 매개변수의 수정이 발생하게 됩니다.

여기서 왜 사이드 이펙트가 발생했다고 말할 수 있습니다. side effect는 기대하지 않았던 효과입니다. 이렇게 짜여진 코드는 유지 관리하기 매우 어렵습니다. 예를 들어 내가 아래의 코드만 알고 있다고 할 때 second가 왜 Post요청으로 변경되어 요청이 됐는지 알 수가 없습니다.
```java
Request r = new Request("http://...")
	.withMEthod("GET");
String first = this.post(r);
String second = r.fetch();
```

나는 분명히 "GET"으로 요청하도록 request 객체를 만들었고, 그리고 fetch 메소드를 사용하여 HTTP 요청을 수행시켰습니다. 근데 POST요청으로 변경된겁니다. 지금은 코드의 길이가 짧아 변경점을 찾을 수 있을 것 같지만 메서드에서 수행하는 코드의 길이가 30줄이 넘어간다고 했을 때 어디서 어떤 라인에서 이 side-effect가 발생했는지 찾을 수가 없습니다. 이 메서드가 다른 메서드를 사용하고 있고, 그 메서드가 다른 메서드를 호출하고 어디론가 가고.. 또 가고.. 그렇다면 디버깅하기 어려워집니다.

그래서 객체를 불변 객체로 만들고 누군가에게 주었다면, 변경되지 않도록 해야 합니다. 이렇게 되면 올바른 코드, 내가 기대했던 코드가 완성됩니다.

typeB 코드를 보시게 되면 어떻게 사용했는지 알 수 있습니다. .withMethod("POST")라는 메서드로 새로운 객체를 생성하기 때문에 생성된 request 객체는 변경되지 않습니다.

#### 2. Thread safety (쓰레드 안전성)

**[typeA]**
```java

public class Books {
    private final int c = 0;

    public void add() {
        this.c = this.c + 1;
    }

    public int getCount() {
        return this.c;
    }

    public static void main(String[] args) {
        ExecutorService e = Executors.newCachedThreadPool();
        final Books books = new Books();
        for (int i = 0; i < 1000; i++) {
            e.execute(() -> {
                books.add();
            });
        }
        e.shutdown();
        while (!e.isTerminated()) {
            // wait for all tasks to finish
        }
        System.out.println(books.getCount()); 
    }
}
```

위 코드를 보게되면 Books이라는 객체가 존재하고, add 메서드를 호출하면 값이 +1 되는 간단한 코드입니다. 그리고 메인 메서드에서는 쓰레드 풀을 생성하고, 생성된 풀에 1000개의 쓰레드를 추가합니다. 그리고 1000개의 병렬 라인을 구축하고, 병렬 쓰레드에서 실행되게 만들었습니다. 각각의 쓰레드에서 숫자 1을 증가시키려고 합니다.<br>

그러면 결과는 어떻게 될까요?<br>

내가 기대하는 결과는 1000이 나와야 합니다. 그러나 결과는 항상 1000이 보다 작을 것입니다. 아마 700 이나 900 정도의 예측할 수 없는 숫자가 됩니다. 동시성 처리에 관련된 내용 중 제가 알고 있는 방법은 synchronized 키워드로 동기화시켜서 문제를 해결한다 인데 이렇게되면 병렬처리가 안되니 나중에 동시성 관련해서는 알아보고 다시 포스팅을 하겠습니다.<br>

돌아와서 병렬 쓰레드에서 add메서드의 수행작업을 했을 때 잘못된 결과가 반환되게 됩니다. 그 이유는 바로 "가변성"입니다. 위 코드에서 c라는 필드에 값을 설정하는 것을 금지하기 위해 final 키워드를 붙였습니다. 이렇게 되면 문제는 해결됩니다.<br>

> 위 코드에 그대로 final만 붙이게 된다면 add 메서드 내부의 this.c = this.c + 1; 코드는 컴파일 오류를 일으키게 됩니다. 이는 final이 지정된 필드의 값을 변경하려 했기 때문입니다.

따라서 setter는 가변성, 동시성 문제를 가지고 있습니다. 이렇게 setter의 사용은 충돌과 동기화 문제의 가장 큰 근본적인 원인이됩니다.

객체가 변경 가능하다면 동시성 문제가 발생하게 되어 문제를 해결하기 어렵게 됩니다. 이를 해결하기 위해 가장 쉬운 방법은 클래스를 불변으로 만드는 겁니다.

#### 3. Temporal Coupling

Temporal Coupling

```java
Request r = new Request("http://...");
r.method("POST");
String first = r.fetch();
r.body("text=hello");
String second = r.fetch();
```

```java
Request r = new Request("http://...");
//r.method("POST");
//String first = r.fetch();
r.body("text=hello");
String second = r.fetch();
```

Temporal Coupling을 설명드리기 전에 coupling(결합력)은 프로그래밍 세계에서는 좋지 않다고 알고 있습니다. 간단하게 설명하자면 두 객체 또는 두 클래스가 서로에 대해 너무 많이 알고 있으면 유지 보수하기가 어렵습니다. A 객체의 어떤 값을 수정했으니, B 객체를 수정해야지 이렇게 하나의 변경점이 생겼을 때 연결된 모든 코드들이 수정되어야 하기 때문에 좋지 않습니다.<br>

위 코드를 보게되면 request 객체를 생성합니다. 그리고 메서드를 Post로 설정합니다. 그 다음 fetch를 통해 값을 요청합니다. 그리고 body에 값을 넣어 본문의 내용을 변경합니다. . 그 다음 fetch를 통해 새로운 값을 요청합니다. 여기서 second에 들어있는 fetch는 해당 POST 메서드가 이미 들어있을 것으로 예상됩니다. 이것이 바로 Temporal Coupling 입니다.<br>

어떤 것이 예상 되어야 하고, 내가 동작하기 전에 어떤 것이 일어나야 합니다. 그래서 나는 그 시간에 의존하게 됩니다.<br>

두 번째 코드를 보게 되면 나는 첫 번째 요청이 필요 없게 되어 주석 처리 이후 사용하려고 하면 에러가 나게됩니다. 그 이유는 POST 메서드가 세팅 되어있지 않기 때문입니다. 여기서 r.method가 필요하다는 것을 우리는 어떻게 알 수 있을까요? 알 수 있는 방법이 없습니다. 따라서 불변 객체를 사용하게되면 이런 Temporal Coupling 문제는 사라지게 됩니다. 왜냐하면 객체를 수정하고 수정된 버전을 정확하게 사용하기 때문입니다.

Temporal Coupling이란 두 개 이상의 동작이 순서에 의존적인 경우를 말합니다. 즉, 특정 코드 부분을 다른 코드 부분 뒤에 실행해야만 올바르게 동작하는 의존성입니다.

예를 들어, setter 메서드를 사용하여 객체의 상태를 설정하는 부분에서는 Temporal Coupling이 발생하는데, 이는 setter 메서드 호출의 순서가 객체의 최종 상태와 동작에 영향을 줄 수 있기 때문입니다

```java
MyObject obj = new MyObject();
obj.setA(1);  // setA() must be called before setB()
obj.setB(2);
```
위 코드에서 setA()와 setB() 메서드 호출의 순서가 바뀌면, MyObject 객체의 상태가 예기치 않게 변경될 수 있는데, 이럴 경우 Temporal Coupling이 발생한다고 할 수 있습니다.

Temporal Coupling을 피하려면, 객체 생성 시에 모든 필요한 상태를 가진 상태를 갖추도록 하는 것이 좋다. 이를 위해 생성자를 통해 객체의 상태를 설정하거나 빌더 패턴을 사용하는 방법이 있습니다.
```java
MyObject obj = new MyObject(1, 2);
```
혹은 빌더 패턴을 사용하여 객체를 생성하는 경우:
``` java
MyObject obj = new MyObject.Builder().setA(1).setB(2).build();
```
위 두 방법 모두 객체 생성 시점에 모든 상태를 설정하기 때문에 Temporal Coupling을 피하는 좋은 관행입니다.
다시 말해, 이 두 방식은 필드를 final로 선언하여 불변성을 강조하고, Temporal Coupling 문제를 개선하면서 동시에 가독성과 유지보수성을 향상시킬 수 있습니다.


#### 4. Identity Mutability (객체 식별자 값 변경 가능성)

Identity Mutability는 객체의 식별자 값이 변경 가능하다는 개념이며, setter를 사용함으로써 발생하는 문제 중 하나입니다.

예를 들어, 객체의 ID와 같은 유일한 식별자 항목을 변경할 수 있게하는 setter 메서드를 제공하면, 전체 시스템에서 일관성을 유지하기 어렵게 됩니다. 객체를 참조하고 있는 다른 부분의 코드가 객체의 식별자가 변경되면 기대하는 동작을 하지 않게 되는 문제가 생길 수 있습니다

```java
class User {
    private String id;
    
    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }
}
```

위 예시에서, setId 메서드를 사용하여 User 객체의 식별자를 언제든지 바꿀 수 있습니다. 이는 그 객체를 참조하는 모든 코드에 영향을 미칠 수 있습니다.

이런 문제를 해결하기 위한 한 가지 방법은 객체지향 원칙인 불변성(immutability)을 강조하는 것입니다. 불변 객체는 생성된 후에 상태를 변경할 수 없으므로, 항상 일관된 상태를 가지게 되어 안정성이 향상됩니다. 이는 setter 메서드가 없는 상태로, 모든 필드를 final로 선언하고, 값을 한 번만 할당하며, 그 후에는 변경할 수 없게 하는 방식으로 구현해볼 수 있습니다.

```java
class User {
    private final String id;

    public User(String id) {
        this.id = id;
    }

    public String getId() {
        return id;
    }
}
```

위와 같이 구현하게 되면, User 객체는 생성 시에 지정된 ID를 가지게 되며, 이후에는 변경할 수 없게 됩니다. 이러한 접근법은 코드의 안정성을 향상시키며, 보다 예측 가능한 동작을 제공합니다.

추천하는 서적입니다<br>
Java Concurrency inf Practice by Goetz et al (동시성 문제에 관한 책)<br>

