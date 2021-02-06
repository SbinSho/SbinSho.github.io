---
title:  "Chap5_JavaStudy"
excerpt: "스프링 입문을 위한 자바 객체지향의 원리와 이해라는 책을 읽으며 중요한 내용을 단원별로 정리한 글입니다."
categories:
  - Java_Study
  - java
  - SOLID

toc: true
toc_sticky: true
toc_label: "목차"

last_modified_at : 2021-01-18

---

## 5장의 주요 내용 정리

#### SOLID
SOLID는 객체지향 설계 5대 원칙이라 부르며, 객체 지향 4대 특성을 발판으로 하고 있다. 스프링 프레임워크의 근간이기도 하다.

1. SRP( Single Responsibility Principle ) : 단일 책임 원칙
    - 모든 클래스는 각각 하나의 책임만 가져야한다.
    - "어떤 클래스를 변경해야 하는 이유는 오직 하나뿐이어야 한다" - 로버트 C.마틴
2. OCP ( Open Close Principle ) : 개방폐쇄의 원칙
    - 확장에는 열려있고 수정에는 닫혀있는, 기존의 코드의 변경하지 않으면서 기능을 추가할 수 있도록 설계가 되어야 한다는 원칙을 말한다.
    - "소프트웨어 엔티티(클래스, 모듈, 함수 등)는 확장에 대해서는 열여 있어야 하지만 변경에 대해서는 닫혀 있어야한다." - 로버트 C.마틴
3. LSP ( The Liskov Substitution Principle ) : 리스코브 치환의 원칙
    - 자식 클래스는 언제나 자신의 부모 크래스를 대체할 수 있다는 원칙이다.
    - "서브 타입은 언제나 자신의 기반 타입(base type)으로 교체할 수 있어야 한다." - 로보트 C.마틴
4. ISP (Interface Segregation Principle) : 인터페이스 분리 원칙
    - 한 클래스는 자신이 사용하지 않는 인터페이스는 구현하지 말아야 한다.
    - 하나의 일반적인 인터페이스보다 여러개의 구체적인 인터페이스가 낫다.
    - "클라이언트는 자신이 사용하지 않는 메서드에 의존 관계를 맺으면 안된다." - 로버트 C. 마틴
5. DIP (Dependency Inversion Principle) : 의존 역전 원칙
    - 의존관계를 맺을 떄 변화하기 쉬운것 또는 자주 변화하는 것보다는 변화하기 어려운 것, 거의 변화가 없는것에 의존하라는 것이다.
    - 구체적인 클래스보다 인터페이스나 추상 클래스와 관계를 맺으라는 뜻이다.
    - "고차원 모듈은 저차원 모듈에 의존하면 안된다. 이 두 모듈 모두 다른 추상화된 것에 의존해야한다."<br>"추상화된 것은 구체적인 것에 의존하면 안된다. 구체적인 것이 추상화된 것에 의존해야 한다."<br>"자주 변경되는 구체(Concrete)클래스에 의존하지 마라" - 로버트 C. 마틴
    - "자신보다 변하기 쉬운 것에 의존하지 마라."

* SOLID 정리
SOLID는 객체 지향을 올바르게 프로그램에 녹여내기 위한 원칙이다. SOLID를 이야기 할 때 빼놓을 수 없는 것이 Soc다. Soc는 관심사의 분리 (Separation Of Concerns)의 머리 글자다. 하나의 속성, 하나의 메서드, 하나의 클래스, 하나의 모듈, 또는 하나의 패키지에는 하나의 관심사만 들어 있어야 한다는 것이 SoC다.

> SoC를 적용하면 자연스럽게 단일 책임 원칙(SRP), 인터페이스 분리 원칙(ISP), 개방 폐쇄 원칙(OCP)에 도달하게 된다. 스프링 또한 SoC를 통해 SOLID를 극한까지 적용하고 있다.

SOLID 원칙을 적용하면 소스 파일의 개수는 더 많아지는 경향이 있다. 하지만 많아진 파일이 논리를 더욱 잘 분할하고, 잘 표현하기에 이해하기 쉽고, 개발하기 쉬우며, 유지와 관리, 보수하기 쉬운 소스가 만들어진다. SOLID 원칙을 적용함으로써 얻는 혜택에 비하면 늘어나는 소스 파일 개수에 대한 부담은 충반히 감수하고도 남을 만하다.

객체 지향은 현실 세계를 모델링한다. 객체 지향 세계는 현실 세계 같아야 한다는 것이 하나이고, 또 다른 하나는 모델링을 통해 추상화됐다는 것이다.
SOLID는 현실 세계 같아야 한다는 첫 번째 요건보다는 추상화됐다는 두 번째 요건에 초첨을 맞추고 있다.

#### 결합도와 응집도
좋은 소프트웨어 설계를 위해서는 결합도(coupling)는 낮추고 응집도(cohesion)는 높이는 것이 바람직하다.

* 결합도
    - 모듈(클래스)간의 상호 의존 정도로서 결합도가 낮으면 모듈 간의 항호 의존성이 줄어들어 객체의 재사용이나 수정, 유지보수가 용이하다.
    
* 응집도
    - 하나의 모듈 내부에 존재하는 구성 요소들의 기능적 관련성으로, 응집도가 높은 모듈은 하나의 책임에 집중하고 독립성이 높아져 재사용이나 기능의 수정, 유지보수가 용이하다.


#### 참고자료
> 저자 '김종민'님의 스프링 입문을 위한 자바 객체 지향의 원리와 이해

> https://medium.com/@hckcksrl/solid-%EC%9B%90%EC%B9%99-182f04d0d2b