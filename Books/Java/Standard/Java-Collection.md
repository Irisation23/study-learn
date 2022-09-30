# ArrayList

1. Collection Interface 안의 List 의 구현 클래스
2. 한 요소가 삭제될 때 마다 빈 공간을 채우기 위해 나머지 요소를 자리이동 함.
3. Vector 는 배열의 크기를 증가할 때 ex) Vector v = new Vector(7) 인 v 에 8번째 데이터를 넣을 때 14 로 2배(100%) 증가 시키는 반면 ArrayList 는 (50%) 증가
   시킨다.
4. 멀티 스레드 환경에서 정의를 해준다면 사용가능하다. 하지만 Vector 는 단일 스레드에서만 구현 가능하다.
5. 크기를 변경할 수 없다.
6. 비순차적인 데이터의 추가 또는 삭제에 시간이 많이 걸린다.
7. Stack(LIFO) 구조에 적합.

> > 한줄요약 = 읽기는 빠르며, 추가 삭제는 느리다. 그리고 순차적인 추가삭제는 더 빠르며,
> > 비효율적인 메모리를 사용한다.

***

# LinkedList

1. 말은 LinkedList 이지만 실제 구현은 ‘더블 써큘러 링크드 리스트(이중 원형 연결리스트, doubly circular linked list)’ 이다.
2. 티비 채널 맹키로 맨 끝 채널에서 처음 채널로 돌아오는 구조이다.
3. 추가, 수정, 삭제가 ArrayList 보다 빠르다.
4. 하지만 조회는 쓰레기다.
5. 리스트 다음 값과 이전 값을 알고있다.
6. Queue(FIFO) 구조에 적합.

> 한줄요약 = 읽기는 느리며, 추가 삭제는 빠르다,
> 하지만 데이터가 많을수록 접근성이 떨어진다.

*** 

## 위의 두 리스트는 서로 병행해서 사용한다면 좋을 수 있다.

```java

ArrayList al=new ArrayList(100000);
    for(int i=0;i< 10000;i++){
    al.add(i+"");
    }

    LinkedList ll=new LinkedList(al);
    for(int i=0;i<1000;i++){
    ll.add(500,"X");
    }
```

# Iterator

- `Iterator`
- 컬렉션 프레임웍에서는 컬렉션에 저장된 요소들을 읽어오는 방법을 표준화 했음.
- 컬렉션 인터페이스에는 `Iterator(Iterator 를 구현한 클래스의 인스턴스)` 를 반환하는 iterator() 를 정의 하고있다.

```java
public interface Iterator {
    boolean hasNext();

    Object next();

    void remove();
}

public interface Collection {
    Iterator iterator();
}
```

- iterator() 는 Collection 인터페이스에 정의된 메소드이므로 `List` , `Set` 에 포함되어 있다.

```java
public class Main {

    Collection c = new ArrayList();
    Iterator it = c.iterator();

    public static void main(String[] args) {
        while (it.hasNext()) {
            log.info(it.next());
        }
    }
}
```

### Q. 참조변수의 타입을 ArrayList 타입이 아니라 Collection 타입으로 한 이유는?

> Collection 에 없고 ArrayList 에만 있는 메소드를 사용하는게 아니라면, Collection 타입의 참조변수로 선언하는 것이 좋음.
> 만일 Collection 인터페이스를 구현한 다른 클래스, 예를 들어 `LinkedList` 로 바꿔야 한다면 선언문 하나만 변경하면 나머지 코드는 검토하지 않아도 됨.
> 참조변수 타입이 Collection 이므로 Collection

# TreeSet

- 이진 검색 트리(binary search tree)라는 자료구조의 형태로 데이터를 저장하는 컬렉션
- 정렬, 검색, 범위검색(range search)에 높은 성능
- 이진 검색 트리의 성능을 향상시킨 `레드-블랙 트리(Red-Black tree)` 로 구현
- <b>중복된 데이터의 저장을 허용하지 않으며 정렬된 위치에 저장하므로 저장순서를 유지하지 않는다.</b>

> ### 이진 검색 트리(binary search tree)
> - 모든 노드는 최대 두 개의 자식노드를 가질 수 있다.
> - 왼쪽 자식노드의 값은 부모노드의 값보다 작아야 한다.
> - 오른쪽 자식노드의 값은 부모노드의 값보다 커야 한다.
> - 노드의 추가 삭제에 시간이 걸린다.(순차적으로 저장하지 않기 때문)
> - 검색(범위검색)과 정렬에 유리하다.
> - 중복된 값을 저장하지 못한다.

***

| 컬렉션                             | 특징                                                        |
|---------------------------------|-----------------------------------------------------------|
| ArrayList                       | 배열기반, 데이터의 추가와 삭제에 불리, 순차적인 추가삭제에 유리, 임의의 요소에 대한 접근성이 뛰어남 |
| LinkedList                      | 연결기반, 데이터의 추가와 삭제에 유리, 임의의 요소에 대한 접근성이 좋지않음.              |
| HashMap                         | 배열과 연결이 결합된 형태, 추가, 삭제, 검색, 접근성이 모두 뛰어남, 검색에는 최고성능을 보임.   |
| TreeMap                         | 연결기반, 정렬과 검색(특히 범위검색)에 적합. 단순 검색성능은 HashMap 보다 떨어짐.       |
| Stack                           | Vector를 상속받아 구현                                           |
| Queue                           | LinkedList가 Queue인터페이스를 구현                                |
| Properties                      | Hashtable을 상속받아 구현                                        |
| HashSet                         | HashMap을 이용해서 구현                                          |
| TreeSet                         | TreeMap을 이용해서 구현                                          |
| LinkedHashMap<br/>LinkedHashSet | HashMap과 HashSet에 저장순서유지기능을 추가                            |
