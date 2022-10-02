# 열거형(enums)

1. 서로 관련된 상수를 편리하게 선언하기 위한 클래스로 여러 상수를 정의할 때 사용함.
2. 열거형이 갖는 값뿐만 아니라 타입도 관리하기 때문에 논리적인 오류가 줄어든다.

```java
class Card {
    static final int CLOVER = 0;
    static final int HEART = 1;

    static final int TWO = 0;
    static final int THREE = 1;

    final int kind;
    final int num;
}
```

> 작성된 코드를 Enum 클래스로 바꾸면?

```java
class Card {
    enum Kind {
        CLOVER, HEART;  // 열거형 Kind 정의
    }

    enum Value {
        TWO, THREE;     // 열거형 Value 정의
    }

    final Kind kind;    // 타입이 int 가 아닌 Kind 인것을 체크
    final Value value;
}
```

- 자바의 열거형은 `타입에 안전한 열거형(typesafe enum)`이라서 실제 값이 같아도 타입이 다르면 컴파일 에러가 발생한다.

```
    if(Card.CLOVER == Card.TWO)             //true 지만 false를 원함.
    if(Card.Kind.CLOVER == Card.Value.TWO)  // 컴파일 에러. 값은 같지만 타입이 다름
```

- 상수의 값이 바뀌면, 해당 상수를 참조하는 모든 소스를 다시 컴파일해야 함.
- `But` 열거형 상수를 사용하면, 기존의 소스를 다시 컴파일하지 않아도 된다.

## 열거형의 정의와 사용

```
    //선언
    enum 열거형이름 {
        상수명1, 상수명2;
    }
    
    //ex 방향을 상수 정의
    enum Dircetion {
        EAST, SOUTH, WEST, NORTH;
    }
```

- 열거형 상수간의 비교에는 '==' 사용 가능. equals()가 아닌 '=='로 비교가 가능하다는 것은 그만큼 빠른성능을 제공한다.
- 비교연산자('<' '>')는 사용할 수 없고 compareTo()는 두 비교대상이 같으면 0, 왼쪽이 크면 양수, 오른쪽이 크면 음수를 반환한다.

```
    if(dir == Direction.EAST) {
        x++;
    } else if (dir > Direction.WEST) {  // 에러. 열거형 상수에 비교연산자 사용불가
        ...
    } else if (dir.compareTo(Direction.WEST) > 0) { // compareTo()는 가능
        ...
    } 
```

## 열거형에 멤버 추가하기

```java
    enum Direction {
    EAST(1), SOUTH(5), WEST(-1), NORTH(10); // 끝에 ';'를 추가해야 한다.

    private final int value;    // 정수를 저장할 필드(인스턴스 변수)를 추가

    Direction(int value) {  // 생성자를 추가
        this.value = value;
    }

    public int getValue() {
        return value;
    }
}
```

- 새로운 생성자가 추가되었지만, 위와 같이 열거형의 객체를 생성할 수 없다.
- 열거형의 생성자는 제어자가 묵시적으로 private 이기 때문이다.

```java
    enum Direction {
    EAST(1, ">"), SOUTH(5, "V"), WEST(-1, "<"), NORTH(10, "^"); // 끝에 ';'를 추가해야 한다.

    private final int value;    // 정수를 저장할 필드(인스턴스 변수)를 추가
    private final String symbol;

    Direction(int value, String symbol) {  // 생성자를 추가 접근 제어자 private 이 생략됨
        this.value = value;
        this.symbol = symbol;
    }

    public int getValue() {
        return value;
    }
}
```

- 열거형 상수에 여러 값을 지정할 수도 있다.
- 그에 맞게 인스턴스 변수와 생성자 등을 새로 추가해주어야 한다.

### 열거형에 추상 메서드 추가

```java
enum Transportation {
    BUS(100) {
        int fare(int distance) {
            return distance * BASIC_FARE;
        }
    }
    
    TRAIN(150) {
        int fare(int distance) {
            return distance * BASIC_FARE;
        }
    }
    
    protected final int BASIC_FARE; // protected 로 해야 각 상수에서 접근가능
    
    Transportation(int basicFare) { // private Transportation(int basicFare)
        BASIC_FARE = basicFare;
    }
    
    public int getBasicFare() {
        return BASIC_FARE;
    }
    
    abstract int fare(int distance);    // 거리에 따른 요금 계산
}
```

## 열거형의 이해

```java
enum Direction {
    EAST,
    SOUTH,
    WEST,
    NORTH
}
```
> 위의 문장을 클래스로 정의한다면?
```java
class Direction {
    static final Direction EAST = new Direction("EAST");
    static final Direction SOUTH = new Direction("SOUTH");
    static final Direction WEST = new Direction("WEST");
    static final Direction NORTH = new Direction("NORTH");
    
    private String name;
    
    private Direction(String name) {
        this.name = name;
    }
}
```

- Direction 클래스의 static 상수 EAST, SOUTH, WEST, NORTH 의 값은 객체의 주소이고,
- 이 값은 바뀌지 않는 값이므로 '==' 로 비교가 가능하다.
- 모든 열거형은 `추상 클래스 Enum` 의 자손이다.

```java
abstract class MyEnum<T extends MyEnum<T>> implements Comparable<T> {
    static int id = 0;  //객체에 붙일 일련번호 (0부터 시작)
    
    int ordinal;
    String name = "";
    
    public int ordinal() {
        return ordinal;
    } 
    
    MyEnum(String name) {
        this.name = name;
        ordinal = id++;     //객체를 생성할 때마다 id 의 값을 증가시킨다.
    }
    
    public int compareTo(T t) {
        return ordinal - t.ordinal;
    }
}
```

- 객체가 생성 될 때마다 번호를 붙인다(id++)
- Comparable 인터페이스를 구현해서 열거형 상수간의 비교가 가능하도록 되어있다.
> 🌟 정리하자면 열거형에 추상 메서드를 추가하면 각 열거형 상수가 추상 메서드를 구현해야한다.