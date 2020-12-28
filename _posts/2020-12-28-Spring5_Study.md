---
title:  "Spring5_Study"
excerpt: "Spring5 공부한 내용 정리"
categories:
  - SpringFramework

toc: true
toc_sticky: true
toc_label: "목차"

last_modified_at : 2020-12-28

---

#### Spring_framework 공부 내용 정리

## 스프링 MVC 핵심 구성요소

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
    
