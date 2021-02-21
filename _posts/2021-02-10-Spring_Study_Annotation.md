---
title:  "Spring_Annotation"
excerpt: "springframework Annotation 위주로 정리 한 글입니다."
categories:
  - SpringFramework

toc: true
toc_sticky: true
toc_label: "목차"

last_modified_at : 2021-02-21

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

### @ModelAttribute
- 클라이언트가 전송하는 여러 파라미터들을 1대1로 객체에 바인딩하여 다시 Veiw로 넘겨서 출력하기 위해 사용되는 오브젝트이다.
- Setter 메소드 혹은 생성자가 필수적으로 필요함, 둘 중 하나라도 존재하지 않으면 바인딩 되지 않고 오류가 발생한다.

### @ResponseBody
- 자바 객체를 HTTP 요청의 body 내용으로 매핑하는 역할

### @RquestBody
- HTTP 요청의 body 내용을 자바 객체로 매핑하는 역할