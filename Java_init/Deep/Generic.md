# 제네릭스 (Generic)

# 제네릭스

### 제네릭스 ( Generics)

> **클래스 내부에서 사용할 데이터 타입을 외부에서 지정**한 것
>
- JDK 1.5부터 적용
- 컴파일 시 타입을 체크해주는 기능
  - 객체의 **타입 안정성**을 높여줌
  - **타입 체크(instanceof)**와 **형변환**의 번거로움이 줄어든다.
- Generic Type은 클래스와 메서드에 선언할 수 있다.

### 컴파일 타입 체크

**List Test**

```java
public class GenericTest {
    public static void main(String[] args) {

        List list = new ArrayList();

        list.add(1);
        list.add("String"); 
    }
}
```

- 컬렉션에 특정 타입을 지정하지 않으면 Object 타입의 상속 객체를 다형성에 의해 전부 넣을 수 있다.
- 위와 같은 방식은 과연 이점만 있을까???? 2가지 상황으로 설명을 하겠다.

**First**

```java
// 1 데이터를 꺼낼 때

Object cur = list.get(0);

if(cur instanceof Integer){
    System.out.println((Integer) cur);
}
```

- 다형성에 의해 다양한 클래스 타입의 객체를 저장했기 때문에 해당 클래스를 다시 꺼낼 때 우리는 해당 타입으로 **다시 형변환** 해줘야 한다.

**Second**

```java
// 2 잘못 꺼낸다면
        
Integer tmp = (Integer)list.get(1);
```

- 해당 코드는 Compile 시에는 문제점을 찾을 수 없다. 하지만 실제로 실행한다면 Runtime Exception이 발생한다.
- 즉 **타입에 대한 안전성을 보장할 수 없다는 것**이다.

> Object 타입과 다형성을 사용할 경우 타입 체크는 런타임시 동작한다. 이러한 방식은 유연한 코드 작성과 객체 지향 프로그래밍의 장점을 제공하지만 동시에 타입에 대한 안전성을 보장해주지는 못한다. 그리고 이러한 이유가 Generics가 등장하게 된 배경이다.
>

## Generic Class

- User<T> : Generic Class
- T : 타입 변수 or **타입 매개 변수**
- User : 원시 타입(raw type)

```java
class User<T>{
    T item;

    public void setItem(T item) {
        this.item = item;
    }
}
```

### 타입 변수

- 다양한 기호를 사용하지만 기호가 내포하는 의미에는 차이가 있지만 ‘임의의 참조형 타입’을 의미한다는 것은 똑같다.

### 사용

- Generic 클래스의 객체를 생성할 때에는 타입 매개 변수에 자신이 사용할 클래스 타입을 명시해주면 된다.

```java
public class GenericTest {
    public static void main(String[] args) {
        User<Sword> user = new User<>();
    }
}
class User<T>{
    T item;

    public void setItem(T item) {
        this.item = item;
    }
}
class Sword{}
```

- 오 그러면 아예 별도의 User<T>라는 클래스가 생성되는 것인가요 ?

  **그것은 아니다.**


```java
System.out.println(user.getClass().getName());

// chapter11.User
```

- 클래스의 이름을 찍어보면 원시 타입만 출력되는 것을 알 수 있다.  그 이유로는 **컴파일 후에는 이러한 제네릭 타입은 원시 타입으로 변환**된다.

### Generic의 제한

1. **static 멤버에 타입 변수 T를 사용할 수 없다.**

```java
class User<T>{
    static T item; // error

    public static void setItem(T item) {
        this.item = item;
    } // error
}
```

> Generic의 타입 변수는 인스턴스 별로 다른 동작을 제공한다. 즉 인스턴스에 상관없이 동일하게 다루어지는 static 멤버에는 사용할 수 없다.
>
1. **타입 변수 T의 배열을 생성할 수 없고 instanceof 연산자 또한 사용 불가능하다.**

```java
class User<T>{
    T[] item; // 선언만 가능

    public void makeArray(){
        item = new T[10]; // ERROR
    }
}

class User<T>{
    T[] item;

    public void makeArray(){
        if(T instanceof String){} // ERROR
    }
}
```

- new 연산자를 사용하기 위해서는 컴파일 시점에 클래스 타입을 알아야 한다. 하지만 컴파일 시점에 해당 클래스 내의 T 타입에 대해 알 수 없기 때문에 사용할 수 없다.

### 제네릭 클래스의 제한

- <T extends ClassName>을 통해 해당 클래스의 객체 생성 시 특정 클래스와 자식 클래스의 타입 변수로만 생성할 수 있도록 할 수 있다.
- 인터페이스와 함께 사용 시 **<T extends ClassName & InterfaceName>**

```java
public class GenericTest {
    public static void main(String[] args) {
        
        User<Item> user1 = new User<>();
        User<Sword> user2 = new User<>();
        
        User<Item> user3 = new User<Sword>(); // ERROR 으잉?? 다형성은 안되네....
        
    }
}
class User<T extends Item>{
    T item;
    public User(){

    }
    public User(T item) {
        this.item = item;
    }
}
class Sword extends Item{}

class Item{}
```

## 객체 생성

1. **참조 변수와 생성자에 대입된 타입 변수가 일치해야 한다. (상속 관계여도 적용되지 않는다.)**
  - 원시 타입간의 상속 관계는 인정된다. (원시 타입간의 공변성)
  - 타입 변수간의 상속 관계는 허용되지 않는다. (타입 변수간의 무공변성)

```java
public class GenericTest {
    public static void main(String[] args) {
        User<Item> user = new User<Sword>(); // ERROR
    }
}
class User<T>{
    T item;
    public User(){
        
    }
    public User(T item) {
        this.item = item;
    }
}
class Sword extends Item{}

class Item{}
```

### 변성

> 변성이란 특정 클래스들의 상속 관계가 다른 타입간에 어떠한 관계가 형성되는지를 나타내는 개념이다.
>

| 공변성 | 반공변성 | 무공변성 |
| --- | --- | --- |

**공변성**

- S가 T의 하위 타입이면 S[]는 T[]의 하위 타입이다.

**반공변성**

- S가 T의 하위 타입이면 Somthing(S)는 Somthing(T)의 하위 타입이다.

**무공변성**

- S가 T의 하위 타입이건 아니건 List<S>와 List<T>는 서로 다른 타입이다.

> 예시를 보면 알겠지만 Java는 배열에 대해서는 공변성을 제공하지만 컬렉션(제네릭)에 대해서는 무공변성이다.
>
- **왜 이따구로 만들었을까?**

  지금부터 하는 말은 스스로 생각해본 내용이다. 제네릭을 만든 이유는 앞서 말했듯 다형성으로 인한 타입의 불안전성을 해결하기 위해 만든 것이다. 그렇기 때문에 타입 변수에 공변성이 적용된다면 만든 이유가 없지 않아서일까????


### LSP (리스코프 치환 법칙)

> 클래스 간 상속 관계를 정의하기 위해 발표된 것으로서 **서브 타입은 언제나 슈퍼 타입으로 교체** 할 수 있어야 한다.
>
- 즉 상속과 다형성의 성질을 나타내는 OOP의 원칙 중 하나이다.
- 즉 객체간의 가변성을 제공하는 것을 의미한다.

### 와일드 카드

> 자바의 제네릭은 기본적으로 **무공변성**이다.  하지만 와일드 카드를 사용하여 제네릭에 가변성을 제공할 수 있고 이것을 **사용처 가변성**이라고 한다.
>
- 공변
  - A가 B의 서브타입이라면 List<A>는 List<? extends B>의 서브타입이다.
- 반공변성
  - A가 B의 서브타입이라면 List<B>는 List<? super A>의 서브타입이다.

### 효과

- 와일드 카드는 static 메서드와 제네릭 클래스가 아닌 기본 클래스에도 사용할 수 있다.
  - 매개변수와 리턴 타입에 사용 가능하다.
- **메서드 오버로딩의 효과**를 가질 수 있다.

### <?>

- Unbounded Wildcard
- 비한정적 와일드 카드 (? extends Object 와 동일)

### <? extends A>

- Upper Bounded Wildcard ( 상한 경계 와일드 카드 )
  - 상위 클래스 제한 ( A와 해당 자손 클래스들만 가능 )
  - 공변성 적용

```java
public class GenericTest {
    public static void main(String[] args) throws IOException {
        Attack.attack(new User<Sword>());
        Attack.attack(new User<Item>());
    }
}

class Attack{
    public static void attack(User<? extends Item> user){
        
    }
}
```

### <? super A>

- Under Bounded Wildcard ( 하한 경계 와일드 카드 )
  - 하위 클래스 제한 ( A와 해당 조상 클래스만 가능 )
  - 반공변성 적용

```java
public class GenericTest {
    public static void main(String[] args) throws IOException {
        Attack.attack(new User<Sword>()); // ERROR
        Attack.attack(new User<Item>());
    }
}

class Attack{

    public static void attack(User<? super Item> user){

    }

}
```

## Wildcard getter , setter

> wildcard 사용 시 굉장히 어려운 부분이다.
>

### **< ? extends A > : 상한 경계**

- **GET** : 안전하게 타입을 꺼내기 위해서는 **A 타입으로 받아야 한다.**

```java
class UserCheck{

    public static void check(User<? extends Item> user){
        Item item = user.getItem();

				// Sword sword = (Sword)user.getItem(); -> 만약 User의 Item이 Bow라면?
    }
}
```

> 이 내용은 쉽다. 예제를 들어 설명하겠다.
>
- Item 타입 변수에 대한 상한 경계 제한이다. 즉 가능한 타입 변수는 Item과 해당 클래스의 자손 클래스이다.
- 이런 상황에서 Item을 상속받은 **Sword**와 **Bow**가 있다 가정하자?
  - 우리는 과연 해당 Item이 어떤 타입인지 알 수 있을까????? → **없다.**

- **SET** : **null만 가능하다.**

```java
class User<T>{
    T item;

    public User(T item){
        setItem(item);
    }

    public void setItem(T item) { this.item = item; }

    public T getItem() { return item; }
}

public class GenericTest {
    public static void main(String[] args) throws IOException {

        User<? extends Item> user = new User<>();
        
        user.setItem(new Sword()); // ERROR
        user.setItem(new Item()); // ERROR
				user.setItem(null);
    }
}
```

> 개인적으로 정말 어려운 부분이라고 생각한다.
>
- 상한 경계로 매개 타입에 Item 클래스와 해당 자식 클래스가 가능하다. 하지만 상한 경계 타입을 설정한 경우 우리는 데이터를 SET할 수 없다.
- 그 이유는 **‘타입 추론’** 과 관련이 있다.
  - 타입 추론이란 컴파일러가 모든 타입에 대해 가장 구체적인 타입을 찾는 것을 의미한다.
  - 와일드 카드의 **상한 경계에 대한 컴파일러의 타입 추론**은 어디까지일까?

  > 정확히 말하자면 말 그대로 **‘ A 클래스이거나 A의 자식 클래스겠지 ‘** 인거다.
  >
  - 고로 **구체적으로 하나의 클래스로 결정하지 못한다는 것**이고 이로 인해 쓰기 연산을 제한하는 것이다.

![Untitled](%E1%84%8C%E1%85%A6%E1%84%82%E1%85%A6%E1%84%85%E1%85%B5%E1%86%A8%E1%84%89%E1%85%B3%20(Generic)%20bef44b6c45414b6cb2cb5451f7ba9f47/Untitled.png)

### 나는 그래도 꼭 지정을 하고 싶어 !

- **생성자**를 이용하자
- 위의 코드에서 매개변수가 있는 생성자를 사용해보자

```java
public class GenericTest {
    public static void main(String[] args) throws IOException {
				 User<? extends Item> user = new User<>(new Sword());
    }
}
```

- 놀랍게도 Error가 발생하지 않는다. 왜 그럴까?
  - 생성자는 딱 한번만 호출된다. 이것이 의미하는 바는 무엇일까?
  - **해당 인스턴스에 대해 너가 호출한 매개 변수의 클래스 타입으로 확정지을게 !**
- 컴파일러가 해당 타입으로 추론을 할 수 있다는 것이다.
- 하지만 위의 방식이 올바른지는 잘 모르겠다. 그냥 오기가 생겨서 이것저것해보다 알게 된 것이다.

> Generic에서 사용되는 변성 즉 super와 extends는 이론 자체도 어렵고 무엇보다 실제 적용에 있어 헷갈리기 쉽다.
>

### **< ? super A > : 하한 경계**

- **GET** : 안전하게 타입을 꺼내기 위해서는 Object 타입으로 받아야 한다.

```java
class UserCheck{

    public static void check(User<? super Item> user){
        Object item = user.getItem();
    }
}
```

- 간단하다. Item과 해당 조상 클래스들이 가능한 하한 경계에서 어느 조상인지 타입 추론할 수 없기 때문에 최고 조상 클래스인 Object로 받는게 안전하다.
- **SET** : A 클래스와 해당 클래스의 자손 클래스만 Set 가능하다.

```java
public class GenericTest {
    public static void main(String[] args) throws IOException {

        User<? super Sword> user = new User<>();

        user.setItem(new PowerSword());
        user.setItem(new Sword());
				user.setItem(new Item()); // ERROR

    }
}
```

- 간단하다. **타입 매개 변수로 하한 경계를 지정할 경우 다형성에 의해 자식 클래스들은 전부 참조**할 수 있다.

### PECS

- **produce - extends** , **consume - super**
  - generic 타입을 넣는다면 (소비) : super
  - generic 타입을 꺼낸다면 (생상) : extends
- Generic의 Wild Card를 사용해야 하는 상황을 정의

## Generic Method

- 메서드 선언부에 지네릭 타입이 선언된 메서드
- 선언 위치는 **반환 타입 바로 앞**
  - 타입 매개변수와 지네릭 메서드에 정의된 타입 매개변수는 별개의 것 !!!!
  - 메서드 안에서만 유효 (지역 변수 느낌)
  - static 메서드에서도 사용 가능

```java
public <ㅁ> void someItem(ㅁ item) {}
```

## Generic-Type Erasure

- 지네릭 타입은 JDK 1.5에 도입되었기 때문에 이전 소스 코드와의 호환성을 유지 필요
  - 컴파일러는 지네릭 타입을 통해 타입 체크 + 필요한 곳에 형변환
  - 컴파일 후에는 지네릭 타입에 대한 정보가 제거된다.

- 제거 과정
  1. 지네릭 타입의 경계를 제거한다.
  2. 타입을 제거해도 타입이 일치하지 않으면 형변환을 추가

![Untitled](%E1%84%8C%E1%85%A6%E1%84%82%E1%85%A6%E1%84%85%E1%85%B5%E1%86%A8%E1%84%89%E1%85%B3%20(Generic)%20bef44b6c45414b6cb2cb5451f7ba9f47/Untitled%201.png)

[☕ 자바 제네릭 타입 소거 컴파일 과정 알아보기](https://inpa.tistory.com/entry/JAVA-☕-제네릭-타입-소거-컴파일-과정-알아보기)

### 참고

[자바 제네릭스(9) Java Generics: 타입추론(Type Inference) - durtchrt](https://durtchrt.github.io/blog/java/generics/9/)

[제네릭, 그리고 변성(Variance)에 대한 고찰 (1) - Java](https://asuraiv.tistory.com/16?category=813980)

[Java 제네릭과 가변성(variance) 1편](http://happinessoncode.com/2017/05/21/java-generic-and-variance-1/#와일드카드)
