---
title:  "Spring_Annotation"
excerpt: "springframework Annotation 위주로 정리 한 글입니다."
categories:
  - SpringFramework

toc: true
toc_sticky: true
toc_label: "목차"

last_modified_at : 2021-02-10

---

## springframework Annotation
### @ExceptionHandler
- @Controller, @RestController가 적용된 Bean 내에서 발생하는 예외를 잡아서 하나의 메서드에서 처리해주는 기능을 한다.

### @ControllerAdvice
- @ExceptionHandler는 해당 컨트롤러에서 발생한 익셉션만을 처리하지만, @ControllerAdvice는 지정한 범위의 Controller에 공통으로 사용될 설정을 지정 할 수 있다.

### @Configuration
- 해당 클래스를 스프링 설정 클래스로 설정

### @EnableWebMvc
- 스프링 MVC 설정을 활성화