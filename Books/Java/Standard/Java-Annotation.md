# Annotation 어노테이션

- 해당 Annotation 정리는 만드는 법 위주이다.

## 어노테이션 타입 정의

- 새로운 어노테이션을 정의하는 방법은 인터페이스를 정의하는 것과 비슷하다.

```java
@interface AnnotationName {
    Type TypeName();    // 어노테이션의 요소를 선언한다.
    ...
}
```

### 어노테이션 요소

- 어노테이션 내에 선언된 메서드를 `어노테이션의 요소(element)` 라고 한다.
- 어노테이션에도 인터페이스처럼 상수를 정의할 수 있다. 하지만 디폴트 메서드는 정의할 수 없다.
- 어노테이션 요소는 반환값이 있고 매개변수는 없는 추상 메서드의 형태를 가지며 상속을 통해 구현하지 않아도 된다.
- 하지만 어노테이션을 적용할 때 이 요소들의 값을 빠짐없이 지정해주어야 한다. 순서는 상관없다.

```java
@interface TestInfo {
    int count();

    String testedBy();

    String[] testTools();

    TestType testType();    //enum TestType{ FIRST, SECOND}

    DateTime testDate();    //자신이 아닌 다른 어노테이션(@DateTime)을 포함할 수 있다.
}

@interface DateTime {
    String yymmdd();

    String hhmmss();
}

@TestInfo(count = 3, testedBy = "Kim", testTools = {"JUnit", "AutoTester"},
    testType = TestType.FIRST,
    testDate = @DateTime(yymmdd = "150101", hhmmss = "235912"))
public class NewClass {

}
```

- 어노테이션의 각 요소는 기본값을 가질 수 있으며,
- 기본값이 있는 요소는 어노테이션을 적용할 때 값을 지정하지 않으면 기본값이 사용된다.

```java
@interface TestInfo {
    int count() default 1;      // 기본값을 1로 지정
}

@TestInfo   // @TestInfo(count = 1)과 동일
public class NewClass {

}
```

- 어노테이션 요소가 오직 하나뿐이고 이름이 value 인 경우,
- 어노테이션을 적용할 때 요소의 이름을 생략하고 값만 적어도 된다.

```java
@interface TestInfo {
    String value();
}

@TestInfo("passed")
class NewClass {

}
```

### 어노테이션 요소의 규칙

> - 요소의 타입은 기본형, String, enum, 어노테이션, Class 만 허용된다.
> - ()안에 매개변수를 선언할 수 없다.
> - 예외를 선언할 수 없다.
> - 요소를 타입 매개변수로 정의할 수 없다.

```java
@interface AnnoTest {
    int id = 100;       // OK. 상수 선언. static final int id = 100;

    String major(int i, int j);     //에러. 매개변수를 선언할 수 없음

    String minor() throws Exception;    //에러. 예외 선언 불가

    ArrayList<T> list();            //에러. 요소의 타입에 타입 매개변수 사용불가
}
```