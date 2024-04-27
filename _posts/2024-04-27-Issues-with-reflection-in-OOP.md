---
title: Issues with Setters in Object-Oriented Programming (OOP)
permalink: /posts/2024/04/Issues-with-setter-in-OOP/
date: 2024-04-10
tags:
  - OOP
  - Reflection
  - Annotation
  - side effect
  - 객체 지향 프로그래밍
---

객체 지향 프로그래밍에서 리플렉션 이 기능을 사용하는 것이 왜 나쁜지 설명하려고 합니다. <br>

왜 이 reflection이 기술적으로 프로그램에 그렇게 좋지 않은지, 그리고 마지막으로 왜 그것이 개념적으로, 철학적으로 말하면 좋은 생각이 아닌지 설명하려고 노력할 것입니다.


# OOP (Object-Oriented-Programing)의 문제 Reflection편

## Java의 리플렉션 API? 


### 타입 캐스팅이란?
**[type A]**
```java
int sizeOf(Iterable items) {
  int size = 0;
  if (items instanceof Collection) {
    size = ((Collection) items).size();
  } else {
    for (Object item : items) {
      ++size;
    }
  }
  return size;
}

```

**[type B]**
```java
int sizeOf(Iterable items) {
  int size = 0;
  for (Object item : items) {
    ++size;
  }
  return size;
}

int sizeOf(Collection items) {
  return items.size();
}
```

typeA의 코드에서 Collection을 하위 클래스인 iterable이 캐스팅하여 사용하고 있습니다.

> Reflection에 해당하는부분
```java
  if (items instanceof Collection) {
    size = ((Collection) items).size();
  } 
```


이 코드는 명시적 타입 변환을 사용하여 Collection 인스턴스 여부에 따라 'size()' 메소드 호출을 가능하게 합니다.<br>
그러나 이렇게 reflection을 사용하면 몇 가지 이슈를 부르게 됩니다.

기술적인 문제
- 성능: 위 코드와 같이 Reflection을 사용하면 Java Virtual Machine 이(JVM) 직접 메서드를 호출하는 것보다 상당히 느릴 수 있습니다. 간단한 사이즈 계산에 대해서 단순 반복 작업이 더 효율적일 수 있습니다.
- 안정성: 명시적 형변환을 사용하면 ClassCastException을 발생시킬 가능성이 있습니다. 이 경우 컴파일 시 오류를 탐지하지 못하고 런타임시 오류를 발생시키는 경우가 됩니다. 이는 코드의 안정성을 저해합니다.


instenceof 키워드는 객체가 어떤 클래스의 인스턴스인지 확인하는 키워드입니다. <br>
해당 키워드는 Reflection에 해당한다고 볼 수 있습니다.

>왜 Reflection에 해당한다고 보냐면, instanceof 키워드는 런타임에 객체의 타입을 확인하는 기능을 제공하며, 객체 지향 프로그래밍에서 유형 처리를 다루는 방법 중 하나입니다.
>
>1. 런타임 타입 확인: instanceof는 객체의 인스턴스를 확인하여 프로그램이 런타임에 동적으로 작동하는 것을 가능하게 합니다. 이는 Reflection의 핵심 원리인 런타임에 대한 메타정보 액세스와 공통적인 특성을 공유합니다.
>
>2. 동적 메서드 호출: instanceof가 true를 반환하면 그 다음에 일반적으로 명시적 타입 캐스팅이 이루어집니다. 이 시점에서 메서드 호출은 실제 런타임 타입에 따라 동적으로 결정됩니다. 이 접근 방식은 Reflection 통해 일반적으로 수행되는 런타임 메서드 호출과 유사합니다.

개념적인 문제
- Reflection은 우리가 코드의 상세한 구현 부분에 대한 정보를 숨기려는 캡슐화 원칙에 위배됩니다. 이 원칙은 코드의 각 부분이 가능한 독립적으로 작동하도록 보장하여 유지 보수성과 코드의 가독성을 높이는데 중요합니다. 따라서 명시적인 타입 변환을 사용하는 대신, 해당 메소드를 오버로드하면 간편하게 처리할 수 있습니다. 이것이 가장 권장되는 방법인데, 그렇게 하면 메서드 오버로드을 사용하여 컴파일 타임에 맞는 메서드를 호출 할 수 있기 때문입니다.


다운캐스팅(downcasting)이란, 상위 클래스 타입의 객체를 하위 클래스 타입으로 변환하는 것을 말합니다. <br>
다운캐스팅은 객체의 원래 타입으로 되돌려서 원래 클래스의 메소드나 필드를 사용할 수 있게 하는 변환 방식입니다.<br>
> 그러나 이 작업은 주의가 필요합니다. 다운캐스팅은 반드시 안전하게 수행되어야 하는데, items가 실제로 Collection의 인스턴스가 아닌 경우 위 라인에서 ClassCastException이 발생할 수 있기 때문입니다.<br>

그래서 권장하는 방식은 메소드 오버로딩입니다. <br>


typeB코드를 보시면 메소드 오버로딩을 하고있습니다.<br>
오버로딩은 하나의 메소드 이름이 있지만 여러 구현으로 이를 오버로드한다는 의미입니다.

> 메서드 오버로딩은 한 클래스 내에서 같은 이름을 가지지만 파라미터 형태가 다른 여러 개의 메서드가 존재하는 것을 의미합니다. 컴파일러는 메서드의 시그니처(이름과 파라미터 형태)를 보고 어떤 메서드를 호출할지 결정합니다.


코드에서  sizeOf 메서드를 호출할 때 JVM은, 인자로 넘어간 객체가 Collection 인스턴스인지 Iterable 인스턴스인지 확인합니다. 만약 인자가 Collection 인스턴스라면, Collection을 파라미터로 받는 sizeOf(Collection items) 메서드를 호출합니다.
```java
int sizeOf(Iterable items) {}
int sizeOf(Collection items) {}
```

이 메서드는 Collection 인터페이스의 size 메서드를 이용해 크기를 바로 반환할 수 있습니다.
``` java
int sizeOf(Collection items) {
  return items.size();
}
```
인자가 Collection이 아니라 Iterable 인스턴스라면, Iterable을 파라미터로 받는 sizeOf(Iterable items) 메서드를 호출합니다. 이 메서드는 Iterable 인터페이스의 iterator 메서드를 이용해 모든 요소를 순회하며, 전체 개수를 반환합니다.
```java
int sizeOf(Iterable items) {
  int size = 0;
  for (Object item : items) {
    ++size;
  }
  return size;
}
```


이렇게 오버로딩을 사용하면 컴파일러가 컴파일 시점에 정확한 메서드를 결정해야 하므로, 런타임에 동적으로 결정하게 되는 reflection을 피할 수 있습니다. 이로써 메서드 호출의 속도 향상과 코드의 안정성을 기대할 수 있습니다.

또한, 이런 형태는 전형적인 다형성의 예로, 동일한 작업에 대해 여러 형태의 입력을 처리할 수 있게 해줍니다. 이는 프로그래밍에서 중요한 객체 지향 원칙 중 하나인 개방-폐쇄 원칙(Open-Closed Principle)을 잘 따르는 방법이기도 합니다.
이 원칙은 "소프트웨어 구성요소(컴포넌트, 클래스, 모듈, 함수 등)는 확장에는 열려 있어야 하고, 수정에는 닫혀 있어야 한다"는 원칙입니다. 즉 새로운 타입의 파라미터를 처리하려면 새로운 메서드를 추가(확장)하면 되고 기존 코드는 변경할 필요가 없습니다.

결합이라는 개념은 한 코드 조각이 다른 코드 조각에 대해 너무 많이 알고 있으면 두 코드가 너무 많이 결합된다는 의미입니다.

방금 설명드린 문제를 조금 더 키워보겠습니다. 더 큰 규모로 말입니다.

위 예시 코드에서는 그다지 치명적이게 보이지 않습니다.

하지만 이 모든 인스턴스가 있는 곳에 수백 줄의 코드가 있다고 상상해 보세요.

1.  타입 안정성:
    - 다운캐스팅은 타입 체계를 우회하는 방법으로, 이로 인해 런타임에 타입 관련 오류가 발생할 수 있습니다. 예를 들어, 개발자가 잘못된 타입으로 다운캐스팅했다면, 이는 ClassCastException을 발생시킬 것입니다. 대규모 프로젝트에서는 이런 에러를 찾아내고 디버깅하는 데 많은 시간이 소요될 수 있습니다.

2. 가독성과 유지보수성 저하:
    - 다운캐스팅은 코드의 가독성을 저하시키고 유지 보수를 어렵게 만듭니다. 다운캐스팅이 발생하는 코드 부분에서는 개발자가 어떤 타입을 기대하고 있는지 명확하게 알 수 없으므로, 이해하고 수정하는 데 더 많은 시간과 노력이 필요합니다.

3. 설계 결함 표시:
    - 다운캐스팅은 종종 개체 지향 설계 원칙이 잘 지켜지지 않았음을 시사합니다. 설계가 잘 되어 있다면 다운캐스팅 없이도 각각의 클래스가 적절한 동작을 수행해야 합니다. 대규모 프로젝트에서는 이런 설계 결함이 프로젝트 전체의 품질에 영향을 미칠 수 있습니다.

다운캐스팅은 필요하거나 불가피한 경우에 사용되어야 하며, 사용할 때는 항상 주의가 필요합니다. 가능하다면, 다형성, 제네릭, 인터페이스 등의 다른 방법을 통해 이런 문제를 방지해야 합니다.

그러니 한 번에 여러 단계를 건너뛰어 다운캐스팅을 하려고 하지 마세요.

## 프록시와 리플렉션

Java에서 프록시는 특정 인터페이스를 구현하는 클래스를 동적으로 생성할 수 있게 해주는 특징입니다.<br> 
프록시 클래스의 모든 메서드 호출은 InvocationHandler를 통해 라우트되므로, 이를 통해 추가적인 동작을 수행할 수 있습니다(예: 메서드 호출 로깅, 접근 제어 등). 대부분의 프록시 생성은 리플렉션 API를 내부적으로 사용합니다.

``` java
interface Service { 
    void perform(); 
}

class DefaultService implements Service { 
    @Override 
    public void perform() { 
        System.out.println("Performing service...");
    } 
}

InvocationHandler handler = new InvocationHandler() { 
    private final Service service = new DefaultService();

    @Override 
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable { 
        System.out.println("Before " + method.getName());
        Object result = method.invoke(service, args); 
        System.out.println("After " + method.getName());
        return result; 
    } 
};

Service service = (Service) Proxy.newProxyInstance(Service.class.getClassLoader(), 
new Class<?>[] { Service.class }, handler);
service.perform();
```

위 코드에서 Proxy.newProxyInstance() 메서드는 리플렉션을 내부적으로 사용해 프록시 클래스를 생성하고 인스턴스화합니다. <br> 
메서드 호출을 인터셉트해서 콘솔에 로그를 출력하게 했습니다.<br>

그런데 프록시와 리플렉션을 사용하면 어떤 문제가 생길 수 있을까요? <br> 
다음과 같은 문제점들이 있다는 것을 고려해 볼 수 있습니다.<br>
- 타입 안전성(Type Safety) 문제: 리플렉션은 컴파일 타임에 체크되지 않습니다. 대상 객체의 메서드를 문자열로 호출하기 때문에, 존재하지 않는 메서드를 호출하거나 잘못된 타입의 인자를 전달하는 실수를 범하면 런타임 에러가 발생합니다.
- 성능 문제: 리플렉션 메서드는 직접 호출하는 것에 비해 상당한 오버헤드가 있습니다. 따라서 리플렉션은 성능이 중요한 상황에서는 피해야 합니다.
- 보안 문제: 리플렉션을 사용하면 접근 제어자를 무시하고 프라이빗 메서드나 필드에 접근할 수 있습니다. 이는 캡슐화를 깨뜨리고 객체의 상태를 예기치 않게 변경할 수 있으므로 보안 문제를 일으킬 수 있습니다.
- 복잡성 문제: 런타임에 객체의 타입을 조사하고 메서드를 호출하는 코드는 일반적인 코드에 비해 훨씬 이해하기 어렵습니다. 따라서 코드의 복잡성을 증가시키며, 유지보수성을 떨어뜨립니다.

즉, 프록시와 리플렉션은 효율적이고 안전한 코드를 작성하는데 있어서 여러 제약사항이 생기며, 잘못 사용될 경우 심각한 문제를 가져올 수 있기 때문에, 사용에 주의해야 합니다.<br>


## 애노테이션 처리와 리플렉션
애노테이션 처리는 리플렉션을 많이 사용하는 주요 영역중 하나입니다. 애노테이션이 어떤 클래스, 메소드, 필드에 적용되어 있는지 알아내고 그에 따른 동작을 수행하기 위한 용도로 리플렉션을 사용합니다.
```java
@Retention(RetentionPolicy.RUNTIME)
@interface CustomAnnotation {
}

@CustomAnnotation
public class AnnotatedClass {
}

Class<?> clazz = AnnotatedClass.class;
boolean isAnnotated = clazz.isAnnotationPresent(CustomAnnotation.class);
```
위 코드에서, AnnotatedClass는 @CustomAnnotation 애노테이션을 받고 있습니다. isAnnotationPresent 메소드를 통해 이 애노테이션이 적용되어 있는지 확인할 수 있고, 이러한 동작은 리플렉션을 사용하여 수행됩니다.

애노테이션과 리플렉션은 JVM 언어에서 매우 강력한 도구입니다. <br> 
아래는 애노테이션 처리와 리플렉션 그리고 그 관계에 대한 자세한 내용입니다. <br>
애노테이션(annotation)은 프로그램의 메타데이터를 분석하고 처리하기 위해 사용되는 도구입니다.<br>
코드에 애노테이션을 추가함으로써 코드의 구조와 시맨틱 정보를 제공할 수 있습니다. 주석에 비해 애노테이션이 더욱 유용한 이유는 JVM이나 프레임워크가 이를 해석하고 처리할 수 있기 때문입니다.<br>

리플렉션(reflection)은 프로그램이 실행 중인 동안에 자신의 구조를 파악하고 조작할 수 있게 합니다. <br>
예를 들어, 리플렉션을 사용하면 런타임에서 객체의 클래스를 알아내거나, 클래스의 필드와 메소드에 접근하거나, 애노테이션 정보를 확보하거나, 심지어는 새로운 객체를 생성하거나 메소드를 실행할 수도 있습니다.<br>
애노테이션 처리는 자연스럽게 리플렉션이 요구됩니다.<br>
애노테이션이 부여된 요소들을 검색하기 위한 메타데이터 조회 방법이 필요하며, 이 때문에 리플렉션은 애노테이션 처리의 핵심적인 부분입니다. 이는 애노테이션이 정의하는 프로그래밍 계약을 검사하고 유효성을 검증하거나, 특정 애노테이션이 적용된 모든 클래스 혹은 메소드를 검색하는 경우 등 다양한 상황에서 이용됩니다.<br>
하지만 리플렉션을 사용할 때 주의해야할 점이 있습니다. 리플렉션은 매우 유연하지만 오용 시 타입 안정성을 저해하고, 런타임에서 예상치 못한 오류를 발생시킬 수 있으므로 주의해야 합니다.<br>
또한 리플렉션을 잘못 사용하면 성능을 저하시킬 수 있으며, 이런 이유로 리플렉션은 필요한 경우에만 사용하고, 가능하면 런타임에 동적으로 코드를 조작하기 보다는 애노테이션과 같은 메타데이터를 사용해서 프로그램의 동작을 파악하는 것이 좋습니다.<br> 


그래서 리플렉션(Reflection)은 자바같은 객체 지향 프로그래밍 언어에서 객체의 내부 구조를 파악하고 조작할 수 있는 강력한 기능입니다. <br>
직접 코드를 만지지 않고도 클래스의 구조를 알아내거나, 메서드를 생성하고 실행할 수 있는 역동적인 프로그래밍을 가능하게 합니다. <br>=
하지만 리플렉션이 프로그램에 좋지 않은 영향을 끼칠 수 있습니다. <br>

- 성능 이슈: 리플렉션 연산은 일반적인 자바 연산에 비해 상당히 느립니다. 객체의 메서드를 직접 호출하는 것은 리플렉션을 이용해 동적으로 같은 메서드를 호출하는 것보다 더 빠릅니다.
- 보안 이슈: 리플렉션은 보통 클래스의 private나 protected 필드나 메서드에 접근하기 위해 사용됩니다. 이 점이 리플렉션 사용을 위험하게 만듭니다. 잘못 사용하면 보안상 문제를 일으킬 수 있습니다.
- 유지보수성 문제: 리플렉션을 사용하면 코드의 가독성이 떨어지고 디버깅이 어렵습니다. 코드를 읽는 데 추가적인 이해가 필요하고 오류 발생 시 어디서 문제가 발생했는지 찾기 어려울 수 있습니다.


또한, 리플렉션을 사용하는 것이 객체 지향 프로그래밍의 철학에 부합하지 않다고 생각합니다. <br>
객체 지향 개념에서는 데이터와 그 데이터를 처리하는 메서드가 같은 객체 안에 캡슐화되어야 합니다. 이런 캡슐화 원칙을 지키는 것이 코드의 구조를 명확하게 유지하고, 추후 변경이나 확장을 쉽게 만들어주는 객체 지향 프로그래밍의 핵심 원칙중 하나입니다. <br>
그런데 리플렉션은 이런 캡슐화 원칙을 위반합니다. <br>
많은 경우 리플렉션이 객체의 빈틈에서 끼어들어 객체의 상태를 변경하거나 동작을 간섭하는 용도로 사용됩니다. 이렇게 되면 객체의 내부 구조를 외부에서 쉽게 변경할 수 있게 되어, 객체의 무결성을 보장하기 어렵게 됩니다. 따라서 이것이 리플렉션이 객체 지향 철학적으로 말하면 좋은 생각이 아닌 이유라고 봅니다. <br>
총평하자면, 리플렉션은 적절히 사용하면 매우 유용한 도구입니다만, 사용에 신중해야 하며 코드의 다른 부분을 이해하고 변경하는 데 어려움을 겪을 수 있습니다. 좋은 설계 원칙을 따르고 리플렉션 없이 문제를 해결할 수 있는 방법을 항상 고려해야 합니다.