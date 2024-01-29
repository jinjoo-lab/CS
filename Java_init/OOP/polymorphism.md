# 다형성

# 다형성(多形性) : polymorphism

### 객체 지향 프로그래밍의 3대 특징 ( OOP is A P. I . E)

- **다형성**
- 상속
- 캡슐화

## 다형성

- 말 그대로 다양한 형태가 가능하다는 것이다.
- 많은 사람들은 **다형성**이라는 개념에 대해 참조 타입 즉 클래스에 대해서 많이 기술한다. 하지만 객체 지향 프로그래밍 그 중에서 **JAVA에서는 다양한 다형성이 존재한다.**

## 데이터의 다형성

- JAVA의 데이터 타입에는 크게 기본형과 참조형이 존재한다.

### 기본형

```

byte -> short -> int -> long -> float -> double
				char
```

- 위 표는 많은 사람들이 알고 있다. 자바에서의 기본형 데이터 타입들간의 자동 형변환의 방향을 나타낸 것이다. 작은 기본형 타입은 큰 기본형 타입으로 자동 형변환 가능한 것은 데이터의 다형성을 나타내는 것 중 하나이다.

## 참조형 다형성

> **한 타입의 참조변수로 여러 타입의 객체를 참조 가능하다.**
>
- 업캐스팅 : 모든 SUB 객체는 SUPER 타입으로 자동 형변환된다.
- 조상 클래스 타입의 참조 변수로 자손 클래스의 인스턴스를 참조할 수 있다.
    - 즉 참조변수의 타입으로 인스턴스가 사용할 수 있는 멤버의 개수를 결정

```java
class Person{
		int age;
		void eat();
}

class SpiderMan extends Person{
		void fight();
}

public class Test{
	public static void main(String[] args){
			Person p = new SpiderMan(); // 다형성

			SpiderMan sp = new Person(); // X
	}
}
```

- 다형성을 통해 Super 타입의 참조 변수로 Sub 타입의 객체를 참조하였다.
    - 참조변수 p로는 SpiderMan 클래스 인스턴스의 fight() 메서드를 사용할 수 없다.

### Shadow Impact

- 다형성에 의해 메모리에는 있지만 해당 멤버에 접근하지 못하는 것

> **참조변수의 타입에 따라 인스턴스가 사용할 수 있는 멤버의 개수가 달라진다.**
>

### 데이터의 표현 범위

> 하지만 반대는 허용하지 않는다. → 왜 불가능할까?
>
- 다형성을 결정하는 것은 부모 클래스와 자식 클래스간의 관계이지만 더 근본적으로 **데이터의 표현 범위**에 의한 것이다.
- SpiderMan sp
    - 자식 클래스의 참조 변수인 sp를 선언했다. 해당 참조 변수를 통해 우리는 **age , eat() , fight()** 에 접근할 수 있다.
- = new Person()
    - 부모 클래스의 객체(인스턴스)를 생성하였다. 그렇다면 Heap 메모리에는 age, eat() 만 가지고 있는 인스턴스가 생성되는 것이다.
    - sp의 fight()를 호출할 수 (객체에 대응)되지 않기 때문에 즉 데이터의 표현 범위가 대치되지 않기 때문에 컴파일러가 오류를 발생한다.

> **참조변수가 사용할 수 있는 멤버의 개수는 인스턴스의 개수보다 같거나 적어야 한다 !**
>

## 참조변수의 형변환

- 기본형 변수와 마찬가지로 참조형 변수도 형변환 가능하다. 하지만 형변환하기 위해서는 **두 인스턴스의 클래스가 상속관계**여야만 한다.
- 형태
    - 업캐스팅 : **자식 타입**의 참조변수를 **조상 타입**의 참조변수로 자동 형변환
    - 다운캐스팅 : **조상 타입**의 참조변수를 **자식 타입**의 참조변수로 명시적 형변환

### 업캐스팅 (Upcasting)

- sub 타입에서 super 타입으로 형변환
- 모든 sub 타입의 참조변수는 super 타입으로 자동 형변환

### 다운캐스팅(Downcasting)

- 자식에 추가된 속성이나 메서드를 추가 접근하기 위해
- 명시적 형변환
- super 타입에서 sub 타입으로 형변환
    - super 타입이 참조하고 있는 인스턴스의 sub 타입으로만 형변환 가능
    - 참조하고 있는 객체의 타입이 아닌 다른 타입으로 DownCasting 하면 ClassCastException 발생

> 형변환은 참조 변수의 타입을 바꾸는 것이지 인스턴스를 변환하는 것이 아니다. 즉 **참조변수의 형변환은 인스턴스에 접근 가능한 멤버의 개수를 조절하는 것이지 인스턴스에 아무런 영향을 주지 못한다.**
>

```java
class Person{
		int age;
		void eat();
}

class SpiderMan extends Person{
		void fight();
}

public class Test{
	public static void main(String[] args){
			Person p = new Person(); 
			SpiderMan sp = (SpiderMan)p; // X
	}
}
```

- 위 코드를 실행할 경우 ClassCastException이 발생한다. 그 이유가 바로 참조 변수의 형변환이 의미하는 것을 알려준다.
    - **참조변수가 가리키는 인스턴스의 타입이 무엇인지 파악하는 것이 중요하다.**
    - 참조변수가 접근할 수 있는 멤버의 개수가 달라지는 것이지 인스턴스 자체가 바뀌는 것은 아니다. 여전히 p가 가리키는 인스턴스는 Person 타입이기 때문에 SpiderMan 타입의 참조변수로 참조할 수 없다.

### instanceof

- 다운캐스팅은 이처럼 오류를 발생할지 발생할 수 없을 지 파악하기 어렵다. 이를 해결하기 위해 **해당 참조변수가 가리키는 인스턴스가 무슨 타입인지 알려주는 연산자**가 **instanceof**이다.
    - ‘**객체 instanceof 클래스이름’** 을 통해 해당 인스턴스의 타입을 파악할 수 있다.
        - 즉 결과가 true라면 해당 타입으로 형변환 가능하다는 것이다.

```java
public class test {
    public static void main(String[] args) {
        Person peterPacker = new SpiderMan();
				System.out.println(perterPacker instanceof Person); // true
        System.out.println(peterPacker instanceof SpiderMan); // true
    }
}
```

- **중요 !!!!!**
    - 해당 인스턴스의 타입을 알려주는 것이지 참조 변수의 타입을 알려주는 것이 아니다!!!!!!

## 중복

- 부모 클래스와 자식 클래스의 **멤버 변수**와 **멤버 메서드**가 중복되었을 때 부모 클래스의 참조변수로 자식 클래스의 인스턴스를 참조한다면?

> 앞서 Override를 배웠기 때문에 멤버 메서드가 호출된다는 것은 감으로 알 수 있다. 하지만 중요한 점은 멤버 변수이다.
>

```java
class Parent{
	int age = 1000;
	
	void eat(){System.out.println("식사를 잡수시다");}
}
class Child extends Parent{
	int age = 100;

	void eat(){System.out.println("밥 묵자");}
}
class test{
	public static void main(String[] args){
			Parent p = new Child();
			// p.x -> 1000 ??????
			// p.eat() -> 밥 묵자 (뭐 이건 느낌상)
	}
}
```

- **멤버 변수가 중복 정의된 경우 참조변수의 type에 따라간다 !!!!**

## 정적 바인딩 & 동적 바인딩

![Untitled](https://private-user-images.githubusercontent.com/84346055/300486626-90e1b551-5a5d-4055-ac92-3d7fe01fb3ca.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDY1MzcyNzQsIm5iZiI6MTcwNjUzNjk3NCwicGF0aCI6Ii84NDM0NjA1NS8zMDA0ODY2MjYtOTBlMWI1NTEtNWE1ZC00MDU1LWFjOTItM2Q3ZmUwMWZiM2NhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMjklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTI5VDE0MDI1NFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWI5YmM3Y2U1NmM5Y2MxY2I0NTI1YjY5NzU0OTUyMzJjZWFlNTczMjg5N2ZjYzcwZDFiZWY0OTM5MmUxZTkwYmMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.jRSx7JTd8Y-kMVBWz63eQXbqIbPspwPo4FrY7CtXjEg)

- 멤버 변수와 멤버 메서드를 부모 타입 참조변수로 호출했을 때 적용이 다른 이유는 **바인딩 방식의 차이** 때문이다.
- **변수**
    - 바인딩이란 변수에 구체적인 값을 할당하는 과정이다.
- **메서드**
    - 메서드를 호출할 때 그 메서드가 위치한 메모리 주소로 연결하는과정

```java
class Parent{
	int x = 10;
	void eat(){}
}

class Child extends Parent{
	int x = 20;
	void eat(){}
}

class Main{
	public static void main(String[] args){
			Parent p = new Child();
			// p.x : 10 (인스턴스 변수는 정적 바인딩 대상 : 참조 변수의 타입을 따라간다)
			// p.eat() : (인스턴스 메서드는 동적 바인딩 대상 : 인스턴스의 타입을 따라간다)
	}
}
```

### 정적 바인딩

- 컴파일 시점에 바인딩된다는 것이다.
- 인스턴스 변수 + static 멤버는 정적 바인딩 대상이다.

## 동적 바인딩이 이루어지는 과정

- Runtime 중에 바인딩된다는 것이다.
- Java에서 동적 바인딩 대상은 instance 메서드이다.

### 가상함수

- Override된 함수를 호출하는 방식
- 업캐스팅을 이용하여 슈퍼클래스를 오버라이드한 메소드 여러 개를 표시

### 가상함수 호출

![Untitled](https://private-user-images.githubusercontent.com/84346055/300486656-cc5e0bf3-8983-47d2-976c-2319616fe856.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDY1MzcyNzQsIm5iZiI6MTcwNjUzNjk3NCwicGF0aCI6Ii84NDM0NjA1NS8zMDA0ODY2NTYtY2M1ZTBiZjMtODk4My00N2QyLTk3NmMtMjMxOTYxNmZlODU2LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMjklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTI5VDE0MDI1NFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTFjZDAzZmE0ZjQyYzI5NjEzMjAyNTQ3YjVjMGRiZWQzNWQzOGM1ZTM5NmNiM2YxODYwZjc5Yzk2MDM1MDY0NWQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.5xdJFB6EXUnKJ58G6_5IDU9l02rWLdU_jspV_W8dUMg)

- 동적 바인딩
    - 참조 변수에 대해 메소드를 호출할 때 메서드를 결정하는 방법

> 가상함수의 호출에서 (상속한 여러 클래스에서 오버라이드한 동일한 함수가 여러 개 있을 때) 어느 함수를 호출하는가는 실행시점에  (동적으로) 결정한다는 의미다.
>
- 가상 함수는 실행 시점에 동적바인딩
    - VMT(가상 함수 테이블) 이용
- 자식 클래스는 자신으로부터 Object 까지의 모든 조상들까지 정의된 메서드를 가진다.
    - Override된 메서드의 경우 자신의 것을 가리킨다.
- 자바는 모든 메서드가 가상 함수 형태이다.

### VMT

자바에서 다형성을 지원하기 위한 메커니즘으로는 가상 함수 테이블(Virtual Method Table, VMT)이 사용됩니다. VMT는 객체 지향 프로그래밍에서 다형성을 구현하는 데 중요한 역할을 합니다.

1. **VMT (가상 함수 테이블):**
    - **개념:**
        - VMT는 객체의 런타임 다형성을 지원하기 위한 구조체로, 클래스의 가상 함수(오버라이딩된 메서드)에 대한 정보를 담고 있습니다.
        - 각 클래스의 VMT는 해당 클래스에 정의된 가상 함수들에 대한 포인터들의 배열로 구성됩니다.
    - **구성:**
        - VMT는 주로 Heap 메모리 영역에 위치하며, 객체의 메타데이터와 함께 생성됩니다.
        - VMT의 각 엔트리는 특정 메서드에 대한 포인터로, 해당 메서드의 실제 주소를 가리킵니다.
    - **동작 원리:**
        - 객체가 생성될 때 해당 클래스의 VMT가 생성되고 객체에 연결됩니다.
        - 메서드 호출은 VMT를 통해 동적으로 이루어지며, 객체의 실제 타입에 따라 올바른 메서드가 호출됩니다.
    - **예시 코드:**

        ```
        javaCopy code
        class Animal {
            void makeSound() {
                System.out.println("Some generic sound");
            }
        }
        
        class Dog extends Animal {
            @Override
            void makeSound() {
                System.out.println("Bark");
            }
        }
        
        Animal myDog = new Dog();
        myDog.makeSound(); // 동적 바인딩을 통해 Dog 클래스의 makeSound가 호출됨
        
        ```

2. **코드 섹션:**
    - **역할:**
        - 코드 섹션은 JVM이 실행 가능한 프로그램 코드를 저장하는 메모리 영역입니다.
        - 각 클래스의 메서드 코드는 이 영역에 저장되며, CPU가 실행할 때 이 코드를 참조하여 동작합니다.
    - **특징:**
        - 코드 섹션은 텍스트 섹션 또는 실행 섹션이라고도 불립니다.
        - 주로 프로그램이 실행 중에 읽기 전용으로 사용되며, 프로그램 실행 중 수정되지 않습니다.
    - **연결:**
        - 클래스의 메서드 코드는 클래스 로딩 시에 메서드 영역(PermGen 또는 Metaspace)에 로드되고, 이후 실행 시 코드 섹션에서 참조되어 실행됩니다.
