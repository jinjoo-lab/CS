# List

# Data Structure

> 컴퓨터 과학에서 효율적 연산과 관리를 가능하게 하는 **데이터의 조직, 관리, 저장**
>

# Collection

- 데이터 Group을 관리하기 위한 단일화된 구조
- Java에서 컬렉션 프레임워크는 인터페이스를 사용하여 구현한다.
- 종류
    - List : 순서가 있고 중복을 허용
    - Set : 순서가 없고 중복을 허용하지 않는다.
    - Map : 순서가 없고 Key는 중복 불가 + Value는 중복 허용

> List와 Set은 Collection Interface를 구현하지만 Map은 구조 상 분리되어 있다.
>
- 컬렉션 프레임웍의 모든 컬렉션 클래스들은 List , Set , Map 중의 하나를 구현하고 있다.

## List

> **순서가 있고 중복을 허용하는** 데이터의 집합
>
- 배열과 동일하게 index의 개념이 존재한다. ( 0 ~ size() -1 )까지
    - **중간 삽입**을 할 경우 index는 0부터 size()까지 가능하다.
    - 범위를 벗어나면 `IndexOutOfBoundsException` 이 발생한다.
    - `indexOf , lastIndexOf , contains , remove` 는 해당 메서드를 수행하기 내부적으로 equals 함수를 호출하여 객체가 동일한지 비교한다.
        - 즉 사용자 정의 클래스를 사용할 때에는 equals 메서드를 오버라이딩하는 것이 좋다.
    - `remove` 연산의  고찰
        - 데이터 타입이 Integer일 경우 remove 연산은? : 기본형으로 접근한다면 인덱스 삭제 , (Integer)로 오토박싱하면 데이터 삭제
- 가장 대표적인 종류
    - ArrayList
    - LinkedList

### ArrayList

- List 인터페이스의 구현체
- 내부적으로 **Object[]** 를 사용한다.

```
public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }
```

- 배열을 이용하기 때문에 데이터를 순차적으로 저장할 수 있다.
- index를 이용한 접근이 배열과 동일하기 때문에 **빠른 시간 내에 접근**할 수 있다.
- 중간 삽입과 삭제 시 뒤의 데이터가 전부 이동(index를 맞추기 위해)하기 때문에 효율적이지 못하다.

### 생성에 대한 고찰

- ArrayList를 기본 생성자를 통해 생성할 경우 크기가 0인 배열 생성
    - 값을 한개라도 추가하면 기본 크기 10으로 지정
- 이 과정에 있어 grow와 newCapactity 메서드가 호출

> ArrayList는 내부적으로 배열을 사용하기 때문에 용량이 가득 찬 경우 **기존의 배열을 버리고 증가된 크기의 배열을 다시 할당**한다.
>
- 즉 ArrayList 사용 시 사용할 크기를 알면 위의 작업을 생략하여 단축할 수 있다.

## LinkedList

![Untitled](https://private-user-images.githubusercontent.com/84346055/299671840-3b53589e-1908-4ba5-90e1-9b37f0560e47.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDYxODc0NTUsIm5iZiI6MTcwNjE4NzE1NSwicGF0aCI6Ii84NDM0NjA1NS8yOTk2NzE4NDAtM2I1MzU4OWUtMTkwOC00YmE1LTkwZTEtOWIzN2YwNTYwZTQ3LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMjUlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTI1VDEyNTIzNVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTEwODhjNjE5NzI1MGNlYTcwZDI5YTI5N2VlNTFlOWNkY2JmOTY4NmZiMzhiMDFjYjc2MGRmMTk3MjdjYmZmYTQmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.QerbuiGwVNa-e4NayJm-3dx-8ZyC4ZG5TByi4uSEh-E)

- List 인터페이스의 구현체
- DoubleLinkedList 자료구조의 형태로 구현되어 있다. (previous 와 next로 양방향 접근)
- 내부적으로 Node 클래스를 사용한다.
    - 데이터를 추가할 때마다 Node 클래스를 생성하여 List에 이어붙인다.

```java
private static class Node<E> {
        E item;
        Node<E> next; // 이전 노드를 가리킴 (c언어에서 pointer로 가리키는 것과 비슷)
        Node<E> prev; // 다음 노드를 가리킴

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
```

### ArrayList VS LinkedList

- 순차적으로 추가 / 삭제하는 경우 ArrayList가 LinkedList보다 빠르다.
- 중간 데이터를 추가 / 삭제하는 경우 LinkdedList가 더 빠르다.

![Untitled](https://private-user-images.githubusercontent.com/84346055/299671875-ed206e87-36dc-4763-9c03-4d005a1d91be.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDYxODc0NTUsIm5iZiI6MTcwNjE4NzE1NSwicGF0aCI6Ii84NDM0NjA1NS8yOTk2NzE4NzUtZWQyMDZlODctMzZkYy00NzYzLTljMDMtNGQwMDVhMWQ5MWJlLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMjUlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTI1VDEyNTIzNVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWJkYjc1N2RhOWJmN2E0N2ZmNTRkNWI5OTM0ZTE4OWI4MjcwZTYxMzA4ZjJhZWIxOWM5YjcxZjhkMDY1NDBkMDMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.GWmEfz3w_haAYrhru7GuggfalWltL8BdsSY-Rn3R6QU)

### Iterator

- 컬렉션에 저장된 요소를 접근하는데 사용하는 인터페이스
    - cursor를 통해 위치를 옮기면서 탐색
- Collection 인터페이스를 구현한 컬렉션 클래스에 대해서 사용 가능
    - Map은 바로 접근할 수 없고 KeySet이나 Values , EntrySet으로 변환하여 사용 가능
- Iterator는 재사용 불가
    - 한번 순회가 끝났다면 다시 순회할 수 없기 때문에 iterator 객체를 다시 받아야 한다.
    - ListIterator를 통해 되돌아가는 로직을 추가한다면 활용이 가능하다. (List 계열만 사용 가능)
- 주요 메서드
    - hasNext()
    - next()
    - **remove()**

> Iterator의 remove() 메서드가 호출된다는 것은 **실제 컬렉션에서 해당 데이터가 삭제**된다.
>

```
    public void remove() {
        if (lastRet < 0)
            throw new IllegalStateException();
        checkForComodification();

        try {
            ArrayList.this.remove(lastRet); // 실제 삭제가 수행
            cursor = lastRet;
            lastRet = -1;
            expectedModCount = modCount;
        } catch (IndexOutOfBoundsException ex) {
            throw new ConcurrentModificationException();
        }
    }
```

### Comparator & Comparable

- **정렬 기준을 선정**하기 위한 인터페이스 : 컬렉션의 정렬 기준으로 사용된다.
- Comparable
    - **기본 정렬 기준을 구현**하는데 사용 : Arrays.sort 나 Collections.sort에서 다른 인자 미 사용시 해당 인터페이스가 구현된 클래스라면 기본 기준으로 사용된다.
- Comparator
    - **다른 정렬 기준으로 정렬**하고자 할 때 사용

### Comparable

```java
public interface Comparable {
	public int compareTo(T o);
}

// 예시
class Node implements Comparable<Node>{
    int x;
    int y;
    
    Node(int x,int y){
        this.x = x;
        this.y = y;
    }
    
    @Override
    public int compareTo(Node n){
        return this.x - n.x;
    }
}
```

- 반환 결과가 1 , 0 , -1이다.
    - 비교 기준에 있어 자기 자신이 상대방(인자)보다 크다면 1
    - 같다면 0
    - 작다면 -1
- 기억하기 쉽게 !
    - 오름 차순 : this  - o
    - 내림 차순 : o - this

### Comparator

```
public interface Comparator {
	public int compareTo(T 1, T 2);
}
```

- 오름차순 : 1 - 2
- 내림차순 : 2 - 1

> Comparable이나 Comparator 클래스에서 -(마이너스) 연산 시 오버플로우가 발생할 수 있으니 주의해야 한다. Wrapper 클래스에서 지원해주는 Compare메서드를 활용하는 것을 추천한다.
>
- Comparator의 구현은 **람다식 or 익명객체**를 사용할 수 있다.
