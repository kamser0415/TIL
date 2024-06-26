# 1장 프로그래밍이란 무엇인가?

+ **프로그래밍**은 하나 이상의 관련된 추상 알고리즘(할 일)을 특정 프로그래밍 언어를 이용해 구체적인 컴퓨터 프로그램으로 구현하는 기술입니다.
+ **프로그래밍 언어**는 컴퓨터와 사람사이의 의사소통을 위해 사용되는 언어입니다.
+ **프로그램**이란 컴퓨터에서 실행될 때 특정 작업(specific)을 수행하는 일련의 명령어들의 집합이다.

정리하면, 프로그래밍은 컴퓨터에게 할 일을 프로그래밍 언어로 작성하여 프로그램으로 만드는 것을 말합니다.

웹 환경에서 클라이언트인 사용자는 서버에 요청하여 가공된 정보를 HTML로 받을 수 있으며,
서버가 전달한 자바스크립트,CSS,HTML을 웹 클라이언트가 가공하여 클라이언트에게 보여줍니다.

이때 필요한 정보를 사용자가 원하는 정보로 가공하는 기능을 프로그램으로 수행하며,
프로그래밍 언어로 자바를 사용합니다.

## 메소드
자바 프로그래밍 언어에서 제일 작은 프로그램 단위를 메소드라고 합니다.  
정리하면 자바의 메소드는 하나의 특정 작업을 수행하는 일련의 명령어 집합이라고 할 수 있습니다.

언어는 상호간의 약속이 필요하며, 메소드도 작성하여 사용하기 위해서는 정해진 규칙이 있습니다.
```Java
public boolean checkPassword(String password){
    //
}
```  
메소드는 반환타입, 메소드명, 매개변수로 나누어지며 매개변수는 생략이 가능합니다.

메소드나 변수나 모두 이름으로 저장하여 사용하는데 이때 구분하는 방법은 `()`이 붙어있을 경우에는 메소드로 인식합니다.  
그래서 동일한 클래스내에 메소드명과 변수명은 동일해도 오류가 발생하지 않습니다.


[정리글](https://st-lab.tistory.com/151)

## 클래스
자바에서 클래스란 현실에 있는 것을 추상화하여 프로그래밍으로 작성한 것을 말하며, 상태와 행동이 있어야합니다.

클래스는 특성을 결정짓는 상태가 있으며 **클래스 안**에 있고, **메소드 밖**에 있어야합니다.

```Java
public class DoorLockManager {
    public boolean checkPassword(String password){
    //
    }
}
```  

객체지향 프로그래밍은 코딩하는 방식이나 방법이 상태와 행동을 묶어 하나의 객체로 사용하는 것을 말합니다.  
그러면 C언어도 유사 객체지향 프로그래밍이 가능하지만, 객체지향 프로그래밍 언어라고 하지 않는 이유는
+ 갭슐화, 다형성, 클래스 상속을 지원하는가?
+ 데이터 접근 제한이 가능한가?

위 기준을 만족하면 객체지향, 만족하지 못한다면 절차적 성격이 강해집니다.

그러면 절차적 프로그래밍 패러다임에서 객체지향 프로그래밍으로 넘어가게 된 이유가 궁금합니다.

소프트웨어의 발전 속도가 빨라지면서 작성해야하는 코드의 양과 복잡도가 올라갑니다.
복잡한 알고리즘을 작성해야할 때 절차적 프로그래밍을 하면 순서도가 꼬이기 시작하면서 코딩한 코드가 사람이 읽으면서
동작을 이해할 수 없는 코드가 단점으로 대두되었습니다.

이러한 문제를 해결하기 위해 객체지향 프로그래밍이 나오게 됩니다.

그렇기 때문에 클래스는 객체로 사용하려면 상태와 행동이 있어야합니다.
객체는 인간 중심에서 객관적으로 인지할 수 있는 대상들을 객체라고 정의를 내렸습니다.  
객체는 각작 고유한 속성과 행동을 가지고 있습니다.

다시 정리하면,
자바는 객체지향 프로그래밍 언어로 객체라는 말은 객관적으로 인지할 수 있는 대상으로 정의합니다.
객체를 구분하기 위해서 속성과 행위를 통해서 나누게 됩니다. 자바는 클래스를 통해 객체를 추상화하였고
속성과 행위를 포함하게 됩니다.

### 절자적 프로그래밍과 객체지향 프로그래밍
정답이라는게 없습니다. 현재는 객체지향 프로그래밍 방식이 대세인건 맞지만 상황에 맞게 사용하면 됩니다.

**절차적 프로그래밍 장점**
1. 객체나 클래스를 만들 필요없이 바로 프로그래밍을 코딩할 수 있다.
2. 필요한 기능을 함수로 만들어 두기 때문에 같은 코드를 복사하지 않고 호출하여 사용한다.
3. 프로그램의 흐름을 쉽게 추적할 수 있다.

**절차적 프로그래밍 단점**
1. 각 코드가 결합도가 매우 높기 때문에 수정하기 힘들다.
2. 프로그램 전체에서 코드를 재사용할 수 없어 프로젝트 개발 비용과 시간이 늘어날 수 있다.
3. 디버그가 어렵다.

**객체지향 프로그래밍 장점**
1. 모듈화, 캡슐화로 유지보수에 용이하다.
2. 객체지향적이기 때문에 현실 세계와 유사성에 의해 코드를 이해하기 쉽게만든다.
3. 객체는 그 자체가 하나의 프로그램이기 때문에 다른 프로그램에서 재사용이 가능하다.

**객체지향 프로그래밍 단점**
1. 대부분 객체 지향 프로그램은 속도가 상대적으로 느리고 많은 양의 메모리를 사용하는 경향이 있다.
2. 현실 세계와 유사성에 의해 코드를 이해하기 쉽게 만드는 만큼 설계 과정에 시간이 많이 소비된다.

## 세미콜론
프로그래밍 언어로 프로그램을 만들때 하나의 명령어를 끝맺기 위해 구분자를 사용하여야하는데 이때 자바는
세미콜론을 사용합니다.

## 예약어
프로그래밍 언어에서 특정한 목적을 수행하기 위해 미리 예약되어 있는 단어를 말하며,
언어의 일관성과 안정성을 유지하기 위해서 동일한 단어를 변수명,메소드명,클래스명 등에서 사용할 수 없습니다.  
  
