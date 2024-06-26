# 자바의 자료형

사용자는 참조 자료형은 자유롭게 만들 수 있지만, 기본 자료형은 정해져있다.
new를 사용하여 초기화를 하는 건 참조 자료형, 그렇지 않고 바로 사용하는 것을 기본 자료형이라 한다.

초기화가 뭘까?
클래스 안에 변수든, 지역 변수든 데이터를 저장하기 위해 만듭니다.
항상 저장할 값이 정해져 있지는 않기 때문에 변수를 선언할 때 초기화를 통해 변수를 사용할 수 있도록 하는 겁니다.

## 기본 자료형
기본 자료형은 8가지입니다.  
숫자와 Boolean으로 크게 나눌 수 있으며
숫자는 정수에는 byte,short,char,int,long 이 있으며
실수형에는 float,double이 있습니다.

자바는 기본적으로 signed 타입으로 양수와 음수를 모두 표현할 수 있습니다.  
+0,-0중에서 -0대신에 숫자 하나를 더 표기하기 위해 , -0 에는 해당 자료형의 제일 작은 숫자가 들어갑니다.

기본 자료형에서 계산이 아니라 단순 값을 저장하는 용도로 사용한다면 해당 범위에 맞는 자료형을 사용하는것도 방법이지만
해당 자료형끼리 연산을 한다면 int나 long을 사용하는 것을 권장합니다.

이유는 금액같은 경우 자료형의 overflow가 발생하면 큰 문제가 되기 때문입니다.

그리고 소수점이 더 클 경우에는 double보다 BigDecimal을 사용합니다.

char는 unsigned 자료형 타입으로 2byte로 글자를 저장할 수 있습니다.  
주의할점은 16진수로 표시하여 저장할 때는 무조건 4자리를 맞춰야합니다 `\u0017` 처럼

char에 2^16 까지 저장할 수 있습니다.


## 참조 자료형
참조자료형은 기본 자료형을 제외한 모든 자료형을 말합니다.  
if문이나 while문의 조건에 참조자료형이 그대로 조건식에 사용될 수 없습니다.
이때에는 메소드를 활용하는 방식을 사용합니다.

참조자료형끼리 연산은 메소드를 통해 해야합니다. 메소드 내에서 기본 자료형끼리 연산은 가능하지만,
메소드의 반환값을 기본자료형으로 하여 외부에서 연산하는 방식은 절차적 프로그래밍과 별반 다르지 않다고 생각됩니다.

객체끼리 연산을 하기 위해서는 인스턴스 메소드나, 정적 메소드를 통해서 연산을 해야한다.

String 타입은 자바에서 별도로 `String Pool`에서 관리하기되기 때문에 new 연산자를 사용하지 않아도 가능합니다.
단 불변이기 때문에 새로운 것을 호출할 때마다 새로운 상수가 생성되고 힙메모리에서 관리가 됩니다.

> CPU가 클래스를 사용하기 위해서는 메모리에 로드가 되고, 초기화가 되어야한다.
> 그렇기 때문에 static은 생성자 함수를 사용하지 않아도 호출만 되어도 초기화가 되기 때문에 사용할 수 있는 것이고
> 그래서 static은 인스턴스 변수와 메소드를 사용할 수 없다. 초기화가 되었는지 알 수 없기 때문입니다.

### 생성자는 왜 필요할까?
1. 클래스를 사용하기 위해서는 메모리에 로드가 되어야 한다.
2. 기본 자료형과 다르게 참조자료형은 필요한 메모리 크기가 다르기 때문에 동적으로 할당받아야 하며, 프로세스 공유 자원인 내 힙 메모리에 로드가 된다.
3. 공유자원이기 때문에 올바르게 동작하기 위해서 클래스의 상태를 초기화를 해야한다.
4. 그래서 자바는 생성자가 없을 경우 기본생성자를 컴파일 시점에 만들어준다.
5. 생성자를 통해 클래스의 상태를 기본 값이나 원하는 상태로 초기화를 해야한다.

> 정리  
> 프로세스가 사용하는 힙 메모리는 공유 자원이기 때문에 로드된 클래스를 사용할 때 올바른 상태로 사용하기 위해 초기화를 해야하는데 이걸 자바 프로그래밍에서 유연하게 초기화를 할 수 있도록 제공하는 함수가 생성자 함수다.
>

### 생성자 함수는 리턴이 없을까
생성자 함수는 상태를 초기화하고 인스턴스의 참조 값을 반환하기 때문이죠.

### DTO
데이터를 전달하기 위한 객체를 DTO라고 하고, 데이터를 저장하기 위한 객체를 VO라고 한다.  
DTO를 사용하지 않고 데이터를 전달하기 위해서 배열을 사용해야한다.  
메소드는 하나의 반환타입만 사용할 수 있는데 같은 타입의 자료형이 필요하다면 유연성이 떨어진다.

그래서 데이터를 전달하기 위해서는 DTO가 유연하고, 유지보수하기 쉽다.

그리고 생성자 함수가 다양한 이유는 사용하는 사용자마다 초기화하려는 필드가 다 다르기 때문이고
목적에 따라 사용하는 생성자 함수가 다르기 때문이다.

```Java
public class MemberDto {
    private int age;
    private String name;
    private String address;
    
    // 아무것도 모를때
    public MemberDto(){}  
    
    // 이름만 알때
    public MemberDto(String name){
        this.name = name;
    }
    
    // 모든 정보를 알때
    public MemberDto(int age,String name, String address){
        this.age = age;
        this.name = name;
        this.address = address;
    }
}
```  
다양한 생성자 패턴을 통해 사용하는 유연성을 기를 수 있지만, 주의할 점도 발생합니다.  
기본자료형을 그대로 받기 때문에 값이 잘못들어올 수 있고, 해당 클래스를 열어보지 않는 이상 해당 생성자 함수가 각각 어떤 역할로 쓰는지 알수 가 없습니다.
따라서 별도의 팩토리 메소드를 만들어서 의미를 부여할 수 있습니다.

### this
이 객체를 가리키는 변수입니다.
그래서 synchronized를 사용할 때도 this를 사용할 수 있고, 매개변수 명과 필드 명이 동일할 경우에도 구분하기위해 사용됩니다.

그러면 중첩구조에서 this는 누굴 의미하는 걸까
해당 메소드 내에서 this는 자기 자신을 가리킨다.

## 오버로딩
동일한 역할을 하는 메소드가 매개변수의 갯수, 순서, 자료형이 다를 경우 하나의 메소드 명을 사용할 수 있는 것을 오버로딩이라한다.

[String Pool](https://www.baeldung.com/java-string-pool)

자바 7버전 이후 부터는 `String Pool`이 힙 영역에 관리가 되기 때문에 가비지 컬렉터 대상이 되어 메모리 부족 현상을 줄일 수 있습니다.  
  
