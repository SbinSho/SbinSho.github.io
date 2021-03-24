---
title:  "spring_Log4j 설정"
excerpt: "log4j 설명 및 설정 방법"
categories:
  - SpringFramework

toc: true
toc_sticky: true
toc_label: "목차"

last_modified_at : 2021-03-23

---

## Log4j

프로그램을 작성하는 도중에 로그를 남기기 위해 사용되는 자바 기반 로깅 유틸리티이다. 디버그용 도구로 주로 사용된다.

### Log4j 레벨

- FATAL : 아주 심각한 에러가 발생한 상태를 나타낸다.
- ERROR : 어떠한 요청을 처리하는 중 문제가 발생한 상태를 나타낸다.
- WARN : 프로그램의 실행에는 문제가 없지만, 향후 시스템 에러의 원이이 될 수 있는 경고성 메시지를 나타낸다.
- INFO : 어떠한 상태 변경과 같은 정보성 메시지를 나타낸다.
- DEBUG : 개발시 디버그 용도로 사용하는 메시지를 나타낸다.
- TRACE : 디버그 레벨이 너무 광범위한것을 해결하기 위해서 좀 더 상세한 이벤트를 나타낸다.

* FATAL <- ERROR <- WARN <- INFO <- DEBUG <- TRACE
    * DEBUG 레벨로 처리시, INFO ~ FATAL까지 모두 logging됨


### Log4j 구성요소

| 요소 | 설명 |
|----|---|
| Logger | 출력할 메시지를 Appender에 전달한다. |
| Appender | 전달된 로그를 어디에 출력할 지 결정한다. (콘솔 출력, 파일 기록, DB 저장 등) |
| Layout | 로그를 어떤 형식으로 출력할 지 결정한다. |


### Log4j 패턴

| 패턴 | 설명 |
|---|---|
| %p | debug, info, warn, error, fatal 등의 priority 출력 |
| %m | 로그내용 출력 |
| %d | 로깅 이벤트가 발생한 시간을 출력 <br>ex)포맷은 %d{HH:mm:ss} 같은 형태의 SimpleDateFormat |=
| %t | 로그이벤트가 발생된 쓰레드의 이름 출력 |
| %F | 로깅이 발생한 프로그램 파일명 출력 |
| %l | 로깅이 발생한 caller의 정보 출력 |
| %L | 로깅이 발생한 caller의 라인수 출력 |
| %M | 로깅이 발생한 method 이름 출력 |
| % | % 표시 출력 |
| %n | 플랫폼 종속적인 개행문자 출력 |
| %c | 카테고리 출력 <br>ex)카테고리가 a.b.c 처럼 되어있다면 %c{2}는 b.c 출력 |
| %C | 클래스명 출력 <br>ex)클래스구조가 org.apache.xyz.SomeClass 처럼 되어있다면 %C{2}는 xyz.SomeClass 출력 |
| %r | 어플리케이션 시작 이후 부터 로깅이 발생한 시점의 시간(milliseconds) 출력 |
| %x | 로깅이 발생한 thread와 관련된 NDC(nested diagnostic context) 출력 |
| %X | 로깅이 발생한 thread와 관련된 MDC(mapped diagnostic context) 출력 |


### Appender

| 요소 | 설명 |
| --- | --- |
| ConsoleAppender | org.apache.log4j.ConsoleAppender<br>콘솔에 로그 메시지 출력 |
| FileAppender | org.apache.log4j.FileAppender<br>파일에 로그 메시지 기록 |
| RollingFileAppender | org.apache.log4j.rolling.RollingFileAppender<br>파일 크기가 일정 수준 이상이 되면 기존 파일을 백업파일로 바꾸고 처음부터 기록 |
| DailyRollingFileAppender | org.apache.log4j.DailyRollingFileAppender<br>일정 기간  단위로 로그 파일을 생성하고 기록 |
| JDBCAppender | org.apache.log4j.jdbc.JDBCAppender<br>DB에 로그를 출력. 하위에 Driver, URL, User, Password, Sql과 같은 parameter를 정의할 수 있음 |
| SMTPAppender | 로그 메시지를 이메일로 전송 |
| NTEventAppender | 윈도우 시스템 이벤트 로그로 메시지 전송 |


### pom.xml

```xml

<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-core -->
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.12.1</version>
</dependency>

```

### log4j 1.x 버전 XML 환경설정

```xml

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE log4j:configuration PUBLIC "-//APACHE//DTD LOG4J 1.2//EN" "log4j.dtd">
<log4j:configuration xmlns:log4j='http://jakarta.apache.org/log4j/'>
  <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
    <layout class="org.apache.log4j.PatternLayout">
      <param name="ConversionPattern" value="%d %-5p [%t] %C{2} (%F:%L) - %m%n"/>
    </layout>
  </appender>
  <category name="org.apache.log4j.xml">
    <priority value="info" />
  </category>
  <Root>
    <priority value ="debug" />
    <appender-ref ref="STDOUT" />
  </Root>
</log4j:configuration>

```

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

### log4j 설정

* logger 설정시 "패키지 레벨" 적용, 클래스명을 기술 하기도 한다.
* additivity 옵션은 "log4j"는 기본적으로 부모 ( root logger )를 정의된 속성을 상속 받는데 additivity="false"를 사용하면 상속을 거부 할 수 있다.
    * root logger에서 상속 시 자식에게 level이 존재하면 그 levle을 바꾸지 않고 사용, 없을 경우 부모에게 선언된 level을 상속 받는다.
    * appender 같은 경우 Additivity를 fals로 사용하지 않으면 자식에게 선언된 appender와 부모에게 상속 받은 appender을 둘 다 사용한다.

### 참고

* https://cofs.tistory.com/354
* https://to-dy.tistory.com/20
* https://beyondj2ee.wordpress.com/2012/07/11/%ED%8A%B9%EC%A0%95-%ED%81%B4%EB%9E%98%EC%8A%A4%EC%97%90-log4j-%EC%84%A4%EC%A0%95-%EC%A0%81%EC%9A%A9-%ED%95%98%EA%B8%B0/