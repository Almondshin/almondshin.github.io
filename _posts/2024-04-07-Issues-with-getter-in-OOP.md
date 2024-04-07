---
title: Issues with Getters in Object-Oriented Programming (OOP)
permalink: /posts/2024/04/Issues-with-getter-in-OOP/
date: 2024-04-07
tags:
  - OOP
  - getter
  - Encapsulation
  - Abstraction
  - 캡슐화
  - 추상화
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
getter가 실제로 객체지향 프로그래밍을 더 효과적으로 작성하게 해주며, 디자인을 더욱 객체 지향적으로 만드는데 도움이 된다.
라고들 하지만 내 생각에는 아닌 것 같습니다.

이것에 대한 근거로 몇몇 유명한 작가들의 말, 유명한 참고자료를 내가 이해한 내용을 기반으로 같이 첨부하여 설명드리겠습니다.


# OOP (Object-Oriented-Programing)의 문제 getter편

>첫 번째 추상화에 대해서 설명하고, <br>
> 그런 다음 대한에 대해 몇가지 아이디어를 소개하겠습니다. <br>
> getter 없이 사용하는 방법 <br>
>위 getter 없이 사용이 가능한 것인지에 대해 논의하고 자료를 찾아보며 정리할 예정입니다.

# 1. getter란? 무엇인가?

**[type A]**
``` java
public class MiddleSquare {  
    private short seed;  
  
    MiddleSquare(short s) {  
        this.seed = s;  
    }  
  
    short getNextNumber() {  
        int s = (int) seed * seed;  
        seed = (short) (s >> s);  
        return seed;  
    }
}
```

[type B]
``` java
public class MiddleSquare {  
    private short seed;  
  
    MiddleSquare(short s) {  
        this.seed = s;  
    }  
  
	short getSeed(){  
	    return this.seed;  
	}
}
```

여기 type A, B로 작성된 코드를 보자. 둘 다 이름 앞에 get이라는 수식어가 붙어있고, 둘 다 class에서 무언가를 가져오고 있다.

여기서 둘중에 어떤게 getter일까?<br>
우리는 여기서 type A를 getter라고 부를 수 있을까?<br>
아니면 type B를 getter라고 부를 수 있을까??<br>

<img src="/images/accessor.png" alt="accessor.png">

먼저 type B를 보겠습니다.<br>
여기서 우리는 그 누구도 해당 필드를 직접 건드리지 않기를 바라며, 접근 제어자를 **private**으로 설정한 필드와 get 수식어가 붙은 **getSeed()** method를 볼 수 있습니다.

type B에 해당하는 getSeed를 사용하려면 아래 코드와 같이 사용할 수 있습니다.
```java
MiddleSquare m = new MiddleSquare((short) 45);  
short t = m.seed();  // [사용 불가능] 기술적으로 불가능
short t = m.getSeed();
System.out.println(t);  
```

여기에 나와있는 m.getSeed(); 이것을 우리는 getter / get이라고 부릅니다.

그렇다면 type A의 코드에 getNextNumber도 method 이름앞에 get이 붙어있는데 이것은 왜 getter가 아닌지를 보여주려고 합니다.

type B는 캡슐화 된 것을 반환하기만 하고,<br>
type A는 동일하게 캡슐화 된 것을 반환하고, 데이터를 사용하여 일부 작업을 수행한 다음 결과를 반환하고 있으므로, getter가 아니라고 말할 수 있겠습니다.

type B는 어떠한 작업, 동작을 하지 않고, private 필드에 대해 엑세스만 제공하고 있습니다.

> getter = 어떠한 작업, 동작을 하지 않고, private 필드에 대해 엑세스만 제공하는것

그럼 일단 혼돈을 피하기 위해 이름 접두사에 get이 보인다면 getter입니다.<br>
그렇기 때문에 같은 접두사인 get을 사용하지 않는 것이 좋겠습니다.<br>
```text
getNextNumber -> nextNumber or calculatedNumber
```

이렇게 명명규칙을 정해놓는다면, method에 get이 붙어있다면 확인 해보세요. 과연 개인필드에 대해 엑세스만 제공하고 있는지...

우리가 IDE에서 class를 생성하고, private 필드를 만든다음 "generate getter and setter" 버튼을 통해서 getter와 setter를 생성하는데, 이건 완전히 잘못된 생각이라고 믿습니다.

왜 인지는 조금 있다가 설명해드리겠습니다.

이렇게 엑세스를 제공하는 것 외에 일반적으로 getter를 사용하는 용도는 무엇일까요?<br>
"private필드가 있고, 예를 들어 getField, getSeed가 있는 method인 경우 왜 이런 짓을 한 것일까?"

이에 대해 찾아보기 위해 ChatGPT에 질문을 해봤습니다.
> ChatGPT에 질문한 이유는 인터넷에서 검색을 통해 답변을 알아보려고 했기 때문에 인터넷에 그러한 답변들이 많은 것 같으니 ChatGPT가 어떻게 답변했는지 살펴보겠습니다.

<img src="/images/GPTQuestion1.png" alt="GPTQuestion1.png" width="690px" height="70px">
<img src="/images/GPTQuestion2.png" alt="GPTQuestion2.png" width="690px" height="70px">
<br>
<br>
<br>

<img src="/images/GPTAnswer.png" alt="GPTAnswer.png">


위 ChatGPT가 말한 내용을 살펴봅시다. <br>
1. 캡슐화는 객체 지향 프로그램의 중요한 개념 중 하나로 getter를 사용하면 외부에서 직접적으로 접근하지 못하게 한다.라고 합니다.

	엥...?<br>
	지금 우리는 getSeed()를 통해 외부에서 직접적으로 접근이 가능하도록 되어있는데 어떻게 외부에서 직접적으로 접근하지 못하게 한다는 것인가?<br>
	그래서 이게 내 가장 큰 관심사입니다. <br>
	getter를 제공했다면 어떻게 캡슐화하여 외부로부터 보호한다는 것인지 모르겠습니다.

2. 유연성입니다. 유연성은 getter를 사용하는 합리적인 이유라고 생각합니다.<br>

	예를들어 getSeed 메소드가 있고, 내 class를 사용하는 사용자가 있고, 사용자는 내 class에 있는 getSeed를 호출합니다. <br>
	그리고 나는 내일 seed라는 필드명 대신에 이름을 mySeed로 바꾸기로 결정했습니다.<br>
	하지만 내 class를 사용하는 사용자는 getSeed와 연결되어있기 때문에 이에 대해 알지 못합니다.<br>
	따라서 사용자는 계속 getSeed()를 사용할 것이므로 내 class 내부에서 발생한 이름 변경에 대해 걱정할 필요가 없습니다.<br>
	또는 seed 필드를 완전히 제거하고 class 내부를 완전히 재구성 했지만 여전히 이에 대해 모르고 getSeed를 사용한다고 가정해 보면 이런 유연성이 getter를 사용하는 이유라는 것이 말이 됩니다.<br>
 
이 유연성은 객체와 사용자간의 결합력을 낮추기 때문에 이점이 있습니다.<br>
하지만 getter가 직접 필드에 있는 값을 꺼내서 이동시켜 사용하고있는 경우, 이 유연성에 이점은 사라집니다.<br>

getter의 사용은 내 class를 사용하는 클라이언트가 내 class가 바뀌지 않고, 내가 구조를 바꿀 것이라고 기대하지 않는다는 것을 의미합니다. 따라서 내가 class의 내부 구조를 변경하게 된다면 내 class를 사용하는 로직들은 실패하고, 내 class를 더 이상 사용 할 수 없게 될 것입니다.

그래서 내 class를 사용하는 클라이언트는 단지 내가 제공하는 method에만 의존하면 됩니다.

일반적으로 주로 getter가 사용되는 곳은 DTO(Data Transfer Objects) 일 겁니다.
> DTO는 디자인 패턴입니다. 주로 다른 계층 간 데이터를 캡슐화 하여 전송하기 위해 사용됩니다.

```java 
public class BookDTO {  
    private int id;  
    private String title;  
    private String author;  
  
    BookDTO(int id, String title, String author) {  
        this.id = id;  
        this.title = title;  
        this.author = author;  
    }  
  
    int getId() {  
        return this.id  
    }  
  
    String getTitle() {  
        return this.title;  
    }  
  
    String getAuthor() {  
        return this.author;  
    }  
}

class JsonApi   {  
	BookDTO getById(int id) {/*... */}  
}	
```

클라이언트(Client)는 API를 호출하여 서버에 요청합니다. 서버는 해당 요청을 받아 처리하고, 필요한 경우 데이터베이스(DB)나 외부 서비스, 또는 웹 서비스 등을 호출합니다. 이후 서버는 반환된 데이터를 클라이언트에게 전달합니다. 이 때, 반환된 데이터를 내 프로그램 내부의 코드로 변환하기 위해 JsonApi를 디자인합니다.

이를 간단히 정리하면 다음과 같습니다:

1. 클라이언트(Client)가 API를 호출하여 서버에 요청합니다.
2. 서버는 요청을 받아 처리하고, 필요한 데이터를 데이터베이스(DB)나 외부 서비스, 또는 웹 서비스 등을 통해 가져옵니다.
3. 서버는 가져온 데이터를 클라이언트에게 반환합니다.
4. 반환된 데이터를 내 프로그램 내부의 코드로 변환하기 위해 JsonApi를 디자인합니다.


예를 들어, 클라이언트에서는 다음과 같이 JsonApi의 getById() 메서드를 호출하여 데이터를 가져올 수 있습니다.
``` java
BookDTO dto = api.getById(42);
```

이때, getById() 메서드는 내부적으로 해당 id에 해당하는 데이터를 찾아서 BookDTO 객체로 반환해야 합니다. 따라서 클라이언트는 반환된 BookDTO 객체를 통해 제목과 저자 정보에 접근할 수 있습니다.

```java
println(dto.getTitle);
println(dto.getAuthor);
```

이러한 내용을 종합하면, 클라이언트가 JsonApi의 getById() 메서드를 호출하여 특정 id에 해당하는 데이터를 요청하고, 이를 내부 처리한 후 BookDTO 객체로 반환받아 제목과 저자 정보를 출력할 수 있습니다.

이것은 매우 인기 있는 패턴이고 많이 사용되고 있으며, 사람들은 이것이 객체 지향 프로그래밍이라고 믿고습니다. 그래서 우리는 객체를 데이터를 위한 임시 컨테이너로 사용하고 있고, 이것이 객체 지향 프로그래밍의 주 목적이라고 믿습니다.

그러나 이렇게 믿는 것은 잘못됐다고 말할 수 있습니다.<br>
이렇게 사용하면 안됩니다.<br>

왜냐구요? 지금부터 이야기 하겠습니다.<br>
하지만... 제가 여태 짜왔던 코드, 여러분의 코드에 이와 같이 짜여져 있을 것이라고 확신합니다.<br>

JSON API를 호출하고 데이터를 반환해야 하며 이 데이터를 사용해야하는데 어떻게 해야 할까요?<br>
최근 자바 17버전 이상부터는 Record라는 객체를 도입했습니다.<br>
이 Record는 DTO랑 유사하게 생겼지만 객체라고 부르지 않습니다.<br>

class BookDTO처럼 작성된 객체는 anemic(빈혈)합니다.<br>
이 객체는 행동, 동작이 없어요. 왜 DTO의 이름을 달고 존재하는지 모르겠습니다.<br>
BookDTO의 형태는 살아 있지도 않고, 행동하지도 않으며 일시적으로 데이터를 보관한 다음 데이터가 나가기 때문에 어떤 의미도 없고, 작동하지 않습니다. 마치 컨테이너의 역할만 하는 것 같습니다.

위 방식으로 데이터를 한 위치에서 다른 위치로 옮기는 것은 괜찮을 수 있습니다. 그러나 이것을 객체 지향 프로그래밍이라고 부를 수 없습니다. 이것은 절차 지향 프로그래밍입니다.

이러한 행위는 우리가 알고 있는 C에서 한 일들입니다.

예를 들어 C언어에는 구조체(struct)가 있고 이것을 구조체라고 말할 수 있으며, 구조체에서 int id, char \*author, char \*title 를 정의할 수 있습니다.

``` javascript
struct BookStruct
{
	int id;
	char *author;
	char *title;
 ...  
};
```

이것은 객체 지향 프로그래밍과 아무 관련이 없으며 단지 메모리 구조일 뿐입니다.

getter를 사용하는 것은 객체 지향 언어라는 언어를 사용하고 있지만 프로그래밍 패러다임, 프로그래밍 사고 방식은 절차적이며 C로 코드를 작성하고 있는 것이나 다름없습니다.

이렇게 작성하는 것이 나쁘다는 것이 아닙니다.<br>
그렇지만 객체 지향 프로그래밍 관점에서는 이것은 잘못 됐다고 말 할 수 있습니다.<br>

API에 데이터를 사용하고 객체 지향 패러다임을 유지하려고 하는 경우 BookDTO와 다르게 수행해야 합니다.

객체가 method에서 method로 데이터를 전달 하도록 허용하지 마세요.<br>
객체는 그러기 위한 것이 아닙니다.<br>

객체 지향 프로그래밍에서 객체를 anemic(빈혈) 상태로 만드는 것은 일종의 무례한 일이라고 볼 수 있습니다. 객체는 살아 있고, 행동하기를 원하는데 BookDTO의 get~은 행동이 아닙니다.<br>
어떤 사람은 말할 것입니다 이것은 객체의 상태(private int id; ... )이고 동작(get...)입니다.ㅋㅋㅋ

함수가 있지만 실제 동작은 아니기 때문에 이러한 getter는 실제 동작이라고 할 수는 없습니다.

함수는 method이므로 실제로 무언가를 수행해야 합니다. 따라서 getId()를 호출 할 때 거기서 몇가지 byte 코드로 명령이 실행되면 이것을 동작이라고 말할 수 있습니다.

위 DTO는 데이터의 임시 저장소로만 사용되므로 객체는 데이터에 대해 아무것도 모르고 객체는 데이터에 아무것도 추가하지도 않으며, 이 객체는 이 데이터가 어떻게 사용되는지 전혀 모르고 그저 단순히 데이터를 보유할 뿐입니다.

------

# 2. 캡슐화

위에서 getter에 대해서 얼마나 나쁜지 충분히 설명했습니다.<br>
이제 글을 읽던 여러분은 아마도 다음과 같이 묻고 싶을 것입니다.<br>

그럼 대안은 무엇이고 어떻게 사용해야 합니까?

제 제안을 드리기 전에 캡슐화가 무엇인지 먼저 알아보겠습니다.

객체 지향 프로그래밍이라는 용어에 대해서 들어보셨을겁니다. 일반적으로 사람들은 이것은 캡상추다 (캡슐화, 추상화, 상속 , 다형성)이라고 말합니다. 그래서 보통 이 네 단어를 사용하고 객체 지향 프로그래밍에 관련된 단어가 더 많이 있더라도 적어도 이 네가지 원칙은 있다고 말합니다.

4가지 원칙은 실제로 객체지향 프로그래밍의 기초이고, 이 단어중 하나가 캡슐화 입니다.

캡슐화는 JAVA와 객체 지향의 아버지 중 한 명인 Java의 아버지 Grady Booch(그래디 부치)가 한 이야기를 보겠습니다.

<img src="/images/booch_grady.jpg" alt="booch_grady.jpg" width="150">
<img src="/images/Grady_Booch.png" alt="GradyBooch.png">



그러니까 캡슐화는 간단히 말해서 정보 은닉, 데이터 은닉을 의미하므로 추상화에 아주 가깝다는 뜻이고 추상화는 당신에게서 무언가를 추상화 하므로 당신은 그 안에 무엇이 있는지 알 필요가 없습니다. 당신은 내가 말하는 내용만 알면 됩니다. 따라서 알고리즘이 어떻게 작동하는지 알 필요가 없습니다. 나는 당신을 위한 알고리즘 입니다. 여기서 캡슐화는 당신이 알 수 없다는 것을 의미하며, 시도하더라도 알고리즘이 어떻게 작동하는지 알 수가 없다는 것을 말합니다.<br>

일반적으로 캡슐화시킨 당사자가 아니고 일반 프로그래머는 무슨 일이 일어나는지 알 수 없기 때문에 알 필요가 없는 내용들을 내부에 캡슐화하고 디자인 합니다.<br>

예시로 내가 난수를 계산하는 알고리즘이라면 당신은 화면의 어떤 공간에 값을 채워 넣고 좌우로 움직일지 결정하는 게임이라고 합시다, 내가 어떻게 작동하는지는 당신은 알 수가 없습니다. 그리고 나는 당신이 왼쪽으로 가던 오른쪽으로 가던 어떻게 일하든 상관하지 않습니다. 당신은 나보다 더 높은 추상화 수준에 있으므로 당신이 존재한다는 것조차 나는 모르지만 당신은 내가 존재한다는 것을 알고 있습니다. 당신은 나에게 말하거나,물어보면 나는 난수를 제공합니다.<br>

당신에게 나는 단지 알고리즘일 뿐이고 당신은 내가 어떻게 작업하는지에 대한 세부사항을 볼 수도 없고 알 필요도 없습니다.<br>

이렇게 정보를 숨기는 것이 디자인에 좋습니다. 그것은  확실히 객체 지향 디자인의 미덕입니다.<br>

정말 이 정보를 숨기는데 왜 좋은가요?<br>
이유는 당신의 사용자 중 어느 누구도, 당신을 사용한 class와 Object 중 어느 것도 당신을 학대할 수 없고, 잘못된 방식으로 사용할 수 없을 것입니다. 그래서 그들은 당신을 사용하는데에 있어 매우 제한적이고 통제된 방법을 갖게 될 것입니다.<br>

그리고 그것은 결합력(coupling)을 감소시키는 역할을 합니다.<br>

객체의 결합력, 응집력이 있는데 결합력은 감소 할수록 좋고, 응집력을 증가 할수록 좋습니다.<br>

1. **결합력 감소**: 정보 은닉은 결합력을 감소시킵니다. 이는 클래스나 객체 사이의 의존성을 줄여서 변경이나 수정이 발생했을 때 다른 부분에 미치는 영향을 최소화합니다.
2. **응집력 향상**: 정보 은닉은 클래스나 객체의 응집력을 향상시킵니다. 클래스나 객체는 자신의 데이터와 동작을 함께 포함하므로, 관련된 작업들이 한 곳에 모여 있어 유지보수와 이해가 쉬워집니다.
3. **유연한 구현 변경**: 정보 은닉은 클래스나 객체를 변경할 때 다른 부분에 미치는 영향을 최소화합니다. 따라서 데이터 변경 후에 시스템의 다른 부분이 영향을 받지 않고 구현을 쉽게 변경할 수 있습니다.

이를 종합하면, 정보 은닉은 소프트웨어의 안정성, 유연성, 유지보수성을 향상시키는 데 중요한 역할을 합니다. 따라서 정보를 숨기는 것이 좋은 설계 원칙이라고 볼 수 있습니다.<br>

예를 들어, 내 알고리즘은 숫자 생성을 위한 무작위 알고리즘인데, 내일 나는 더 강력한 것을 구현하기로 결정했다고 가정해봅시다. 사용자에게는 그냥 다음 숫자를 얻는 방법을 알려주면 됩니다. 내부적으로는 getSeed() 메서드를 제공하지 않고 nextNumber() 메서드만을 제공하여, 이 숫자를 어떻게 어디서 얻고 동작하는지, 어떻게 계산하는지 사용자는 걱정하지 않아도 됩니다.<br>

이러한 방식은 개발자에게도 이점을 제공합니다. 코드 수정이 빨라지고 버그가 줄어들며, 릴리스 시 문제가 줄어듭니다. 또한, 코드가 크게 변경될 때 발생하는 많은 문제를 완화해줍니다. 이는 느슨한 결합이 소프트웨어 엔지니어링에서 중요한 역할을 한다는 것을 보여줍니다.<br>

그래서 캡슐화는 느슨한 결합으로 이어집니다.

<img src="/images/DavidWest.jpg" alt="DavidWest.jpg" width="150">
<img src="/images/David_West.png" alt="DavidWest.png">

David West는 말합니다 대부분의 캡슐화는 실제 장벽이라기보다는 규율이며, 객체의 무결성은 절대적인 의미에서 보호되는 경우는 없으며, 특히 소프트웨어에서는 더욱 그러하므로 객체의 캡슐화를 존중하는 것은 객체를 사용하는 사용자에게 달려있다.<br>

여기서의 요점은 기술적으로 언어 도구를 사용하면 실제로 정보를 보호할 수 없다는 점입니다.<br>
이 말은 getter를 사용하는 것에 대해 말하는 것이라고 볼 수 있습니다.<br>
```java
private int seed;
```
이렇게 생긴 private field가 있고, 이 필드의 접근제어자로 보호하고 있는 것처럼 보이지만 아닙니다.<br>
이것은 reflection API를 가지고 있습니다. 그래서 이 필드에 외부에서 접근할 수 있습니다.<br>
따라서 프로그래머, 객체 사용자가 규율을 준수하고 객체의 작성자를 존중한다면, 사용자는 해당 seed 필드를 건드리지 않을 것입니다.<br>

위 인용된 두 작가의 책은 객체 지향 프로그래밍에 대한 다양한 관점을 제시합니다. Grady Booch 등의 저자들은 정보 은닉을 통한 캡슐화를 강조하며, 객체의 비밀성을 유지하고 다른 추상화와 명확한 관심사 분리를 제공한다고 주장합니다. 이는 객체의 핵심 특성에 기여하지 않는 비밀을 숨기는 것이 중요하며, 이는 객체의 구조와 메서드의 구현을 일반적으로 숨김으로써 달성됩니다.<br>

반면에 David West 등의 저자들은 캡슐화를 엄격한 장벽으로 보는 것보다는 더 나은 프로그래밍을 위한 행동 규칙으로 이해하는 것이 중요하다고 제안합니다. 이는 객체의 무결성이 절대적으로 보호되지 않는다는 점을 강조하며, 사용자가 객체의 캡슐화를 존중해야 한다고 설명합니다.<br>

위에서 인용한 두 작가의 책중에 객체 지향 프로그래밍의 전통적인 책을 보시려면 Object-Oriented Analysis and Design with Applications <br>
더 나은 객체 지향 프로그래밍을 배우고 싶다면 Object Thinking을 추천합니다.<br>

<img src="/images/myOpinion.png" alt="myOpinion.png">

하지만 우리는 getter, pubilc int getSeed() 이렇게 사용하고 있죠...<br>

이렇게 되면 캡슐화는 어디에 있고, 무결성은 어디에 있습니까? 다 사라졌습니다. 객체의 캡슐화가 깨지고, 객체의 상태를 보호하지 못하면서 객체의 진정한 의미를 잃어버린 것입니다.<br>

우리가 getter를 도입하면 모든 사람들에게 공개적으로 허용하고, 발표하고, 사용하도록 하는 것입니다. 그리고 그들이 그렇게 사용하도록 장려하는 것입니다.<br>

그렇게 되면 사용자는 내가 만든 seed가 무엇이고, 내 안에 무엇이 있는지 알 수 있습니다.<br>

David West는 말했습니다. 캡슐화는 실제 장벽이 아니라 규율이라고, 따라서 기술적으로 우리에게는 캡슐화하여 필드를 보호할 수단은 없습니다. 어떤 프로그래밍 언어든지 간에 getter를 사용할 수 있다면? 전체적인 무결성은 사라지게 되는 겁니다.<br>

그리고 이 규율을 지키고 존중하는 프로그래머가 있겠지라고 생각하지만은 주위에 작성된 코드를 본다면 그다지 많지 않다는 걸 알게 될 겁니다.<br>

그렇습니다. 우린 기술적으로 무결성을 보호할 기술적인 수단이 없으며, 오직 규율에만 의존하고 있습니다. 사람들은 다른 객체를 존중하고 객체와 디자이너를 존중하여 다른 사용자들이 자신의 내부에 액세스 할 수 없도록 규율을 받아드리고 지켜야 합니다.<br>

그러나 언어, IDE, 커뮤니티, 사람, 책, 블로그 글들 모두가 소개한 객체 지향에 대한 내용을 읽으면 getter를 사용하고 이것이 코드를 작성하는 방법이라고 소개하기 시작합니다.<br>

private 필드를 도입하고, 다음은 getter, setter를 도입하고......<br>

---
# 3. 무결성

이제 무결성에 대해 이야기 해보겠습니다.

무결성을 보호하는 것이 무엇인가?
> Which class hides data better?

type A
```java
public class User {  
    private String name;  
    User(String name) {this.name = name}  
    int getName() { return Integer.parseInt(name); }  
  }
  
if(employees.contains(user.getName())){  
	/* pay a salary*/  
}  
```

type B
```java
public class User {  
    private String name;  
    User(String name) {this.name = name}  
    boolean isEmployee(){  
        return employees.contains(this.name);  
    }  
}

if(user.isEmployee()){  
	/* pay a salary*/  
}  
```

type A를 보시면 class는 user이고 사용자의 이름, 즉 name을 캡슐화 했습니다.<br>
여기에 getter, getName이라는 getter가 있으므로 사용자의 이름을 가져올 수 있습니다.<br>
그런 다음 나중에 uesr Class에서 사용자의 이름을 가져오고 직원인지 결정합니다.<br>
이 사용자가 직원이라면 급여를 사용자에게 지불합니다.<br>

type B를 보시면 class는 user이고 사용자의 이름도 동일하게 캡슐화 했습니다.<br>
isEmployee라는 method를 도입합니다 따라서 이 사용자가 직원인지 여부는 외부에서가 아닌 내부에서 User class가 결정하므로 이 class는 훨씬 더 효과적으로 보호할 수 있게 됩니다.<br>

여기서 보면 type B가 type A보다 무결하다고 할 수 있습니다.<br>
왜? type A는 모든 사람에게 그의 이름이 무엇인지 알려줄 뿐이고,  알고 싶어하는 모든 사람에게 이름을 알려주고, 이름을 공개합니다. 이는 무결성이 없음을 의미하므로 type A는 의미가 없는 class입니다. 완전히 anemic합니다.<br>

`이건 완전 바보입니다ㅋㅋㅋ 생각도 하지 않고, 뇌가 없습니다. 무슨 이름? 내 이름 나중에 어떻게 되든 상관없어, 누가 직원인지 확인하고, 이름을 확인하고 내 이름으로 뭘 하든 상관없어 그냥 가져가서 써 `

type B는 훨씬 더 정교하고, 훨씬 더 똑똑하고, 훨씬 더 독립적이고, 무결성이 더 높기 때문에 이렇게 말합니다.<br>

<img src="/images/typeB.png" alt="typeB.png">


이것이 type A보다 type B가 훨씬 더 객체 지향적인 디자인이다라고 말할 수 있는 것입니다.<br>

type B의 객체는 꽤 일관된 객체입니다. type A를 보게되면 계층화 되어있어 있습니다. 한 부분은 다른 곳으로 가고, 다른 한 부분은 또 다른곳으로 가니까요<br>

그래서 하나의 객체를 두 조각으로 펼쳐지게 만듭니다.<br>
```java 
public class User {  
    private String name;  
    User(String name) {this.name = name}  
    int getName() { return Integer.parseInt(name); }  
  }
```
```java
if(employees.contains(user.getName())){  
	/* pay a salary*/  
}  
```

한번 더 예제를 보겠습니다.

type A
```java
public class Box {  
    private int weight;  
    Box(int kg) {  
        this.weight = kg;  
    }  
    int getWeight() {  
        return weight;  
    }  
    public static void main(String[] args) {  
        int kg = 50;  
        Box box = new Box(kg);  
        int w = box.getWeight();  
        int lbs = (int) (w / 0.454);  
        System.out.printf("The weight is %d. %n", lbs);  
    }  
}
```

type B
```java
public class Box {  
    private int weight;  
    Box(int kg) {  
        this.weight = kg;  
    }  
    int getLbs() {  
        return (int) (this.weight / 0.454);  
    }  
    public static void main(String[] args) {  
        int kg = 50;  
        Box box = new Box(kg);  
        int lbs = box.getLbs();  
        System.out.printf("The weight is %d. %n", lbs);  
    }  
}
```

What happens if the Box decides to store the weight in pounds instead of kilograms? How will it know how many of its clients still assume that the weight is in kilograms

데이터를 가져오고 데이터 객체에 액세스 하는 것이 왜 큰 문제를 유발하는지 보여드리겠습니다.

위 코드들은 어떤 상자에 대해 이야기 하고 있습니다.<br>
type A를 보시면 어떤 상자에는 무게가 있고 무게는 kg단위이고 무게를 캡슐화 합니다.<br>
무게라고 불리우는 필드에 캡슐화 하면 getter가 무게를 반환합니다.<br>

그런 다음 사용자는 무게를 파운드 단위로 출력하려고 합니다.<br>

그래서 사용자가 하는 일은 w를 만들고 getWeight를 사용하여 상자에서 무게를 검색하는 것입니다.<br>
weight는 kg 단위이므로 파운드로 변환하고, 만들어진 무게를 파운드 단위로 출력합니다.<br>

type B를 보시면 kg단위로 캡슐화 되어있고, getLbs method가 있습니다. 객체 내부에서 변환을 수행하면 사람들이 이것을 가져가 사용합니다.<br>

type A 코드는 무엇이 문제일까요?<br>
`type A = BAD, type B = GOOD`<br>

차이점이 뭘까요? type A에서는 어떻게 알 수 있을까요? 내 상자의 무게가 kg단위인지 어떻게 알 수 있을까요?<br>

두 사람이 이 코드를 작성하고 있다고 상상해보세요.<br>
한 사람이 type A의 Box 객체를 만들고 있고, 다른 한 사람이 객체를 사용하는 method를 만들고 있습니다. 첫 번째 개발자가 가중치를 내부에 포함되도록 변경하여 구현했습니다. (kg -> lbs)<br>
이렇게 되면 type A의 weight 데이터의 의미는 그냥 정수이고, 킬로그램도 아니고, 파운드도 아니며 그냥 정수가 되어버립니다. 그 안에 무엇이 들어있는지 아무런 정보도 없는 데이터가 됐습니다. 왜냐구요? 사용자가 유일하게 가진 정보, 힌트는 텍스트로 되어있는 무게(weight) 뿐입니다.<br>
이제 int가 온도도 아니고, id값도 아니고 무게라는걸 알았습니다.ㅋㅋㅋ<br>

그렇다면 어떤 무게일까요? 아마 kg단위일 겁니다. 처음에는 운이 좋아서 kg라고 작성되어있었고, 아마도 kg을 의미했을 것이라고 생각하는 겁니다. 그래서 사용자인 두 번째 개발자는 기발한 추측력을 가지고 이런 가정을 통해 이런 식으로 구현했습니다.<br>

``` java
	int kg = 50;  
	Box box = new Box(kg);  
	int w = box.getWeight();  
	int lbs = (int) (w / 0.454);  
	System.out.printf("The weight is %d. %n", lbs);  
```

내일 첫 번째 객체 개발자가 코드에 와서 "아 나는 kg단위로 저장하는 것을 좋아하지 않아, 그러니 g(그램)으로 저장하면 어떨까요?" 라고 말하면 어떻게 될까요? 그럼 gram이 더 좋으니 * 1000을 할 것입니다.<br>
이렇게 되면 어떻게 무게를 사용하는 코드들의 모든 위치를 찾고, 살펴보고, kg에서 g으로 수정할 수 있을까요? 과연 몇군데나 있을까요?<br>

위 예시 코드에서는 단 몇 줄의 코드만 존재하기 때문에 금방 찾을 수 있습니다.<br>

하지만 대규모 프로그램이라고 상상해보세요. 추가 의미가 없는 getter를 통해 필드를 노출하는 class가 있고, 이걸 사용하고 있는 다른 애플리케이션들이 있습니다. 그렇다면 원래 Box class에서 이 weight의 의미를 변경하면 어떻게 될까요? 얼마나 많은 버그가 발생하게 될까요? 컴파일러가 다 버그를 찾아줄까요? 아마 컴파일러는 이게 kg을 의미하지 않고 g을 저장한다는 사실을 모를겁니다.<br>

요점은 이겁니다. getWeight() getter의 방법은 객체에서 필드가 탈출했다는 겁니다.<br>
이 데이터는 순수하고 주소나 정보가 없으며 단지 정수일 뿐입니다. 운이 좋게도 우리는 그게 무게라고 알고 있습니다. 하지만 이건 정보가 너무 적습니다. 너무 제한적입니다.<br>

type B도 따지고 본다면 좋지도 않고 나쁘지도 않습니다. type B 또한 데이터가 밖으로 나가고, 정보도 밖으로 나가고, 데이터도 튀어나옵니다. 하지만 getLbs() method는 나쁘지 않습니다.<br>
getLbs() method는 method 이름에 lbs라는 단위가 고정되어있어 바뀔 수 없기 때문입니다. 그렇지만 여전히 문제는 있습니다. 무게가 총 중량일 수도 있고, 순 중량일 수도 있습니다. 객체는 그 값이 어떤 무게를 나타내는지 모릅니다. 따라서 무게가 의미하는 바를 가져와서 class로 옮기고, class에서 가중치를 출력하도록 하는 것이 훨씬 좋습니다. 그리고 class가 이 출력 기능을 캡슐화 하도록 합니다.<br>

데이터가 class 밖으로 튀어나오도록 하지 마세요.<br>

데이터가 밖으로 나가는 순간 우리는 어디로 가는지 알지 못하고 통제력을 잃게 됩니다. 누가 우리 데이터를 사용하고 있나요? 그들이 어떻게 우리 데이터를 사용합니까? 그들이 우리 데이터에 대해 무엇을 알고 있나요? 그들이 우리 데이터에 대해 얼마나 많은 가정을 했나요?<br>

데이터가 밖으로 나가는 순간 우리는 그것을 통제할 수 없습니다.<br>

캡슐화는 이러한 문제를 해결하기 위해 데이터를 적절히 처리하고 통제하는 기능을 제공합니다. 개발자는 데이터를 처리하는 방법을 결정할 수 있지만, 외부에서는 해당 데이터에 직접 접근할 수 없도록 막을 수 있습니다. 이는 데이터를 캡슐화하여 외부로부터 보호하고, 데이터에 대한 접근을 제어함으로써 데이터의 안전성과 무결성을 유지할 수 있게 합니다.<br>

따라서 getter를 통해 데이터에 접근하거나, 데이터를 출력하거나, 데이터베이스에 저장하는 등의 작업을 수행할 때에도 데이터의 캡슐화를 유지하여 데이터의 안전성을 보장하는 것이 중요합니다. 이는 개발자가 데이터를 적절하게 다루고, 외부로부터 보호하여 데이터의 무결성을 유지하는데 도움이 됩니다.<br>

따라서 캡슐화는 데이터가 원시 형식의 개체를 탈출시키지 않는다는 것을 의미합니다.<br>

원시 형식의 데이터? type B 보시면 boolean이 밖으로 탈출하네요?<br>
하지만 이것은 내 데이터가 아닙니다.<br>
나는 boolean을 저장하지 않습니다. boolean 데이터에 의존하지 않습니다.<br>
이것은 제가 직원인지 아닌지를 나타내는 임시 데이터일 뿐입니다.<br>
이 데이터는 여러분께 제공이 가능합니다.<br>
isEmployee()이므로 쉽게 이해할 수 있고, 모두가 그 의미를 이해할 것입니다.<br>

그렇다면 이런 질문이 있을 수 있습니다.<br>

[질문 & 답변]
왜 상자 자체가 값 변환을 담당해야하는가?<br>
위에서 설명 했듯이 Box는 데이터를 보호하고 싶어합니다. 이것은 데이터의 무결성을 원하는 것입니다. 당신이 실수할 것이라고 예상하고 있기 때문에 내 필드 값 변환을 당신이 하는 것을 원하지 않습니다. 내가 weight를 밖으로 제공하면, 이게 나는 순수한 kg인지 총 kg인지 알 수 없습니다. 또 kg인지 lbs인지 알 수 없습니다. 당신은 weight에 대해 많은 것을 알고 있지 못합니다. 그리고 나는 당신이 알기를 원하지 않습니다. 왜냐면 weight에 대한 의미가 바뀌어 나중에 당신을 찾아가서 "이거 바꿔주세요" 라고 묻고 싶지 않기 때문입니다.<br>
<br>
때로는 객체 내부에서의 구현이 비효율성을 초래할 수 있습니다.<br>
사람들은 Box에 모든 것을 캡슐화 하는 대신 데이터를 그냥 보내주고, 사용자가 데이터를 처리하도록 하는 것이 더 효과적일 것이라고 말합니다. 맞습니다 사용자가 구현하는 것보다 객체 내부에서의 구현의 코드 길이가 더 길어 보일 수 있습니다. 객체에 많은 것을 넣고 객체가 자체 데이터에 대해 너무 스마트하게 만들면 코드가 더욱 길어 보일 수 있습니다.<br>
<br>
하지만 괜찮습니다. 이 문제는 데이터가 밖으로 탈출해서 애플리케이션들의 알 수 없는 위치로 이동되는 문제보다 작습니다.<br>
<br>
위에서 말했듯이 getLbs()도 나쁘지 않지만 좋은 코드는 아닙니다.<br>
왜냐면 무게의 총량인지, 순수량인지? 모릅니다 그래서 총량을 말할때는 getGrossLbs라고 말해야 합니다. 따라서 method의 길이를 더 길게 만들어야 하며, 사용 방법에 대한 API 문서를 작성해야 할 수도 있습니다. 이것이 대부분 우리가 보던 JAVA 프레임워크가 수행하는 작업입니다.<br>

spring framework에 대한 많은 농담이 있습니다.<br>

> spring framework의 method 이름이 너무 길다.

왜냐면 이 method 이름에서 무슨 일이 일어나고 있는지를 설명해야 하기 때문입니다.<br>
method 이름이 너무 길면 디자인이 좋지 않다는 증거입니다. 긴 변수 이름은 잘못된 디자인을 나타냅니다.<br>

예를 들어 kg 무게를 말해야 하는 경우입니다. kgWeight를 사용하면 복합명사가 됩니다. kg / weight 그리고 더 큰 이름을 짓기 위해 단어를 연결해야 한다면 디자인에 문제가 있는 것입니다. 이런 상황이라면 일단 그 자리에서 멈추고 작은 단위로 추상화 합니다. 항상 필드, 속성이 하나의 단일 명사여야 하고, method 내부의 변수여야 합니다. 확장자도 필요 없고, 다른 명사의 연결도 없는 그저 단일 명사입니다. method에 대해서도 마찬가지입니다. 명사이거나 동사여야 합니다. 두 개의 동사? 명사? 또는 명사와 동사? 안됩니다.<br>

몇 가지 예외가 있을 수 있는데 그게 isEmployee 입니다. java에서는 ?를 이름에 넣을 수 없기 때문입니다. isEmployee는 뒤에 ?가 붙을 것이라고 예상이 됩니다 "직원입니까?" 그냥 물어보는 것입니다.<br>

그리고 객체 내의 적당한 method의 개수는 3 ~ 5가 되지 않을까 합니다. 이 이상으로 넘어가게 된다면 왜 그렇게 많이 생성해야 하는지 생각해 보아야합니다.<br>

이를 해결하기 위해선 일단 단계적으로 해결해 나가야 합니다.
1. getter가 첫 번째 문제이므로 getter를 먼저 제거합니다.
   이렇게 된다면 많은 기능들이 객체로 이동되므로 객체의 볼륨이 커지게 됩니다
2. 이제 그럼 객체에 수많은 method가 들어있습니다. 이제 이 문제를 해결하게 됩니다.
   추상적인 개념들을 추출하여 Employee에서 했던 것처럼 여기서 5개의 method를 추출하여 추상화 고용이라고 부를 수 있습니다. 그렇다면 5개의 method를 제거하고 하나의 메서드와 5개의 method를 가진 또 다른 클래스를 가지게 되었습니다. 그런다음 또 다른 추상화를 추출하고..반복하면 됩니다.

이렇게 하면 매우 간결하고 견고한 class가 생성 됩니다.


# 4. 해결책

그래서 해결책이 무엇입니까? 어떻게 해결해야하나요?

첫 번째 해결책은 공식적으로 말하기, 묻지 않기라고 하는 것입니다.<br>
이게 무슨 의미냐면 내 내부 정보를 알려 달라고 요청하지 않는다는 의미입니다.<br>
이것이 바로 캡슐화 입니다. 그러니 내게 와서 무엇을 해야 할지 말해주세요.<br>

``` java
public class BookDTO {  
    private int id;  
    private String title;  
    private String author;  
  
    BookDTO(int id, String title, String author) {  
        this.id = id;  
        this.title = title;  
        this.author = author;  
    }
    void print(){/* ... */}
}

class JsonApi {
	Book getById(int id) {/* ... */}
}

BookDTO dto = api.getById(42);
dto.print();

```

BookDTO에서 여러가지 필드를 가지고 있고, 몇가지 getter가 있습니다. 여기서get ~, get~ 등을 전 부 제거합니다. 이제 남은건 print() method 뿐입니다. 이제 이 BookDTO에게 무엇을 해야 할지 지시하고, 직접 출력하라고 하면 책이 출력 되는 코드를 볼 수 있습니다.<br>

항상 이렇게 코드를 디자인 하지는 않을 것입니다. 때로는 쉽지 않으며, 때로는 구성 방법에 문제가 있을 수 있습니다. 하지만 getter를 사용하지 않도록 강요한다면, 이와 같은 방향으로 생각하기 시작할 것입니다.<br>
> 왜 내가 내 데이터를 제공해야 하는지, 왜 내가 이것을 그들에게 주고 그들이 그것을 사용하는지, 왜 그들이 원하는 것을 할 수 없는 지에 대해 생각하게 됩니다.

위 코드의 목적은 화면에 출력하여 사용자에게 보여주기 위한 것처럼 보여집니다. BookDTO을 가르치고 BookDTO를 더 스마트하게 만들면 책이 저절로 출력하게 됩니다.<br>

이렇게 하면 객체는 더 커지고 확실히 뚱뚱해질 것입니다.<br>

이것이 하나의 접근 방식입니다. 기능을 객체로 옮기는 것

```java
public class BookDTO {  
    private int id;  
    private String title;  
    private String author;  
  
    BookDTO(int id, String title, String author) {  
        this.id = id;  
        this.title = title;  
        this.author = author;  
    }  
  
    int id() {  
        return this.id;  
    }  
  
    String title() {  
        return this.title;  
    }  
  
    String author() {  
        return this.author;  
    }  
  }
  
class JsonApi {  
	Book getById(int id) {/*... */}  
}  
	Book book = api.getById(42);  
	println(book.title);  
	println(book.author);
```

또 다른 접근법은 단지 단신에게 "만약 당신이"라는 말을 없애라고 제안하는 것입니다.

이 방법은 좋은 방법은 아니지만 괜찮습니다. 무엇을 해야 할 지 모르겠고, 여전히 데이터를 제공하고 싶다면 최소한 getter를 사용하여 getId, getAuthor, getTitle 이렇게 사용하지 말고, id(), author(), title()와 같이 호출하세요.<br>

이 경우는 내부 구현을 변경할 수 있는 방향으로 나아가고 있기 때문에 여러분은 사용자에게 이것이 실제로 getter가 아니라서 내 내부의 필드를 제공하는 것이 아닙니다. 단지 일시적일 수도 있고 getter처럼 구현되어 있을 뿐입니다.<br>

세 번째 방법은 getter를 사용하는대신 그냥 field를 public으로 만드세요.<br>
getter는 실제로 캡슐화하고 있다고 말하지만 당신은 아무것도 캡슐화 하고 있지 않기 때문입니다.<br>
당신은 그냥 모든 사람과 당신을 오해하게 만들고 있으며 실제로 무언가를 보호하고 있다고 생각하게 만들기 때문입니다. 그렇기 때문에 그냥 필드를 공개하세요.<br>
필드를 공개하는 것이 getter보다 낫습니다. 왜냐면 당신은 그 누구도 속이지 않고, '이봐, 이건 단지 멍청한 DTO이고, 나는 완전이 anemic하며 그냥 단순한 기록일 뿐이야'라고 말하고 있기 때문입니다.<br>

행동이 있는 척을 하는 것이 아니다. 난 아무 행동 어떠한 행동도 없고 모두 나에게서 가져갈 수 있다. 그리니 나를 컨테이너처럼 활용하세요 내가 생각할 것이라고 기대하지도 마세요.<br>

추천하는 3개의 서적입니다<br>
- GetterEradicator by Martin Fowler<br>
- https://martinfowler.com/bliki/TellDontAsk.html by Martin Fowler<br>
- Wht getter and setter methods are evel by Allen Holub<br>

<hr>

[질문 & 답변]
사용자 객체에 너무 많은 동작을 도입하면 그게 보호되는 객체가 맞을까요??
isEmployee를 사용하여 저를 고용하고 급여를 계산한 다음 다음 급여를 인상하고 고용하고 해고하고 다른 부서로 옮기는 등의 작업을 한다고 가정해 보겠습니다.
이렇다고 한다면 하나의 객체에 대해 매우 많은 method가 있을 것 입니다.

이를 피하기 위해 우리는 그것을 원하지 않습니다.
우리는 첫 번째 하나의 method에 10개의 method가 들어있다면 디자인을 잘못하고 있는 것입니다.
따라서 이 가드를 피하기 위해서 하나, 둘, 세가지 방법이 필요합니다.
더 큰 추상화를 더 작은 추상화로 분해합니다.

예를 들어 여기서 User class는 큰 추상화입니다.
고용 추상화를 하나 더 만들고 아래와 같이 사용해봅니다.
```java
User user = new User();
user.employment().isEmployee();
```

사용자에게 가서 사용자의 고용 상태를 추상화 하는 또다른 객체를 만들어달라고 합니다.
그런 다음 이 고용상태에서 당신이 고용주이신가요? 해고해 주세요 라고 물을 수 있습니다.
자신을 고용하거나 다른 부서로 옮기면 또 다른 일이 있을 것입니다.

사용자라고 말하고 다음 급여에서 예를 들어 급여 인상한다고하면 또 다른 추상화이므로 사용자로부터 빼앗아야 합니다.

```java
User user = new User();
user.employment().isEmployee();
user.salary().raise(+5)
```

---

또 다른 질문은 사용자에 대한 정보에 따라 달라지는 기능이 너무 많다는 것입니다.
물론 다루는 내용, 도메인에 따라 다르므로 항상 추상화를 직접 살펴보고 class가 얼마나 커질지 결정합니다.
꽤 작은 class가 있고 추상화도 아주 작습니다. 그렇다고 한다면 하나의 class로 충분합니다. 때로는 20개, 50개의 class 50개의 다른 추상화가 있을 수도 있습니다.
왜냐하면 우리 주변의 세계 real world, 여러분이 시도하려는 세계가 있기 때문입니다.
코드의 모델이 꽤 크고 커질 것이므로 한 명의 사용자와 많은 method가 아니라 많은 객체가 필요합니다.

또 다른 방법으로는 Employee의 객체를 만들고 추상화하는 것입니다.
```java
new Employee(new User(60)).isEmployee()
```

이 방법은 정확히 Employee와 User가 무엇을 교환할지, 어떤 종류의 정보 등을 정의하고 사용해야 합니다.
두 가지 방법 중에는 아래의 방법이 어렵고, 위에 있는 방법이 일반 적으로 구현하기 쉽습니다.

---

[질문&답변]
여러개의 동작을 혼합해서 사용해도 괜찮을까요?
예를들어 하나 getter를 사용하면 다음과 같이 상호 작용하는 라이브러리를 만들고 있습니다. Telegram Bot API가 있고, 메세지를 받아오는 객체가 있습니다. 들어오는 메세지의 텍스트를 분석해야하는 경우 텍스트 가져오기를 스스로 정의하도록 할 수 있나요?
-> 왜 그런 일이 발생하는지, 데이터를 제공하기 위해 객체가 필요한 이유를 먼저 생각해보세요

---

### 참고한 글
[getter 쓰지 말라고만 하고 가버리면 어떡해요](https://velog.io/@backfox/getter-%EC%93%B0%EC%A7%80-%EB%A7%90%EB%9D%BC%EA%B3%A0%EB%A7%8C-%ED%95%98%EA%B3%A0-%EA%B0%80%EB%B2%84%EB%A6%AC%EB%A9%B4-%EC%96%B4%EB%96%A1%ED%95%B4%EC%9A%94) <br>
[setter 쓰지 말라고만 하고 가버리면 어떡해요](https://velog.io/@backfox/setter-%EC%93%B0%EC%A7%80-%EB%A7%90%EB%9D%BC%EA%B3%A0%EB%A7%8C-%ED%95%98%EA%B3%A0-%EA%B0%80%EB%B2%84%EB%A6%AC%EB%A9%B4-%EC%96%B4%EB%96%A1%ED%95%B4%EC%9A%94) <br>
[Why getter and setter methods are evil](/files/WhyGetterAndSetterMethodsAreEvil.pdf)<br>
[Object-Oriented Analysis and Design with Applications 책 by Grady Booch]()<br>
[Object Thinking 책 by David West]()<br>
[yegor256의 블로그](https://www.yegor256.com/)<br>
[Why Getters-and-Setters Is An Anti-Pattern? (webinar #4)](https://www.youtube.com/live/WSgP85kr6eU?si=QYOx7vwIe9N-Hmyw)<br>
