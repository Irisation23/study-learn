# Generics 

- 다양한 타입의 객체들을 다루는 메소드나 컬렉션 클래스에 컴파일 시의 타입체크(compile-time type check)를 해주는 기능임.

### Generics 장점
1. 타입 안정성을 제공한다.
2. 타입체크와 형변환을 생략할 수 있으므로 코드가 간결해 진다.

> 다룰 객체 타입을 미리 명시해줌으로서 번거로운 형변환을 줄여준다.

***

## Generic Class 선언

```java
class Box<T> {
    T item;
    void setItem(T item) {
        this.item = item;
    }
    T getItem() {
        return item;
    }
}
```

- `Box<T>` 에서 T를 '타입 변수(type variable)'라고 한다.
- Box 클래스의 객체를 생성할 때 참조변수와 생성자에 실제 타입을 지정해주어야 한다.
```java
class Main {
    Box<String> b = new Box<String>(); // 타입 T 대신, 실제 타입을 지정
    b.setItem(new Object());           //에러, String 이외의 타입은 지정불가
    b.setItem("ABC");                  //OK String 타입이므로 가능
    String item = (String) b.getItem;  //형변환 필요없음
}
```

***

## 제한된 Generic Class

- 타입 문자로 사용할 타입을 명시하면 한 종류의 타입만 저장할 수 있도록 제한 할 수 있다
- 타입 매개변수 T 에 지정할 수 있는 타입의 종류를 제한 할 수 있는 방법은?
```java
public class Main {
    class FruitBox<T extends Fruit> { // Fruit 의 자손만 타입으로 지정가능
        Array<T> list = new ArrayList<T>();
    }

    /**
     * 만일 클래스가 아니라 인터페이스를 구현해야 한다는 제약이 필요하다면, 이때도 'extends' 를 사용한다.
     * 'implements' 를 사용하지 않는다 !!!!!
     */
    interface Eatable {}
    
    class Fruit<T extends Eatable> { ... }

    /**
     * 제약에 제약을 더하는 방법
     * 클래스 Fruit 의 자손이면서 Eatable 인터페이스도 구현해야한다면??
     */
    
    class FruitBox<T extends Fruit & Eatable> { ... }
}
```
***

## 와일드 카드

```java
class Juicer {
    static Juice makeJuice(FruitBox<Fruit> box) {
        String tmp = "";
        for(Fruit f : box.getList()) {
            tmp += f + " ";
            return new Juice(tmp);
        }
    }
}
```

- Juicer 클래스는 Generic Class 가 아니다.
- Generic Class 라고하더라도 static 메소드에는 타입 매개변수 T 를 매개변수에 사용할 수 없다.
- 메소드 오버로딩 `makeJuice(Fruit<Fruit>) makeJuice(Fruit<Apple>)` 하면 컴파일 에러난다.
- 하지만 이를 해결 할 수 있는것이 '와일드 카드' 로서 `?` 로 포현된다.

> 1. `<? extends T>` 와일드 카드의 상한(upper bound) 제한. T 와 그 자손들만 가능
> 2. `<? super T>` 와일드 카드 하한(lower bound) 제한 T 와 그 조상들만 가능
> 3. `<?>` 제한 없음. 모든 타입이 가능. <? extends Object> 와 동일.

- 매개변수의 타입을 `FruitBox<? extends Object>` 로 하면, 모든 종류의 FruitBox 가 이 메소드의 매개변수로 가능해진다.
- 대신, 전과 달리 box 의 요소가 Fruit 의 자손이라는 보장이 없다.
```java
public class Main {
    static Juice makeJuice(FruitBox<? extends Object> box) {
        String tmp = "";
        for (Fruit f : box.getList()) {
            tmp += f + " "; // 에러. Fruit 이 아닐 수 있음
        }
        return new Juice(tmp);
    }
}
```

- `Optional<?> EMPTY = new Optional<>()`;
- `<?>` 는 <? extends Object> 를 줄여 쓴 것이며, <> 안에 생략된 타입은 '?' 가 아니라 'Object' 이다.

## Generic Method

- Generic 타입의 선언 위치는 반환타입 바로 앞이다.

```
static <T> void sort (List<T> list, Comparator<? super T> c) 
```

```java
class FruitBox<T> {
    static <T> void sort(List<T> list, Comparator<? super T> c) {
        
    }
}
```
- static 멤버에는 타입 매개변수를 사용할 수 없지만, 이처럼 메소드에 Generic 타입을 선언하고 사용하는 것은 가능하다.


