---
title:  "spring_mybaits"
excerpt: "mybaits에 관한 설명 및 spring 프로젝트에서의 설정 방법"
categories:
  - SpringFramework

toc: true
toc_sticky: true
toc_label: "목차"

last_modified_at : 2021-04-03

---

## mybatis란?
마이바티스는 개발자가 지정한 SQL, 저장프로시저 그리고 몇가지 고급 매핑을 지원하는 퍼시스턴스 프레임워크이다. 마이바티스는 JDBC로 처리하는 상당부분의 코드와 파라미터로 설정 및 결과 매핑을 대신해준다.

객체 지향 언어인 자바의 관계형 데이터베이스 프로그래밍을 좀 더 쉽게 할 수 있게 도와주는 개발 프레임워크이다.

모든 JDBC 코드 및 매개 변수의 중복작업을 제거 한다.

Mybatis에서는 프로그램에 있는 SQL쿼리들을 한 구성파일에 구성하여 프로그램 코드와 SQL을 분리할 수 있는 장점을 가지고 있다.

## MyBatis 특징

* 복잡한 쿼리나 다이나믹한 쿼리에 강함
* 프로그램 코드와 SQL 쿼리의 분리로 코드의 간결성 및 유지보수성 향상
* 조호결과를 사용자 정의 DTO, MAP 등으로 맵핑하여 사용 가능

## spring 프로젝트에 Mybatis 연동하기 위한 의존설정
* MySQL Connector
* Mybatis
* Mybatis-spring (spring에서 Mybatis 연동을 위한 모듈)
* spring-jdbc(기본 자바 JDBC가 아닌 스프링의 JDBC)
* common-dbcp2(톰캣에서 커넥션풀을 이용할 수 있도록 아파치에서 제공해주는 라이브러리)

```xml
    
		<!-- Mysql Connector -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.39</version>
		</dependency>

		<!-- Mybatis -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.2.8</version>
		</dependency>
		
		<!-- Mybatis-Spring -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>1.2.2</version>
		</dependency>
		
		<!-- Spring-JDBC -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
		
		<!-- common-dbcp -->
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-dbcp2</artifactId>
			<version>2.7.0</version>
		</dependency>

```

### root-context.xml 설정


```xml




```

* DataSource
    *
* SqlSessionFactory
    *
* SqlSessionTemplate
    *


### mapper 파일 생성


### mybatis 환경설정 파일 생성


###

### Mybatis 연결 테스트




## 참고
* https://mybatis.org/mybatis-3/ko/index.html

* https://codevang.tistory.com/249

* https://khj93.tistory.com/entry/MyBatis-MyBatis%EB%9E%80-%EA%B0%9C%EB%85%90-%EB%B0%8F-%ED%95%B5%EC%8B%AC-%EC%A0%95%EB%A6%AC