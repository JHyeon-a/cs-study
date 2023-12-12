# ✨17. 컬렉션 프레임워크✨

## 📌 컬렉션 프레임워크 사용 이유

- 프로그래밍을 하다보면 여러 객체를 어딘가에 저장해두고 꺼내 사용하는 상황이 많다.
- 이 때, 객체들을 모아 배열의 형태로 관리하게되는데 필요한 자료구조의 형태에 맞게 사용하려면 자바의 배열만으로는 효율적인 관리가 어려울 수 있다.
- 그래서 **자바 1.2** 부터는 **일반적으로 알려진 자료구조**의 특징과 형태를 바탕으로 **객체**를 효육적으로 관리할 수 있게 `컬렉션 프레임워크`를 지원한다.

## 📌 Collection 인터페이스

- List와 Set 인터페이스의 많은 공통된 부분을 Collection 인터페이스에서 정의하고, 두 인터페이스는 그것을 상속받음

- Collection 인터페이스는 컬렉션을 다루는데 가장 기본적인 동작들을 정의하고, 그것을 메소드로 제공하고 있음

Collection 인터페이스에서 제공하는 주요 메소드
| 메소드 | 설명 |
| ------------------------------ | ------------------------------------------------------------- |
| **boolean add(E e)** | 해당 컬렉션(collection)에 전달된 요소를 추가함. (선택적 기능) |
| **void clear()** | 해당 컬렉션의 모든 요소를 제거함. (선택적 기능) |
| **boolean contains(Object o)** | 해당 컬렉션이 전달된 객체를 포함하고 있는지를 확인함. |
| **boolean equals(Object o)** | 해당 컬렉션과 전달된 객체가 같은지를 확인함. |
| **boolean isEmpty()** | 해당 컬렉션이 비어있는지를 확인함. |
| **Iterator<E> iterator()** | 해당 컬렉션의 반복자(iterator)를 반환함. |
| **boolean remove(Object o)** | 해당 컬렉션에서 전달된 객체를 제거함. (선택적 기능) |
| **int size()** | 해당 컬렉션의 요소의 총 개수를 반환함. |
| **Object[] toArray()** | 해당 컬렉션의 모든 요소를 Object 타입의 배열로 반환함. |

## 📌 상속 구조

![](img/framework_01.png)

- 위 다이어그램에서 초록색 박스는 인터페이스, 파란색 박스는 클래스를, 빨간 실선 화살표는 상속, 보라색 점선 화살표는 구현을 나타낸다.
- 컬렉션 프레임워크는 추상화된 여러 인터페이스를 각각의 클래스에서 구현하는 방식으로 이루어져있다.
- 위 상속 구조도에 나와있는 클래스 외에도 `Arrays`와 같은 클래스들이 `컬렉션 프레임워크`에 포함되어 있다.

## 📌 자료구조를 구현한 대표적인 컬렉션

- 컬렉션 프레임워크에는 많은 자료구조를 구현한 클래스들이 있다.
- 대표적인 인터페이스의 몇가지의 특징과 구현 클래스에 대해 알아보자.

### ✔️ 1. List

- List 컬렉션은 배열과 유사한 구조로 객체를 저장하게되면 각각의 주소에 인덱스가 부여되고 그 인덱스로 검색 삭제 기능을 제공할 수 있다.
- 순서를 가지고, 같은 객체의 중복저장이 가능하다.
- 대표적으로 `ArrayList`, `LinkedList`, `Vector`가 있다. → [LinkedList?](https://github.com/Fancy96/2023-CS-Study/blob/main/Algorithm/algorithm_linkedlist.md) - [Fancy](https://github.com/Fancy96)

<br>

> #### 각각의 차이?
>
> - `ArrayList`와 `Vector`는 동적 크기를 가지는 **배열**의 형태, `LinkedList`는 **연결리스트**의 형태를 가진다.
> - `ArrayList`는 **동기화** 되어있지 않아 멀티스레드 환경에서 명시적인 **동기화**가 필요하고 `Vector`의 경우 **동기화**된 자료구조로 멀티스레스 환경에서 안전하게 사용 가능하다.
> - 연산 속도를 비교해보면 **동기화** 여부의 차이로 인해 `ArrayList`가 `Vector`보다 빠르다.

> #### `ArrayList vs Vector` 예시
>
> ArrayList는 두개 이상의 스레드에서 동시에 add메서드를 호출 할 경우 발생되는 경합때문에 하나만 추가되고 Vector의 add메서드는 동기화라 경합이 발생하지 않음

### ✔️ 2. Set

- Set 컬렉션은 집합형태의 구조를 가진다.
- 중복된 데이터를 저장 할 수 없고, 순서가 존재하지 않는다.
- 저장된 데이터를 인덱스로 관리하지 않기때문에 인덱스를 매개변수로 가지는 메서드가 존재하지 않는다.
- 대표적으로 순서가 없는 `HashSet`, 순서가 있는 `TreeSet` 클래스가 있다.

  <br>

> #### 중복된 데이터?
>
> - Set 에서는 객체의 `hashCode`로 중복을 판별한다.
> - 따라서 분명 다른 객체지만 같은 `hashCode`를 갖는다면 **같은 객체로 판별**한다.
> - 한마디로 `==`연산자가 아닌 `equals()`메서드로 비교했을 경우 `true`값이 나오면 **같은 객체**로 판별한다.
> - 다음 예제로 확인해보자.
>
>   ```
>   import java.util.Set;
>   import java.util.HashSet;
>
>   public class MyClass {
>       public static void main(String args[]) {
>
>           Set<String> set = new HashSet<>();
>           String str1 = "hi";
>           String str2 = new String("hi");
>
>           System.out.println("str1\nhashCode : " + str1.hashCode());
>           System.out.println("identity : " + System.identityHashCode(str1));
>           System.out.println("str2\nhashCode : " + str2.hashCode());
>           System.out.println("identity : " + System.identityHashCode(str2));
>
>           set.add(str1);
>           set.add(str2);
>
>           System.out.println("set size : " + set.size());
>       }
>   }
>
>   Result----------------------------------------------------------------
>
>   str1
>   hashCode : 3329
>   identity : 116211441
>   str2
>   hashCode : 3329
>   identity : 607635164
>   set size : 1
>   ```
>
> - 새로운 String 객체를 만들어 Set에 저장했지만 str1과 str2의 해시코드가 같으므로, set에 저장된 객체 수를 출력한 결과는 1이다.

### ✔️ 3. Map

- Map 컬렉션은 **Key**와 **Value**로 구성된 **Entry** 객체를 저장하는 컬렉션이다.
- 이 때, **Key**의 객체는 중복될 수 없지만 **Value**의 객체는 중복될 수 있다.
- **Key**로 객체를 관리하기때문에 대부분의 메서드의 파라미터로 **Key**를 가진다.
- 대표적으로 엔트리 순서를 보장하지 않는 `HashMap`와 `HashTable`, 순서를 가지는 `TreeMap` 클래스가 있다.

  <br>

> #### HashMap과 HashTable?
>
> - 둘의 가장 큰 차이점으로는 동기화 여부를 들 수 있다.
> - HashTable은 멀티스레드 환경에서 thread safe 하고, HashMap은 thread safe하지 않음
> - 멀티스레드 환경이 아니라면 Hashtable은 HashMap 보다 성능이 떨어짐

### ✔️ 4. TreeSet과 TreeMap

- 이름에서도 알 수 있듯 `TreeSet`은 `Set` 인터페이스를 구현한 클래스고 `TreeMap`은 `Map` 인터페이스를 구현한 클래스다.
- 이 둘은 **트리** 자료구조를 통해 검색기능에 특화된 클래스임
- 이 둘은 순서를 가지기 때문에 `TreeSet`에 저장되는 객체와 `TreeMap`의 `Key`에 저장되는 객체는 **Comparable 인터페이스를 구현**하고 있어야한다.

  <br>

> #### 🧐 저장되는 객체에서 반드시 `Comparable` 인터페이스를 구현해야하나요?
>
> - TreeSet과 TreeMap 객체를 생성할 때 다음과 같이 비교자를 제공하는 방법이 있다.
>
> ```
> new TreeSet<>(new ComparatorImpl());
> ```

### ✔️ 5. Stack과 Queue

- Stack은 클래스로 Vector 클래스를 상속받았지만 사실상 독자적인 메서드들로 이루어져 있다.
- Queue는 인터페이스로 LinkedList 클래스로 구현되어있다.

```
Stack<String> stack = new Stack<>();
Queue<String> queue = new Queue<>();
```

### ✔️ 6. 수정할 수 없는 컬렉션

- 프로그래밍을 하다보면 간혹 수정할 수 없는(unmodifiable) 컬렉션을 생성 해야할 때가 있다.
- `List`, `Set`, `Map` 에서 생성하는경우 `.of()`나 `.copyOf()` 메서드를 상황에 맞게 사용할 수 있다.
- `배열`을 `List`로 만들때 수정 할 수 없는 컬렉션을 생성하는경우 `Arrays`클래스의 `.asList()` 메서드를 사용할 수 있다.

## 📌 Arrays와 Collections 클래스

- 각각 배열과 컬렉션에서 동작하는 정적 메서드로 구성된 클래스.
- 채우기, 복사, 정렬, 검색 등 각종 알고리즘 메서드를 제공한다.

## 📌 컬렉션 클래스 정리 & 요약

| 컬렉션                                  | 특징                                                                                                                    |
| --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **ArrayList**                           | 배열기반. 데이터의 추가와 삭제에 불리. 순차적인 추가삭제는 제일 빠름. 임의의 요소에 대한 접근성(accessibility)이 뛰어남 |
| **LinkedList**                          | 연결기반. 데이터의 추가와 삭제에 유리. 임의의 요소에 대한 접근성이 좋지 않음                                            |
| **HashMap**                             | 배열과 연결이 결합된 형태. 추가, 삭제, 검색, 접근성이 모두 뛰어남. 검색에는 최고성능을 보임                             |
| **TreeMap**                             | 연결기반. 정렬과 검색(특히 범위검색)에 적합. 검색성능은 HashMap보다 떨어짐                                              |
| **Stack**                               | Vector를 상속받아 구현                                                                                                  |
| **Queue**                               | LinkedList가 Queue인터페이스를 구현                                                                                     |
| **Properties**                          | Hashtable을 상속 받아 구현                                                                                              |
| **HashSet**                             | HashMap을 이용해서 구현                                                                                                 |
| **TreeSet**                             | TreeMap을 이용해서 구현                                                                                                 |
| **LinkedHashMap**<br> **LinkedHashSet** | HashMap과 HashSet에 저장순서유지기능을 추가                                                                             |
