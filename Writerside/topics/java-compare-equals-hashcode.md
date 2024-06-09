# Java의 equals 메소드와 hashCode 메소드

## 역할과 관계

`equals` 메소드와 `hashCode` 메소드는 Java에서 객체의 동등성을 비교하는데 중요한 역할을 합니다. 두 메소드는 밀접한 관계가 있으며, 이들에 대한 이해는 Java 컬렉션 프레임워크에서 올바르게 동작하는 사용자 정의 객체를 만드는 데 필수적입니다.

## equals 메소드

`equals` 메소드는 객체의 속성을 비교하여 같으면 `true`, 다르면 `false`를 반환합니다. 이는 두 객체가 논리적으로 동등한지를 판단하는 데 사용됩니다. 기본적으로 `Object` 클래스에서 제공되며, 객체의 참조값(즉, 객체의 메모리 주소)을 비교합니다. 하지만 대부분의 경우 논리적인 동등성을 정의하기 위해 `equals` 메소드를 오버라이드합니다.

예를 들어, `Person` 클래스에서 두 객체가 동일한 이름과 나이를 가지면 동등하다고 판단할 수 있습니다.

```java
public class Person {
    private String name;
    private int age;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age && Objects.equals(name, person.name);
    }
}
```

## hashCode 메소드

`hashCode` 메소드는 객체의 해시 코드를 반환합니다. 해시 코드는 객체를 고유하게 식별하는 정수값으로, 주로 해시 기반 컬렉션(예: `HashMap`, `HashSet`)에서 객체를 빠르게 검색하거나 저장하는 데 사용됩니다. `Object` 클래스의 기본 `hashCode` 메소드는 객체의 메모리 주소를 기반으로 해시 코드를 생성하지만, `equals` 메소드를 오버라이드하는 경우 항상 `hashCode` 메소드도 오버라이드해야 합니다.

```java
public class Person {
    private String name;
    private int age;

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return age == person.age && Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```

## equals와 hashCode의 관계

`equals`와 `hashCode` 메소드 간에는 중요한 규약이 존재합니다:

1. **동일한 객체**는 항상 동일한 해시 코드를 가져야 합니다. 즉, `x.equals(y)`가 `true`인 경우 `x.hashCode() == y.hashCode()`가 `true`여야 합니다.
2. **동일하지 않은 객체**는 반드시 다른 해시 코드를 가질 필요는 없지만, 가능하면 다른 해시 코드를 가져야 합니다. 이는 해시 기반 컬렉션의 성능을 향상시키는 데 도움이 됩니다.

이 규약을 준수하지 않으면, 해시 기반 컬렉션(예: `HashMap`, `HashSet`)에서 객체를 올바르게 처리할 수 없게 됩니다.

## 주의사항 및 문제점

- **equals 메소드를 재정의할 때 주의사항**: null 체크를 포함해야 합니다. 대상 객체가 null일 경우 속성에 접근하면 예외가 발생할 수 있기 때문입니다.
- **hashCode 메소드를 재정의하지 않을 때 발생할 수 있는 문제**: HashMap과 같은 자료구조에서 해시 충돌이 발생하거나 값을 제대로 조회하거나 저장하기 어려울 수 있습니다. `equals`로는 같은 객체로 판단되지만, `hashCode`가 다르면 서로 다른 버킷에 저장되어 논리적으로 동일한 객체를 찾을 수 없습니다.

## 요약

- `equals` 메소드는 객체의 논리적 동등성을 비교합니다.
- `hashCode` 메소드는 객체의 해시 코드를 반환합니다.
- `equals`를 오버라이드하면 반드시 `hashCode`도 오버라이드해야 하며, 이 두 메소드 간의 규약을 준수해야 합니다.