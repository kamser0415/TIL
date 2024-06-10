# 자바에서 HashMap과 불변 객체

## HashMap과 hashCode 메소드의 관련성

`HashMap`은 키와 값을 매핑하는 자료구조로, 해시 테이블 기반으로 작동합니다. `HashMap`은 키를 해시 코드로 변환하여 버킷에 저장하며, `hashCode` 메소드는 객체의 해시 코드를 반환합니다. `HashMap`은 이 해시 코드를 사용해 내부 배열에서 키를 찾습니다. 각 키는 고유한 해시 코드를 가지며, 이 해시 코드를 기반으로 배열의 인덱스를 결정합니다. 동일한 해시 코드를 가진 키들이 충돌할 경우에는 연결 리스트나 트리를 사용해 저장합니다.

## 해시 충돌 처리와 hashCode, equals 메소드의 관계

**해시 충돌 처리**:
- **체이닝(Chaining)**: 같은 해시 코드를 가진 여러 엔트리를 단일 버킷에 저장합니다. 해시 충돌이 발생하면 해당 버킷의 리스트나 트리에 새로운 엔트리를 추가합니다. 자바 8부터는 연결 리스트가 일정 크기 이상 커지면 성능 향상을 위해 트리로 변환됩니다.
- **개방 주소법(Open Addressing)**: 자바의 `HashMap`은 기본적으로 사용하지 않지만, 해시 충돌을 피하기 위해 다음 빈 슬롯을 찾아가는 방식입니다.

**hashCode와 equals 메소드의 관계**:
- **hashCode**: 두 객체가 같다면(`equals` 메소드가 true를 반환하면) 반드시 같은 해시 코드를 가져야 합니다.
- **equals**: 두 객체가 같을 때만 true를 반환해야 하며, `hashCode`가 g같으면 equals도 true여야 합니다.

### 제대로 구현하지 않았을 때 발생하는 문제

**문제 발생 예시**:
- **중복 키 저장**: `hashCode`와 `equals`를 제대로 구현하지 않으면 동일한 키를 여러 번 저장할 수 있습니다.
  ```java
  class Person {
      String name;
      int age;
      // hashCode와 equals 메소드가 구현되지 않음
  }

  HashMap<Person, String> map = new HashMap<>();
  Person p1 = new Person("John", 25);
  Person p2 = new Person("John", 25);
  map.put(p1, "Engineer");
  map.put(p2, "Doctor");
  System.out.println(map.size()); // Output: 2
  ```

- **키 검색 실패**: `hashCode`나 `equals`를 제대로 구현하지 않으면 존재하는 키를 찾지 못합니다.
  ```java
  HashMap<Person, String> map = new HashMap<>();
  Person p1 = new Person("John", 25);
  map.put(p1, "Engineer");
  Person p2 = new Person("John", 25);
  System.out.println(map.get(p2)); // Output: null
  ```

**고려 사항**:
- `hashCode`와 `equals`의 일관성 유지: 두 객체가 같다면 같은 `hashCode`를 가져야 합니다.
- 효율적인 해싱: `hashCode`는 가능한 한 해시 분포가 균등하게 이루어지도록 구현되어야 합니다.
- 불변성: `hashCode`와 `equals`가 사용하는 필드는 불변이어야 합니다.

### 객체의 일부 필드가 변경될 때 해시 코드가 변경되는 것을 방지하기 위한 방법

객체의 필드를 불변하게 유지하기 위해 다음을 고려해야 합니다:
- **final 키워드 사용**: 필드를 `final`로 선언하여 초기화 이후 변경할 수 없도록 합니다.
- **생성자에서 초기화**: 모든 필드를 생성자에서 초기화하고, 생성자 외에는 값을 변경할 수 없도록 합니다.
- **변경 메소드 제거**: setter 메소드를 제공하지 않습니다.

**예제: 불변 클래스 설계**:
```java
public final class ImmutablePerson {
    private final String name;
    private final int age;
    private final Date birthDate;

    public ImmutablePerson(String name, int age, Date birthDate) {
        this.name = name;
        this.age = age;
        this.birthDate = new Date(birthDate.getTime()); // 방어적 복사
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public Date getBirthDate() {
        return new Date(birthDate.getTime()); // 방어적 복사
    }
}
```

### 방어적 복사의 성능을 향상시킬 수 있는 방법

방어적 복사를 통해 불변성을 유지하는 방법 중 성능에 부정적인 영향을 미칠 수 있습니다. 이를 해결하기 위한 다른 방법은 다음과 같습니다:
- **불변 컬렉션 사용**: `Collections.unmodifiableList` 또는 `Guava`의 `ImmutableList`를 사용하여 방어적 복사의 필요성을 줄입니다.
- **필요할 때만 복사**: 방어적 복사를 매번 수행하지 않고, 실제로 변경이 발생하는 경우에만 복사합니다.

**예제: 불변 컬렉션 사용**:
```java
import com.google.common.collect.ImmutableList;

public final class ImmutablePerson {
    private final String name;
    private final int age;
    private final List<String> hobbies;

    public ImmutablePerson(String name, int age, List<String> hobbies) {
        this.name = name;
        this.age = age;
        this.hobbies = ImmutableList.copyOf(hobbies); // Guava 불변 리스트
    }

    public List<String> getHobbies() {
        return hobbies;
    }
}
```

### 불변 컬렉션 구현 및 수정 작업 해결

**구현 방식**:
- **변경 작업 차단**: `Collections.unmodifiableList`나 `Guava`의 불변 컬렉션을 사용합니다.
- **읽기 전용 메소드 제공**: 요소를 읽기 위한 메소드만 제공하고, 변경 작업은 차단합니다.

**수정 작업 해결**:
- **새로운 불변 컬렉션 생성**: 기존 컬렉션을 수정하지 않고, 새로운 불변 컬렉션을 반환합니다.
```java
public class ImmutableCollectionUtil {
    public static List<String> addElement(List<String> originalList, String element) {
        List<String> newList = new ArrayList<>(originalList);
        newList.add(element);
        return Collections.unmodifiableList(newList);
    }
}
```

**주의사항**:
- 성능 문제: 빈번한 수정 작업이 필요한 경우 가변 컬렉션을 사용하고, 최종 결과를 불변 컬렉션으로 변환하는 것이 효율적입니다.
- 메모리 사용량 증가: 매번 새로운 컬렉션을 생성하기 때문에 메모리 사용량이 증가할 수 있습니다.
- 스레드 안전성: 불변 컬렉션은 본질적으로 스레드 안전합니다.

### 불변 객체의 특성과 설계 원칙

**불변 객체의 특성**:
- 상태 변경 불가
- 스레드 안전성
- 복사본 제공

**설계 원칙**:
- 모든 필드를 `final`로 선언
- 클래스를 `final`로 선언
- 변경 메소드 제거
- 방어적 복사
- 생성자에서 모든 필드 초기화
- 내부 필드에 있는 참조 자료형도 불변형으로 만들기

**장점**:
- 스레드 안전성
- 안전한 공유
- 단순성
- 에러 방지
- 캐싱 및 재사용