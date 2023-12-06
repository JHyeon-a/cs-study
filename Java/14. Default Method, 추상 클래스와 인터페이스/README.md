# 14. 디폴트 메서드, 추상 클래스와 인터페이스

# ✨디폴트 메서드(Default Method)✨

## 📌 디폴트 메서드 개념

### ✔️ 디폴트 메서드란?

- 추상 메서드의 기본적인 구현을 제공하는 메서드
- 추상 메서드가 아니기 때문에 디폴트 메서드가 추가 되어도 해당 인터페이스를 구현한 클래스를 변경하지 않아도 됨
- 디폴트 메서드는 앞에 키워드 `default`를 붙이며, 추상 메서드와 달리 일반 메서드처럼 몸통 `{}`이 있어야 함
- 디폴트 메서드 역시 접근 제어자가 `public`이며 생략 가능

### ✔️ 디폴트 메서드가 생겨난 배경

- 인터페이스에 메서드를 추가한다는 것
- => 추상 메서드를 추가하는 것
- => 이 인터페이스를 구현한 기존의 모든 클래스들이 새로 추가된 메서드를 구현해야 함
- => `수정이 굉장히 복잡함`

### ✔️ 디폴트 메서드 장점

- 디폴트 메서드의 출현으로 기존의 배포된 인터페이스에서 `수정이 어려운 단점을 극복` 할 수 있게 됨

## 📌 예제로 알아보기

```java
interface Foo {

  String getName(); // 추상 메서드, 구현하는 클래스에서 무조건 구현해줘야 하는 메서드.

}

---

public class Bar implements Foo {
  private String name;

  public Bar(String name) { // 생성자
    this.name = name;
  }

  @Override
  public String getName() { // 추상 메서드 구현
    return this.name;
  }
}

---

public class Main {
  public static void main(String[] args) {
    Foo foo = new Bar("Neo");
    System.out.println(foo.getName());
  }
}

---

Result

>> Neo

```

- 위의 예제에서 인터페이스 Foo에 이름을 콘솔에 출력하는 기능을 추가하고싶다고 가정해보자.
- 이전의 방식으로는 인터페이스에 새로운 printName() 메서드를 추가하고 인터페이스 Foo를 구현한 모든 클래스에서 새로운 메서드를 구현해주어야했다.
- 하지만 디폴트 메서드의 출현으로 다음과 같이 사용 할 수 있게 됐다.

```java
interface Foo {

  String getName(); // 추상 메서드, 구현하는 클래스에서 무조건 구현해줘야 하는 메서드.

  void printName() {
    System.out.println(getName());
  }

}

---

public class Bar implements Foo {
  private String name;

  public Bar(String name) { // 생성자
    this.name = name;
  }

  @Override
  public String getName() { // 추상 메서드 구현
    return this.name;
  }
}

---

public class Main {
  public static void main(String[] args) {
    Foo foo = new Bar("Neo");
    foo.printName();
  }
}

---

Result

>> Neo
```

## 📌 주의할 점

- 다중상속시 같은 이름의 메서드가 두 인터페이스에 모두 존재할 때, 구현 클래스에서 재정의해서 사용해야 함.

### ✔️ 예제

```java
interface Foo {
  void sameNameMethod();
}

---

interface Bar {
  void sameNameMethod();
}

---

class Impl implements Foo, Bar {
  ...
}

---

public class Main {
  public static void main(String[] args) {
    Impl impl = new Impl();
    impl.sameNameMethod(); // 모른다! 메서드! 뱉어낸다! 에러!
  }
}

---

Result

>> Error : Main inherits unrelated defaults...
```

- 해결방법: 구현 하는 클래스에서 메서드를 재정의

```java
interface Foo {
  void sameNameMethod();
}

---

interface Bar {
  void sameNameMethod();
}

---

class Impl implements Foo, Bar {
  @Override
  publci void sameNameMethod() {
    System.out.println("sameNameMethod called!");
  }
}

---

public class Main {
  public static void main(String[] args) {
    Impl impl = new Impl();
    impl.sameNameMethod(); // 안다! 메서드! 뱉어낸다! 결과!
  }
}

---

Result

>> sameNameMethod called!
```

- 디폴트 메서드는 equals(), hashCode() 같은 Object클래스의 메서드를 재정의 할 수 없다.

- 정적 메서드는 재정의 할 수 없다.

- 인터페이스간 상속관계에 따라 우선순위가 달라진다.
  - 만약 interface A extends B 처럼 인터페이스 A가 B를 상속받고 B에서 정의된 디폴트 메서드를 재정의 할 경우 A의 디폴트 메서드가 우선순위를 갖는다.
  - 만약 class C implements A에서 위처럼 A가 B를 상속받고 그 상속받은 인터페이스를 C가 구현해 디폴트 메서드를 재정의 한 경우 C의 재정의된 메서드가 우선권을 갖는다.

# ✨추상 클래스와 인터페이스✨

### ✔️ 추상 클래스란?

- 자바에서는 하나 이상의 추상 메소드를 포함하는 클래스
- 추상 메서드를 선언해 놓고 상속을 통해 자식 클래스에서 메서드를 완성하도록 유도하는 클래스
- `미완성 설계도`

### ✔️ 인터페이스란?

- 다른 클래스를 작성할 때 기본이 되는 틀을 제공하면서, 다른 클래스 사이의 중간 매개 역할까지 담당
- `기본 설계도`

## 📌 공통점

- 스스로 new 연산자를 통해 객체를 생성할 수 없음
- 추상 메서드를 가짐
- 자손클래스를 이용해 정의된 추상 메서드들을 구현해야 함

## 📌 차이점

### ✔️ 용도, 태생

`추상 클래스`

- 여러 클래스를 추상화해 공통점을 묶은 추상클래스를 상속받아 그 기능을 사용하거나 확장하기 위함.

`인터페이스`

- 여러 구현체들의 맥락에 맞는 동일한 동작을 보장하기 위한 명세.

### ✔️ 다중 상속 가능 여부

`추상 클래스`

- 다중 상속 불가능

`인터페이스`

- 다중 상속 가능

### ✔️ 선언가능한 메서드

`추상 클래스`

- 일반 클래스와 동일 하고, 추상 메서드를 선언 할 수 있다.

`인터페이스`

- 추상 메서드만 선언할 수 있었으나 자바8 부터 디폴트 메서드, 자바9 부터 정적 메서드까지 선언 가능하게 됨.

### ✔️ 변수와 생성자, 메서드

`추상 클래스`

- 생성자와 변수, 메서드를 모두 가질 수 있다.

`인터페이스`

- 메서드와 정적 상수만 가질 수 있다.
- 자바8 이전까지는 추상 메서드만, 자바9 이전까지는 추상 메서드 + 디폴트 메서드, 그 이후로는 추상 메서드 + 디폴트 메서드 + private 메서드만 가질 수 있다.

### ✔️ 자식 클래스에서의 구현의 강제성

`추상 클래스`

- 선택적으로 오버라이딩이 가능하다.

`인터페이스`

- 정의된 모든 추상메서드를 구현해야 한다.
  디폴트 메서드의 경우 선택적으로 구현 가능.

### ✔️ 키워드

`추상 클래스`

- extends

`인터페이스`

- implements

## 📌 결론

- `추상 클래스`: 연관성 있는 클래스들의 속성을 뽑아 다시 한 번 추상화 한 클래스, 어떤 객체의 범위를 확장할 때 사용
- `인터페이스`: 명세한 기능을 여러 클래스에서 재사용하여 구현할 수 있을 때 사용
- 동물을 예로 들면 사자, 호랑이, 개 등을 추상화한 계층으로 포유류라고 정의할 수 있고 닭, 오리, 독수리 등을 추상화 한 계층으로 조류라고 표현 할 수 있음 (`추상 클래스`)
- 이 상황에서 각각의 동물들을 구현할 때 Walkable, Flyable 등의 인터페이스를 정의해 각 동물의 기능을 명세할 수 있음 (`인터페이스`)

# ✨✨

## 📌

### ✔️
