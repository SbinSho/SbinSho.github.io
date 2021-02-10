---
title:  "Spring_Study"
excerpt: "springframework 공부한 내용 정리"
categories:
  - SpringFramework

toc: true
toc_sticky: true
toc_label: "목차"

last_modified_at : 2021-02-10

---

## 스프링 MVC 핵심 구성요소

### 핵심요소
1. HandlerMapping 
    - 요청 URL과 매칭되는 컨트롤러 검색
2. HandlerAdapter
    - 컨트롤러 실행 결과를 ModelAnView로 변환해서 리턴
3. ViewResolver
    - 컨트롤러의 실행 결과를 보여줄 View를 검색

웹브라우저에서 요청이 들어와면 처리 과정은 DispatcherServlet이 모든 연결을 담당한다.

## 스프링 컨테이너 ( 아직 공부가 더 필요함, 이론정리 예정 )
스프링은 빈을 생성하고 관리하는 컨테이너를 가지고 있다. 이를 통해서 주개념인 IOC나 AOP에 대해서 관리하곤 한다.

### 스프링 컨테이너의 종류
1. Bean Factory
    - 스프링 설정파일에 등록된 Bean 객체를 생성하고 관리하는 기본적인 기능만 제공한다. 컨테이너가 구동될 때 Bean 객체를 생성하는 것이 아니라 Lazy Loading 방식으로 객체를 생성한다.
    - Lazy Loading : 클라이언트의 요청에 의해서 Bean 객체가 사용된는 지점을 뜻한다.
    
2. Application Context
    - Bean객체 생성 및 관리하는 기능을 가지고 있다. 그 외에 트랜잭션 관리, 메시지 기반의 다국어 처리, AOP 처리 등 DI(Dependency Injection) 과 Ioc(Inverse of Conversion) 외에도 많은 부분을 지원한다.
    - Bean Factory와는 다르게 Pre-Loading 방식으로 객체를 생성한다.
    - Pre-Loading : 컨테이너가 구동되는 시점에 객체들을 생성한다.

### WebApplication Context 설정 파일
* servlet-context.xml ( Servlet WebApplicationContext )
    - 요청과 관련된 객체를 정의한다. url과 관련된 controller나, @어노테이션, ViewResolver, Interceptor, MultipartResolver등의 설정을 해준다.
    - DispatcherServlet( servlet-context.xml )은 요청에 대응할 수 있는 Controller, ViewResolver, HandlerMapping 과 같은 스프링 빈(Beans)을 구성


* root-context.xml ( Root WebApplicationContext )
    - 스프링의 환경설정 파일, view와 관련되지 않은 객체를 정의한다.
    - Service, Repositroy, DB등 비지니스 로직과 관련된 설정을 해준다.
    - WebApplicationContext( root-context.xml )에는 모든 서블릿이 공유할 수 있는 Service, Repository 와 같은 스프링 빈을 구성합니다.

![ DipatcherServlet 구조 ](https://docs.spring.io/spring-framework/docs/current/reference/html/images/mvc-context-hierarchy.png)

> 출처: https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc


### web.xml
- 웹프로젝트의 배치 기술서(deploy desriptor, 웹프로젝트의 환경설정파일)
- WAS가 최초로 구동될 때, 각종 설정을 정의한다.
- 여러 xml파일을 인식하도록 각 파일을 가리켜 준다.

### 스프링 컨테이너 참고 블로그
1. https://jeong-pro.tistory.com/225 [기본기를 쌓는 정아마추어 코딩블로그]
2. http://wonwoo.ml/index.php/post/1571 [ 머루의 개발 블로그 ]


## spring 프로젝트 폴더
1. src/main/java
    - java 파일 폴더
2. src/main/resource
    - 자바 클래스에서 사용하는 리소스 폴더
3. src/main/webapp/resources
    - 웹에 필요한 자원 폴더, 사용자가 직접 접근 가능
4. src/main/webapp/WEB-INF
    - 웹에 필요한 view파일과 컴파일된 파일, 환경설정(ex : spring)  파일 폴더
    - 외부 사용자가 직접 접근 X, 컨트롤러를 통해 내부적으로만 접근 가능
5. 프로젝트 시작 경로
    - workspace/프로젝트폴더명/src/main/webapp

## Template Engine
Template Engine이란 지정된 템플릿 양식과 데이터가 합쳐져 HTML 문서를 출력하는 소프트웨어를 이야기 한다.

### 서버 템플릿 엔진 vs 클라이언트 템플릿 엔진
- 서버 템플릿 : 서버에서 코드로 문자열을 만든뒤 이 문자열을 HTML로 변환하여 브라우저로 전달한다.
- 클라이언트 템플릿 : 작성된 자바스크립트 코드가 서버에서 실행되는게 아닌 브라우저에서 실행되어 화면을 생성한다. 


### Spring Template Engine
- JSP, Thymeleaf, Groovy, Freemarker, Jade4j, JMustache, Pebble, Handlebars
- 권장 템플릿
    - Thymeleaf
- 권장하지 않는 템플릿
    - Velocity는 Spring 4.3부터 사용중단.

### Spring Boot Template Engine
- Mustache, Thymeleaf, Groovy, Freemarker
- 권장 템플릿
    - Handlebars
    - Mustache
- 권장하지 않는 템플릿
    - Freemarker
    - JSP, Velocity


## spring Log4j

### Log4j
프로그램을 작성하는 도중에 로그를 남기기 위해 사용되는 자바 기반 로깅 유틸리티이다. 디버그용 도구로 주로 사용된다.

### Log4j 레벨
- FATAL : 아주 심각한 에러가 발생한 상태를 나타낸다.
- ERROR : 어떠한 요청을 처리하는 중 문제가 발생한 상태를 나타낸다.
- WARN : 프로그램의 실행에는 문제가 없지만, 향후 시스템 에러의 원이이 될 수 있는 경고성 메시지를 나타낸다.
- INFO : 어떠한 상태 변경과 같은 정보성 메시지를 나타낸다.
- DEBUG : 개발시 디버그 용도로 사용하는 메시지를 나타낸다.
- TRACE : 디버그 레벨이 너무 광범위한것을 해결하기 위해서 좀 더 상세한 이벤트를 나타낸다.
<br>

### Log4j 구성요소
- Logger : 출력할 메시지를 Appender에 전달한다.
- Appender : 전달된 로그를 어디에 출력할 지 결정한다. ( 콘솔 출력, 파일 기록, DB저장 등)
    - ConsoleAppender : org.apche.log4j.ConsoleAppender ( 콘솔에 로그 메시지를 출력한다. )
    - FilerAppender : org.apche.log4j.FilerAppender ( 로그 메시지를 지정된 파일에 기록한다. )
    - RollingFileAppender : org.apche.log4j.RollingFileAppnder ( 파일 크기가 일정 수준 이상이 되면 기존 파일을 백업파일로 두고 처음부터 다시 기록한다. )
    - DailyRollingFilerAppender : org.apche.log4j.Daily.Rolling.File.Appender ( 일정 기간 단위로 로그 파일을 생성하고 기록한다. )
- Layout : 로그를 어떤 형식으로 출력할 지 결정한다.
    
<br>

### Log4j 패턴
- %m : 로그 내용 출력
- %C : 클래스명 출력
- %d : 이벤트 발생 날짜 출력
- %M : 로깅이 발생한 method 이름 출력
- %F : 로깅이 발생한 프로그램 파일명 출력
- %l : 로깅이 발생한 caller 정보 출력
- %L : 로깅이 발생한 caller 라인수 출력

<br>

### Log4j 초기 프로젝트 생성시 xml 파일

```xml

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE log4j:configuration PUBLIC "-//APACHE//DTD LOG4J 1.2//EN" "log4j.dtd">
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
 
    <!-- Appenders -->
    <appender name="console" class="org.apache.log4j.ConsoleAppender">
        <param name="Target" value="System.out" />
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%-5p: %c - %m%n" />
        </layout>
    </appender>
    
    <!-- Application Loggers -->
    <logger name="com.tody.lovely" >
        <level value="info" />
    </logger>
    
    <!-- 3rdparty Loggers -->
    <logger name="org.springframework.core">
        <level value="info" />
    </logger>
    
    <logger name="org.springframework.beans">
        <level value="info" />
    </logger>
    
    <logger name="org.springframework.context">
        <level value="info" />
    </logger>
 
    <logger name="org.springframework.web">
        <level value="info" />
    </logger>
 
    <!-- Root Logger -->
    <root>
        <priority value="warn" />
        <appender-ref ref="console" />
    </root>
    
</log4j:configuration>

```

## WebMvcConfigurer 

### addResourceHandlers
- 리소스 등록 및 핸들러를 관리하는 객체이다. ( 이미지, js, css, html 등 정적인 리소스 )
- 요청 경로와 물리적인 저장 경로 매핑 역활
- 파일에 대한 접근과 HTTP 캐시 제어 가능
- java code

```java
	@Override
	public void addResourceHandlers(ResourceHandlerRegistry registry) { // 정적자원 처리를 위한 addResourceHandlers
		registry.addResourceHandler("/resources/**")  // -> /resources/ 이하로 오는 모든 요청을 resourceHandler에서 처리
				.addResourceLocations("/static/"); // -> 요청에 맵핑될 정적자원들의 원하는 위치 지정
	}
```


## 참고자료
1. 위키백과 - https://ko.wikipedia.org/wiki
2. todyDev - https://to-dy.tistory.com/