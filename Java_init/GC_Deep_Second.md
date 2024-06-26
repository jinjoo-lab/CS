# GC (2)

# GC 알고리즘

## 세대 단위 컬렉션

> 세대 단위 컬렉션이란 자바 힙(객체의 상주 공간)을 관리하는 방법의 패러다임으로서 핵심은 ‘모든 객체를 동일하게 바라봐야 하는가?’이다.
>

### 약한 세대 가설

- 대다수의 객체의 생명 주기가 짧다.

### 강한 세대 가설

- GC 과정에서 살아남은 객체일수록 더욱더 살아남을 가능성이 높다.

### 의미하는 바 ?

> 자주 참조되는 객체는 정해져 있고 해당 객체와 단타성 객체를 한번에 관리하는 것은 GC에 있어 효율을 떨어뜨리는 가장 큰 이유이다. 이들을 구분하여 관리하는 것이 합리적인 방법이라는 것이다.
>

### 세대 단위 컬렉션

- Java 힙 영역을 구분하고 각 객체들을 기준에 맞게 나누어 각기 다른 영역에서 관리하는 것
    - 기준 : Age (GC과정에서 살아남은 횟수)
    - 이점 : 영역(Old , New)에 따라 다른 **GC 횟수 + 방법**을 통해 GC 시간을 줄이고 메모리 활용을 높인다.
- 세대를 나누고 각기 다른 GC를 명명하여 Minor GC, Major GC , Full GC라 한다.
- 영역에 따라 다른 GC 알고리즘을 사용한다.

> 세대 단위 컬렉션에서 Heap을 Old와 New의 큰 구분으로 나누고 New에서 GC를 통해 대부분의 객체를 죽이고 살아남은 객체를 New로 승격한다.
>

## 세번째 가설 : 세대 간 참조 가설

> 위에서 Java Heap을 크게 2가지(Old, New)로 나누고 관리한다 했다. 하지만 객체의 가장 큰 특징 중에 하나는 참조의 특성이다. 만약 Old 영역과 New 영역 객체 사이에 참조가 존재한다면?
>
- 세대 간의 참조의 개수는 같은 세대 안의 참조보다 훨씬 적다. 또한 이는 **일시적인 참조가 될 가능성이 높다.**
    - New 영역의 객체는 Old 객체와의 참조 덕분에 오래 생존할 가능성이 높고 이를 통해  New → Old 객체로 승격할 가능성이 높다.

### 기억 집합

- New 영역에 전역적으로 존재하는 데이터 구조로서 모든 참조 관계를 기록하는 것이 아니라 **구세대의 특정 조각에 세대 간 참조가 존재한다고 기록**
- Minor GC 시 기억 집합의 객체들만 GC 루트에 추가

## Mark - Sweep 알고리즘

- Mark : 객체의 도달 가능성을 파악하고 도달 불가능한 객체들을 기록 ( 반대도 가능 )
- Sweep : 도달할 수 없는 객체를 GC 대상으로 지정하고 청소(제거)

### 장점

- 기존 참조 카운팅 알고리즘의 단점이었던 **순환 참조의 문제를 해결**할 수 있다.

### 단점

> Mark - Sweep 알고리즘은 기본적인 GC 알고리즘이다. 그리고 향후 GC 알고리즘은 이러한 Mark - Sweep 알고리즘의 단점을 해결하기 위해 등장한 것이다.
>
- **실행 효율이 일정하지 않다.**
    - 객체의 수가 많다면 mark - sweep의 과정이 효율이 떨어진다.
- **메모리 파편화가 심하다.**
    - GC를 통해 청소되는 영역에 불연속적인 메모리 파편이 발생한다. (이는 나중에 **큰 객체를 생성할 때 충분한 크기의 연속 메모리**를 찾는데 어려울 수 있다.)

![image](https://github.com/jinjoo-lab/CS/assets/84346055/d6960295-d532-4ade-9f5f-e8abe5188304)

## Mark - Copy 알고리즘

- 객체의 수가 많아질수록 효율이 저하되는 Mark - Sweep 알고리즘을 개선하기 위해 등장

### 세미스페이스 복사

> Mark Copy 알고리즘의 핵심 패러다임으로서 **가용 메모리를 2개로 분할하여 GC에 사용**하는 것이다.
>
- 한 블록이 가득차면 GC를 수행하고 살아남은 객체를 다른 블록에 복사하고 기존 블록을 한번에 청소

### 장점

- 대다수의 객체가 회수되는 경우 복사 과정에서 성능이 개선된다.
- 또한 살아남은 객체를 다른 블록으로 이동시키는 과정에서 메모리 파편화를 해결할 수 있다.

### 단점

- 객체의 회수율이 낮다면 복사 과정에서 오버헤드가 크다. (대부분의 객체를 옮겨야 하기 때문)
- 또한 메모리 가용 공간이 줄어든다. (2개의 공간으로 나눠서 한쪽만 가용하기 때문)

> 이러한 특성에 의해 Mark - Copy 알고리즘은 객체의 수명주기가 작은 New 영역에서 사용된다.
>

### 무식하게 5 : 5 ?

> 앞서 Mark - Copy 알고리즘은 가용 메모리라는 개념을 통해 메모리 파편화를 해결하였다. 하지만 가용 메모리를 5:5로 사용해야 할까? 이러한 단순한 접근은 메모리적으로 큰 손해를 가져다 준다.
>
- 핵심은 New 영역의 메모리를 1 : 1로 나눌 필요가 없다는 것이다.

### 아펠 스타일 컬렉션 방식

> New 영역을 하나의 Eden 공간과 2개의 Survivor 공간으로 나눈다.
>
- 가용 메모리를 Eden과 1개의 Survivor를 사용함으로서 가용 메모리의 비율을 높이는 방식이다.
    - 핫스팟 가상 머신에서 비율은 8 : 1 : 1이다.
- 이러한 아펠 스타일 컬렉션 방식을 적용한 것이 우리가 흔히 아는 Minor GC인 것이다.

![image](https://github.com/jinjoo-lab/CS/assets/84346055/f221b4b3-398c-477e-82ac-0cea9934f4f7)

## Mark - Sweep - Compact 알고리즘

- 앞서 살펴본 Mark - Copy의 가장 큰 장점이자 동시에 단점은 객체의 생존율에 따른 것이다. 이러한 이유로 New 영역의 GC로 사용되는 것이었다. 하지만 Old 영역의 경우 대부분의 객체의 생존율은 높다 (강한 세대 가설)
- Old 영역의 객체 특징을 살려 Mark -Sweep 알고리즘에 Compact 과정을 적용한 새로운 알고리즘을 사용하기로 하였다.

### 과정

- Mark : 객체의 도달 가능성을 파악하고 도달 불가능한 객체들을 기록 ( 반대도 가능 )
- Sweep : 도달할 수 없는 객체를 GC 대상으로 지정하고 청소
- Compact : 생존한 객체를 메모리 영역의 한쪽 끝으로 모은 다음, 나머지 영역을 한꺼번에 비운다.

> 즉 메모리 이동이 일어난다는 것이다. 이를 통해 메모리 파편화를 해결할 수 있다. 하지만 이는 동시에 이동된 객체들의 기존 참조를 모두 갱신해야 하고 이는 **Stop - the - World의 시간이 증가되는 이유**이다.
>

![image](https://github.com/jinjoo-lab/CS/assets/84346055/ab11c4da-3ce6-4536-b02c-f7f6abf61b2a)

## 실제 GC는 이렇게 간단하지 않다 !

> 위에서 설명한 것은 단순한 GC 알고리즘이다. JVM에서 실제 사용되었고 사용되는 GC들은 위 알고리즘을 구현하기 위해 다양한 적용과 장치를 사용한다. 이를 이해한다면 뒤에 설명할 실제 GC 알고리즘에 대해 이해하기 수월할 것이다.
>

### 루트 노드 열거

- 도달 가능성 파악 알고리즘은 GC 루트로부터 참조 체인을 찾는 작업을 통해 수행된다.
- GC 루트 노드는 **주로 전역 참조**와 **스택 프레임**에 존재한다.

> 루트 노드 열거의 가장 중요한 점은 **사용자 스레드가 일시 정지하는 Stop - the - World가 발생**한다는 것이다. 일관성이 유지된 스냅샷에서 찾는 것이 오류를 발생시키지 않는 유일한 방법이기 때문이다.
>
- 그래서 대부분의 GC에서 루트 노드 열거에 대한 최적화를 ‘Stop-the-world를 없애자 !’가 아닌 ‘Stop-the-world의 시간을 줄이자’인 것이다.

### OopMap

- **루트 노드 열거의 최적화**를 위해 사용되는 **데이터 구조**
    1. 클래스 로딩 후 객체의 각 데이터 타입을 확인
    2. JIT 컴파일 과정에서 스택의 어느 위치와 어느 레지스터의 데이터가 참조인지 기록
        - 위 과정을 통해 추가적인 스캔 없이 빠르게 루트 노드를 열거할 수 있다.

### 안전 지점

> 참조 관계나 OopMap의 내용을 변경할 수 있는 명령어가 많을 경우 모든 명령어에 OopMap을 만들어 처리할 경우 메모리 낭비가 발생한다.
>
- 모든 명령어에 각각의 OopMap을 생성하지 않고 **안전 지점이라는 특정 위치**에만 기록
    - 안전 지점에 도달할 경우 GC 처리
    - 안전 지점 선정 기준으로는 명령어의 흐름 다중화 기점을 우선적으로 지정한다.

### 안전 지역

> 실행 중이 아닌 프로그램(프로세서를 할당 받지 못한 프로그램)은 JVM의 인터럽트에 응답이 불가능하기 때문에 안전 지점의 개념을 사용하기 어렵다. 이를 위해 ‘안전 지역’의 개념이 등장했다.
>
- 일정 코드 영역에서 참조 관계가 변하지 않음을 보장한다.
- 안전 지역으로 선정된다면 해당 영역 안에서는 GC 사용이 어디든 가능하다.

### 기억 집합 & 카드 테이블

> 세대 간 참조 문제를 해결하기 위해 New 영역에 기억 집합(데이터 구조)를 둔다.
>
- 비회수 영역에서 회수 영역을 가리키는 포인터를 기록
    - 단순 배열로 기억 집합을 구현한다면 공간과 관리 비용에서 낭비가 발생할 것이다.
- 카드 정밀도
    - **카드라는 레코드 하나**가 **메모리 블록 하나**에 매핑
        - 특정 레코드가 마킹되어 있다면 해당 블록에 세대 간 참조를 지닌 객체가 존재
- 카드 정밀도를 바탕으로 기억 집합을 구현한 것을 ‘카드 테이블’이라 한다.
    - 카드 테이블은 Java 언어 내에서 **바이트 배열**로 구현되어 있다.

```java
CARD_TABLE[this_address >> 9] = 1;

1. CARD_TABLE 원소 각각이 메모리 영역의 메모리 블록(카드 페이지)에 매핑
2. 0 , 1 , 2 -> 0x000 ~ 0x01FF / 0x200 ~ 0x03ff 식으로 매핑
3. 카드 페이지 하나는 하나 이상의 객체를 포함하고 이러한 객체 중 하나라도 '세대 간 참조'를 포함한다면 1로 표시
```

### 쓰기 장벽

- 카드 테이블의 원소가 1로 마킹되는 시점 ?
    - 다른 세대의 객체가 현 블록의 객체를 참조하면 카드 테이블의 원소가 1로 마킹된다.
- 마킹 시점
    - 인터프리터 방식이라면 바이트 코드를 해석하고 실행 할 때 수행하면 된다.
    - JIT 컴파일러 방식에서는 ‘기계어 자체’에 대한 진입이 필요하다.
- 쓰기 장벽 : **참조 타입 객체가 대입**되면 **Around Advice**가 생성되어 **대입 전후 추가 동작 수행**
    - AOP가 적용된다는 것이다.

## 동시 접근 가능성

> 객체 간의 참조 관계를 파악하기 위해 도달 가능성 분석 알고리즘을 수행하는데 이는 일관성이 보장되는 스냅샷 상태에서 전체 과정을 진행해야 한다. 즉 **사용자 스레드는 분석 과정 중 멈춰야 한다.**
>
- 루트 노드 열거 과정은 앞서 살펴본 OopMap 데이터 구조를 통해 최적화 작업을 수행했다.
- GC는 루트 노드 열거 후 **객체 그래프 탐색을 수행**하는데 이 과정은 Heap 크기에 비례한다.

> 객체 그래프 탐색과 사용자 스레드를 동시에 수행한다는 것은 일반적인 관점에서 오류가 발생할 것이라 생각한다. (사용자 스레드가 참조 관계를 임의로 변경하기 때문)
>
1. 죽은 객체를 살았다고 잘못 표시할 수 있다.
2. 살아 있는 객체를 죽었다고 표시할 수 있다. (치명적인 데이터 에러로 발생할 수 있기 때문에 반드시 해결해야 한다.)

### 해결 관점

- (**이미 탐색이 완료된 즉 도달 가능한 객체→ 새로운 객체**) 를 추가 : 새로운 객체는 도달이 가능하지만 삭제될 것이다.
- 탐색 중인 객체 (연결된 모든 객체에 대한 탐색이 완료되지 않은 상태) X→ 아직 탐색이 완료되지 않은 객체
    - 즉 참조를 삭제하는 것

> 객체 사라짐 문제를 해결하기 위해서는 **두 조건 중 한 개만 깨트리면 된다.**
>

### 증분 업데이트 - CMS 체택

- 첫 번째 조건을 깨트리는 방법이다.
- (**이미 탐색이 완료된 즉 도달 가능한 객체→ 새로운 객체**)가 추가되는 경우 **새로 추가된 참조를 따로 기록**
    - 동시 스캔이 완료되고 나서 재탐색 과정을 수행

### 시작 단계 스냅샷 - G1 / ZGC

- 두 번째 조건을 깨트리는 방식이다.
- 두 번째 조건이 발생하려 하면 해당 이벤트를 기록한다.
    - 동시 스캔이 끝난 후 다시 스캔 수행
    - 이때 **참조 관계 삭제 여부와 상관 없는 첫번째 상태의 그래프 스냅샷을 기준**으로 스캔한다.

> 증분 업데이트와 시작 단계 스냅샷을 통해 객체 그래프 탐색 과정 즉 Marking 과정을 일부분 사용자 스레드와 동시에 수행할 수 있다.
>

# 기본적 가비지 컬렉터

## Serial Collector

> ‘단일 스레드’로 동작하는 가비지 컬렉터
>
- New 영역의 GC는 Mark - Copy 알고리즘으로 동작
- Old 영역의 GC는 Mark - Compact 알고리즘으로 동작

![image](https://github.com/jinjoo-lab/CS/assets/84346055/d173e802-33d5-49ad-b672-cced14ca3ad2)

### 장점

- 단일 코어 환경에서 사용하기 적합한 컬렉터이다.

### 단점

- GC가 시작되면 다른 모든 스레드가 멈쳐야 하고 이러한 Stop - the -world의 시간이 단일 스레드의 동작으로 인해 길다.

## Paraller Collector - Java 8 Default

> New 영역에 대해 ‘마크 - 카피’ 알고리즘에 기초하며 멀티 스레드를 통해 GC를 병렬적으로 회수 ( Old 영역의 경우 mark - compact 사용
>

![image](https://github.com/jinjoo-lab/CS/assets/84346055/572b1e70-9073-4efd-8338-4ec71b6a2265)

### 특징

- **처리량 제어에 목표**를 두고 있다.
    - 처리량 = (사용자 코드 실행 시간) / (사용자 코드 실행 시간 + GC 실행 시간)
    - -XX:MaxGCPauseMillis : **가비지 컬렉션 정지 시간의 최댓값** 지정 가능
    - -XX:GCTimeRatio : **처리량을 직접 지정**
- Java 8의 Default GC 방식이다.

### 장점

- Serial GC와 비교하여 높은 처리량과 Stop - the -World 시간의 감소화
- 멀티 코어의 활용 가능

### 단점

- 여전히 긴 시간의 Stop - The - World를 가지고 있다.
    - Stop - the - world를 최적화하기 위해 멀티 스레드를 활용하지만 이 시간 동안 사용자 스레드들이 일시 정지해야 한다.

## CMS Collector ( Concurrent Mark Sweep )

> Mark와 Sweep 단계 모두를 사용자 쓰레드와 동시 수행한다. ( Stop - the- World 시간을 최소로 줄이기 위해 등장)
>
- GC 작업을 어플리케이션과 동시에 실행 가능한 획기적 방식이었지만 모든 것은 상대적이다. → G1 등장으로 Deprecated

![image](https://github.com/jinjoo-lab/CS/assets/84346055/e51167f0-1212-4db5-9b9e-725bdc9e8c03)

### 동작 과정

> **최초 표시 (Stop the world)** - 동시 표시 - **재표시 (Stop the world)** - 동시 쓸기
>
- 최초 표시 (inital Mark)
    - Stop - the - World (사용자 스레드가 멈춤)
    - **GC 루트와 직접적으로 연결된 객체들**만 표시
- 동시 표시 (Concurrent Mark)
    - **객체 그래프 전체를 탐색**
    - Gc thread와 사용자 스레드가 동시에 실행된다.
- 재표시 (Remark)
    - Stop - the - World (사용자 스레드가 멈춤)
    - **동시 표시 중 사용자 스레드에 의해 참조 관계가 변경된 객체들을 바로잡는다.**
- 동시 쓸기 (Concurrent Sweep)
    - GC 작업 수행

### 장점

- 사용자 스레드와 GC 스레드의 동시 수행( 동시 표시 + 동시 쓸기 중)으로 지연 시간 최소화

### 단점

- CMS는 **프로세서 자원에 매우 민감한 방법**이다.
    - 동시성을 추구하는 방식이기 때문에 프로세서의 연산 능력을 쓰레드들이 나누게 된다.
- CMS가 **부유 쓰레기를 처리하지 못해** **동시 모드 실패** 유발 가능성
    - 동시 표시와 동시 쓸기 중 사용자 쓰레드가 새로운 GC 대상 객체를 만들 수 있고 표시 단계가 이미 지나쳤다면 회수 대상이 안된다.
    - 이러한 부유 쓰레기가 많이 발생한다면 Full GC가 수행되므로 성능 상 떨어지게 된다.
- Mark - Sweep 방식이기 때문에 **메모리 파편화가 발생**한다.

## G1(Garbage First) Collector - Java 9 Default

![image](https://github.com/jinjoo-lab/CS/assets/84346055/cd7d697e-fe46-456c-a608-86f5fcf1af99)

# ZGC
