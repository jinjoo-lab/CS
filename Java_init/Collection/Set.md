# Set

## Set

> **순서가 없고 중복을 허용하지 않는** 데이터 집합
>
- 대표적인 구현체
    - HashSet
    - LinkedHashSet < 순서 유지 >
    - TreeSet

### HashSet

- Set 인터페이스의 구현 컬렉션
- **중복을 허용하지 않고 순서가 유지되지 않는다.**
- HashSet은 내부적으로 **HashMap을 이용**하여 만들어졌다.
    - 즉 데이터 자체를 HashMap의 Key값으로 이용하는 것이다.
    - Map의 **Key 자체가 중복이 허용되지 않기 때문에** 이를 이용하는 것이다.
    - PRESENT란 빈 Object 객체를 의미한다.

```java
public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable {
    static final long serialVersionUID = -5024744406713321676L;

    private transient HashMap<E,Object> map;

		public HashSet() {
        map = new HashMap<>();
    }

		public boolean contains(Object o) {
        return map.containsKey(o);
    }

		public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }

		public boolean remove(Object o) {
        return map.remove(o)==PRESENT;
    }
}
```

### hashcode()

- 객체의 고유 정수값을 반환하는 메서드 , Object 클래스에 정의되어 있는 기본 메서드
- Hash 계열의 컬렉션 클래스를 사용할 때 **중복 확인을 위해 사용**된다.
- 사용자 정의 클래스에 대하여
    - equals() 메서드가 정의되어 있어도 hashcode() 메서드가 정의되어 있지 않다면 중복 객체로 바라보지 않는다.
    - 즉 중복 저장된다는 것이다.

```java
class Node{
    int x;
    int y;

    Node(int x,int y){
        this.x = x;
        this.y = y;
    }

    @Override
    public boolean equals(Object obj) {
        return this.x == this.y;
    }
}

public class Test {
	public static void main(String[] args){
				Node n1 = new Node(1,2);
        Node n2 = new Node(1,3);

        HashSet<Node> set = new HashSet<>();
        set.add(n1);
        set.add(n2);

        System.out.println(set.size()); // 2
	}
}
```

### TreeSet

- Set 인터페이스의 구현체
- **레드 - 블랙 트리** (이진 검색 트리의 성능 향상)로 구현되어 있다.
- 중복된 데이터를 허용하지 않고 순서가 유지되지 않는다.
    - 하지만 정렬된 위치에 저장되어 있다.
- TreeSet 또한 내부적으로 TreeMap으로 이루어져 있다.

```
public TreeSet() {
        this(new TreeMap<>());
}

public TreeSet(Comparator<? super E> comparator) {
        this(new TreeMap<>(comparator));
}
```

### 정렬 기준

- TreeSet은 트리 형태로 구현되어 있다. 그리고 해당 트리는 레드 블랙 트리 근본적으로는 이진 검색 트리이다. 즉 TreeSet에 저장되는 데이터는 내부적으로 **항상 정렬 상태를 유지**한다는 것이다.
- 사용자 클래스 타입을 TreeSet에 저장할 경우 정렬 기준을 선정해야 한다. 그렇지 않을 경우 오류가 발생한다.
    - 즉 Comparable 또는 Comparator 인터페이스를 통해 정렬 기준을 알려줘야 한다.

```java
public class TreeSetTest {
    public static void main(String[] args) {

        TreeSet<Node> set = new TreeSet<>();
        
        set.add(new Node(1,1));
        set.add(new Node(2,1));
    }
}

class Node{
    int x;
    int y;

    Node(int x,int y){
        this.x = x;
        this.y = y;
    }

}
```

**오류**

```
Exception in thread "main" java.lang.ClassCastException: class chapter08.Node cannot be cast to class java.lang.Comparable (chapter08.Node is in unnamed module of loader 'app'; java.lang.Comparable is in module java.base of loader 'bootstrap')
	at java.base/java.util.TreeMap.compare(TreeMap.java:1291)
	at java.base/java.util.TreeMap.put(TreeMap.java:536)
	at java.base/java.util.TreeSet.add(TreeSet.java:255)
	at chapter08.SortTest.main(SortTest.java:15)
```

### HashSet VS TreeSet

- TreeSet은 내부적으로 정렬 상태를 유지하기 때문에 **범위 검색에 특화**되어 있다.
- HashSet은 내부적으로 Hash를 사용하기 때문에 **빠른 삽입**과 **단일 검색**이 가능하다.

### LinkedHashSet

- 순서가 유지되고 중복이 불가능한 Set
- HashSet 클래스를 상속받아 구현되어 있다.
- **어떻게 순서 유지가 가능할까?**

  [A Guide to LinkedHashSet in Java | Baeldung](https://www.baeldung.com/java-linkedhashset)

  위 글에 의하면 내부적으로 모든 Entry를 연결시켜놓은 DoubleLinkedList를 가지고 있다고 한다.


```
// 생성자를 타고 거슬러 올라간다면
HashSet(int initialCapacity, float loadFactor, boolean dummy) {
        map = new LinkedHashMap<>(initialCapacity, loadFactor);
 }
```

- 즉 내부적으로 LinkedHashSet은 LinkedHashMap을 가지고 있다.

```
static class Entry<K,V> extends HashMap.Node<K,V> {
        Entry<K,V> before, after;
        Entry(int hash, K key, V value, Node<K,V> next) {
            super(hash, key, value, next);
        }
    }

    private static final long serialVersionUID = 3801124242820219131L;

    /**
     * The head (eldest) of the doubly linked list.
     */
    transient LinkedHashMap.Entry<K,V> head;

    /**
     * The tail (youngest) of the doubly linked list.
     */
    transient LinkedHashMap.Entry<K,V> tail;
```

- Head와 Tail의 더미 노드를 가지고 있다. (DoubleLinkdeList 형태)
- before , after를 통해 양방향으로 연결이 되어 있다.
