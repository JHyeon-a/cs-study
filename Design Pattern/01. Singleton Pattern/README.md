# 01. 싱글톤 패턴

## ✨싱글톤이란?✨

```java
public class Singleton {

    private static Singleton instance = new Singleton();

    private Singleton() {
        // 생성자는 외부에서 호출못하게 private 으로 지정해야 한다.
    }

    public static Singleton getInstance() {
        return instance;
    }

    public void say() {
        System.out.println("hi, there");
    }
}
```

### 📌 정의

**단 하나의 유일한 객체를 만들기 위한 코드 패턴**
메모리 절약을 위해 인스턴스가 필요할 때 똑같은 인스턴스를 새로 만들지 않고 기존의 인스턴스를 가져와 활용하는 기법

### 📌 장점

#### ✔️ 메모리의 효율성

최초 한 번의 `new` 연산자를 통해서 고정된 메모리 영역을 사용하기 때문에, 추후 해당 객체에 접근할 때 메모리 낭비를 방지할 수 있음

#### ✔️ 데이터 공유의 쉬움

싱글톤 인스턴스가 전역으로 사용되는 인스턴스이기 때문에 다른 클래스의 인스턴스들이 접근하여 사용할 수 있음

### 📌 단점

#### ✔️ 의존성이 높아짐

싱글톤의 인스턴스가 변경되면 해당 인스턴스를 참조하는 모든 클래스들을 수정해야하는 문제가 발생

#### ✔️ private 생성자 때문에 상속이 어려움

기본 생성자를 private로 만들어씩 때문에 상속을 통한 자식 클래스를 만들 수 없다는 문제점이 있음

- 이는 자바의 객체지향 언어의 장점 중 하나인 다형성을 적용하지 못한다는 문제로 이어짐
  - 독립적인 테스트가 진행이 되려면 전역에서 상태를 공유하고 있는 인스턴스의 상태를 매번 초기화해야 함
  - 초기화해주지 않으면 전역에서 상태를 공유 중이기 때문에 테스트가 정상적으로 수행되지 못할 가능성이 존재한다.

#### ✔️ 테스트의 어려움

싱글톤 패턴의 인스턴스는 자원을 공유하고 있다는 특징이 있음.

- 이는 서로 독립적이어야 하는 단위 테스트를 하는데 문제가 됨

### 📌 사용처

#### ✔️ 인스턴스를 오직 1개만 만들어야 하는 경우에 모든 곳에서 사용이 가능

ex) 애플리케이션 내에서 데이터베이스에 접근하는 객체를 개발하고 있다고 가정
데이터베이스 연결을 단 한 번만 생성하고, 어디서든 해당 연결에 접근하여 데이터를 처리해야 함

=> 싱글톤 패턴 사용하여 구현 가능

#### ✔️ 다른 디자인 패턴의 기반으로도 사용할 수 있음

ex)추상 팩토리, 팩토리 메소드, 빌더 및 프로토타입 패턴

## ✨싱글톤 구현 방법 6가지✨

### 1. Eager Initialization

가장 간단한 형태의 구현 방법

- 한번만 미리 만들어두는, 가장 직관적이면서도 심플한 기법
- static 특성으로 사용하지 않아도 일단 올려두기 때문에 리소스 큰 객체일 경우 공간 자원 낭비 심함
  - 객체가 크지 않은 객체라면 이 기법 적용해도 무리 X
- 예외 처리 불가능

#### ✔️ 예제코드

```java
public class Singleton {
    // 싱글톤 클래스 객체를 담을 인스턴스 변수
    private static final Singleton instance = new Singleton();

    // 싱글톤 private로 선언 (외부에서 new 사용 X)
    private Singleton(){}

    public static Singleton getInstance(){
        return instance;
    }
}
```

### 2. Static block initialization

`static block` 을 이용해 예외를 잡을 수 있는 방법

- `static block`: 클래스가 로딩되고 클래스 변수가 준비된 후 자동으로 실행되는 블록
- 여전히 static 특성으로 사용하지 않아도 공간 차지함

#### ✔️ 예제코드

```java
class Singleton {
    // 싱글톤 클래스 객체를 담을 인스턴스 변수
    private static Singleton instance;

    // 생성자를 private로 선언 (외부에서 new 사용 X)
    private Singleton() {}

    // static 블록을 이용해 예외 처리
    static {
        try {
            instance = new Singleton();
        } catch (Exception e) {
            throw new RuntimeException("싱글톤 객체 생성 오류");
        }
    }

    public static Singleton getInstance() {
        return instance;
    }
}
```

### 3. Lazy Initialization

앞선 두 방식과 달리 나중에 초기화 하는 방법

- 메서드를 호출했을 때 인스턴스 변수의 null 유무에 따라 초기화 하거나 있는 걸 반환하는 기법
- 앞선 두 개의 고정 메모리 차지의 한계 극복
- But!! **multi-thread 환경에서 동기화 문제**가 있음
  - 스레드 A, 스레드 B 존재
  - A가 아래 예제코드에서 `instance == null`을 판단하는 if문을 진입 -> 아직 초기화 진행 X
  - B도 같은 if문을 평가 -> 아직 A가 인스턴스화 코드 실행시키지 않았기 때문에 if문 참으로 나옴
  - A, B 모두 인스턴스 초기화 코드를 두 번 실행(싱글톤 클래스인데 객체가 두 개가 만들어짐)

#### ✔️ 예제코드

```java
class Singleton {
    // 싱글톤 클래스 객체를 담을 인스턴스 변수
    private static Singleton instance;

    // 생성자를 private로 선언 (외부에서 new 사용 X)
    private Singleton() {}

    // 외부에서 정적 메서드를 호출하면 그제서야 초기화 진행 (lazy)
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton(); // 오직 1개의 객체만 생성
        }
        return instance;
    }
}
```

### 4. Thread Safe Singleton

synchronized 키워드를 통해 메서드에 쓰레드들을 하나하나씩 접근하게 하도록 설정하는 방법

- Lazy Initialization의 문제를 해결하기 위한 방법
- But!! **성능 하락 문제**가 있음
  - 여러개의 모듈들이 매번 객체를 가져올 때 synchronized 메서드를 매번 호출하여 동기화 처리 작업에 overhead가 발생

```java
class Singleton {
    private static Singleton instance;

    private Singleton() {}

    // synchronized 메서드
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### 5. Bill Pugh Singleton Implementaion ⭐

(권장되는 2개의 방식 중 하나)

클래스 안에 내부 클래스(holder)를 두어 JVM의 클래스 로더 매커니즘과 클래스가 로드되는 시점을 이용한 방법

- 멀티쓰레드 환경에서 안전, Lazy Loading(나중에 객체 생성) 도 가능한 싱글톤 기법
- static 메소드에서는 static 멤버만을 호출할 수 있기 때문에 내부 클래스를 static으로 설정
- But!! **클라이언트가 임의로 싱글톤을 파괴할 수 있다는 단점**은 있음

#### ✔️ 예제코드

```java
class Singleton {

    private Singleton() {}

    // static 내부 클래스를 이용
    // Holder로 만들어, 클래스가 메모리에 로드되지 않고 getInstance 메서드가 호출되어야 로드됨
    private static class SingleInstanceHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingleInstanceHolder.INSTANCE;
    }
}
```

### 6. Enum Singleton ⭐

(권장되는 2개의 방식 중 하나)

`enum`을 이용한 싱글톤 패턴

- `enum`: 열겨형 타입

- `enum` 애초에 멤버를 만들때 private로 만들고 한번만 초기화 하기 때문에 thread safe함.
- `enum`내에서 상수 뿐만 아니라, 변수나 메서드를 선언해 사용이 가능하기 때문에, 이를 이용해 싱글톤 클래스 처럼 응용이 가능
- 5번(Bill Pugh Singleton Implementaion)의 문제점도 없음

- But!! 클래스 상속이 필요할 때 `enum` 외 클래스 상속 불가

#### ✔️ 예제코드

```java
enum SingletonEnum {
    INSTANCE;

    private final Client dbClient;

    SingletonEnum() {
        dbClient = Database.getClient();
    }

    public static SingletonEnum getInstance() {
        return INSTANCE;
    }

    public Client getClient() {
        return dbClient;
    }
}

public class Main {
    public static void main(String[] args) {
        SingletonEnum singleton = SingletonEnum.getInstance();
        singleton.getClient();
    }
}
```
