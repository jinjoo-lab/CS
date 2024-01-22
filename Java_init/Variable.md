# 변수

### 변수

> 값을 저장할 수 있는 임시 저장 공간 (일정한 범위 내에서 유지된다)
>

### 변수의 선언 , 초기화

```
int a ;
a = 10; // 선언 , 초기화를 분리
int b = 10; // 선언 + 초기화를 함께 수행
```

- 변수의 선언은 Data Type과 Data Name을 사용한다.
    - 변수 선언 → Data Type에 맞는 저장 공간이 확보 , 해당 공간에 대한 접근은 Data Name을 통해 가능

**주의 !**

- 변수의 종류에 따라 변수의 초기화를 생략 가능 !

> 지역변수 → 초기화하지 않으면 접근시 에러 , (클래스 변수,인스턴스 변수) → 초기화 생략 가능
>

## 자료형

> 값(data)가 **저장될 공간의 크기**와 **저장형식**을 결정
>

**분류**

```
Java Data Type 
ㄴ Primitive Type
    ㄴ Boolean Type(boolean)
    ㄴ Numeric Type
        ㄴ Integral Type
            ㄴ Integer Type(short, int, long)
            ㄴ Floating Point Type(float, double)
        ㄴ Character Type(char)
ㄴ Reference Type
    ㄴ Class Type
    ㄴ Interface Type
    ㄴ Array Type
    ㄴ Enum Type
    ㄴ etc.
```

- 자료형은 크게 **기본 타입**과 **참조 타입**으로 구분할 수 있다!
    - 기본형 → 실제 값을 저장 !
    - 참조형 → 값이 저장되어 있는 주소를 값으로 가진다 !

**주의 !**

- 자바는 참조형 변수 간의 연산이 불가하다 !

### 기본형

|  | 1Byte | 2Byte | 4Byte | 8Byte |
| --- | --- | --- | --- | --- |
| 논리형 | boolean |  |  |  |
| 정수형 | byte | short / char | int  | long |
| 실수형 |  |  | float | double |
1. Java에서는 8가지의 Primitive type(기본형)을 정의하고 제공
2. OS에 따라 자료형의 길이가 변하지 않는다.
3. 비객체 타입이므로 null값을 가질 수 없다 ! → Wrapper Class와는 다른 개념
4. JVM의 Stack Memory에 저장된다 !

---

- boolean
    - 논리형으로서 **true , false** 두 가지 값만 저장할 수 있다 (기본값 : false)
    - Java에서 **논리형은 다른 기본형과의 연산이 불가능하다 ! (형변환도 불가능)**
    - Java에서 데이터를 다루는 최소 단위 : Byte → 1 Byte 사용

---

- byte
    - 이진 데이터를 다루는데 사용
- short
    - C언어와의 호환을 위해서 추가 !
    - char와는 다르게 **음수 값을 다룰 수 있다 !**
- **int**
    - 자바에서 정수 **연산을 하기 위한 기본 타입**
    - byte 혹은 short 타입이 연산이 된다면 결과는 int 형
- **일반적으로 가장 많이 사용 !**
    - CPU가 가장 효율적으로 처리할 수 있는 타입이기 때문 !
    - JVM의 피연산자 스택이 피연산자를 4 byte 단위로 저장하기 때문 → 연산 시 byte나 short값도 4byte로 변환되어 연산이 수행된다.
- long
    - int 값을 벗어나는 정수를 다루기 위해 사용

---

- char
    - 정수형 + **문자형**
    - char 타입에는 한 개의 문자만 저장할 수 있다
        - Fact ) 문자가 저장되는 것이 아닌 **문자의 유니코드가 저장**되는 것이다 !

---

- float, double
    - 실수를 가수와 지수 형식으로 저장 → **부동 소수점 방식** 사용
    - 가수 부분(정밀도)에 있어 double 의 크기가 더 크기 때문에 정밀도가 높다.
    - 실수의 기본 타입 → double

### 참조형

- Java에서는 기본형 타입을 제외한 모든 타입들은 참조형이다.
- Java에서 최상위 클래스(Object)클래스를 상속하는 모든 클래스
    - 메모리 영역인 Heap 영역에 생성된다.

| 클래스 타입 | 인터페이스 타입 | 배열 타입 | 열거 타입 |
| --- | --- | --- | --- |
- 빈 객체를 의미하는 null이 존재한다.
- Heap 메모리에 생성된 인스턴스는 메소드나 각종 인터페이스에서 접근하기 위해 JVM의 Stack 영역에 존재하는 Frame에  참조값을 가지고 있어 이를 통해 인스턴스를 핸들링합니다.

![Untitled](https://private-user-images.githubusercontent.com/84346055/298599394-d3da5f7b-6c89-4814-ac6a-bbe25d8d3e88.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDU5MzAxNzAsIm5iZiI6MTcwNTkyOTg3MCwicGF0aCI6Ii84NDM0NjA1NS8yOTg1OTkzOTQtZDNkYTVmN2ItNmM4OS00ODE0LWFjNmEtYmJlMjVkOGQzZTg4LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMjIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTIyVDEzMjQzMFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTkyNjI1YzFhYjc1YWE4NjkzNDU4NTA1YWQwNjY5MDEyZWQ1YjY0MWRjMmFhMWFkYWZkMmVmOGQ2NDhkMTVhM2YmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.pFT5kGAEhrDBNe8R4yhLbpv4WyuttLlNzEbGvAC1rrI)

- **Java의 참조와 C언어의 포인터 차이 ?**

  공통점 : 포인터와 참조 모두 주소를 통해 데이터에 접근한다.

  차이점 : C언어 → 메모리 핸들링 가능 , Java → 메모리 핸들링 불가능

    - **주소값을 변경할 수 없다 ! , Java에서 변경되는 것은 객체의 프로퍼티일 뿐 , 객체의 주소값은 변경되지 않는다.**
- **Why 변경 불가능 ?**

  JVM의 구성 요소이자 Java의 강점 중 하나인 **가비지 컬렉터**때문이다.

    - GC의 기본 동작으로는 기본적으로 Heap 영역 내에서 이동이 발생한다. (Eden → Survivor)
        - 즉 Heap영역에서 객체의 주소는 고정적이지 않다는 것이다.

## 형변환 ( Casting )

> 변수 또는 상수의 타입(자료형)을 다른 자료형으로 변환하는 것
>

```
int score = (int)doubleScore ; // double -> int 형변환
```

- 자동 형변환
- 수동 형변환 (직접 형변환을 명시해줘야 한다)

### 자동 형변환

- 형변환 과정을 생략 가능 → **컴파일러가 생략된 형변환을 자동적으로 추가**해준다 !

```
/*대입 형변환*/
float f = 1234; // -> float f = (float)1234;

/*산술 형변환*/
int i = 3;
double d = 1.0 + i; // -> double d = 1.0 + (double)i;

/*형변환 중 손실 발생*/
double d = 3.14;
int i = (int)d; // -> 자동 형변환 불가능 , 값이 3으로 변경 ( 무조건 내림 - 값의 손실 발생 )
```

**자동 형변환 조건**

> 기존의 값을 최대한 보존할 수 있는 타입으로 자동 형변환한다.
>
- **표현 범위가 좁은 타입 → 넓은 타입으로의 형변환**은 자동 형변환 가능 !

**주의 !**

1. boolean을 제외한 기본형끼리만 형변환 가능
2. 기본형과 참조형은 서로 형변환이 불가능하다 !
3. 표현 범위가 좁은 타입 → 넓은 타입만이 자동 형변환 가능

## [ 참고 ]

[[Java] Java에 포인터가 없는 이유 - 포인터(Pointer) vs 참조(Reference)](https://sorjfkrh5078.tistory.com/278)

[https://github.com/GimunLee/tech-refrigerator/blob/master/Language/JAVA/Primitive type](https://github.com/GimunLee/tech-refrigerator/blob/master/Language/JAVA/Primitive%20type%20%26%20Reference%20type.md)
