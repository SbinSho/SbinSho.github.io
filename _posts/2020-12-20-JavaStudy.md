---
title:  "JavaStudy 2장"
excerpt: "스프링 입문을 위한 자바 객체지향의 원리와 이해라는 책을 읽으며 중요한 내용을 단원별로 정리한 글입니다."
categories:
  - Java_Study

toc: true
toc_sticky: true
toc_label: "목차"
---

## 2장의 주요 내용 정리

#### 자바의 개발과 구동 방식

JDK를 이용해서 프로그램을 개발하고 개발된 프로그램은 JRE에 의해 가상의 컴퓨터인 JVM 상에서 구동된다.

|현실세계|가상세계(자바월드)||
|:---|:---|:---|
|소프트웨어 개발 도구     |JDK - 자발 개발 도구  | JVM용 소프트웨어 개발 도구|
|운영체제               |JRE - 자바 실행 환경  | JVM용 OS|
|하드웨어 - 물리적 컴퓨터 | JVM - 자바 가상 기계 | 가상의 컴퓨터 |

#### 자바의 특성
JDK는 자바 소스 컴파일러인 javac.exe를 포함하고, JRE는 자바 프로그램 실행기인 java.exe를 포함하고 있다.
JVM은 JRE에 의해 구동된다.
<br>
<br>
이러한 구조를 가지고 있기 때문에 하나의 소스 파일로도 여러 플랫폼에서 자바 프로그램이 실행이 가능하다. 이를 Write Once Run Anywhere 라고 말한다.
<br>
<br>
JDK를 설치하면 JRE를 포함하고 있고, JVM은 JRE에서 포함하고 있기 때문에 JDK에 설치시 JRE, JVM이 자동으로 설치된다.

* JDK, JRE, JVM
    - JDK : Java Devleopment Kit / 자바 개발 도구
    - JRE : Java Runtime Environment / 자바 실행 환경
    - JVM : Java Vitual Machine / 자바 가상 기계

#### 자바의 구동 과정
1. 자바 소스 파일 작성한다. 확장자명 .java
2. JDK가 가지고 있는 컴파일러(javac.exe)로 바이트코드로 변환한다. 이때 변환된 파일이 .class 확장자인 자바 목적 파일이다.
3. JRE가 포함하고 있는 java.exe를 이용하여 JVM을 구동 시킨다.
4. JVM이 바이트코드를 해석하여 자바 프로그램이 실행된다.

#### 참고자료
> 저자 '김종민'님의 스프링 입문을 위한 자바 객체지향