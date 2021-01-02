---
title:  "Spring5_Study"
excerpt: "Spring5 공부한 내용 정리"
categories:
  - SpringFramework

toc: true
toc_sticky: true
toc_label: "목차"

last_modified_at : 2021-01-02

---

## Spring_framework 공부 내용 정리

#### 스프링 MVC 핵심 구성요소

*핵심요소
  1. HandlerMapping 
    - 요청 URL과 매칭되는 컨트롤러 검색
  2. HandlerAdapter
    - 컨트롤러 실행 결과를 ModelAnView로 변환해서 리턴
  3. ViewResolver
    - 컨트롤러의 실행 결과를 보여줄 View를 검색

웹브라우저에서 요청이 들어와면 처리 과정은 DispatcherServlet이 모든 연결을 담당한다.

* Annotation
    - @Configuration : 해당 클래스를 스프링 설정 클래스로 설정
    - @EnableWebMvc : 스프링 MVC 설정을 활성화
    
---    
#### WebMvcConfigurer 

* addResourceHandlers
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

#### spring 프로젝트 폴더
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

#### Template Engine
Template Engine이란 지정된 템플릿 양식과 데이터가 합쳐져 HTML 문서를 출력하는 소프트웨어를 이야기 한다.

* 서버 템플릿 엔진 vs 클라이언트 템플릿 엔진
    - 서버 템플릿 : 서버에서 코드로 문자열을 만든뒤 이 문자열을 HTML로 변환하여 브라우저로 전달한다.
    - 클라이언트 템플릿 : 작성된 자바스크립트 코드가 서버에서 실행되는게 아닌 브라우저에서 실행되어 화면을 생성한다. 


* Spring Template Engine
    - JSP, Thymeleaf, Groovy, Freemarker, Jade4j, JMustache, Pebble, Handlebars
    - 권장 템플릿
        - Thymeleaf
    - 권장하지 않는 템플릿
        - Velocity는 Spring 4.3부터 사용중단.

* Spring Boot Template Engine
    - Mustache, Thymeleaf, Groovy, Freemarker
    - 권장 템플릿
        - Handlebars
        - Mustache
    - 권장하지 않는 템플릿
        - Freemarker
        - JSP, Velocity

