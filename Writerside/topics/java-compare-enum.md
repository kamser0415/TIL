# 자바 enum

## 불변 상수 변수, 인터페이스 상수, 클래스 내 클래스 생성 방식과 그 단점

자바 `enum`이 도입되기 이전에는 상수를 정의하기 위해 다양한 방법들이 사용되었습니다. 여기서는 불변 상수 변수, 인터페이스 상수, 클래스 내 클래스 생성 방식을 소개하고, 각각의 단점을 설명한 후 왜 `enum`이 더 나은지 알아보겠습니다.

### 불변 상수 변수 (Static Final Constants)

이 방식은 `static final` 키워드를 사용하여 상수를 정의하는 가장 일반적인 방법입니다.

```Java
public class Constants {
    public static final int SUNDAY = 0;
    public static final int MONDAY = 1;
    public static final int TUESDAY = 2;
    public static final int WEDNESDAY = 3;
    public static final int THURSDAY = 4;
    public static final int FRIDAY = 5;
    public static final int SATURDAY = 6;
}
```

**단점:**
- **타입 안전성 부족**: 상수들은 단순한 정수 값으로 정의되기 때문에 잘못된 값을 사용하거나 비교할 위험이 있습니다.
- **가독성 저하**: 상수의 의미를 이해하기 어려울 수 있습니다.
- **확장성 부족**: 추가적인 메서드나 필드를 정의하기 어렵습니다.

### 인터페이스 상수 (Constant Interface)

인터페이스에 상수를 정의하여 사용하는 방법입니다.

```Java
public interface Day {
    int SUNDAY = 0;
    int MONDAY = 1;
    int TUESDAY = 2;
    int WEDNESDAY = 3;
    int THURSDAY = 4;
    int FRIDAY = 5;
    int SATURDAY = 6;
}
```

**단점:**
- **상속의 오용**: 인터페이스 상수는 상수 값을 공유하기 위해 상속을 사용하는데, 이는 객체지향 설계 원칙에 어긋납니다.
- **타입 안전성 부족**: 여전히 정수 값으로 정의되기 때문에 잘못된 값을 사용할 위험이 있습니다.
- **가독성 저하**: 상수의 의미를 명확하게 전달하기 어렵습니다.

### 클래스 내 클래스 생성 (Enum-like Class)

상수를 가진 클래스를 생성하고, 인스턴스를 생성하지 못하도록 private 생성자를 사용하는 방법입니다.

```Java
public class Day {
    public static final Day SUNDAY = new Day("SUNDAY");
    public static final Day MONDAY = new Day("MONDAY");
    public static final Day TUESDAY = new Day("TUESDAY");
    public static final Day WEDNESDAY = new Day("WEDNESDAY");
    public static final Day THURSDAY = new Day("THURSDAY");
    public static final Day FRIDAY = new Day("FRIDAY");
    public static final Day SATURDAY = new Day("SATURDAY");

    private String name;

    private Day(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

**단점:**
- **복잡성 증가**: 단순한 상수 정의를 위해 복잡한 클래스를 만들어야 합니다.
- **인스턴스 비교의 불편함**: `==` 연산자가 아닌 `equals()` 메서드를 사용해야 하므로 코드가 불편해집니다.
- **메모리 낭비**: 각 상수마다 별도의 객체를 생성하므로 메모리를 더 많이 사용합니다.

## Enum의 특징

자바 5부터 도입된 `enum`은 위의 단점들을 극복하고 여러 가지 장점을 제공합니다:

1. **타입 안전성**:
    - `enum`은 특정 집합의 값들만 가질 수 있도록 제한하므로, 잘못된 값이 사용될 가능성을 방지합니다.

2. **가독성**:
    - 열거형을 사용하면 코드의 의미가 명확해집니다. 상수의 의미를 직관적으로 이해할 수 있습니다.

3. **조작 용이성**:
    - `enum`은 메서드와 속성을 가질 수 있어, 관련된 상수 값들을 조작하는 데 유용합니다.

4. **싱글톤 패턴**:
    - `enum`은 암묵적으로 싱글톤 패턴을 따르므로, 특정 상수 값을 전역에서 공유하고자 할 때 유용합니다.

5. **상수 그룹화**:
    - `enum`은 관련된 상수들을 하나의 그룹으로 묶어 관리할 수 있어, 코드의 구조를 더 체계적으로 만들 수 있습니다.

6. **패턴 매칭과 정렬**:
    - `enum`은 `Comparable` 인터페이스를 구현하므로, 자연스럽게 정렬할 수 있습니다.

7. **JSON 및 데이터베이스 매핑**:
    - 다양한 라이브러리에서 JSON 직렬화/역직렬화와 데이터베이스 매핑에 유용하게 사용됩니다.

## 기본적인 정의와 생성 방법, 그리고 장점

### 기본적인 정의
`enum`(열거형)은 관련된 상수들을 그룹화하는 특수한 클래스입니다. 예를 들어, 요일, 달, 카드의 무늬 등과 같은 고정된 상수 값을 열거하는 데 유용합니다. `enum`은 상수들을 좀 더 의미 있게 관리하고 코드의 가독성을 높이는 데 사용됩니다.

### 생성 방법
자바에서 `enum`을 정의하는 기본적인 방법은 다음과 같습니다:

```Java
public enum Day {
    SUNDAY,
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY
}
```

위의 예시에서는 `Day`라는 이름의 `enum`을 정의하였고, `SUNDAY`부터 `SATURDAY`까지의 요일을 상수로 열거하였습니다.

`enum`은 클래스처럼 필드, 메서드, 생성자를 가질 수 있습니다. 이를 통해 보다 복잡한 기능을 갖춘 열거형 타입을 정의할 수도 있습니다.

```Java
public enum Planet {
    MERCURY(3.303e+23, 2.4397e6),
    VENUS(4.869e+24, 6.0518e6),
    EARTH(5.976e+24, 6.37814e6),
    MARS(6.421e+23, 3.3972e6);

    private final double mass;   // 단위: kg
    private final double radius; // 단위: m

    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
    }

    public double mass() { return mass; }
    public double radius() { return radius; }

    // 표면 중력을 계산하는 메서드
    public double surfaceGravity() {
        final double G = 6.67300E-11;
        return G * mass / (radius * radius);
    }
}
```

위 예제는 각 행성의 질량과 반지름을 속성으로 가지는 `Planet` 열거형을 정의하고 있습니다. `Planet` enum은 생성자, 필드, 메서드를 가질 수 있음을 보여줍니다.

### 3. 장점

1. **타입 안전성**:
    - `enum`은 특정 집합의 값들만 가질 수 있도록 제한하므로, 잘못된 값이 사용될 가능성을 방지합니다. 이는 코드의 안정성과 신뢰성을 높입니다.
   ```Java
   public void setDay(Day day) {
       // 오직 Day enum에 정의된 값들만 허용됩니다.
   }
   ```

2. **가독성**:
    - 열거형을 사용하면 코드의 의미가 명확해집니다. 상수의 의미를 직관적으로 이해할 수 있습니다.
   ```Java
   Day today = Day.MONDAY;
   ```

3. **조작 용이성**:
    - `enum`은 메서드와 속성을 가질 수 있어, 관련된 상수 값들을 조작하는 데 유용합니다. 복잡한 로직을 처리하기 위해 메서드를 추가할 수 있습니다.
   ```Java
   Planet earth = Planet.EARTH;
   double gravity = earth.surfaceGravity();
   ```

4. **싱글톤 패턴**:
    - `enum`은 암묵적으로 싱글톤 패턴을 따르므로, 특정 상수 값을 전역에서 공유하고자 할 때 유용합니다.

5. **상수 그룹화**:
    - `enum`은 관련된 상수들을 하나의 그룹으로 묶어 관리할 수 있어, 코드의 구조를 더 체계적으로 만들 수 있습니다.

## 열거 상수의 사용 및 메서드와 필드 추가 방법

### 1. 열거 상수의 사용
열거 상수는 `enum` 타입에서 정의된 고유의 값입니다. 이 값들은 `enum` 타입을 통해서만 접근할 수 있습니다. 열거 상수는 마치 정적 상수처럼 사용되며, 예를 들어 `Day.MONDAY`처럼 사용할 수 있습니다.

```Java
public class Main {
    public static void main(String[] args)

 {
        Day today = Day.MONDAY;
        
        if (today == Day.MONDAY) {
            System.out.println("Today is Monday!");
        }
        
        for (Day day : Day.values()) {
            System.out.println(day);
        }
    }
}
```

### 2. 메서드와 필드 추가
`enum`은 단순히 상수의 집합일 뿐만 아니라, 필드와 메서드를 가질 수 있습니다. 이를 통해 열거 상수에 추가적인 속성이나 동작을 부여할 수 있습니다.

다음은 각 요일에 작업 시간을 추가하고, 작업 시간이 있는지 여부를 확인하는 메서드를 가진 `Day` 열거형의 예제입니다:

```Java
public enum Day {
    SUNDAY(false),
    MONDAY(true),
    TUESDAY(true),
    WEDNESDAY(true),
    THURSDAY(true),
    FRIDAY(true),
    SATURDAY(false);

    private final boolean workDay;

    Day(boolean workDay) {
        this.workDay = workDay;
    }

    public boolean isWorkDay() {
        return workDay;
    }
}

public class Main {
    public static void main(String[] args) {
        Day today = Day.SUNDAY;
        
        if (today.isWorkDay()) {
            System.out.println(today + " is a workday.");
        } else {
            System.out.println(today + " is not a workday.");
        }
    }
}
```

### 열거형에 부가적인 기능 추가
`enum`은 더 복잡한 로직을 포함할 수 있으며, 생성자, 필드, 메서드뿐만 아니라 `abstract` 메서드를 정의하고 각 열거 상수에서 이를 구현할 수도 있습니다.

예를 들어, 각 열거 상수가 다른 구현을 가지는 `abstract` 메서드를 추가하는 예제입니다
이 예제에서는 각 열거 상수가 `apply` 메서드를 고유하게 구현하여, 더 복잡한 행동을 할 수 있게 합니다.

```Java
public enum Operation {
    PLUS {
        double apply(double x, double y) { return x + y; }
    },
    MINUS {
        double apply(double x, double y) { return x - y; }
    },
    TIMES {
        double apply(double x, double y) { return x * y; }
    },
    DIVIDE {
        double apply(double x, double y) { return x / y; }
    };
    abstract double apply(double x, double y);
}
```

## 요약
- **열거 상수 사용**: `Day.MONDAY`처럼 사용하며, `enum` 타입을 통해 접근.
- **필드와 메서드 추가**: 각 열거 상수에 추가적인 속성과 동작을 부여하여 더 풍부한 기능을 제공.
- **추가 기능**: `abstract` 메서드와 이를 각 열거 상수에서 구현하여 다형성을 활용.

## `enum`과 `switch`의 사용 예제

자바에서 `enum`을 사용할 때, `switch` 문과 함께 사용하면 열거형의 모든 가능한 값을 처리하도록 강제할 수 있습니다. 자바 컴파일러는 `switch` 문에서 `enum`의 모든 값을 처리하지 않으면 경고를 줄 수 있습니다. 또한, 모든 가능한 `enum` 값을 처리했는지 확인하기 위해 `default` 절을 생략할 수 있습니다.

### 기본 예제

```Java
public enum Day {
    SUNDAY,
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY
}

public class Main {
    public static void main(String[] args) {
        Day today = Day.MONDAY;

        switch (today) {
            case SUNDAY:
                System.out.println("It's Sunday!");
                break;
            case MONDAY:
                System.out.println("It's Monday!");
                break;
            case TUESDAY:
                System.out.println("It's Tuesday!");
                break;
            case WEDNESDAY:
                System.out.println("It's Wednesday!");
                break;
            case THURSDAY:
                System.out.println("It's Thursday!");
                break;
            case FRIDAY:
                System.out.println("It's Friday!");
                break;
            case SATURDAY:
                System.out.println("It's Saturday!");
                break;
        }
    }
}
```

위의 코드에서는 `Day` `enum`의 모든 가능한 값을 `switch` 문에서 처리합니다. 컴파일러는 모든 `enum` 값을 처리하지 않으면 경고를 줄 수 있습니다.

### `default` 절 생략

모든 가능한 `enum` 값을 처리했는지 확인하기 위해 `default` 절을 생략할 수 있습니다. 이렇게 하면 새로운 `enum` 값이 추가될 때 컴파일러가 이를 감지하도록 도와줍니다.

```Java
public class Main {
    public static void main(String[] args) {
        Day today = Day.MONDAY;

        switch (today) {
            case SUNDAY:
                System.out.println("It's Sunday!");
                break;
            case MONDAY:
                System.out.println("It's Monday!");
                break;
            case TUESDAY:
                System.out.println("It's Tuesday!");
                break;
            case WEDNESDAY:
                System.out.println("It's Wednesday!");
                break;
            case THURSDAY:
                System.out.println("It's Thursday!");
                break;
            case FRIDAY:
                System.out.println("It's Friday!");
                break;
            case SATURDAY:
                System.out.println("It's Saturday!");
                break;
        }
    }
}
```

이렇게 `default` 절을 생략하면, 만약 새로운 `enum` 값이 추가되었을 때 `switch` 문에서 이를 처리하지 않으면 컴파일러가 경고를 발생시키므로 유지보수에 유리합니다.

## 추가 장점

1. **강력한 기능의 클래스**:
    - `enum`은 단순한 상수 집합이 아닌, 다양한 메서드와 필드를 가질 수 있는 강력한 클래스입니다. 이를 통해 복잡한 기능을 구현할 수 있습니다.

   ```Java
   public enum Planet {
       MERCURY(3.303e+23, 2.4397e6),
       VENUS(4.869e+24, 6.0518e6),
       EARTH(5.976e+24, 6.37814e6),
       MARS(6.421e+23, 3.3972e6);
   
       private final double mass;   // kg
       private final double radius; // meters
   
       Planet(double mass, double radius) {
           this.mass = mass;
           this.radius = radius;
       }
   
       public double mass() { return mass; }
       public double radius() { return radius; }
   
       public double surfaceGravity() {
           final double G = 6.67300E-11;
           return G * mass / (radius * radius);
       }
   }
   ```

2. **싱글톤 패턴의 구현**:
    - `enum`을 사용하여 싱글톤 패턴을 쉽게 구현할 수 있습니다. 이는 하나의 인스턴스만을 가지도록 보장하는 디자인 패턴입니다.

   ```Java
   public enum Singleton {
       INSTANCE;
   
       public void doSomething() {
           System.out.println("Doing something...");
       }
   }
   
   public class Main {
       public static void main(String[] args) {
           Singleton singleton = Singleton.INSTANCE;
           singleton.doSomething();
       }
   }
   ```

3. **스레드 안전성**:
    - `enum`은 스레드 안전성을 자연스럽게 보장합니다. 이는 멀티스레드 환경에서 안전하게 사용할 수 있음을 의미합니다.

4. **패턴 매칭**:
    - 자바에서는 아직 패턴 매칭이 언어 수준에서 완전히 지원되지 않지만, `enum`과 `instanceof`를 사용한 패턴 매칭 기법을 활용할 수 있습니다. 이는 가독성을 높이고, 특정 열거 상수에 대한 처리를 명확하게 합니다.

5. **정렬과 비교**:
    - `enum`은 `Comparable` 인터페이스를 구현하므로, 자연스럽게 정렬할 수 있습니다. 이는 정렬이나 비교 연산에서 유용합니다.

   ```Java
   public enum Level {
       LOW, MEDIUM, HIGH;
   }
   
   public class Main {
       public static void main(String[] args) {
           Level[] levels = Level.values();
           Arrays.sort(levels);
   
           for (Level level : levels) {
               System.out.println(level);
           }
       }
   }
   ```

6. **JSON 및 데이터베이스 매핑**:
    - `enum`은 다양한 라이브러리에서 JSON 직렬화/역직렬화와 데이터베이스 매핑에 유용하게 사용됩니다. 예를 들어, Jackson 라이브러리를 사용하면 `enum`을 JSON 데이터와 쉽게 매핑할 수 있습니다.

   ```Java
   public enum Status {
       ACTIVE, INACTIVE, PENDING;
   }
   
   public class User {
       public String name;
       public Status status;
   
       public User(String name, Status status) {
          
    
    this.name = name;
    this.status = status;
    }
    }
    
    public class Main {
    public static void main(String[] args) throws JsonProcessingException {
    User user = new User("John", Status.ACTIVE);
    
               ObjectMapper mapper = new ObjectMapper();
               String jsonString = mapper.writeValueAsString(user);
       
               System.out.println(jsonString);  // {"name":"John","status":"ACTIVE"}
           }
    }
    ```

## 요약

`enum`을 사용하면 타입 안전성, 가독성, 강력한 기능의 클래스, 싱글톤 패턴, 스레드 안전성, 패턴 매칭, 정렬 및 비교, JSON 및 데이터베이스 매핑 등 다양한 장점을 얻을 수 있습니다. 이러한 특징들은 코드를 더욱 안전하고 효율적으로 만들며, 유지보수성을 높이는 데 큰 도움이 됩니다.

## 결론
자바 `enum`은 상수 값을 그룹화하여 타입 안전성을 높이고, 코드의 가독성과 유지보수성을 개선하는 데 큰 장점이 있습니다. 이는 불변 상수 변수, 인터페이스 상수, 클래스 내 클래스 생성 방식의 단점을 극복하며, 보다 직관적이고 신뢰성 있는 코드를 작성할 수 있게 합니다.