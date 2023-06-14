# Stack 과 Heap

---

### Stack 과 Heap 에 대해서 학습하기 전 JAVA 의 메모리 영역에 대해서 알아보자.

**JAVA 의 메모리 영역**

---

자바 프로그램이 실행되면 **JVM(자바 가상 머신)**은 OS로 부터 메모리를 할당 받고, 그 메모리를 용도에 따라서 여러 영역으로 나누어 관리를 한다.

JVM의 **메모리 공간(Runtime Data Area)**은 크게 **Method(Static) 영역, Stack 영역, Heap 영역**으로 구분되고 데이터 타입(자료형)에 따라 각 영역에 나눠서 할당 되게 된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6d06aaaa-df19-48b1-9c5b-068489f0d5bd/Untitled.png)

위 사진의 Rutime Data Area 의 공간에 속해 있는 빨간 박스 3 가지는 무엇 일까 ? 

( Runtime Data Area = 런타임 데이터 영역은 JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역입니다. ) 

### Heap 영역 **(모든 스레드가 공유해서 사용 , GC의 대상)**

---

- JVM이 관리하는 프로그램 상에서 데이터를 저장하기 위해 런타임 시 **동적**으로 할당하여 사용하는 영역
- 참조형(Reference Type) 데이터 타입을 갖는 객체(인스턴스), 배열 등이 저장 되는 공간
- stack은 스레드 갯수마다 각각 생성되지만, heap은 몇개의 스레드가 존재하든 상관없이 단 하나의 heap 영역만 존재
- Heap 영역은 Stack 영역과 다르게 보관되는 메모리가 호출이 끝나더라도 삭제되지 않고 유지된다.그러다 어떤 참조 변수도 Heap 영역에 있는 인스턴스를 참조하지 않게 된다면, GC(가비지 컬렉터)에 의해 메모리에서 청소된다.

=== > 간단 요약.

**1.** new 키워드로 생성된 객체와 배열이 생성되는 영역입니다.

**2.** 주기적으로 GC가 제거하는 영역입니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6ed1a9dd-2fcc-4f52-bdd2-8e4305a432f5/Untitled.png)

참고 : https://coding-factory.tistory.com/828

---

### Stack  ( **스레드(Thread) 마다 하나씩 생성)**

- 지역변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값 등이 생성되는 영역입니다.
- Heap 영역에 생성된 Object 타입의 데이터의 참조값이 할당된다.
- 지역변수들은 scope 에 따른 visibility 를 가진다.
- primitive 타입의 데이터(int, double, byte, long, boolean 등) 에 해당되는 지역변수, 매개 변수 데이터 값이 저장
- 메소드가 호출 될 때 메모리에 할당되고 종료되면 메모리에서 사라짐
- Stack 은 후입선출 LIFO(Last-In-First-Out) 의 특성을 가지며, 스코프(Scope) 의 범위를 벗어나면 스택 메모리에서 사라진다.

*** 추가 .

- 자바 코드를 실행할때 따로 ~~-Xms~~과 ~~-Xmx~~ 옵션을 사용하면 힙 메모리의 초기 사이즈와 최대 사이즈를 조절할 수 있다.
- 스택 메모리가 가득차면 자바에서는 java.lang.StackOverFlowError를 발생.
- 힙 메모리가 가득차면 java.lang.OutOfMemoryError : Java Heap Space 에러를 발생
- 스택 메모리 사이즈는 힙 메모리와 비교했을 때 매우 적다. 하지만 스택 메모리는 간단한 메모리 할당 방법(LIFO)를 사용하므로 힙 메모리보다 빠르다.
- 불변 객체의 경우 Heap 에서 바로 참조 되는 것이 아니라 새로운 공간에 복사가 된다 .
