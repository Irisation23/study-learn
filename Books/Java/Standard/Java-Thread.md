# 프로세스와 스레드
- `프로세스(process)`란 간단히 말해서 '실행 중인 프로그램(program)'이다. 
- 프로그램을 실행하면 OS 로부터 실행에 필요한 자원(메모리)을 할당받아 `프로세스`가 된다.
- `프로세스`는 프로그램을 수행하는 데 필요한 데이터와 메모리 등의 자원 그리고 `쓰레드`로 구성되어 있으며 프로세스의 자원을 이용해서
- 실제로 작업을 수행하는 것이 바로 `쓰레드` 이다.

### 멀티태스킹과 멀티쓰레딩
- 현재의 우리 OS 는 `멀티태스킹(multi-tasking, 다중작업)`을 지원하기 때문에 여러 개의 프로세스가 동시에 실행될 수 있다.

### 멀티쓰레딩의 장점
- CPU 의 사용률을 향상시킨다.
- 자원을 보다 효율적으로 사용할 수 있다.
- 사용자에 대한 응답성이 향상된다.
- 작업이 분리되어 코드가 간결해진다.

> 멀티쓰레드 프로세스는 여러 쓰레드가 같은 프로세스 내에서 자원을 공유하면서 작업을 하기 때문에
> 작업에 신중 해야한다. 동기화(synchronization), 교착상태(deadlock)과 같은 문제들을 고려해야한다.

# 쓰레드의 구현과 실행
- 쓰레드 구현에는 두가지 방법이 있다.
- Thread 클래스를 상속받는 방법과 Runnable 인터페이스를 구현하는 방법, 모두 두 가지가 있다.
- 하지만 객체지향적인 프로그래밍에는 Runnable 인터페이스를 구현하는 방법이 더 선호된다.
```java
class MyThread extends Thread {
    @Override
    public void run() {     //Thread 클래스의 run() 을 오버라이딩
        
    }
}
```
- Thread 클래스를 상속
```java
class MyThread implements Runnable {
    @Override
    public void run() {     //Runnable 인터페이스의 run() 을 구현
        
    }
}
```
- Runnable 인터페이스를 구현

```java
class Main {
    ThreadEx1_1 t1 = new ThreadEx1_1(); // Thread 의 자손 클래스의 인스턴스를 생성
    
    Runnable r = new ThreadEx1_2();     // Runnable 을 구현한 클래스의 인스턴스를 생성
    Thread t2 = new Thread(r);          // 생성자 Thread(Runnable target)
}
```