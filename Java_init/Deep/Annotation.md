# Annotation

### 주석 (/**/ , //)

> 소스 코드를 작성한다면 우리는 작성한 코드에 대한 설명을 주석으로 남긴다. 소스코드에 대한 문서를 별도로 생성하지 않고 **주석으로 관리**한다는 것이다.
>
- 주석을 통해 해당 코드를 보는 사람에게 **특정 정보를 제공**할 수 있다.
- 주석은 **코드에 영향을 주지 않는다.**

### 메타데이터

- 데이터를 위한 데이터이다. (Data for Data)
- 속성 정보로 불리며 다른 데이터를 설명해주는 데이터라고 생각하자

> 캐런 코일은 ‘어떤 목적을 가지고 만들어진 데이터’라고 기술한다.
>

**목적**

- 데이터를 **표현하기 위한 목적**
- 데이터를 **빨리 찾기 위한 목적** ( 데이터베이스의 인덱스 역할 )

### MetaData in Program

- 외부의 데이터를 프로그램에서 사용할때는 다양한 방법이 있지만 그 중 한가지 방법이 MetaData로 관리하는 것이다.
- **MetaData를 정의**했다면 내가 사용하는 프로그램에서 해당 **정보를 읽고 사용하는 과정**을 수행했었다.

### 등장배경

- Java 4 이전의 버전에서는 **XML이 메타데이터를 표현하는 데에 있어서 일반적으로 사용** Java 4까지는 주로 XML을 이용하여 다양한 설정 정보, 배포 서술자, 프로퍼티 파일 등을 작성하고 활용

> Java 5부터는 **어노테이션(Annotation)이 도입**되면서, **코드에 메타데이터를 더 쉽게 표현**할 수 있게 되었습니다. 이전에는 XML을 사용하여 외부 파일에 메타데이터를 기술하고, 프로그램이 실행될 때 이를 읽어들여 사용하는 방식이 일반적이었습니다. 그러나 어노테이션은 코드 내에 직접 메타데이터를 포함시킬 수 있게 하여 설정 정보를 더 효과적으로 관리할 수 있게 되었습니다.
>

## Annotation

> 소스코드에 추가하여 사용할 수 있는 메타데이터 , 컴파일러에게 **컴파일 과정에 있어 코드 처리에 대한 부가 정보 제공**
>

### 주요 기능

- 컴파일러에게 **코드 작성 문법 에러**를 체크하도록 정보 제공

```java
public class Example {
    
    @Override
    public void myMethod() {
        // 잘못된 메서드 이름으로 작성하면 컴파일 에러 발생
    }
    
    @SuppressWarnings("unchecked")
    public void someMethod() {
        // 컴파일러 경고 무시
        List<String> list = new ArrayList();
    }
}
```

- **빌드나 배포시 코드를 자동으로 생성** 가능
    - **Annotation Processor**를 사용하여 가능
        - 소스 코드에 적용된 Annotation을 분석하여 추가적인 코드 생성

```java
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class Person {
    private String name;
    private int age;
}
```

- **런타임에 특정 기능을 실행**하도록 정보 제공
    - **리플렉션** 사용
        - 어노테이션 정보를 분석하고, 해당 정보에 따라 동적으로 동작을 변경

```java
@Service
public class MyService {

    @Autowired
    private MyRepository myRepository;

    // ...
}
```

### 표준 어노테이션

- **@Override**
    - 메서드 앞에만 가능 : **슈퍼클래스의 메서드를 오버라이딩**한다는 의미

> 컴파일시 컴파일러가 해당 어노테이션을 확인하고 슈퍼클래스의 메서드와 메서드 시그니처를 비교하여 올바른 규칙으로 오버라이딩되었는지 확인하고 이 과정에서 잘못된 경우 에러 표시
>
- **@Deprecated**
    - 클래스의 필드나 메서드에 사용 : **더 이상 사용을 권장하지 않는다**는 의미
- **@FunctionalInterface**
    - ‘함수형 인터페이스’를 올바르게 선언했는지 확인 (추상 메서드가 한 개인 인터페이스)
- @SuppressWarnings
    - 컴파일러가 보여주는 경고메시지가 나타나지 않게 억제

### 메타 어노테이션

> **Annotation 정의 시 사용되는 어노테이션**이며 적용 대상이나 유지 기간을 지정
>

### @Target

> Annotation이 적용 가능한 대상을 지정
>
- **적용 대상**

| TYPE | 클래스,인터페이스, ENUM | CONSTRUCTOR | 생성자 | TYPE_PARAMETER | 타입 매개변수 |
| --- | --- | --- | --- | --- | --- |
| FIELD | 멤버 , ENUM 변수 | LOCAL_VARIABLE | 지역변수 | TYPE_USE | 타입의 모든 곳 |
| METHOD | 메서드 | ANNOTATION_TYPE | 애너테이션 | MODULE | 모듈 |
| PARAMETER | 매개변수 | PACKAGE | 패키지 |  |  |

### @Retention

> Annotation이 유지되는 기간 지정
>
- SOURCE : 소스 파일에만 존재 , 클래스 파일에는 존재 X
- CLASS : 클래스 파일에 존재 , **실행 시 사용 불가**
    - 컴파일러가 정보를 클래스 파일에 저장 , JVM에 로딩 시에는 무시되어 실행 시 정보를 얻을 수 없다.
- RUNTIME : 클래스 파일에 존재 , **실행 시 사용 가능**
    - 실행 시 리플렉션을 통해 클래스 파일에 저장된 Annotation 정보를 읽어서 처리

### @Documented

> Annotation에 대한 정보가 javadoc으로 작성한 문서에 포함
>

### @Inherited

> 슈퍼클래스의 인터페이스에 붙으면 자손클래스에도 해당 어노테이션이 상속된다.
>

### 정체가 무엇인가 ?

> Annotation은 정확히 말하자면 **특별한 형태의 interface**이다. 다음은 Annotation을 선언할 때 사용하는 구조이다.
>

```java
public @interface MyAnnotation {
    String getName();
}
```

- 어노테이션에는 위와 같이 **요소**를 포함할 수 있다. (**상수 또한 정의 가능** But **디폴트 메서드 정의 불가능**)
- 요소
    - **반환값이 있고** (void 불가능) **매개변수는 없는** **추상 메서드** 형태
    - 어노테이션 적용 시 요소들의 값을 지정해줘야 한다.
- 요소 규칙
    - 요소의 타입은 **기본형** , **String**,  **Enum**, **애너테이션**, **Class** 가능
    - 매개변수 없다 + 반환형 필수
    - 예외 선언 불가능
    - 요소를 **타입 매개변수로 정의 X**

```java
public @interface MyAnnotation {
    String name();
		int age()  default  1;
    String[] album();
}

@MyAnnotation(
        name = "Beenzino",
        age = 26,
        album = {"24:26","12","NOWITZKI"}
) class MyClass{}
```

- 가져오는 코드 (리플렉션 이용) → 단순한 예시입니다.

```java
@MyAnnotation(
        name = "Beenzino",
        age = 26,
        album = {"24:26","12","NOWITZKI"}
) class MyClass{
    String name;
    int age;
    String[] album;

    MyClass(){
        MyAnnotation annotation = MyClass.class.getAnnotation(MyAnnotation.class);
        if (annotation != null) {
            this.name = annotation.name();
            this.age = annotation.age();
            this.album = annotation.album();
        }
    }
}
```

### java.lang.annotation.Annotation

- 모든 Annotation의 조상 인터페이스
- Annotation은 상속이 허용되지 않으므로 명시적 조상 지정 불가능

```java
public interface Annotation {

    boolean equals(Object obj);

    int hashCode();

    String toString();

    Class<? extends Annotation> annotationType();
}
```

### Annotation Processor

> **컴파일 시 어노테이션을 분석**하고 **처리**하는 기능을 수행
>
- 컴파일러에 의해 호출되어 소스 코드에 적용된 어노테이션을 분석하고, 그에 따라 추가적인 작업을 수행
- **`javax.annotation.processing.Processor`** 인터페이스를 구현
- Lombok이 대표적인 예시이다.

```java
public interface Processor {
    
    Set<String> getSupportedOptions();

    Set<String> getSupportedAnnotationTypes();

	  SourceVersion getSupportedSourceVersion();

    void init(ProcessingEnvironment processingEnv);

    boolean process(Set<? extends TypeElement> annotations,
                    RoundEnvironment roundEnv);
    
    Iterable<? extends Completion> getCompletions(Element element,
                                                  AnnotationMirror annotation,
                                                  ExecutableElement member,
                                                  String userText);
}
```

1. **`getSupportedOptions`**
    - 해당 프로세서가 인식하는 옵션들의 이름을 반환
        - 옵션은 주로 컴파일러나 빌드 도구에 특정 프로세서에 대한 설정을 제공하는 데 사용
2. **`getSupportedAnnotationTypes`**
    - 프로세서가 지원하는 어노테이션 타입들의 이름을 반환합니다.
    - 각 문자열은 지원하는 어노테이션 타입을 나타냅니다.
    - **`"*"`**를 반환하면 모든 어노테이션을 지원한다는 의미입니다.
3. **`getSupportedSourceVersion`**
    - 프로세서가 지원하는 소스 버전을 반환합니다.
    - 어노테이션 프로세서가 지원하는 가장 최신의 소스 버전을 반환해야 합니다.
4. **`init`**
    - 프로세서를 초기화합니다.
    - **`ProcessingEnvironment`**를 통해 프로세서가 컴파일 환경에 접근할 수 있습니다.
5. **`process`**
    - 주요 로직이 구현되는 메서드로, 주어진 어노테이션 타입에 대한 처리를 수행합니다.
    - **`annotations`** 매개변수에는 처리해야 할 어노테이션 타입들이 전달되며, **`roundEnv`**에는 현재 및 이전 라운드에서의 정보가 전달
    - 반환값이 **`true`**이면 해당 어노테이션 타입들이 이 프로세서에 의해 처리되었음을 나타내고, **`false`**이면 이어지는 프로세서에게 넘김
6. **`getCompletions`**
    - 어노테이션에 대한 자동 완성을 제공
    - 주어진 어노테이션, 요소, 멤버, 사용자의 텍스트 등을 기반으로 완성될 수 있는 후보들을 나타내는 **`Completion`**의 반복 가능한 객체를 반환
