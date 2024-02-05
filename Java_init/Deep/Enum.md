# Enum

### 상수

> 상수란 **프로그래밍 내에서 임시적으로 변하지 않는 값**으로서 초기화 후 재할당이 불가능하다.
>
- 즉 **일종의 규약**으로서 프로그램 내에서 **고유한 값**으로 사용된다.

### 상수에 대한 고찰

- Java에서 상수는 final 제어자를 사용해 상수화한다. 또한 static 연산자를 사용하여 메모리에서 한번만 초기화하도록 한다.
    - final : 초기화 후 변경 불가
    - static : 메모리에 값이 올라갈 때 한번만 초기화

```
static final double PI = 3.14;
```

- 하지만 근본적으로 이러한 상수에 대해서는 **한계가 존재**한다.
1. 상수는 변하지 않는 값이라는 점에서 static final 선언은 조건을 만족한다. 하지만 **‘고유한 값'** 이라는 의미에서는 한계가 존재한다.

```
static final int JANUARY = 1;
static final int MONDAY = 1;
```

- 위 예시를 볼 경우 JANUARY == MONDAY는 true의 값을 출력한다. 왜냐하면 상수로서 할당된 정수의 값(리터럴 값)이 동일하기 때문이다.
- 즉 상수의 비교 시 **할당된 값으로 비교**하기 때문에 고유한 값의 의미를 완전히 표현하지는 못한다.

### Enum

> **타입에 따른 상수를 관리**하기 위한 것
>
- JDK 1.5에 추가
- 여러 상수를 정의 가능

**특징**

- 자바의 열거형은 C언어와는 다르게 **typesafe enum**이다.
- 부분 컴파일이 가능하다.
    - 일반 상수를 이용 시 값이 변한다면 전체 컴파일을 해야 한다. 하지만 Enum을 사용한다면 기존 소스를 컴파일하지 않아도 된다.

### Typesafe enum

> 할당된 값이 동일할지라도 **타입이 다르면 컴파일 에러**가 발생 , 고유한 값으로서 관리가 가능하다.
>

### 열거형 정의

```java
public enum Direction {
    NORTH, EAST, WEST, SOUTH
}

// 접근 : Direction.NORTH
```

- 열거형 상수간에는 == 비교 가능 , equals()대신 ==을 사용한다는 것은 **빠른 성능 제공**
    - == : 동일성 비교
    - equals() : 동등성 비교
- < , > 비교는 컴파일 에러 , compareTo() 메소드 사용
    - compareTo 메서드는 **순번을 비교**한 것

### 동일성과 동등성

> 자바에서 동일성은 == 비교이고 동등성은 equals() 비교이다.
>

**동일성 (Identity)**

- 두 객체가 메모리 상에서 같은 객체인지 비교
- 두 객체의 참조를 비교하는 것

**동등성 (Equality)**

- 두 객체가 클래스(Value)적으로 같은지 비교
- 설정된 비교 기준에 따라 두 객체를 비교

```java
public class EnumTest {
    public static void main(String[] args) {
        Node f = new Node(1,1);
        Node s = new Node(1,1);

        System.out.println(f == s); // false : 메모리적으로 두 객체는 동일하지 않다.
        System.out.println(f.equals(s)); // true : 두 객체는 Value적으로 동등한 객체이다.
    }
}

class Node{
    int x;
    int y;

    Node(int x,int y){
        this.x = x;
        this.y = y;
    }

    @Override
    public boolean equals(Object obj) {
        if(obj instanceof Node){
            Node tmp = (Node)obj;

            return this.x == tmp.x && this.y == tmp.y;
        }

        return false;
    }
}
```

### Enum is Class ?

- Java의 Enum은 인터페이스와 같이 **특수 클래스의 형태**이다.
    - 일종의 객체이기 때문에 **Heap 메모리에 저장**되며 각 상수들은 **별개의 메모리 주소**를 가진다.
- enum은 Thread Safe인 싱글톤 객체이다.

![Untitled](Enum%2044ee42a5259041ac841e66163fac6611/Untitled.png)

### Compile

1. enum은 자동적으로 **final**이고 **public**인 클래스로 컴파일
2. 각 열거형 상수는 해당 클래스의 **정적 상수**로 생성
3. 생성자와 메서드 추가
    - 각 열거형 상수는 해당 클래스의 인스턴스로 취급 → 생성자 호출 가능

```java
public enum Direction {
    NORTH, EAST, WEST, SOUTH
}

public class EnumTest {
    public static void main(String[] args) {
        Direction dir = Direction.NORTH;

        if(dir instanceof Object){
            System.out.println("Enum is son of Object");
        }

        if(dir instanceof Enum){
            System.out.println("Also Enum is son of Enum Class");
        }
    }
}
```

- Enum은 클래스로 컴파일된다고 했다. 즉 **Object 클래스를 상속받는 클래스**인것이다 !!!!!!
- 또한 모든 사용자 정의 enum 클래스는 **Enum 클래스를 상속**받는다.

### Java.lang.Enum

- 모든 열거형 타입의 조상 클래스

```
public abstract class Enum<E extends Enum<E>>
        implements Comparable<E>, Serializable {
    
    private final String name;

    public final String name() {
        return name; // 열거형 상수의 이름을 반환
    }

    private final int ordinal;

    public final int ordinal() {
        return ordinal; // 열거형 상수가 정의된 순서를 반환 (0부터 시작)
    }

    protected Enum(String name, int ordinal) {
        this.name = name;
        this.ordinal = ordinal;
    }
		...
}
```

### ValueOf

```
public static <T extends Enum<T>> T valueOf(Class<T> enumType, String name) {
        T result = enumType.enumConstantDirectory().get(name);
        if (result != null)
            return result;
        if (name == null)
            throw new NullPointerException("Name is null");
        throw new IllegalArgumentException(
            "No enum constant " + enumType.getCanonicalName() + "." + name);
    }
```

- 열거형 상수의 이름(문자열)로 **문자열 상수에 대한 참조**를 얻을 수 있다.

### final Method

- Enum 클래스에서 5개의 메서드를 final로 선언했다. 즉 오버라이딩 불가능하다.

```
    public final boolean equals(Object other) {
        return this==other;
    }

    public final int hashCode() {
        return super.hashCode();
    }

    protected final Object clone() throws CloneNotSupportedException {
        throw new CloneNotSupportedException();
    }

    public final int compareTo(E o) {
        Enum<?> other = (Enum<?>)o;
        Enum<E> self = this;
        if (self.getClass() != other.getClass() && // optimization
            self.getDeclaringClass() != other.getDeclaringClass())
            throw new ClassCastException();
        return self.ordinal - other.ordinal;
    }

    @SuppressWarnings("unchecked")
    public final Class<E> getDeclaringClass() {
        Class<?> clazz = getClass();
        Class<?> zuper = clazz.getSuperclass();
        return (zuper == Enum.class) ? (Class<E>)clazz : (Class<E>)zuper;
    }

    protected final void finalize() { }
```

### 열거형에 멤버 추가

> 앞서 Enum은 하나의 클래스라고 설명했다. 그리고 내부적으로 **선언된 상수 또한 클래스의 인스턴스**라고 설명했다. 즉 생성자와 멤버가 추가 가능하다는 것이다.
>
- 열거형의 인스턴스는 싱글톤 객체이다. 즉 **생성자가 자동적으로 private**이며 외부에서 호출 불가능
- 열거형 상수는 하나의 인스턴스로 만들어져 관리된다. (public static final)

```java
public enum Direction {
    NORTH("북"), EAST("동"), WEST("서"), SOUTH("남");

    String koreanName;

    Direction(String koreanName){
        this.koreanName = koreanName;
    }
}

// public static final Direction North = new Direction("북");
```

- **주의 !**

> 멤버 변수는 private으로 자동 선언되지 않기 때문에 변경 가능하다. 즉 사용자 임의 변경 시 해당 값이 그 순간부터 고정적이다.
>

```java
public class EnumTest {
    public static void main(String[] args) {
        Direction dir = Direction.NORTH;
        System.out.println(dir.koreanName); // 북

        dir.koreanName = "북쪽이다";
        System.out.println(dir.koreanName); // 북쪽이다.

        Direction dir2 = Direction.NORTH;
        System.out.println(dir2.koreanName); // 북쪽이다.
    }
}
```

### 추상 메서드

> Enum 클래스 내부에 추상 메서드를 선언하여 각 상수 인스턴스마다 다른 동작이 가능하도록 구현 가능하다.
>
- 익명 객체의 형태로 접근 가능 + 추상 메서드 구현을 람다식으로도 가능하다.

```java
package Enum;

public enum Direction {
    NORTH("북"){
        public String getPlusName(){
            return "NORTH + 북";
        }
    }, EAST("동"){
        public String getPlusName(){
            return "EAST + 동";
        }
    }, WEST("서"){
        public String getPlusName(){
            return "WEST + 서";
        }
    }, SOUTH("남"){
        public String getPlusName(){
            return "SOUTH + 남";
        }
    };

    String koreanName;

    Direction(String koreanName){
        this.koreanName = koreanName;
    }

    abstract String getPlusName();
}
```

### Enum 사용?

> 너무 잘 정리된 블로그가 있어 첨부한다. Enum의 응용은 무궁무진하기 때문에 잘 알아야 한다.
>

[Java Enum 활용기 | 우아한형제들 기술블로그](https://techblog.woowahan.com/2527/)
