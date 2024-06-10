# 자바 static 키워드

## 1. static 키워드의 활용과 흔히 발생할 수 있는 오류
`static` 키워드는 클래스 수준의 변수를 정의하거나 메소드를 정의할 때 사용합니다. 이는 해당 멤버가 인스턴스가 아닌 클래스에 속하게 됩니다. 다음은 `static` 키워드의 활용 예입니다:

```java
public class Example {
    public static int staticVariable = 0;

    public static void staticMethod() {
        System.out.println("Static method called");
    }
}
```

### 흔히 발생할 수 있는 오류
- **동시성 문제**: `static` 변수는 모든 인스턴스가 공유하기 때문에 여러 스레드에서 동시에 접근하면 데이터 불일치 문제가 발생할 수 있습니다.
- **메모리 누수**: `static` 변수는 프로그램이 종료될 때까지 메모리에 남아 있을 수 있어 메모리 누수의 원인이 될 수 있습니다.
- **의존성 문제**: `static` 변수에 의존하는 코드가 많아질수록 코드의 유연성이 떨어집니다.

### 올바르게 사용해야 하는 이유
- **데이터 일관성**: 여러 인스턴스가 동일한 데이터를 공유해야 할 때 유용하지만, 이를 올바르게 관리하지 않으면 데이터 불일치 문제가 발생할 수 있습니다.
- **메모리 관리**: `static` 변수가 불필요하게 메모리를 점유하지 않도록 주의해야 합니다.

## 2. 동시성 문제와 인스턴스 메소드/변수 호출 예시
동시성 문제는 여러 스레드가 `static` 변수를 동시에 수정할 때 발생합니다.

### 예시
```java
public class Counter {
    public static int count = 0;

    public static synchronized void increment() {
        count++;
    }
}
```

위 코드에서 `increment` 메소드는 `synchronized`로 동기화하여 동시성 문제를 해결합니다.

### 인스턴스 메소드와 변수 호출
```java
public class Example {
    public int instanceVariable = 0;

    public void instanceMethod() {
        System.out.println("Instance method called");
    }
}
```

`static` 메소드는 인스턴스 메소드와 변수를 직접 호출할 수 없습니다. 클래스 인스턴스를 통해 호출해야 합니다.

## 3. static 변수의 동시성 문제와 해결 방법
동시성 문제는 여러 스레드가 `static` 변수를 동시에 수정할 때 발생합니다.

### 문제 예시
```java
public class Counter {
    public static int count = 0;

    public static void increment() {
        count++;
    }
```

여러 스레드가 동시에 `increment` 메소드를 호출하면 `count` 값이 정확하지 않을 수 있습니다.

### 해결 방법
- **synchronized 사용**: 메소드를 동기화하여 한 번에 하나의 스레드만 접근하도록 합니다.
- **Atomic 객체 사용**: `AtomicInteger`와 같은 객체를 사용하여 원자적 연산을 수행합니다.

```java
public class Counter {
    public static AtomicInteger count = new AtomicInteger(0);

    public static void increment() {
        count.incrementAndGet();
    }
}
```

### 상속과 관련된 장점
`static` 메소드는 클래스 수준에서 정의되므로 하위 클래스에서 재정의할 수 없으며, 이는 메소드가 변경되지 않고 일관되게 사용되도록 보장합니다.

## 4. synchronized 키워드와 atomic 객체의 차이
- **synchronized 키워드**: 메소드나 블록을 동기화하여 한 번에 하나의 스레드만 접근하도록 합니다.
- **Atomic 객체**: `AtomicInteger`, `AtomicBoolean` 등은 원자적 연산을 제공하여 동기화 없이도 스레드 안전성을 보장합니다.

```java
public class Counter {
    private static int count = 0;

    public static synchronized void synchronizedIncrement() {
        count++;
    }

    public static AtomicInteger atomicCount = new AtomicInteger(0);

    public static void atomicIncrement() {
        atomicCount.incrementAndGet();
    }
}
```

### static 메소드 재정의
`static` 메소드는 클래스 메소드이므로 하위 클래스에서 재정의할 수 없습니다. 다만, 동일한 이름의 메소드를 정의하여 숨길 수 있습니다.

## 5. static 메소드를 하위 클래스에서 숨기기 및 변경
하위 클래스에서 상위 클래스의 `static` 메소드를 숨길 수 있습니다.

### 숨기기 예시
```java
public class Parent {
    public static void staticMethod() {
        System.out.println("Parent static method");
    }
}

public class Child extends Parent {
    public static void staticMethod() {
        System.out.println("Child static method");
    }
}
```

상위 클래스의 `static` 메소드를 변경하려면 동일한 이름으로 정의하면 됩니다. 이 경우 상위 클래스의 메소드는 숨겨지며, 하위 클래스의 메소드가 호출됩니다.

### 접근 방법
상위 클래스의 `static` 메소드에 접근하려면 클래스 이름을 명시적으로 사용해야 합니다.

```java
Parent.staticMethod(); // Parent static method
Child.staticMethod();  // Child static method
```

이로써 `static` 키워드와 관련된 주요 개념과 문제점, 해결 방법에 대해 설명드렸습니다. 도움이 되셨기를 바랍니다.

## 추가 설명: 클래스 레벨과 인스턴스 레벨의 차이

네, 맞습니다. `static` 메소드가 클래스 레벨에서 정의된다는 것은 클래스 자체에 귀속된다는 의미입니다. 이를 좀 더 자세히 설명하면 다음과 같습니다:

## 클래스 레벨에서 정의된다는 의미

1. **클래스의 구성원**: `static` 메소드는 인스턴스가 아닌 클래스 자체의 구성원입니다. 즉, 클래스가 로드될 때 메소드가 메모리에 할당되고, 특정 객체가 생성되지 않아도 호출할 수 있습니다.

2. **인스턴스와 무관**: `static` 메소드는 특정 객체 인스턴스와 무관하게 호출됩니다. 따라서, 객체 인스턴스가 아닌 클래스 이름을 통해 직접 호출합니다.

   ```java
   public class MyClass {
       public static void myStaticMethod() {
           System.out.println("Static method called");
       }
   }

   public class Main {
       public static void main(String[] args) {
           MyClass.myStaticMethod(); // 클래스 이름으로 직접 호출
       }
   }
   ```

3. **오버라이딩 불가**: `static` 메소드는 클래스에 귀속되기 때문에, 인스턴스 메소드와 달리 하위 클래스에서 오버라이드할 수 없습니다. 오버라이딩은 인스턴스 메소드에만 해당하는 개념입니다. 대신, 하위 클래스에서 동일한 이름과 시그니처의 `static` 메소드를 정의할 수 있으며, 이것은 부모 클래스의 메소드를 숨기는(hiding) 효과를 가집니다.

## 예시로 이해하기

```java
class Parent {
    public static void display() {
        System.out.println("Static method in Parent");
    }
}

class Child extends Parent {
    public static void display() {
        System.out.println("Static method in Child");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent.display(); // "Static method in Parent"
        Child.display();  // "Static method in Child"

        Parent p = new Child();
        p.display(); // "Static method in Parent" - 부모 클래스 타입으로 참조 시 부모 클래스 메소드 호출
    }
}
```

위 예제에서 `Parent` 클래스와 `Child` 클래스 모두 `display`라는 `static` 메소드를 가지고 있습니다. 하지만, 이 메소드들은 클래스 레벨에서 각각 정의되었기 때문에 실제로는 서로 다른 메소드입니다.

- `Parent.display()`를 호출하면 `Parent` 클래스의 `display` 메소드가 호출됩니다.
- `Child.display()`를 호출하면 `Child` 클래스의 `display` 메소드가 호출됩니다.
- `Parent p = new Child();`로 선언된 `p`를 통해 `display`를 호출하면, `p`가 `Parent` 타입이기 때문에 `Parent` 클래스의 `display` 메소드가 호출됩니다.

이처럼 `static` 메소드는 클래스 레벨에서 정의되므로, 클래스 이름을 통해 호출되고, 오버라이딩할 수 없으며, 하위 클래스에서 동일한 이름의 `static` 메소드를 정의하면 부모 클래스의 메소드를 숨기는(hiding) 효과가 있습니다.

네, Java에서 메소드는 주로 두 가지 레벨에서 정의될 수 있습니다: 클래스 레벨과 인스턴스 레벨입니다. 이 두 가지 레벨의 차이점을 이해하면 Java에서 메소드 호출과 관련된 다양한 개념을 명확히 이해할 수 있습니다.

### 1. 클래스 레벨 (Class Level)

클래스 레벨에서는 `static` 키워드를 사용하여 메소드와 변수를 정의합니다. 클래스 레벨에서 정의된 메소드와 변수는 클래스 자체에 귀속되며, 인스턴스를 생성하지 않아도 클래스 이름을 통해 접근할 수 있습니다.

#### 특징

- **클래스에 속함**: `static` 메소드와 변수는 클래스 로드 시점에 메모리에 할당됩니다.
- **인스턴스 불필요**: 클래스 이름으로 직접 호출할 수 있습니다.
- **공유 자원**: 모든 인스턴스가 공유하는 자원으로 사용됩니다.

#### 예제

```java
public class MyClass {
    public static int staticVar = 0;

    public static void staticMethod() {
        System.out.println("Static method called");
    }
}

public class Main {
    public static void main(String[] args) {
        MyClass.staticMethod(); // 클래스 이름으로 직접 호출
        System.out.println(MyClass.staticVar); // 클래스 이름으로 직접 접근
    }
}
```

### 2. 인스턴스 레벨 (Instance Level)

인스턴스 레벨에서는 `static` 키워드를 사용하지 않고 메소드와 변수를 정의합니다. 인스턴스 레벨에서 정의된 메소드와 변수는 객체 인스턴스에 귀속되며, 각 인스턴스마다 별도로 존재합니다.

#### 특징

- **인스턴스에 속함**: 인스턴스를 생성할 때 메모리에 할당됩니다.
- **인스턴스 필요**: 객체 인스턴스를 통해 호출해야 합니다.
- **독립적 자원**: 각 인스턴스가 독립적으로 가지는 자원입니다.

#### 예제

```java
public class MyClass {
    public int instanceVar = 0;

    public void instanceMethod() {
        System.out.println("Instance method called");
    }
}

public class Main {
    public static void main(String[] args) {
        MyClass obj1 = new MyClass();
        MyClass obj2 = new MyClass();

        obj1.instanceMethod(); // 인스턴스를 통해 호출
        obj2.instanceMethod(); // 다른 인스턴스를 통해 호출

        obj1.instanceVar = 5;
        obj2.instanceVar = 10;
        
        System.out.println(obj1.instanceVar); // 5
        System.out.println(obj2.instanceVar); // 10
    }
}
```

## 요약

- **클래스 레벨** (`static` 키워드 사용): 클래스에 귀속되어 인스턴스 없이 호출할 수 있으며, 모든 인스턴스가 공유합니다.
- **인스턴스 레벨** (`static` 키워드 미사용): 인스턴스에 귀속되어 각 인스턴스마다 독립적으로 존재하며, 객체 인스턴스를 통해 호출합니다.

## 메모리 누수가 발생하는 이유
1. **클래스 로딩과 해제**: 클래스가 로드될 때 `static` 변수가 메모리에 할당되고, 프로그램 종료 시까지 해제되지 않기 때문에 장시간 실행되는 애플리케이션에서는 메모리 누수가 발생할 수 있습니다.
2. **불필요한 참조**: `static` 변수가 외부 리소스나 객체를 참조하고 있을 경우, 해당 리소스나 객체가 해제되지 않고 메모리에 남아 있을 수 있습니다.
3. **리소스 관리**: 파일, 데이터베이스 연결 등과 같은 리소스를 `static` 변수에 보관할 경우, 적절히 해제하지 않으면 메모리 누수가 발생할 수 있습니다.

### 메모리 누수를 방지하는 방법
1. **명시적 해제**: 더 이상 필요 없는 `static` 변수나 리소스는 명시적으로 `null`로 설정하거나 해제하여 메모리 누수를 방지할 수 있습니다.
2. **자원 관리**: `static` 변수를 사용할 때는 반드시 필요한 경우에만 사용하고, 가능한 범위를 최소화하여 사용합니다.

```java
public class Example {
    public static List<String> data = new ArrayList<>();

    public static void loadData() {
        // 데이터를 로드하는 코드
    }

    public static void clearData() {
        // 더 이상 필요 없는 경우 명시적으로 데이터 해제
        data.clear();
        data = null;
    }
}
```
위 예제에서 `data` 변수는 `static`으로 선언되어 클래스 로드 시점에 메모리에 할당됩니다. 더 이상 필요 없을 때는 `clearData` 메소드를 통해 데이터를 명시적으로 해제하여 메모리 누수를 방지할 수 있습니다.
따라서, `static` 변수를 사용할 때는 메모리 관리에 주의를 기울여야 하며, 불필요한 메모리 점유를 최소화하는 것이 중요합니다.