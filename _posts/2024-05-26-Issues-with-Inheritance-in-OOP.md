---
title: Issues with Inheritance in Object-Oriented Programming (OOP)
permalink: /posts/2024/04/Issues-with-inheritance-in-OOP/
date: 2024-05-26
tags:
  - OOP
  - Inheritance
  - 상속
  - 다형성
  - Polymorphism
  - 객체 지향 프로그래밍
---

객체지향 프로그래밍 (Object-Oriented Programming, OOP)에서 가장 이슈가 되는 점 중 하나는 상속입니다. <br>
상속이라는 개념이 초래하는 불필요한 복잡성 때문에 여러 클래스가 다른 클래스를 상속받아 사슬 같은 구조를 형성하고, 이는 이해하고 유지하며 작업하기 어렵습니다. 이런 상황을 개선하는 방법 가운데 하나는 다형성을 이용하는 것입니다.

다형성부터 시작해 보겠습니다.<br>
다형성과 상속은 서로 밀접하게 연관되어 있습니다.

# OOP (Object-Oriented-Programing)의 문제 Inheritance(상속)편

## 다형성 (Polymorphism)

### 다형성이란?
다형성은 객체지향 프로그래밍의 핵심 원칙 중 하나로, '많은 형태'라는 의미입니다. 같은 코드라도 실제로 연계된 객체에 따라 다른 행동을 보이게 하는 개념을 말합니다. 즉, 다형성은 한 객체가 여러 타입으로 표현될 수 있는 능력입니다. 이는 부모 클래스 또는 인터페이스를 상속 또는 구현하는 여러 클래스의 객체가 같은 메서드 호출에 대해 각각 다르게 반응하도록 합니다.
```java
interface Vehicle {
    void run();
}

class Car implements Vehicle {
    @Override
    public void run() {
        System.out.println("The car runs on roads.");
    }
}

class Plane implements Vehicle {
    @Override
    public void run() {
        System.out.println("The plane flies in the sky.");
    }
}

class Boat implements Vehicle {
    @Override
    public void run() {
        System.out.println("The boat sails in the sea.");
    }
}
```
예를 들어, 위의 코드에서 Vehicle 인터페이스를 구현하는 각각의 클래스 (Car, Plane, Boat)의 객체는 run() 메서드를 호출할 때 고유의 동작을 수행합니다. 이는 다형성의 한 예입니다.

#### 다형성의 장점
- **유연성**: 다형성은 코드를 더욱 유연하게 만들어주며, 객체 간의 결합도를 줄이고 의존성을 최소화하는 데 도움이 됩니다.
- **확장성**: 클래스나 메서드의 변화가 다른 클래스나 메서드에 영향을 덜 미치도록 돕습니다. 즉, 기존 코드를 변경하지 않고도 새로운 기능을 추가할 수 있습니다.
- **재사용성**: 동일한 로직을 새로 작성할 필요 없이, 상위 클래스의 메서드를 하위 클래스에서 재정의함으로써 좀 더 간결하고 재사용 가능한 코드를 작성할 수 있습니다.
#### 다형성의 단점
- **오버라이딩 오류**: 메서드를 올바르게 오버라이딩하지 않았을 경우, 의도하지 않은 결과를 초래할 수 있습니다.
- **디버깅 어려움**: 실행 시간에 결정되는 동적 바인딩 특성은 코드를 디버깅하기 어렵게 만들 수 있습니다. 어떤 메서드를 호출할지 런타임에 결정되므로 코드의 실행 흐름을 추적하기가 더 어려울 수 있습니다.

다형성의 특징을 잘 도출하고 재사용성과 유연성을 극대화하는 강력한 예다는 것을 보여주기 위해, 'Fruit'이라는 부모 클래스를 상속받는 여러 자식 클래스 ('Apple', 'Banana', 'Orange')의 예를 들어봅시다.

```java

abstract class Fruit {
    abstract String taste();
}

class Apple extends Fruit {
    @Override
    String taste() {
        return "Sweet and a bit tart";
    }
}

class Banana extends Fruit {
    @Override
    String taste() {
        return "Sweet and creamy";
    }
}

class Orange extends Fruit {
    @Override
    String taste() {
        return "Citrusy and tangy";
    }
}

public class Main {
    public static void describeTaste(Fruit fruit) {
        System.out.println(fruit.taste());
    }

    public static void main(String[] args) {
        Fruit[] fruits = {new Apple(), new Banana(), new Orange()};
        for (Fruit fruit : fruits) {
            describeTaste(fruit);
        }
    }
}

```
위 코드에서 `describe_taste` 함수는 `Fruit` 타입의 객체를 인자로 받으며, 실제로는 `Apple`, `Banana`, `Orange` 객체를 처리합니다. 각 객체는 자신의 `taste` 메서드를 구현하고 있으므로, 다형성을 통해 다양한 객체를 일관되게 다룰 수 있습니다.



## 리스코프 치환 원칙 (Liskov Substitution Principle)

상속의 문제를 이해하려면, 먼저 1987년에 바바라 리스코프(Barbara Liskov)에 의해 제시된 "리스코프 치환 원칙"을 알아야 합니다.

리스코프 치환 원칙의 핵심은 "서브타입은 그것의 기본 타입을 대체하기 위해 사용될 수 있어야 한다"입니다. 이는 쉽게 풀어서 설명하자면, 코드에서 특정 객체 타입 (예를 들어, Fruit)을 필요로 할 때, 그 타입의 어떠한 하위 클래스 (예를 들어, Apple)를 사용하여도 프로그램이 정상적으로 작동해야 한다는 의미입니다.


```java
abstract class Fruit {
    abstract void eat();
}

class Apple extends Fruit {
    @Override
    void eat() {
        System.out.println("Eating an apple");
    }
}

class Banana extends Fruit {
    @Override
    void eat() {
        System.out.println("Eating a banana");
    }
}

public class Main {
    public static void eatFruit(Fruit fruit) {
        fruit.eat();
    }

    public static void main(String[] args) {
        Fruit apple = new Apple();
        Fruit banana = new Banana();
        
        eatFruit(apple);  // Eating an apple
        eatFruit(banana);  // Eating a banana
    }
}


```
위 코드에서 `eat_fruit` 함수는 `Fruit` 타입의 객체를 인자로 받아 `eat` 메서드를 호출합니다. `Apple`과 `Banana` 클래스는 각각 `eat` 메서드를 구현하고 있으므로, `eat_fruit` 함수는 문제없이 두 객체를 모두 처리할 수 있습니다.  즉, 모든 사과는 과일일 수 있지만, 모든 과일이 사과는 아니라는 개념을 프로그래밍에 적용한 것입니다.<br>

이 원칙은 로버트 마틴(Robert Martin)의 "SOLID" 원칙에는 리스코프 치환 원칙(L)이 포함되어 있습니다. <br> 
이 원칙에 따르면, 더 구체적인 타입을 요구하는 모든 함수는 그것의 상위 타입을 인수로 받을 수 있어야 한다는 것입니다.<br> 
예를 들어, 함수가 과일을 받도록 설계되었다면 사과, 오렌지, 배 등 과일의 일종을 받더라도 문제없이 작동해야 합니다.<br> 
만일 그렇지 않다면, 클래스들의 계층 구조가 복잡해지고, 상속이 야기하는 복잡성에 대한 문제가 더 심화될 수 있습니다.<br>


## 무의미한 상속

객체지향 프로그래밍의 단점 중 하나는 무의미한 상속 구조입니다. <br>
일부 클래스가 다른 클래스를 상속받고, 그 클래스가 또 다른 클래스를 상속받는 식으로 상속의 사슬이 이어지면 이해하기 어렵고 유지하기 힘든 코드가 됩니다.<br>
이런 무의미한 상속은 객체지향 프로그래밍의 장점을 해치는 요인 중 하나입니다.



```java
abstract class Animal {
    abstract String sound();
}

class Mammal extends Animal {
    @Override
    String sound() {
        return "Generic mammal sound";
    }
}

class Dog extends Mammal {
    @Override
    String sound() {
        return "Bark";
    }
}

class Cat extends Mammal {
    @Override
    String sound() {
        return "Meow";
    }
}

public class Main {
    public static void animalSound(Animal animal) {
        System.out.println(animal.sound());
    }

    public static void main(String[] args) {
        Animal dog = new Dog();
        Animal cat = new Cat();
        
        animalSound(dog);  // Bark
        animalSound(cat);  // Meow
    }
}


```


위 코드에서 `Mammal` 클래스는 실제로 필요한 추가 기능을 제공하지 않으면서 단순히 상속의 중간 계층 역할만 합니다. 이런 경우, `Dog`와 `Cat` 클래스는 바로 `Animal` 클래스를 상속받아야 상속 구조가 간결해지고 이해하기 쉬워집니다.

```java
abstract class Animal {
    abstract String sound();
}

class Dog extends Animal {
    @Override
    String sound() {
        return "Bark";
    }
}

class Cat extends Animal {
    @Override
    String sound() {
        return "Meow";
    }
}

public class Main {
    public static void animalSound(Animal animal) {
        System.out.println(animal.sound());
    }

    public static void main(String[] args) {
        Animal dog = new Dog();
        Animal cat = new Cat();
        
        animalSound(dog);  // Bark
        animalSound(cat);  // Meow
    }
}

```

이렇게 무의미한 상속 구조를 제거하면 코드의 가독성과 유지보수성이 향상됩니다. 객체지향 프로그래밍의 원칙을 준수하면서도 상속의 복잡성을 줄이는 방향으로 코드를 작성하는 것이 중요합니다.



상속은 객체 지향 프로그래밍에서 중요한 개념이지만, 불필요한 복잡성을 초래할 수 있습니다. 다형성과 리스코프 치환 원칙을 잘 활용하면, 상속의 문제를 효과적으로 해결하고, 코드의 유연성과 재사용성을 높일 수 있습니다. 이를 통해 더 유지보수하기 쉽고 이해하기 쉬운 코드를 작성할 수 있습니다.

다형성은 특히 여러 타입의 객체를 일관되게 다룰 수 있도록 해주며, 리스코프 치환 원칙을 준수하면 상속 구조를 더 안전하고 예측 가능하게 만들 수 있습니다. 이 두 가지 개념을 잘 활용하면 객체 지향 프로그래밍의 강력한 장점을 최대한 활용할 수 있습니다.

> 이번 객체지향 프로그래밍의 사실과 오해를 공부하면서, 프로그래밍 언어가 엄격할수록 또는 작성하는 코드를 더 엄격하게 제어할수록 코드 유지 관리가 더 쉬워진다는 것을 깨달았습니다. 코드에 대한 제어가 강할수록 코드 작성 시 실수를 저지르기 어렵고, 컴파일 타임에 더 많은 실수가 포착되어 런타임에 발생하는 오류가 줄어들기 때문입니다.

이번 객체지향의 사실과 오해에 대해서 공부하면서 참고했던 자료 입니다.
- [The Pain of OOP, Lecture for BSc students in Innopolis University](https://apply.innopolis.university/en/bachelors/?ysclid=lt5vc5rxgk260918329).
- [Elegant Objects by Yegor Bugayenko](https://www.amazon.com/Yegor-Bugayenko/e/B01AM1QMDK/ref=dp_byline_cont_book)
- [getter 쓰지 말라고만 하고 가버리면 어떡해요](https://velog.io/@backfox/getter-%EC%93%B0%EC%A7%80-%EB%A7%90%EB%9D%BC%EA%B3%A0%EB%A7%8C-%ED%95%98%EA%B3%A0-%EA%B0%80%EB%B2%84%EB%A6%AC%EB%A9%B4-%EC%96%B4%EB%96%A1%ED%95%B4%EC%9A%94)
- [setter 쓰지 말라고만 하고 가버리면 어떡해요](https://velog.io/@backfox/setter-%EC%93%B0%EC%A7%80-%EB%A7%90%EB%9D%BC%EA%B3%A0%EB%A7%8C-%ED%95%98%EA%B3%A0-%EA%B0%80%EB%B2%84%EB%A6%AC%EB%A9%B4-%EC%96%B4%EB%96%A1%ED%95%B4%EC%9A%94)

이번에 조영호님의 [Object](https://product.kyobobook.co.kr/detail/S000001766367)와 [객체지향의 사실과 오해](https://product.kyobobook.co.kr/detail/S000001628109)를 추가로 학습하고 전반적인 객체지향 프로그래밍 포스트 내용을 정리할 예정입니다.