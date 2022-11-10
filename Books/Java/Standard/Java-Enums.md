# 열거형(enums)

1. 서로 관련된 상수를 편리하게 선언하기 위한 클래스로 여러 상수를 정의할 때 사용함.
2. 열거형이 갖는 값뿐만 아니라 타입도 관리하기 때문에 논리적인 오류가 줄어든다.

```java
class Card {
	static final int CLOVER = 0;
	static final int HEART = 1;
	static final int DIAMOND = 2;
	static final int SPADE = 3;

	static final int TWO = 0;
	static final int THREEE = 1;
	static final int FOUR = 2;

	final int kind;
	final int num;
}
```

```java
class Card {
	enum Kind {
		CLOVER, HEART, DIAMOND, SPADE
	}
	
	enum Value {
		TWO, THREE, FOUR
	}

	final Kind kind;
	final Value value;
}
```

- 자바의 열거형은 `타입에 안전한 열거형(typesafe enum)` 이라서 실제 값이 같아도 타입이 다르면 컴파일 에러가 발생한다.
- 쉽게 설명하면 값뿐만 아니라 타입까지 체크하기 때문에 타입에 안전하다고 하는 것이다.
- 열거형 상수간의 비교에는 `==` 사용이 가능하다.
    - equals() 가 아닌 `==` 로 연산이 가능하다는 것은 그만큼 빠른 성능을 제공한다는 얘기다.
    - compareTo() 두 비교대상이 같으면 0, 왼쪽이 크면 양수, 오른쪽이 크면 음수를 반환한다.

---

### 모든 열거형의 조상 - java.lang.Enum

- 열거형 Direction(임의로 만든 클래스) 에 정의된 모든 상수를 출력하려면, 다음과 같이 해야한다.

```java
Direction[] dArr = Direction.values();
for (Direction d : dArr) {
	sout(d.name(), d.ordianl());
}
```

- 사용되는 메서드
    1. Class<E> getDecalaringCalss(): 열거형의 Class 객체를 반환한다.
    2. String name(): 열거형 상수의 이름을 문자열로 반환한다.
    3. int ordinal(): 열거형 상수가 정의된 순서를 반환한다.(0 부터 시작)
    4. T valueOf(Class<T> enumType, String name): 지정된 열거형에서 name 과 일치하는 열거형 상수를 반환한다.

    ---


### 열거형에 멤버 추가하기

- Enum 클래스에 정의된 ordinal()이 열거형 상수가 정의된 순서를 반환하지만, 이값을 열거형 상수의 값으로 사용하지 않는것을 권장한다.
- 이 값은 내부적인 용도로만 사용되기 때문이다.
- 열거형 상수의 값이 불연속적인 경우에는 이때는 다음과 같이 열거형 상수의 이름 옆에 원하는 값을 괄호()와 함께 적어주면 된다.

```java
enum Direction {
	EAST(1), SOUTH(5), WEST(-1), NORTH(10)
}
```

- 지정된 값을 저장할 수 있는 인스턴스 변수와 생성자를 새로 추가해 주어야 한다.
- 이 때 주의할 점은 먼저 열거형 상수를 모두 정의한 다음에 다른 멤버들을 추가해야한다.

```java
enum Direction {
	EAST(1), SOUTH(5), WEST(-1), NORTH(10); // 끝에 ';' 를 추가해야 한다.

	private final int value; // 정수를 저장할 필드(인스턴스 변수) 를 추가
	
	Direction(int value) {
		this.value = value;
	}
	
	public int getValue() {
		return value;
	}
}

```

- 열거형 생성자는 제어자가 묵시적으로 private 이다.
    - Direction d = new Direction(); // 에러. 컴파일 에러 열거형 생성자가 private 이기 때문 외부호출 불가
- 하나의 열거형 상수에 여러 값 지정 가능
- 인스턴스 변수와 생성자 등을 새로 추가해주어야 함.

```java
enum Direction {
	EAST(1, ">"), SOUTH(5, "V"), WEST(-1, "<"), NORTH(10, ""); 
	
	private final int value;
	private final String symbol;

	Direction(int value, String symbol) { // 접근 제어자 private 가 생략됨
		.
	}
}
```

```java

enum Direction {
    EAST(1, ">"),
    SOUTH(2, "V"),
    WEST(3, "<"),
    NORTH(4, "^");

    private static final Direction[] DIR_ARR = Direction.values();
    private int value;
    private String symbol;

    Direction(int value, String symbol) {
        this.value = value;
        this.symbol = symbol;
    }

    public int getValue() {
        return value;
    }

    public String getSymbol() {
        return symbol;
    }

    public static Direction of(int dir) {
        if (dir < 1 || dir > 4) {
            throw new IllegalArgumentException("Invalid value : " + dir);
        }

        return DIR_ARR[dir - 1];
    }

    // 방향을 회전시키는 메서드. num 의 값만큼 90도씩 시계방향으로 회전한다.
    public Direction rotate(int num) {
        num = num % 4;

        if (num < 0) {
            num += 4;   // num 이 음수일 때는 시계반대 방향으로 회전
        }

        return DIR_ARR[value - 1 + num];
    }

    public String toString() {
        return name() + getSymbol();
    }
}

class EnumEx2 {
    public static void main(String[] args) {
        for (Direction d : Direction.values()) {
            System.out.printf("%s=%d%n", d.name(), d.getValue());
        }

        Direction d1 = Direction.EAST;
        Direction d2 = Direction.of(1);

        System.out.printf("%s=%d%n", d1.name(), d1.getValue());
        System.out.printf("%s=%d%n", d2.name(), d2.getValue());

        System.out.println(Direction.EAST.rotate(1));
        System.out.println(Direction.EAST.rotate(2));
        System.out.println(Direction.EAST.rotate(-1));
        System.out.println(Direction.EAST.rotate(-2));
    }
}
```

### 열거형에 추상 메서드 추가하기

```java
enum Transportation {
	BUS(100) {
		int fare(int distance) {
			return distance * BASIC_FARE;
		}
	}, 
	
	TRAIN(150) {
		int fare(int distance) {
			return distance * BASIC_FARE;
		}
	}, 
	
	SHIP(100) {
		int fare(int distance) {
			return distance * BASIC_FARE;
		}
	}, 
	
	BUS(300) {
		int fare(int distance) {
			return distance * BASIC_FARE;
		}
	};

	abstract int fare(int distance); // 거리에 따른 요금을 계산하는 추상 메서드
	
	protected final int BASIC_FARE; // protected 로 해야 각 상수에서 접근가능
	Transportation(int basicFare) {
		BASIC_FARE = basicFare;
	}

	public int getBasicFare() {
		return BASIC_FARE;
	} 	
}
```

### **열거형의 이해  (핵심)**

```java
enum Direction {
	EAST, SOUTH, WEST, NORTH
}
```

- 위의 코드는 아래의 구조와 같다.

```java
class Direction {
	static final Direction EAST = new Direction("EAST");
	static final Direction SOUTH = new Direction("SOUTH");
	static final Direction WEST = new Direction("WEST");
	static fianl Direction NORTH = new Direction("NORTH");

	private String name;
	
	private Direction (String name) {
		this.name = name;
	}
}
```

- Direction 클래스의 static 상수 EAST, SOUTH, WEST, NORTH 의 `값은 객체의 주소`이고, 이 값은 바뀌지 않는 값이므로 ‘==’ 비교가 가능한 것이다.
- 모든 열거형은 추상 클래스 Enum의 자손이므로, Enum 을 흉내 내어 MyEnum 을 작성 하면 다음과 같다.

```java
abstract class MyEnum<T extends MyEnum<T>> implements Comparable<T> {
    static int id = 0; // 객체에 붙일 일련번호 (0 부터 시작)
    
    int ordinal;
    String name;
    
    public int ordinal() {
        return ordinal;
    }
    
    MyEnum(String name) {
        this.name = name;
        ordinal = id++; // 객체를 생성할 때마다 id 의 값을 증가시킨다.
    }
    
    @Override
    public int compareTo(T t) {
        return ordinal - t.ordinal;
    }
    
}
```

- 만약 해당 코드를 MyEnum 을 상속하지 않고 MyEnum<T> 라고 선언하게 된다면 어떻게 될까?

```java
abstract class MyEnum<T> implements Comparable<T> {
   
	   ... 
    @Override
    public int compareTo(T t) {
        return ordinal - t.ordinal; // 에러. 타입 T에 ordinal() 이 있나??? 음따
    }
    
}
```

- MyEnum<T extends<My Enum<T>> 와 같이 선언한 것이며, 이것은 타입 ‘T’ 가 MyEnum<T> 의 자손이어야 한다는 의미이다. 타입 T 가 MyEnum 의 자손이므로 ordinal() 이 정의되어 있는 것은 분명하므로 형변환 없이도 에러가 나지 않는다.
- 추상 메서드를 새로 추가하면, 클래스 앞에도 ‘abstract’ 붙여줘야 하고, 각 static 상수들도 추상 메서드를 구현해 주어야 한다. 아래의 코드에서는 익명 클래스의 형태로 추상 메서드를 구현하였다.

```java
abstract class Direction extends MyEnum {
	static final Direction EAST = new Direction("EAST") { // 익명 클래스
		Point move(Point p) {
			/* 내용 생략 */
		}
	}
	static final Direction SOUTH = new Direction("SOUTH") {
		Point move(Point p) {
			/* 내용 생략 */
		}
	}
	static final Direction WEST = new Direction("WEST") {
		Point move(Point p) {
			/* 내용 생략 */
		}
	}
	static final Direction SOUTH = new Direction("SOUTH") {
		Point move(Point p) {
			/* 내용 생략 */
		}
	}

	asstract Point move(Point p);
}
```
- 객체가 생성 될 때마다 번호를 붙인다(id++)
- Comparable 인터페이스를 구현해서 열거형 상수간의 비교가 가능하도록 되어있다.
> 🌟 정리하자면 열거형에 추상 메서드를 추가하면 각 열거형 상수가 추상 메서드를 구현해야한다.