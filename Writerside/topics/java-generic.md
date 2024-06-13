# Generic-제네릭

제네릭은 타입을 이후에 결정하는 방식으로 재사용성과 유연성을 높이고, 컴파일 시 타입을 지정하여 타입 안정성도 제공합니다. 불필요한 타입 확인과 캐스팅 과정 없이 객체를 효율적으로 사용할 수 있는 자바 기술입니다.

## 제네릭 이전

데이터를 관리하고 사용하는 데이터 컨테이너를 생성할 때, 참조 자료형마다 새 클래스 파일을 작성해야 했습니다. 클래스 정의 시 타입이 정해지기 때문에 타입 안정성은 있지만, 내부 동작 방식은 동일하기 때문에 코드 재사용성과 유연성은 떨어집니다.

1. 타입 안정성을 위해 클래스마다 컨테이너 클래스를 생성하는 방법
    ```java
    public class MemberContainer { ... }
    public class OrderContainer { ... }
    ```
2. 코드의 유연성을 위해 `Object`로 공통 클래스를 생성하는 방법
    ```java
    public class ShareContainer {
        private Object value;
    }
    ```

## 제네릭 방식

코드의 안정성을 높이려면 타입을 클래스를 정의하거나 메서드를 정의할 때 결정해야 합니다. 유연성을 높이려면 다형성을 활용하여 모든 클래스의 상위 클래스인 `Object`를 자료형 타입으로 설정합니다. 제네릭은 **타입 정의를 지연**하여 코드의 안정성과 유연성을 해결했습니다.

그런데 제네릭은 왜 `<int>`나 `<double>`와 같이 기본 자료형을 사용하지 못할까요?

## 기본 자료형이 제외된 이유

자바 제네릭은 기본 자료형(primitive type)을 직접 지원하지 않습니다.

1. **타입 소거(type erasure)**:
   자바의 제네릭은 타입 소거(Type Erasure)라는 메커니즘을 사용합니다. 즉, 제네릭 타입 정보는 컴파일 시점에만 사용되고, 컴파일된 바이트코드에서는 제거됩니다. 런타임 시점에 제네릭 타입 정보는 존재하지 않으며, 모든 제네릭 타입은 `Object`로 대체됩니다.
   ```java
   List<Integer> list = new ArrayList<>();
   ```
   위 코드는 컴파일 후에 다음과 같은 형태가 됩니다.
   ```java
   List list = new ArrayList();
   ```
   따라서 제네릭 클래스 내부에서는 타입 정보를 알 수 없으며, `List<T>`는 런타임에 `List`로 동작합니다.

2. **기본 자료형과 참조 자료형의 차이**:
   기본 자료형(예: `int`, `char`)은 참조 자료형(예: `Integer`, `Character`)과 메모리 관리 방식이 다릅니다. 기본 자료형은 스택 메모리에 직접 저장되지만, 참조 자료형은 힙 메모리에 객체로 저장되고 해당 객체의 참조가 스택에 저장됩니다. 제네릭 클래스는 객체로 작동해야 하는데, 기본 자료형은 객체가 아니기 때문에 직접적으로 사용할 수 없습니다.

3. **오토박싱과 언박싱**:
   자바는 오토박싱(auto-boxing)과 언박싱(unboxing) 기능을 제공합니다. 이는 기본 자료형과 대응되는 래퍼 클래스 간의 자동 변환을 의미합니다. 예를 들어, `int`는 `Integer`로 자동 변환되고, 그 반대도 가능합니다.
   ```java
   List<Integer> list = new ArrayList<>();
   list.add(1);  // 자동으로 Integer.valueOf(1)로 변환됨 (오토박싱)
   int num = list.get(0);  // 자동으로 intValue() 호출됨 (언박싱)
   ```
   이 기능을 통해 기본 자료형을 제네릭 클래스에서 사용할 수 있는 것처럼 보이지만, 실제로는 제네릭 타입에 래퍼 클래스가 사용됩니다.

### 결론

자바 제네릭은 타입 안전성을 향상시키고 코드 재사용성을 높이기 위해 도입되었습니다. 그러나 제네릭 타입은 런타임에 타입 정보를 유지하지 않고 타입 소거를 사용하기 때문에 기본 자료형을 직접 지원할 수 없습니다. 대신, 자바는 오토박싱과 언박싱을 통해 기본 자료형과 대응되는 래퍼 클래스를 사용하여 제네릭 타입과 함께 사용할 수 있도록 합니다.

제네릭은 JDK 5에 도입되었습니다. 자바는 이전 버전 호환성을 위해 컴파일 이후 `.class` 파일에 제네릭 타입을 소거합니다. 컴파일 시점에는 제네릭 제약 조건을 검사하고, 런타임 시에는 제네릭 타입을 제거합니다.

[관련 문서](https://www.devinline.com/2014/02/how-generics-works-internally-in-java.html)  
[관련 문서](https://m.site.naver.com/1lAwK)

## 소거 방식

```java
class GenericsSample<T extends Number> {
    private T data;
    private T[] intArray;

    public GenericsSample(T data){
        this.data = data;
    }

    public T getData(){
        return data;
    }

    // methods for finding sum of all numbers of intArray.
}
```
컴파일 시점에는 제네릭 타입으로 Number를 상속한 클래스만 올 수 있지만, 컴파일이 되면 Number는 소거가 되고 제네릭을 Number로 변환합니다.

```java
class GenericsSample {
    private Number data;
    private Number[] intArray;

    public GenericsSample(Number data){
        this.data = data;
    }

    public Number getData(){
        return (Number) data;
    }

    // methods for finding sum of all numbers of intArray.
}
```

### 예시: 타입 소거와 형변환 문제

부모 Node와 자식 IntegerNode가 있다고 하면,
```java
public class Node<T> {
    private T t;

    public Node(T t) {
        this.t = t;
    }

    public T getT() {
        return t;
    }

    public void setT(T t) {
        System.out.println("Node<T> 클래스의 메서드 호출");
        this.t = t;
    }
}

public class IntegerNode extends Node<Integer> {
    private Integer data;

    public IntegerNode(Integer integer) {
        super(integer);
    }

    @Override
    public void setT(Integer i) {
        System.out.println("IntegerNode에서 호출");
        this.data = i + 1000;
    }
}
```
부모 노드가 컴파일이 되면 제네릭 소거로 아래와 같이 변경됩니다.
```java
public class Node {
    private Object t;

    public Node(Object t) {
        this.t = t;
    }

    public Object getT() {
        return t;
    }

    public void setT(Object t) {
        System.out.println("Node<T> 클래스의 메서드 호출");
        this.t = t;
    }
}
```
Integer 노드를 부모 노드로 형변환을 하고 부모 노드의 메소드를 호출하여 실행하면, 컴파일 시점에서는 오류를 잡지 못합니다.
```java
public class Main {
    public static void main(String[] args) {
        IntegerNode integerNode = new IntegerNode(1);
        Node node = integerNode;
        node.setT("hello");
    }
}
```
하지만 런타임 시에는 예외가 발생합니다.
```java
Exception in thread "main" java.lang.ClassCastException: class java.lang.String cannot be cast to class java.lang.Integer
    at org.example.generic2.IntegerNode.setT(IntegerNode.java:3)
    at org.example.Main.main(Main.java:10)
```
이유는 컴파일 시점에서는 T가 선언되지 않아 Object가 들어올 수 있고, 문자열이 인수로 전달됩니다.
```java
public void setT(Object t) {
    System.out.println("Node<T> 클래스의 메서드 호출");
    this.t = t;
}
@Override
public void setT(Integer i) {
    System.out.println("IntegerNode에서 호출");
    this.data = data + 1000;
}
```
타입이 소거되면서 오버라이딩이 아닌 오버로딩으로 처리하게 되고, 자바는 이러한 간격을 제거하고자 브릿지 메서드를 컴파일 시점에 생성합니다.
```java
### IntegerNode 클래스 내부 메서드
public synthetic bridge setT(Ljava/lang/Object;)V
L0
LINENUMBER 3 L0
ALOAD 0
ALOAD 1
CHECKCAST java/lang/Integer
INVOKEVIRTUAL org/example/generic2/IntegerNode.setT (Ljava/lang/Integer;)V
RETURN
L1
LOCALVARIABLE this Lorg/example/generic2/IntegerNode; L0 L1 0
MAXSTACK = 2
MAXLOCALS = 2
```
여기서 브릿지 메서드가 실행되고, 문자열을 `CHECKCAST java/lang/Integer`로 캐스팅하려다 보니 예외가 발생합니다. 따라서 제네릭 타입을 상속하여 오버라이딩할 때 부모 클래스로 업캐스팅을 하는 경우, 부모 클래스의

자료형을 명시적으로 입력해야 오류를 방지할 수 있습니다.

컴파일 시점에 제네릭 타입을 검사하기 때문에 제네릭 타입보다 하위 타입은 가능하지만, 상위 타입은 예외가 발생합니다.
```java
public class Main {
    public static void main(String[] args) {
        Node<?> node = new Node<String>();
    }
}
```

## 타입 추론

제네릭을 사용할 때 왼쪽 변수와 객체를 생성할 때 같은 제네릭 타입을 지정해야 하지 않고, 참조 자료형의 타입을 보고 자바는 타입 정보를 추론하여 오른쪽에는 타입 정보를 생략할 수 있습니다.

타입 추론은 자바 컴파일러가 타입을 추론할 수 있는 상황에만 가능하며, 타입을 확인할 수 있는 타입 정보가 주변에 있어야 합니다.
```java
// 타입 추론을 사용하지 않을 경우
Container<Integer> integerContainer = new Container<Integer>();
// 타입 추론이 되는 경우
Container<Integer> integerContainer = new Container<>();
```

## 제네릭 용어 정리

+ 제네릭(Generic) 단어
    + 제네릭이라는 단어는 일반적인, 범용적인이라는 의미
    + 특정 타입에 속한 것이 아니라 일반적으로, 범용적으로 사용할 수 있다는 뜻입니다.
+ 제네릭 타입(Generic Type)
    + 클래스나 인터페이스를 정의할 때 타입 매개변수를 사용하는 것을 말합니다.
    + 제네릭 클래스, 제네릭 인터페이스를 모두 합쳐 제네릭 타입이라고 합니다.
        + `타입`은 클래스, 인터페이스, 기본형(int)를 모두 합쳐서 부릅니다.
    + ex) `class Container<T> { private T value; }`
    + `Container<T>`를 제네릭 타입이라고 합니다.
+ 타입 매개변수(Type Parameter)
    + 제네릭 타입이나 메서드에서 사용되는 변수로, 실제 타입으로 대체됩니다.
    + `T, Z, S` 등을 말합니다.
+ 타입 인자(Type Argument)
    + 제네릭 타입을 사용할 때 제공되는 실제 타입입니다.
    + `Container<Member>`, `Container<Order>`
    + `Member`, `Order`를 타입 인자라고 합니다.

제네릭은 메서드와 유사합니다. 제네릭 매개변수를 메서드의 매개변수라고 생각하고, 실제 제네릭 클래스를 생성자로 생성하거나, 제네릭 메서드를 호출할 때 컴파일러에게 `<T>` 대신 사용할 클래스인 타입 인자를 전달하면 됩니다.

## 제네릭 명명 관례

주로 사용하는 키워드는 다음과 같습니다.
+ E - Element
+ K - Key
+ N - Number
+ T - Type
+ V - Value
+ S, U, V 등 - 2nd, 3rd, 4th types

## 타입 매개변수 T 한계

타입 매개변수 T를 통해 실제 사용할 때 타입을 정하는 방식으로 유연성을 높이고 타입 안정성도 얻었습니다. 다만, 클래스나 메서드를 정의할 때 T는 실제 사용하는 시점에 따라 다르기 때문에 소거에 의해서 모든 클래스의 부모인 Object로 정의된다고 했습니다.
```java
public class Container<T> {
    private T data;
    public void set(T data){
        this.data = data;
    }
}
```
Object 클래스가 제공하는 메서드만 사용할 수 있기 때문에 해당 `Container<T>` 제네릭 클래스는 특화된 컨테이너로 활용할 수 없습니다.

### 타입 매개변수 제한

자바는 타입 매개변수를 제한하여 최소한 어떤 클래스를 상속하거나 인터페이스를 구현한 클래스만 타입 인자로 받을 수 있도록 제한할 수 있습니다.
```java
class Container<T extends Member> {
    private T member;

    public void checkGrade(T member){
        if(member.verify()){
            // 로직 수행
        }
    }
}
```
타입 매개변수는 이제 `Member` 클래스를 상속한 타입 인자만 들어올 수 있습니다. `Member` 클래스의 속성과 기능을 활용하여 `Member` 클래스의 특화된 제네릭 클래스 기능을 사용할 수 있습니다.

## 제네릭 메서드

클래스를 보면 멤버 변수와 지역 변수로 나눌 수 있습니다. 같은 변수명일 경우 우선 순위는 메서드 내에 선언되거나 매개 변수명이 우선 적용됩니다. 이는 클래스에 선언된 것과 별개로 메서드는 자신이 사용하는 매개변수를 정해도 상관없다는 의미입니다.
```java
class Member {
    int age; // 멤버 변수
    public void showInfo(int age){ // 지역 변수
        System.out.println(age);
    }
}
```
이렇게 클래스의 타입 매개 변수와 메서드의 타입 매개 변수명을 동일하게 사용하는 건 좋지 않지만, 메서드 안에서 선언된 `T`가 적용됩니다.
```java
class Container<T> {
    private T data;
    public <T> void showInfo(T data){
        System.out.println(data);
    }
}
```
```java
public <T extends Member> T get(T member){
    // member 로직 수행
    return member;
}
```
접근 제어자 옆에 있는 `<T extends>`는 `Container<T extends Member>`와 동일한 역할을 합니다. 타입 인자를 제한하는 타입 매개변수이며, 반환 타입과 매개변수 타입에 적용됩니다.

### 메서드 타입 추론

메서드 타입 추론이 없이 사용한다면 다음과 같은 구조를 갖습니다.
```java
Container home = new Container<Member>();
Son son = new Son(); // Son 클래스는 Member 하위 클래스
home.goHome();
Member member = home.<Member>get(son);
```
컴파일러는 타입 인자 `son`을 통해 타입 인자를 추론하여 생략할 수 있습니다.
```java
Member member = home.get(son);
```

## 와일드 카드

와일드 카드 `?`는 타입 매개변수를 선언하지 않고 간편하게 타입 인자를 제한할 수 있습니다.
```java
public void showMember(Container<? extends Member> container){
    // 로직 수행 후
    Member member = container.get();
}
```

제네릭 타입 매개변수를 사용하면 컴파일 시점에 타입 소거(`Object`나 상한, 하한 클래스)로 변경하는 과정이 필요하지만, 와일드 카드는 그런 과정 없이 타입 인자를 제한할 수 있다는 장점이 있습니다.

### 활용

단순한 타입 인자를 제한하여 사용하고 싶은 경우라면 와일드 카드를 권장합니다. 다만 타입 제한을 하지 않는다면 `<?>`를 그대로 사용하는 것보다는 `<Object>`로 사용하는 것이 더 좋습니다.
+ 반환 타입이 제네릭 타입이 아닌 경우
    ```java
    public void printMember(Container<? extends Member> container){...}
    ```
+ 타입 인자의 상한과 하한을 간단하게 적용하는 경우
    ```java
    public void printMember(Container<? extends Member> container){...}
    public void printMember(Container<? super Member> container){...}
    ```

`super`의 경우에는 `Son > Member > Person > Object` 구조로 상속 관계가 있다면 `? super Person`으로 선언한다면 `Person`, `Object`만 사용이 가능합니다. 하한 제한을 하는 이유는 특정 클래스를 상속하거나 구현한 클래스가 들어오지 못하도록 제한하는 것이 목적입니다.

### 한계

반환 타입을 이후에 결정해야 하는 경우나 타입 인자가 그대로 매개변수로 들어오는 경우에는 와일드 카드를 사용할 수 없습니다.
1. 반환 타입이 제네릭 타입인 경우
    ```java
    public <T extends Member> T get(Container<T> container){...}
    ```
2. 타입 인자가 매개변수 자료형인 경우
    ```java
    public <T extends Member> void showMember(T member){...}
    ```

1, 2번과 같은 경우에는 와일드 카드를 사용할 수 없습니다.

## 주의사항

제네릭 타입과 제네릭 메서드는 타입 인자가 전달되는 시점이 다릅니다.
+ **제네릭 타입**: 타입 인자 전달: 객체를 생성하는 시점
+ **제네릭 메서드**: 타입 인자 전달: 메서드를 호출하는 시점

제네릭 타입은 클래스를 생성할 때 결정되기 때문에 `static`이 붙은 정적 메서드나 정적 필드에 사용할 수 없습니다. 제네릭 타입은 인스턴스 레벨이지만, 정적 필드와 메서드는 클래스 레벨로 클래스 생성하기 전 호출 단계에서 이미 타입이 정해져야 하기 때문입니다.

대신 제네릭 메서드 내부에서는 `public static <T extends Member> T getMember(T member){...}` 구조는 가능합니다. 이유는 컴파일러가 타입을 체크하는 시점이 메서드를 호출할 때 발생하므로 제네릭 클래스와 다르게 체크하는 시점이 다르기 때문입니다.