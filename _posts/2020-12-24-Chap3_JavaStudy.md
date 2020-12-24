---
title:  "Chap3_JavaStudy"
excerpt: "스프링 입문을 위한 자바 객체지향의 원리와 이해라는 책을 읽으며 중요한 내용을 단원별로 정리한 글입니다."
categories:
  - Java_Study

toc: true
toc_sticky: true
toc_label: "목차"

last_modified_at : 2020-12-24

---

## 3장의 주요 내용 정리

#### 객체지향
객체지향이란 현실세계에서 존재하는 사물을 인지하는 방식대로 표현하는 것을 뜻한다.
즉, 인간의 인지 및 사고 방식까지 프로그래밍에 접목하는 인간지향을 실천하고 있다. 


#### 객체지향 프로그래밍 ( OOP : Object Oriented Programming )
객체지향 프로그래밍(OOP)은 컴퓨터 프로그래밍 패러다임중 하나이다. 독립된 단위의 객체(Object)를 만들고 그 객체들 간의 메시지를 주고 받으며, 데이터를 처리하는 상호작용을 통해 로직을 구성하는 프로그래밍 방법이다. 

* 객체지향 프로그래밍 구성요소
    1. 객체(Object)
        - 세상에 존재하는 유일무이한 사물이다.
        - 클래스의 인스턴스이며, 각각의 객체는 고유한 속성을 가진다..
    2. 클래스(Class)
        - 분류, 집합, 같은 속성과 기능을 가진 여러 객체의 집합 개념. 
        - 객체를 만들어 내기 위한 설계도.
    3. 메서드(Method), 메시지(Message)
        - 클래스로 부터 생성된 객체를 사용하는 방법이다.
        - 객체에 명령을 내리는 행위
        - 메시지는 객체간의 통신이 이루엊며, 이 메시지를 통해 메소드가 호출되어 사용된다.
        
```
객체(Object)와 인스턴스(Instance)는 같은 표현으로도 통하지만, 보통 객체는 소프트웨어
세계에 구현할 대상을 뜻하고, 인스턴스는 클래스를 이용하여 소프트웨어 세계에서 구현된 
실체를 뜻할때 쓰인다. 
```

* 객체 지향의 4대 특성
    1. 캡슐화  ( Encapsulation ) : 정보은닉
    2. 상속 ( Inheritance ) : 재사용
    3. 추상화 ( Abstraction ) : 모델링
    4. 다형성 ( Polymorphism ) : 사용 편의
        
        
#### 추상화 ( Abstraction )
추상화란 불필요한 부분을 생략하고 객체의 속성 중 가장 중요한 것에만 중점을 두어 재조합 하는 것이다. <br><br>
객체지향에서의 추상화는 클래스 설계부터가 추상화이며, 여러 클래스간의 공통된 속성과 기능을 묶어 새로운 클래스를 정의하는것도 추상화라고 한다.

#### 상속 ( Inheritance )
객체 지향에서의 상속은 상우 클래스의 특성을 하위 클래스에서 상속하고 거기기에 더해 필요한 특성을 추가, 즉 확장해서 사용할 수 있다는 의미이다.

```
상위 클래스 = 슈퍼 클래스, 하위 클래스 - 서브 클래스
```

상위 클래스 쪽으로 갈수록 추상화, 일반화됐다고 말하며, 하위 클래스 쪽으로 갈수록 구체화, 특수화됐다고 말한다.

* 객체 지향의 상속
    - 상속은 상위 클래스의 특성을 재사용/확장하는 것이다.
    - 상속은 is a 관계가 아닌 is a kind of 관계를 만족해야한다.
    - 하위 클래스 is a kind of 상위 클래스 -> 하위클래스는 상위클래스의 한 분류다.
    

#### 다중상속
자바에서는 다중 상속은 문법적인 한계로 기능을 막아 두었다. 대표적인 문제로 다이아몬드 문제(Diamond Problem)가 있다.

* 다이아몬드 문제 ( Diamond Problem )
    - ![Diamond Problem](/assets/images/chap3/Diamond Problem.PNG)
    - 최상위 추상클래스 SuperClass, SuperClass를 상속받은 A와 B 클래스가 있다. SuperClass의 Method()를 A와 B는 상속 받았고, 이를 C라는 클래스가 A와B를 다중상속 하면, C에서 test() 메서드 실행시 상속받은 Method()를 실행해야 하는데 어떤 부모의 Method()를 실행해야하는지 알수가 없는 문제이다.

다중 상속의 이점도 있기에, java에서는 다중 상속의 이점만을 가져온 인터페이스를 도입해 사용중이다.

인터페이스는 be able to, 즉 "무엇을 할 수 있는"이라는 표현 형태로 만드는 것이 좋다.
이러한 형태로 만들게 되면 상위 클래스는 하위 클래스에게 특성 ( 속서오가 메서드 )을 상속해 주고, 인터페이스는 클래스가 '무엇을 할 수 있다' 라고 하는 기능을 구현하도록 강제하게 된다.

* 인터페이스 사용하는 이유
    1. 완벽한 추상화를 달성하기 위함이다.
    2. 다중상속의 기능을 지원할 수 있다.
    3. 약한 결합을 이룰 수 있게 한다.
    4. 다형성을 사용할 수 있다.



#### 참고자료
> 저자 '김종민'님의 스프링 입문을 위한 자바 객체 지향의 원리와 이해

   




