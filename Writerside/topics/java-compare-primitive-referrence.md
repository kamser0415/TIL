# Primitive 타입과 Reference 타입 비교

## Primitive 타입
Primitive 타입은 자바의 가장 기본적인 데이터 타입입니다. 자바는 다음과 같은 8개의 Primitive 타입을 제공합니다:
- `byte`
- `short`
- `int`
- `long`
- `float`
- `double`
- `char`
- `boolean`

### 특징
1. **값 저장**: Primitive 타입 변수는 실제 값을 직접 저장합  니다.
2. **메모리 사용**: Primitive 타입은 고정된 크기의 메모리를 사용합니다. 예를 들어, `int`는 4바이트, `double`은 8바이트를 사용합니다.
3. **기본 값**: Primitive 타입 변수는 기본 값을 가집니다 (`int`는 0, `boolean`은 `false` 등).
4. **성능**: Primitive 타입은 간단한 값의 연산에서 Reference 타입보다 성능이 뛰어납니다.

### 사용 시 주의사항
- **Null 값 불가**: Primitive 타입 변수는 `null` 값을 가질 수 없습니다.
- **박싱 및 언박싱**: Primitive 타입을 객체처럼 사용해야 할 경우 자동 박싱/언박싱이 발생하는데, 이는 성능 저하를 유발할 수 있습니다.

## Reference 타입
Reference 타입은 객체를 참조하는 타입입니다. 자바에서 Reference 타입은 모든 클래스, 배열, 인터페이스 등을 포함합니다.

### 특징
1. **참조 저장**: Reference 타입 변수는 객체의 실제 값이 저장된 메모리 주소(참조)를 저장합니다.
2. **메모리 사용**: Reference 타입은 객체의 크기에 따라 메모리 사용량이 달라집니다.
3. **기본 값**: Reference 타입 변수의 기본 값은 `null`입니다.
4. **성능**: Reference 타입은 객체 생성과 관리에 추가적인 메모리와 시간이 소요됩니다.

### 사용 시 주의사항
- **Null 값 처리**: Reference 타입 변수는 `null` 값을 가질 수 있으므로, `null` 체크를 반드시 해야 합니다. `null`을 참조하려고 하면 `NullPointerException`이 발생할 수 있습니다.
- **Garbage Collection**: 객체가 더 이상 참조되지 않으면 Garbage Collector에 의해 메모리에서 해제됩니다. 불필요한 객체 참조를 제거해 메모리 누수를 방지해야 합니다.
- **동등성 비교**: Reference 타입의 동등성 비교는 `==` 대신 `equals()` 메서드를 사용해야 합니다. `==`는 객체의 참조를 비교하고, `equals()`는 객체의 내용을 비교합니다.

## Primitive 타입과 Reference 타입의 메모리 할당 방식 및 전달 방식

### 메모리 할당
- **Primitive 타입**: Primitive 타입 변수는 스택 메모리에 저장됩니다. 스택은 메모리 할당과 해제가 매우 빠른 구조입니다. 변수 자체에 실제 데이터 값을 저장합니다.
- **Reference 타입**: Reference 타입 객체는 힙 메모리에 저장됩니다. 힙은 큰 데이터를 저장하는 데 적합하며, 가비지 컬렉션에 의해 관리됩니다. 참조 변수 자체는 스택에 저장되지만, 변수에는 힙에 저장된 객체의 주소가 저장됩니다.

### 예시
```java
public class Member {
    int age;
    String name;
    int zipcode;
    String address;
    
    // Constructor and other methods...
}

public class Main {
    public static void main(String[] args) {
        Member a = new Member();
        // a는 Member 객체의 주소를 참조
    }
}
```

여기서 `a`는 `Member` 객체의 주소를 저장하는 참조 변수입니다. 이 주소의 크기는 JVM 아키텍처에 따라 32비트 또는 64비트로 일정합니다. 객체의 크기와 관계없이 참조 변수는 항상 일정한 크기의 주소 값을 가지게 됩니다.

## 메소드 인자 전달 방식
- **Primitive 타입**: 변수의 값이 복사되어 전달됩니다. 메소드 내에서 해당 변수의 복사본을 사용하며, 원본 변수는 영향을 받지 않습니다.
- **Reference 타입**: 변수에 저장된 객체의 주소가 복사되어 전달됩니다. 메소드 내에서 이 주소를 통해 원본 객체를 참조하고, 원본 객체를 수정하면 호출자도 영향을 받습니다.

## 요약
- **Primitive 타입**은 실제 값을 저장하고, 고정된 메모리 크기를 가지며 성능이 좋지만, `null` 값을 가질 수 없습니다.
- **Reference 타입**은 객체의 주소를 저장하고, 가변적인 메모리 크기를 가지며, 유연하지만 `null` 값에 대한 처리를 신경 써야 합니다.
- **참조 변수 크기**: JVM 아키텍처(32비트 또는 64비트)에 따라 다르며, 일반적으로 32비트 JVM에서는 4바이트, 64비트 JVM에서는 8바이트입니다.
- **객체 참조**: 참조 변수는 객체의 메모리 주소를 저장하여 객체를 참조합니다. 객체의 실제 데이터는 힙 메모리에 저장되고, 참조 변수는 스택 메모리에 저장됩니다.

  
> [Java SE Documentation](https://docs.oracle.com/javase/8/docs/api/)