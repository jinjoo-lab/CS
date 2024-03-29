# 객체 지향 프로그래밍 (클래스, 변수와 메서드)

# 객체 지향은 왜 태어났을까?

## 절차 지향 언어 (POP)

- (**Top - Down 방식**)코드가 위에서부터 아래로 순차적 진행되는 형태
    - 자주 사용되는 코드를 함수로 만들어 중복 방지

**장점**

- 컴퓨터의 작업 처리 방식과 유사하기 때문에 **시간적 유리**

**단점**

- 유지보수가 어렵다.
    - 하나의 절차가 바뀐다면 ? : 전체에 대한 변경이 불가피하다.
- 코드의 순서가 바뀌면 동일한 결과를 보장하기 어렵다.
- 디버깅이 어렵다.

> 절차 지향 프로그래밍은 **명령어의 흐름(순서)**에 초점을 두고 있는 패러다임으로서 데이터와 함수를 명시적으로 분리한다.
>

## 객체 지향 언어 (OOP)

- 일의 순서가 아닌 **객체의 상호작용**에 의해 수행되는 형태
    - 갹체마다 관리되는 고유한 데이터와 데이터 처리 메소드를 통해 유연하게 처리 가능

장점

- 코드를 재사용하거나 확장하기 좋아서 유지보수가 쉽다.
- 캡슐화 특징으로 실제로 구현되는 부분을 외부에 드러나지 않도록 은닉하여 보안성이 높다.

단점

- 캡슐화의 격리 구조 때문에 실행 속도가 느리다.
- 객체 단위의 구성은 메모리 소모가 크다.

- **왜 태어났을까 ?**

  객체 지향 프로그래밍은 **대규모 소프트웨어 개발 프로젝트에서의 필요성**에 부응하기 위해 개발. 이전의 절차 지향적인 접근은 대규모 시스템에서의 복잡성을 다루기 어려웠으며, 객체 지향 프로그래밍은 이러한 복잡성을 더 효과적으로 다룰 수 있도록 도움을 주었습니다.


---

# 클래스와 객체

### 클래스

- 객체를 정의해 놓은 것 (객체에 대한 설계도)
- 사용자 정의 타입
    - 기본형인 자료형 8개가 아닌 사용자가 정의한 자료형 타입

```
변수 -> 배열 -> 구조체 -> 클래스

1. 변수 : **하나의 데이터**를 저장할 수 있는 공간
2. 배열 : **같은 종류의 데이터를 하나의 집합**으로 저장할 수 있는 공간 (크기 고정)
3. 구조체 : **여러 데이터를 종류에 상관없이** 하나의 집합으로 저장할 수 있는 공간
4. 클래스 : **데이터와 함수의 결합**
```

### 인스턴스

- **인스턴스화** : 클래스로부터 객체를 만드는 과정
- **인스턴스** : 클래스로부터 만들어진 객체
    - 객체는 클래스로부터 만들어진 모든 인스턴스를 포괄하는 의미를 가지고 있지만 사실 그닥 구분하지는 않는다.

### 객체

- 실세계에서 존재하는 유형 + 무형의 모든 사물
    - **속성**과 **기능** 두가지 구성요소로 이루어져 있다.
- 속성과 기능의 집합 (필드와 메서드의 집합)

# 생성자

> **인스턴스가 생성될 때 호출**되는 ‘인스턴스 초기화 메서드’
>
- 생성자의 이름은 클래스의 이름과 같아야 한다.
- 생성자는 리턴 값이 없다. (하지만 void를 붙이지는 않는다)

> **new 연산자가 인스턴스를 생성**하는 것이지 생성자가 인스턴스를 생성하는 것은 아니다.
>

```
MyClass A = new MyClass();
// 1. 연산자 new에 의해 Heap 메모리에 MyClass의 인스턴스가 생성된다.
// 2. 생성자가 호출
// 3. 생성된 인스턴스의 주소가 반환되어 참조변수에 저장 (new의 역할)
```

- new 연산자는 객체를 Heap이라는 메모리 영역에 메모리 공간을 할당해주고 메모리주소를 반환한 후 생성자를 실행

### 기본 생성자

- 매개변수가 없는 기본 생성자
- 클래스 내에 다른 생성자가 존재하지 않을 경우 자동 생성
    - **컴파일러가 자동으로 기본 생성자를 추가하여 컴파일**
- 클래스 내에 다른 생성자가 정의되어 있는 경우 기본생성자는 생성되지 않는다.

### 매개변수가 있는 생성자

- 매개변수를 지정하여 인스턴스를 생성하는 동시에 초기화 가능

### this()

> 같은 클래스의 멤버들 간에 서로 호출할 수 있는 것처럼 **생성자 간에도 서로 호출이 가능**
>
- 생성자 내부에서 다른 생성자 호출 시 **클래스 이름이 아닌 this()를 사용**한다.
- 한 생성자에서 다른 생성자 호출 시 **첫줄에서만 호출 가능 !!!!!**
    - 어길 시 컴파일 에러
- **왜 첫줄이어야 할까?**

  중간에 호출할 경우 이전의 초기화 작업이 무로 돌아갈 수 있기 때문에 !


```java
// 올바른 예
class Node{
    int x;
    int y;

    Node(int x,int y){
        this.x = x;
        this.y = y;
    }
    
    Node(){
        this(1,2);
    }
}
```

### this

- 인스턴스 자신을 가리키는 참조변수 (주소가 저장되어 있다)
- 생성자 내부에서 **자기 자신(인스턴스)를 호출**할 때 사용
    - static 메서드에서는 this 사용 불가능

```java
class Node{
    int x;
    int y;

    Node(int x,int y){
        this.x = x;
        this.y = y;
    }
}
```

- 만약 위 생성자에서 **x = x, y = y가 호출된다면 둘 다 지역 변수로 취급**된다.

---

# 변수와 메서드

> JAVA언어에서는 **변수,함수가 클래스 바깥에 존재할 수 없다 !**
>

## 변수

- JAVA에서는 변수가 선언되는 위치를 기준으로 **클래스 변수**, **인스턴스 변수**, **지역 변수** 3종류로 나눌 수 있다.

```java
class 변수{
	int 인스턴스_변수;
	static int 클래스_변수; // 클래스 영역

	void 함수(){
			int 지역_변수; // 메소드 영역
	}
}
```

**비교**

| 변수의 종류 | 선언위치 | 생성 시기 |
| --- | --- | --- |
| 클래스 변수 | 클래스 영역 | 클래스가 메모리에 올라갈 때 |
| 인스턴스 변수 | 클래스 영역 | 인스턴스가 생성되었을 때 |
| 지역 변수 | 클래스 영역 이외의 영역 | 변수가 선언된 코드가 수행될 때 |

**인스턴스 변수**

- **인스턴스마다 독립적인 저장 공간**으로 가지므로 서로 다른 값을 가질 수 있다.

**클래스 변수**

- **모든 인스턴스가 공통된 저장 공간을 공유**
- 인스턴스를 생성하지 않고도 바로 사용 가능 (클래스이름.클래스변수 로 접근한다.)

**지역_변수**

- 메서드 내에 선언되어 메서드 내에서만 사용 가능, **메서드가 종료되면 소멸**

### 변수의 초기화

- 지역 변수 : 초기화 필수
- 클래스 변수, 인스턴스 변수 : 기본값으로 자동 초기화

### 변수의 초기화 방법

- 명시적 초기화
- 생성자 (인스턴스 변수)
- 초기화 블럭
    - 클래스(static) 초기화 블럭
    - 인스턴스 초기화 블럭

### 초기화 블럭

- 클래스 초기화 블럭
    - 메모리에 클래스가 로딩될 때 한번만 수행
- 인스턴스 초기화 블럭
    - 인스턴스가 생성될 때마다 호출

```
순서
기본값 -> 명시적 초기화 -> 블럭 -> 생성자
```

## 메서드

- **특정 동작을 수행하는 코드를 하나의 묶음**으로 묶은 것
    - 우리가 메서드를 사용하는 경우 입력과 출력을 보통 사용한다. (메서드 → 블랙박스)
- 장점
    - 높은 재사용성
    - 중복된 코드 제거
        - 프로그램의 구조화

### 메서드의 구조

- 메서드는 크게 **선언부**와 **구현부**로 이루어져 있다.
- 선언부
    - 메서드의 이름
    - 매개변수의 선언
        - 매개 변수로 지정된 값은 메서드의 매개변수로 복사된다.
            - 기본형 : 값을 복사 (읽을 수만 있다.)
            - 참조형 : 인스턴스의 주소를 복사 (읽을 수 있고 쓰기(변경)도 가능하다)
    - 반환타입

```
// 선언부
int method(int x,int y){
		return x + y; // 구현부 (반환타입과 일치하거나 자동 형변환이 가능한 것)
}
```

- 같은 클래스 내 메서드끼리는 서로 호출 가능
    - static 메서드는 같은 클래스 내의 **인스턴스 메서드를 호출할 수 없다.**

### 클래스 메서드와 인스턴스 메서드

- 클래스 메서드 : static 이 붙어 있다.
    - 객체를 생성하지 않고 class.메서드이름 으로 호출 할 수 있다.
    - 클래스 메서드는 인스턴스 변수를 사용할수 없다.
- 인스턴스 메서드
    - 객체를 생성해야만 호출할 수 있다.

### 오버로딩( overloading )

- **한 클래스내에서 동일한 이름의 메서드를 여러 개 정의**하는 것

**오버로딩 조건**

1. **메서드 이름이 같아야 한다.**
2. 매개변수의 **개수 or 타입이 달라야 한다.**
- 위 규칙을 어길 경우 컴파일 에러

> 반환 타입은 오버로딩을 구현하는데 아무런 영향을 주지 못한다.
>

```java
class MyMath{
		int sum(int x,int y){
        return x + y;
    }
    
    double sum(int x,int y){
        return x + y;
    } // sum(int, int)' is already defined in 'MyMath'
}
```

- 매개변수의 이름은 구분 요소가 아니다.
- 반환타입은 구분 요소가 아니다.
- **매개 변수의 순서는 구분요소**이다 !

```
long add(int a,long b);
long add(long a,int b);
```

오버로딩 장점

- 같은 기능을 하는 메서드를 하나의 이름으로 묶어 가독성 향상
- 같은 기능을 하는 메서드를 다양한 입력 형태로 처리할 수 있다.

### 가변인자(varargs)와 오버로딩

> JDK1.5에서부터 **매개변수 개수를 동적으로 지정할 수 있는 가변인자**가 추가되었다.
>
- **타입… 변수명**으로 사용
- 가변인자를 매개변수 중에서 **제일 마지막에 선언**해야 한다. (어길경우 에러가 발생)
- 가변인자는 내부적으로 배열을 사용
    - 메서드 내부에서는 가변인자를 배열로 선언

```
int sum(int... num){}
```
