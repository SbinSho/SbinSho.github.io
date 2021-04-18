---
title:  "spring_Log4j 설정"
excerpt: "log4j에 관한 설명 및 spring 프로젝트에서의 설정 방법"
categories:
  - SpringFramework

toc: true
toc_sticky: true
toc_label: "목차"

last_modified_at : 2021-04-18

---

## Log4j

프로그램을 작성하는 도중에 로그를 남기기 위해 사용되는 자바 기반 로깅 유틸리티이다. 디버그용 도구로 주로 사용된다.

### pom.xml

```xml

<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-core -->
<dependencies>
  <dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.14.1</version>
  </dependency>
  <dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.14.1</version>
  </dependency>
</dependencies>

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

### log4j 2.x 버전 XML 환경설정

```xml

<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
  <Appenders>
    <Console name="STDOUT" target="SYSTEM_OUT">
      <PatternLayout pattern="%d %-5p [%t] %C{2} (%F:%L) - %m%n"/>
    </Console>
  </Appenders>
  <Loggers>
    <Logger name="org.apache.log4j.xml" level="info"/>
    <Root level="debug">
      <AppenderRef ref="STDOUT"/>
    </Root>
  </Loggers>
</Configuration>

```

> 참고 : https://logging.apache.org/log4j/2.x/manual/migration.html


### Log4j 로그 레벨

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


### log4j 설정

* Configuration

```xml

<Configuration status="INFO"> <!-- status는 내부 이벤트에 대한 로그 레벨을 의미 -->

```

Configuration은 로그 설정의 최상위 요소 이다. Properties, Appenders, Loggers요소를 자식으로 가진다.

---

* Properties

```xml

<Properties>
    <Property name="logNm">Spring Log4j2 Test</Property>
    <Property name="layoutPattern">%style{%d{yyyy/MM/dd HH:mm:ss,SSS}}{cyan} %highlight{[%-5p]}{FATAL=bg_red, ERROR=red,
            INFO=green, DEBUG=blue}  [%C] %style{[%t]}{yellow}- %m%n -</Property>
</Properties>

```
Properties는 해당 Configuration 파일에서 사용할 프로퍼티를 의미한다. ( 변수와 비슷한 개념 )

---

* layout

Appender는 로그 이벤트를 Layout을 통해 포맷팅gksek. Appender가 받은 로그 이벤트를 Layout에게 전달하면, byte 배열을 반환합니다. Charset을 지정해주면 확실한 값을 얻을 수 있습니다.

---

* Appenders

Appender는 log 메시지를 특정 위치에 전달해주는 역할을 한다. layout을 통해 로그를 포매팅하고, 어떤 방식으로 로그를 제공할지 결정한다.

---

* Logger

로거는 로깅을 직접하는 요소 이다. 로거는 패키지 별로 설정 가능하며, Root패키지의 로거는 필수적이고, 추가적인 로거는 Logger로 설정 할 수 있다.

---

* Additivity

두개의 로거가 같은 appender를 사용하여 동일한 메시지가 중복으로 출력이 될때, 둘 중 하나의 로거에 additivity="false" 태그를 추가하면 중복 출력이 방지된다.

## log4j Mybaits 출력

1. pom.xml 설정 
    - log4jdbc-log4j2 추가

```xml
<dependency>
    <groupId>org.bgee.log4jdbc-log4j2</groupId>
    <artifactId>log4jdbc-log4j2-jdbc4.1</artifactId>
    <version>1.16</version>
</dependency>

```

2. classPath에 log4jdbc-log4j2.properties 파일 추가
    - log4jdbc.dump.sql.maxlinelength : 최대 몇 라인까지 출력할 것인가를 결정 0으로 하면 제한 없이 실행된 그대로 출력이 되고, 설정하지 않으면 그냥 한줄로 쭉 출력된다.
    
```

log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator
log4jdbc.dump.sql.maxlinelength=0

```

3. datasource 변경
    - driverClassName을 기존의 값에서 아래의 값으로 변경
    
```xml
	<bean id="dataSource"
		class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy" />
		<property name="url" value="jdbc:log4jdbc:mysql://localhost:3306/spring_project?serverTimezone=UTC" />
		<property name="username" value="${jdbc.username}" />
		<property name="password" value="${jdbc.userpassword}" />
	</bean>

```

4. log4j.xml 설정 추가

```xml

        <!--  SQL문과 해당 SQL을 실행시키는데 수행된 시간 정보(milliseconds)를 포함한다. 필요시 open -->
        <Logger name="jdbc.sqltiming" level="ERROR" additivity="false">
            <AppenderRef ref="WARN" />
        </Logger>
        <!--  SQL 결과 조회된 데이터의 table을 로그로 남긴다.(빼도됨) -->
        <Logger name="jdbc.resultsettable" additivity="false"> 
	        <level value="WARN"/> 
	        <appender-ref ref="console"/> 
        </Logger>
        <!-- ResultSet을 제외한 모든 JDBC 호출 정보를 로그로 남긴다. 많은 양의 로그가 생성되므로 특별히 JDBC 문제를 추적해야 할 필요가 있는 경우를 제외하고는 사용을 권장하지 않는다.--> 
        <Logger name="jdbc.audit" additivity="false"> 
	        <level value="WARN"/> 
	        <appender-ref ref="console"/> 
        </Logger>
        <!-- ResultSet을 포함한 모든 JDBC 호출 정보를 로그로 남기므로 매우 방대한 양의 로그가 생성된다. -->
        <Logger name="jdbc.resultset" additivity="false"> 
	        <level value="WARN"/> 
	        <appender-ref ref="console"/> 
        </Logger>


```

## 참고

* https://cofs.tistory.com/354
* https://to-dy.tistory.com/20
* https://beyondj2ee.wordpress.com/2012/07/11/%ED%8A%B9%EC%A0%95-%ED%81%B4%EB%9E%98%EC%8A%A4%EC%97%90-log4j-%EC%84%A4%EC%A0%95-%EC%A0%81%EC%9A%A9-%ED%95%98%EA%B8%B0/
* https://velog.io/@bread_dd/Log4j-2-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-%EA%B0%9C%EB%85%90