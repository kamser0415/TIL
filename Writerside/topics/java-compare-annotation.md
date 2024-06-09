# 자바의 어노테이션

## 어노테이션이란 무엇이며, 왜 사용하는가?

어노테이션(Annotation)은 자바 코드에 추가적인 메타데이터를 제공하는 방법입니다. 주석처럼 보이지만, 실제로 코드 실행에 영향을 미치거나 컴파일러에 정보를 제공하는 데 사용됩니다. 어노테이션을 사용하면 코드의 가독성을 높이고, 코드의 의미를 명확히 하며, 컴파일러가 특정 경고를 무시하도록 하거나, 런타임에 특정 동작을 수행하도록 할 수 있습니다.

### 어노테이션의 종류와 사용 예

#### Built-in 어노테이션
- **@Override**: 메서드가 슈퍼클래스의 메서드를 오버라이드하고 있음을 컴파일러에 알립니다.
    ```java
    @Override
    public String toString() {
        return "Example";
    }
    ```
- **@Deprecated**: 특정 요소가 더 이상 사용되지 않음을 나타내며, 사용 시 경고를 발생시킵니다.
    ```java
    @Deprecated
    public void oldMethod() {
        // old implementation
    }
    ```
- **@SuppressWarnings**: 컴파일러가 특정 경고를 무시하도록 합니다.
    ```java
    @SuppressWarnings("unchecked")
    public void method() {
        // code that generates unchecked warning
    }
    ```

#### 메타 어노테이션

메타 어노테이션은 다른 어노테이션을 정의하는 데 사용됩니다. 주요 메타 어노테이션은 다음과 같습니다:

- **@Retention**: 어노테이션이 유지되는 시점을 지정합니다.
- **@Target**: 어노테이션이 적용될 수 있는 자바 요소를 지정합니다.
- **@Documented**: 어노테이션이 Javadoc에 포함되도록 합니다.
- **@Inherited**: 어노테이션이 서브클래스에 상속되도록 합니다.
- **@Repeatable**: 어노테이션을 동일한 요소에 여러 번 적용할 수 있도록 합니다.

### 각각의 값들이 어떤 옵션들을 가질 수 있는지에 대한 설명

#### @Retention

RetentionPolicy 옵션은 다음과 같습니다:
- **SOURCE**: 소스 코드에만 존재하고, 컴파일 시 제거됩니다.
- **CLASS**: 컴파일된 클래스 파일에 존재하지만, 런타임에는 사용할 수 없습니다.
- **RUNTIME**: 런타임 시에도 어노테이션 정보를 사용할 수 있습니다.

#### @Target

ElementType 옵션은 다음과 같습니다:
- **TYPE**: 클래스, 인터페이스, 열거형에 적용됩니다.
- **FIELD**: 필드에 적용됩니다.
- **METHOD**: 메서드에 적용됩니다.
- **PARAMETER**: 메서드 매개변수에 적용됩니다.
- **CONSTRUCTOR**: 생성자에 적용됩니다.
- **LOCAL_VARIABLE**: 지역 변수에 적용됩니다.
- **ANNOTATION_TYPE**: 어노테이션 타입에 적용됩니다.
- **PACKAGE**: 패키지에 적용됩니다.
- **TYPE_PARAMETER**: 타입 매개변수에 적용됩니다.
- **TYPE_USE**: 사용되는 모든 타입에 적용됩니다.

### Java에서 RetentionPolicy를 사용하는 방법과 규칙

RetentionPolicy를 사용하는 방법은 어노테이션 정의 시 @Retention 메타 어노테이션을 사용하는 것입니다.

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface CustomAnnotation {
    String value();
}
```

RetentionPolicy의 각 옵션은 다음과 같은 영향을 미칩니다:
- **SOURCE**: 컴파일 시 사라지므로, 코드 분석 도구에서만 유용합니다.
- **CLASS**: 클래스 파일에 포함되지만 런타임에 사용할 수 없습니다.
- **RUNTIME**: 런타임에 리플렉션을 통해 어노테이션 정보를 사용할 수 있습니다.

#### @Target 어노테이션의 ANNOTATION_TYPE의 사용 예시
```java
@Target(ElementType.ANNOTATION_TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface MetaAnnotation {
}
```

### Java에서 사용되는 다른 메타 어노테이션

- **@Override**: 슈퍼클래스의 메서드를 오버라이드하고 있음을 나타내어 컴파일러가 이 정보를 바탕으로 검사를 할 수 있습니다.
- **@Documented**: Javadoc에 어노테이션 정보를 포함시킵니다.
- **@Inherited**: 서브클래스가 슈퍼클래스의 어노테이션을 상속받도록 합니다.
- **@Repeatable**: 어노테이션을 반복해서 사용할 수 있도록 합니다.

```java
@Inherited
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface CustomInheritedAnnotation {
}

@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface CustomDocumentedAnnotation {
}
```

### @Documented, @Inherited, @Repeatable의 구체적인 역할과 사용 예시

- **@Documented**: Javadoc에서 어노테이션의 설명을 포함하도록 합니다.
    ```java
    @Documented
    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.METHOD)
    public @interface CustomDocumented {
        String value();
    }
    ```

- **@Inherited**: 어노테이션이 서브클래스에 상속되도록 합니다.
    ```java
    @Inherited
    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.TYPE)
    public @interface CustomInherited {
    }

    @CustomInherited
    public class SuperClass {
    }

    public class SubClass extends SuperClass {
    }
    ```

- **@Repeatable**: 어노테이션을 동일한 요소에 여러 번 사용할 수 있도록 합니다.
    ```java
    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.METHOD)
    @Repeatable(CustomAnnotations.class)
    public @interface CustomAnnotation {
        String value();
    }

    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.METHOD)
    public @interface CustomAnnotations {
        CustomAnnotation[] value();
    }

    public class Example {
        @CustomAnnotation("one")
        @CustomAnnotation("two")
        public void method() {
        }
    }
    ```

### 리플렉션을 통한 어노테이션 처리

#### 리플렉션을 통한 메타데이터 조작의 장단점

**장점**:
- 런타임에 동적으로 어노테이션을 확인하고 처리할 수 있습니다.
- 플러그인 시스템이나 유연한 코드를 작성하는 데 유용합니다.

**단점**:
- 성능 오버헤드가 발생할 수 있습니다.
- 코드의 가독성과 유지보수가 어려울 수 있습니다.

#### 접근제어자를 통한 보안 강화

리플렉션 사용 시, 접근제어자를 적절히 설정하여 보안을 강화할 수 있습니다. `setAccessible(true)`를 사용하여 비공개 필드나 메서드에 접근할 수 있지만, 보안 관리자가 이를 제한할 수 있습니다.

#### 리플렉션 API를 통한 어노테이션 변경 시 주의사항

어노테이션 자체는 불변(immutable) 객체이므로, 리플렉션을 통해 직접 변경할 수 없습니다. 따라서 어노테이션의 동작을 변경하려면 프로그래밍적인 방법이 필요합니다.

#### 리플렉션을 통한 필드나 메서드 접근 시 예외 처리

리플렉션을 통해 필드나 메서드에 접근할 때는 다음 예외를 주의해야 합니다:
- **IllegalAccessException**: 접근 권한이 없는 경우 발생합니다.
- **NoSuchFieldException**: 필드를 찾을 수 없는 경우 발생합니다.
- **NoSuchMethodException**: 메서드를 찾을 수 없는 경우 발생합니다.

```java
try {
    Field field = MyClass.class.getDeclaredField("myField");
    field.setAccessible(true);
    Object value = field.get(myObject);
} catch (NoSuchFieldException | IllegalAccessException e) {
    e.printStackTrace();
}
```

#### 메서드 호출 시 발생할 수 있는 예외 처리

- **InvocationTargetException**: 메서드 호출 중 예외가 발생한 경우 발생합니다.
- **IllegalAccessException**: 접근 권한이 없는 경우 발생합니다.

```java
try {
    Method method = MyClass.class.getDeclaredMethod("myMethod");
    method.setAccessible(true);
    method.invoke(myObject);
} catch (InvocationTargetException | IllegalAccessException | NoSuchMethodException e) {
    e.printStackTrace();
}
```

### 예외 체인을 활용한 예외 처리

예외 체인을 사용하여 원본 예외를 감싸서 던질 수 있습니다. 이를 통해 예외의 원인을 추적할 수 있습니다.

```java
try {
    // some reflection code
} catch (NoSuchMethodException e) {
    throw new MyCustomException("Method not found", e);
} catch (IllegalAccessException e) {
    throw new MyCustomException("Access not allowed", e);
}
```

이렇게 하면 원본 예외를 포함한 전체 예외 체인을 확인할 수 있어 디버깅에 유리합니다.

### 안전한 리플렉션 사용

리플렉션을 사용할 때는 다음과 같은 규칙을 따르는 것이 좋습니다:
- 최소한의 접근 권한만 부여합니다.
- 예외 처리를 철저히 합니다
- 성능 오버헤드를 고려하여 필요한 경우에만 사용합니다.

이러한 원칙을 준수하면 리플렉션을 안전하게 사용할 수 있으며, 코드의 신뢰성을 높일 수 있습니다.