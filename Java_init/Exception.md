# Exception

# 프로그램 오류

> 프로그램이 **오작동하거나 비정상적으로 종료**되는 경우 혹은 **정상적인 수행이 불가능한 경우**
>
- 자바에서는 이러한 오류를 **객체로 관리**한다.
- 자바에서 예외와 오류는 **계층적 구조**를 가지고 있다.

## Throwable

> 자바에서 다루는 모든 **예외나 오류의 최상위 부모 클래스** , 예외나 오류는 결국 Throwable 클래스를 상속받는다.
>

### 주요 메서드

- getMessage() : 예외에 대한 **상세한 메시지(사용자 정의 가능)**를 반환한다.
- printStackTrace() : 예외의 **발생 경로**를 출력 (호출스택에 있던 메서드 정보와 예외 메시지)
- getCause() : 예외의 원인을 반환 , **예외 체인**을 구현하는데 사용

```
public synchronized Throwable getCause() {
        return (cause==this ? null : cause);
    }
```

- Throwable 클래스를 상속받은 가장 대표적인 sub class
    - Error
    - Exception

### Error

> 프로그램 코드에 의해 **코드 내에서 처리될 수 없는 오류**
>
- System레벨에서 발생하므로 프로그래머가 내부적으로 처리하기 어렵다.
- Error는 주로 디버깅을 통해 처리한다.

**대표적인 예**

- **OutOfMemoryError**
    - heap 영역의 메모리가 부족한 경우
- **StackOverFlowError**
    - Stack 메모리 부족 : 재귀 함수가 무한 호출되는 경우
- **NoSuchMethodError**
    - main() 메서드가 없는데 프로그램이 수행된 경우

### Exception

> 개발자가 구현한 **로직에서 발생**하여 **수습 가능한** 오류
>
- 예외 처리한 주로 Exception 클래스에 대한 처리를 의미한다.
- Checked Exception
- Unchecked Exception

### Exception Handling

> 예외 발생 시 **프로그램의 비정상 종료를 막고 정상적인 실행 상태를 유지**하는 것
>
- 예외의 감지
- 예외의 처리

![Untitled](https://private-user-images.githubusercontent.com/84346055/298597011-a2525c5c-b14b-4976-b82c-104b1487d070.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDU5Mjk2MzksIm5iZiI6MTcwNTkyOTMzOSwicGF0aCI6Ii84NDM0NjA1NS8yOTg1OTcwMTEtYTI1MjVjNWMtYjE0Yi00OTc2LWI4MmMtMTA0YjE0ODdkMDcwLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMjIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTIyVDEzMTUzOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTJiOWYxYTY4MjdhNTA3OTViMDkwYmZmZWQwMTMwNTU2NWIwZjJjNzA5MzIwZTM4NmQ0NDJiMzJkMzE5NDI2NGQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.RfrFzaqjafOLN3S0LwO2eA9K0-FUa6yUF-hExmCOexk)

### Unchecked Exception (Runtime Error)

- 컴파일 단계에서 드러나지 않고 **Runtime에서 발생하는 Exception**
- 예외처리 하지 않아도 **컴파일 Error가 발생하지 않는다.**
- 종류
    - RuntimeException과 RuntimeException을 상속받는 클래스들

> 주로 프로그래머의 실수에 의해서 발생한다. 즉 프로그래밍적 요소와 관계가 깊다.
>

### Checked Exception (Compile Error)

- 예외 처리하지 않으면 **컴파일 Error**가 발생한다.
- 반드시 예외 처리해야 한다.
- 종류 (Exception)
    - SQLException
    - FileNotFoundException
    - IOException

> 외부의 영향을 받는 예외
>

### 대표적인 예시들

| Exception | 설명 | 종류 |
| --- | --- | --- |
| ArrayIndexOutOfBoundsException | 배열의 접근 범위를 벗어난 경우 | UnCheckedException |
| NegativeArraySizeException | 배열의 크기를 음수로 지정한 경우 | UnCheckedException |
| NullPointerException | 객체를 선언만 하고 생성하지 않은 상태에서 접근한 경우 | UnCheckedException |
| ClassCastException | Reference 타입의 잘못된 형변환시 발생 | UnCheckedException |
| NumberFormatException | 문자열로된 데이터를 Primitive로 변환시 잘못 변경하면 발생 | UnCheckedException |
| IndexOutOfBoundsException | List에서 잘못된 index에 데이터를 추가한 경우 | UnCheckedException |
| ArithmeticException | 0으로 나눈 경우 | UnCheckedException |
| IllegalArgumentException | 메서드에 잘못된 인자를 전달한 경우 | UnCheckedException |
| InterruptedException | 쓰레드 수행중 중단 명려에 따른 오류 | UnCheckedException |
| FileNotFoundException | 지정한 경로에 파일이 없는 경우 | CheckedException |
| IOException | 데이터를 IO하는 중 발생하는 오류 | CheckedException |
| SQLException | DB서버에서 데이터 처리 중 발생하는 오류 | CheckedException |

## Exception Handling

### 예외 처리하기 (try ~ catch)

```
try{
	// 예외가 발생할 수 있는 구문들
}catch(Exception1 e){
	// Exception1이 발생한 경우 처리하기 위한 구문 삽입
}catch(Exception2 e){
	// Exception2가 발생한 경우 처리하기 위한 구문 삽입
}
```

- try 블럭은 여러 종류의 예외를 처리할 수 있도록 하나 이상의 catch 블럭 사용 가능
    - **발생한 예외의 종류와 일치하는 단 한 개의 catch 블럭만 수행**된다.
- try 블럭 내 예외가 발생한 경우
    1. 발생한 예외와 일치하는 혹은 부모 타입의 **catch 블럭을 찾는다.**
    2. 찾은 경우 → **catch 블럭 수행** (찾지 못한 경우 예외를 처리하지 못해 비정상적 종료)
    3. 전체 **try - catch 블럭을 빠져나와서 계속 수행**
- try 블럭 내 예외가 발생하지 않은 경우
    1. catch 블럭을 거치지 않고 프로그램이 수행된다.

```java
class ExceptionTest{
	public static void main(String[] args){
		System.out.println("1");
	  System.out.println("2");

		try{
			System.out.println(0/0);
			System.out.println("3");
		}catch(Exception e){
			System.out.println("4");
		}
		System.out.println("5"); // 은근 헷갈리는 것들 이거 수행됩니다 !!!!
	}
}
```

- 위 예시를 기준으로 3을 제외한 모든 출력문이 수행된다. 즉 오류를 try ~catch 문으로 처리한 경우 try ~ catch 문 밖의 문장 또한 수행된다는 것이다.
- catch(Exception e)
    - 선언된 참조 변수 e와 발생한 예외 인스턴스는 **instanceof 연산자가 내부적으로 수행**되며 일치하는 것을 찾아간다.
    - 즉 **다형성이 적용되는 것이다 !**

### 멀티 catch 블럭

```
catch(ExceptionA | ExceptionB e) // JDK 1.7부터 가능
```

- 만약 ‘|’ 연산자로 연결된 예외들이 **조상 , 자식 클래스 관계**라면 컴파일 에러
- 하지만 구체적으로 어떤 예외인지 파악하기 위해서는 내부적으로 다시 instanceof 연산자를 통한 if 문 추가 !
    - 바로 특정 예외를 규정하는 메서드 호출 시 에러 발생

### 예외 발생시키기

- **throw**

```
throw new Exception("고의로 발생시켰음");
```

### 예외 위임하기 (throws)

- 메서드에 예외 선언하기
- 메서드에 예외를 선언하려면 , 메서드의 선언부에 키워드 throws를 사용

```
void method() throws Exception1, Exception2 ... ExceptionN {}
```

- 메서드에 예외를 선언할 때에는 **상속 관계 또한 파악**해야 한다.

> 예외를 메서드의 throws에 명시하는 것은 예외를 처리하는 것이 아니라, **자신을 호출한 메서드에게 예외를 전달**하여 예외처리를 떠맡기는 것
>
- 예외를 처리하는 것이 아닌 **예외를 전달하는 것**이다.
    - 즉 예외를 발생시킬수 있는 메서드나 실제 예외가 발생되는 코드가 있다면 자신을 호출한 메서드에 예외를 위임하는 것이다.

```java
class MyException extends Exception{
    MyException(String msg){
        super(msg); 
    }
}

class MyUnchecked extends RuntimeException{
    public MyUnchecked(String msg){
        super(msg);
    }
}

class ExceptionUse{
		// Runtime Error
    public static int mod(int i, int j){
        if(j == 0) throw new MyUnchecked("0d으로 나눌 수 없다");
				return i / j;
    }
		
		// Compile Error
    public static int div(int i, int j) throws MyException{
        if(j == 0) throw new MyException("0d으로 나눌 수 없다");
				return i / j;
    }
}

public class ExceptionTest5 {
    public static void main(String[] args) {
        
				ExceptionUse.mod(256,8); // Runtime Error

        try{
            ExceptionUse.div(256,8); // Compile Error
        }catch( MyException e){
            e.printStackTrace();
        }

    }
}
```

### Compile Error (Checked Exception)

```
 		// Compile Error
    public static int div(int i, int j) throws MyException{
        if(j == 0) throw new MyException("0d으로 나눌 수 없다");
				return i / j;
    }
```

- throw 발생시킨 경우 해당 로직 내에서 처리하지 못한 경우 반드시 throws로 위임해야 한다.

### finally

- 예외의 발생 여부에 상관없이 실행되어야 할 코드 포함
- try - catch - finally
- 단 !!!
    - **System.exit(~)에 의해 JVM이 종료되면 finally 문은 수행되지 않는다**

```
try{
	
}catch(Exception e){

}finally{
	// 예외의 발생 여부에 관계없이 항상 수행되어야 하는 문장
	// 맨 마지막에 위치해야 한다.
}
```

> try블럭이나 catch블럭에서 **return문이 실행되는 경우 finally 블럭의 문장이 먼저 실행**되고 나서 메서드가 종료된다.
>

### 사용자 정의 예외

- RuntimeException 클래스를 상속 받아 예외 정의
- Exception 클래스를 상속 받아 예외 정의 (반드시 처리되어야 할 예외)

### 처리 VS 위임

- 정답이 존재하는 것은 아니다. 자신이 구현할 서비스의 로직을 파악하는 것이 중요하다.
- 언제 던질까 ? (API 개발 시)
    - 오류가 발생한 곳에서 직접 처리하면 처리 방법이 고정되므로 메서드 호출한 곳에 맞게 처리되지 않는다.
    - 메서드 호출한 곳에서 맞게 처리하도록 처리 방법을 위임 , 예외 처리의 다양성
- 다양한 함수에서 여러 오류가 발생하지만 프로그램 내에서 처리하는 방법은 제한되어 있는 경우 (로그) 각각의 함수에서 똑같은 예외 처리를 하면 코드 재사용이 안되고 수정이 용이하지 않으므로 오류를 한 곳(이벤트 처리하는 곳, 클라이언트의 요청을 받는 곳) 으로 던져서 한번에 처리한다.
- 메서드가 정상적으로 처리된 경우 결과를 return을 통해서 전달하고 메서드에 문제가 발생한 경우 예외를 통해 메서드 호출한 곳으로 전달한다.
