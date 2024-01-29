# Map

## Map

- 데이터를 Entry(Key : Value) 형태로 저장하고 있다.
    - Key는 중복이 불가능하다.
        - 즉 해당 키로 검색했을 때 **결과가 단 하나**이여야 한다.
    - Value는 중복이 가능하다.
- 데이터를 접근하기 위해서는 Key값을 이용해야 한다.
- 저장 순서가 유지되지 않는다.
- 대표적인 구현체
    - HashMap
    - TreeMap

  [Java HashMap은 어떻게 동작하는가?](https://d2.naver.com/helloworld/831311)


### HashMap

- **해싱**을 사용하는 Map
- 간단한 용어
    - **해시 테이블** : 해시 버킷을 저장하는 배열
    - **Node** : 원래는 Entry였지만 자바 8부터는 Node를 사용한다. 각 해시 버킷에 저장되는 **키 - 값 쌍**
    - 충돌 관리 기법 (**Key의 Hash 값에 중복이 발생한 경우**)
        - Java 7에서는 (Seperate Chaining) 링크드 리스트
        - Java 8부터는 TreeMap

> 해시 버킷 (Key - Value) 쌍 또한 내부적으로 배열로 관리된다. 그리고 다른 컬렉션 클래스와는 다르게 크기 확장 시 2배로 설정하여 새로운 버킷 배열을 생성한다. 즉 이 과정에서 손실이 발생하기 때문에 생성 시 적절한 크기를 지정하도록 하자
>

### Node

- Key : Value 형태의 데이터
- Key와 Value에 모두 null 값 허용

```java
static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;

        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }

        public final K getKey()        { return key; }
        public final V getValue()      { return value; }
        public final String toString() { return key + "=" + value; }

        public final int hashCode() {
            return Objects.hashCode(key) ^ Objects.hashCode(value);
        }

        public final V setValue(V newValue) {
            V oldValue = value;
            value = newValue;
            return oldValue;
        }

        public final boolean equals(Object o) {
            if (o == this)
                return true;
            if (o instanceof Map.Entry) {
                Map.Entry<?,?> e = (Map.Entry<?,?>)o;
                if (Objects.equals(key, e.getKey()) &&
                    Objects.equals(value, e.getValue()))
                    return true;
            }
            return false;
        }
    }
```

![Untitled](https://private-user-images.githubusercontent.com/84346055/300483297-7bee95b0-6f57-448a-9fd8-9d71dd9aa1be.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDY1MzY1NDksIm5iZiI6MTcwNjUzNjI0OSwicGF0aCI6Ii84NDM0NjA1NS8zMDA0ODMyOTctN2JlZTk1YjAtNmY1Ny00NDhhLTlmZDgtOWQ3MWRkOWFhMWJlLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMjklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTI5VDEzNTA0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTAzOTIyNTA3N2U5YTY5ODcyZTMyMWE3ODM0MjQ0ZDAyNGQ2YzllOWMxMGRhZTRkN2NiMGMxNGZlZTYwNTc2OTgmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.RxDyYIk6yINYBQIPvRlQ0pZ6IUnkgKtSdSmEoMwUQII)

### HashMap VS HashTable

- **Thread-safe 여부**
    - Hashtable은 Thread-safe하고, HashMap은 Thread-safe하지 않다는 특징을 가지고 있습니다.
    - 멀티스레드 환경이 아니라면 Hashtable은 HashMap 보다 성능이 떨어진다는 단점을 가지고 있습니다.
- **Null 값 허용 여부**
    - Hashtable은 key와 Value에 null을 허용하지 않지만, HashMap은 key와 Value에 null을 허용
- **HashMap은 보조해시를 사용**
    - 보조 해시 함수를 사용하지 않는 Hashtable에 비하여 해시 충돌(hash collision)이 덜 발생할 수 있어 상대적으로 성능상 이점

### Deep In HashMap

[Java HashMap은 어떻게 동작하는가?](https://d2.naver.com/helloworld/831311)

- 해시 과정이 진행되는데 있어 키 값에 대해 동일한 해시 값이 생성되는 경우가 있다. 이러한 경우를 우리는 **해시 충돌**이라 한다.

### Java 7

- Java 7까지는 이러한 충돌에 대응하기 위해 HashMap에 링크드 리스트를 통한 Chaining을 사용했었다.

![Untitled](https://private-user-images.githubusercontent.com/84346055/300483332-36107220-2ddf-4f6e-968d-01862c23c655.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDY1MzY1NDksIm5iZiI6MTcwNjUzNjI0OSwicGF0aCI6Ii84NDM0NjA1NS8zMDA0ODMzMzItMzYxMDcyMjAtMmRkZi00ZjZlLTk2OGQtMDE4NjJjMjNjNjU1LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMjklMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTI5VDEzNTA0OVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTA5MTBhNjA2MjFkN2UyZWJhZTVlNWI3MjlhOGMwOTY5YmRhM2RjZDhlOWVmOTYwYTA3MTliN2JhNjBjMTkwMDgmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.jmrCAowZvwvxPxYusfms0247EWC0hKPAvfJMD_yGVJ8)

- 다음 그림과 같이 Hash값이 겹치는 경우 해당 버켓의 링크드 리스트를 통해 데이터를 유지했다.
- 이러한 경우 Hash가 중복된 데이터에 대해 **O(N)의 시간 복잡도**가 발생한다.

```
public V put(K key, V value) { if (table == EMPTY_TABLE) { inflateTable(threshold); 
// table 배열 생성 } // HashMap에서는 null을 키로 사용할 수 있다. 
// if (key == null) return putForNullKey(value); // value.hashCode() 메서드를 사용하는 것이 아니라, 보조 해시 함수를 이용하여 
// 변형된 해시 함수를 사용한다. "보조 해시 함수" 단락에서 설명한다.  

        int hash = hash(key);

        // i 값이 해시 버킷의 인덱스이다.
        // indexFor() 메서드는 hash % table.length와 같은 의도의 메서드다.
        int i = indexFor(hash, table.length);

        // 해시 버킷에 있는 링크드 리스트를 순회한다.
        // 만약 같은 키가 이미 저장되어 있다면 교체한다.
        for (Entry<K,V> e = table[i]; e != null; e = e.next) {
            Object k;
            if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }

        // 삽입, 삭제 등으로 이 HashMap 객체가 몇 번이나 변경(modification)되었는지
        // 관리하기 위한 코드다.
        // ConcurrentModificationException를 발생시켜야 하는지 판단할 때 사용한다.
        modCount++;

        // 아직 해당 키-값 쌍 데이터가 삽입된 적이 없다면 새로 Entry를 생성한다. 
        addEntry(hash, key, value, i);
        return null;
    }
```

### Java 8

- Java 8에서는 이러한 O(N)의 시간 복잡도를 줄이기 위해 Seperate Chaining에서 **레드 - 블랙 트리**를 사용한다.
- 그렇기에 시간 복잡도는 **O(logN)**으로 줄일 수 있다.

```
transient Node<K,V>[] table;

static class Node<K,V> implements Map.Entry<K,V> {  
  // 클래스 이름은 다르지만, Java 7의 Entry 클래스와 구현 내용은 같다. 
}

// LinkedHashMap.Entry는 HashMap.Node를 상속한 클래스다.
// 따라서 TreeNode 객체를 table 배열에 저장할 수 있다.
static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {

        TreeNode<K,V> parent;  
        TreeNode<K,V> left;
        TreeNode<K,V> right;
        TreeNode<K,V> prev;   

        // Red Black Tree에서 노드는 Red이거나 Black이다.
        boolean red;

        TreeNode(int hash, K key, V val, Node<K,V> next) {
            super(hash, key, val, next);
        }

        final TreeNode<K,V> root() {
        // Tree 노드의 root를 반환한다. 
        }

        static <K,V> void moveRootToFront(Node<K,V>[] tab, TreeNode<K,V> root) {
        // 해시 버킷에 트리를 저장할 때에는, root 노드에 가장 먼저 접근해야 한다.
        }

        // 순회하며 트리 노드 조회 
        final TreeNode<K,V> find(int h, Object k, Class<?> kc) {}
        final TreeNode<K,V> getTreeNode(int h, Object k) {}

        static int tieBreakOrder(Object a, Object b) {
         // TreeNode에서 어떤 두 키의comparator 값이 같다면 서로 동등하게 취급된다.
         // 그런데 어떤 두 개의 키의 hash 값이 서로 같아도 이 둘은 서로 동등하지 
         // 않을 수 있다. 따라서 어떤 두 개의 키에 대한 해시 함수 값이 같을 경우, 
         // 임의로 대소 관계를 지정할 필요가 있는 경우가 있다. 
        }

        final void treeify(Node<K,V>[] tab) {
          // 링크드 리스트를 트리로 변환한다.
        }

        final Node<K,V> untreeify(HashMap<K,V> map) {
          // 트리를 링크드 리스트로 변환한다.
        }

        // 다음 두 개 메서드의 역할은 메서드 이름만 읽어도 알 수 있다.
        final TreeNode<K,V> putTreeVal(HashMap<K,V> map, Node<K,V>[] tab,
                                       int h, K k, V v) {}
        final void removeTreeNode(HashMap<K,V> map, Node<K,V>[] tab,
                                  boolean movable) {}

        // Red Black 구성 규칙에 따라 균형을 유지하기 위한 것이다.
        final void split (…)
        static <K,V> TreeNode<K,V> rotateLeft(…)
        static <K,V> TreeNode<K,V> rotateRight(…)
        static <K,V> TreeNode<K,V> balanceInsertion(…)
        static <K,V> TreeNode<K,V> balanceDeletion(…)

        static <K,V> boolean checkInvariants(TreeNode<K,V> t) {
        // Tree가 규칙에 맞게 잘 생성된 것인지 판단하는 메서드다.
        }
    }
```

### 보조 해시 함수

- 목적
    - ‘키’의 해시 값을 변형 해시 충돌의 가능성을 줄이는 것

```
static final int hash(Object key) { 
	int h; return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16); 
}
```

### TreeMap

- **레드 - 블랙 트리** 형태의 Map
    - 트리에 저장되는 Entry 자체가 Red - Black Tree의 노드로 이루어져 있다.
- 내부적으로 정렬 상태를 유지한다.
    - 즉 Key에 대해 정렬 기준을 선정해야 한다.
    - 사용자 정의 클래스를 Key 타입으로 사용 시 Comparable이나 Comparator를 통해 정렬 기준을 정해놓자

```java
static final class Entry<K,V> implements Map.Entry<K,V> {
        K key;
        V value;
        Entry<K,V> left;
        Entry<K,V> right;
        Entry<K,V> parent;
        boolean color = BLACK;
}
```
