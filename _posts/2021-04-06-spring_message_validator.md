---

title:  "Validation & spring_message"
excerpt: "Validation 및 spring_message를 이용해 객체 유효성 검증하기"
categories:
  - SpringFramework

toc: true
toc_sticky: true
toc_label: "목차"

last_modified_at : 2021-04-06

---

## Spring 메시지 처리

문자열이 뷰 코드에 하드 코딩 되어 있을 경우 동일 문자열을 변경할 때 문제가 있다. 

예를 들어 '이메일'을 '이메일 주소'로 변경하기로 했다면 각 폼을 출력하는 JSP를 모두 찾아 변경해야한다.

또 한 전 세계를 대상으로 하는 서비스는 다국어 지원에 문제가 있다. 각 언어에 맞게 뷰 코드를 따로 만드는 상황이 발생한다.

이 두가지 문제를 해결하기 위해 spring에서 제공하는 ReloadableResourceBundleMessageSource를 이용해서 공통 메시지를 처리 할 수 있다.

* 사용 방법
    * 문자열을 담은 메시지 파일을 작성한다.
    * 메시지 파일에서 값을 읽어오는 MessageSource 빈을 설정한다.
    * JSP 코드에서 <spring:message> 태그를 사용해서 메시지를 출력한다.
    
> 문자열을 담은 메시지 파일 작성

```properties

NotBlank=필수 항목입니다. 공백 문자는 허용하지 않습니다.
NotEmpty=필수 항목입니다.
Size.password=암호 길이는 6자 이상이어야 합니다.
Email=올바른 이메일 주소를 입력해야 합니다.
name=이름

```

> MessageSource 빈 설정

```xml

    <bean id="messageSource" class="org.springframework.context.support.ReloadableResourceBundleMessageSource">
        <!-- Encoding 설정 -->
        <property name="defaultEncoding" value="UTF-8"/> 
        <!-- Reload Cache 설정 -->
        <property name="cacheSeconds" value="5"/>
        <!-- basenames 설정: 아래처럼 하면 WEB-INF 밑의 message 폴더 아래의 labels로 시작하는 모든 Property-->
        <property name="basenames">
            <list>
                <value>classpath:/messages/error</value>
            </list>
        </property>       
    </bean>


```


> JSP 코드에서 <spring:message> 태그 사용

```jsp

<%@ taglib prefix="spring" uri="http://www.springframework.org/tags" %> // spring 태그 사용을 위한 taglib 추가

<spring:message code="name" /> // 메시지 프로퍼티 파일에서 읽어옴 ( "이름" 출력 )



```

> java에서 메시지 사용

```java

import java.util.Locale;
 
import org.springframework.context.MessageSource;
import org.springframework.context.support.ClassPathXmlApplicationContext;
 
public class MessageUtils {
 
    private static MessageSource resources = new ClassPathXmlApplicationContext("classpath:/spring/'메시지 관련 context'.xml");
 
    public static String getMessage(String code){
        return resources.getMessage(code, null, Locale.getDefault);
    }
 
    public static String getMessage(String code, String args[]){
        return resources.getMessage(code, args, Locale.getDefault);
    }
 
}

-------------------------------------------------------------

// 프로퍼티 파일
errors.minlength={0} 은 {1}자 이상 입력해야 합니다.
fail.common.msg=에러가 발생했습니다!


-------------------------------------------------------------
    
String[] args = {"사용자 이름", "2"};
String msg = MessageUtils.getMessage("errors.minlength", args); 
System.out.println("msg : " + msg); // "msg : 사용자 이름 은 2자 이상 입력해야 합니다."
 
String msg2 = MessageUtils.getMessage("fail.common.msg");
System.out.println("msg2 : " + msg2); // "msg : 에러가 발생했습니다!"


```

### 다국어 지원 위한 메시지 파일

다국어 메시지를 지원하려면 각 프로퍼티 파일 이름에 언어에 해당하는 로케일 문자를 추가한다.

* lable_ko.properties
* lable_en.properties

label 뒤에 '_언어' 형식의 접미사가 붙었다. 언어는 두 글자 구분자로 한국어 'ko', 영어는 'en'이다.
특정 언어에 해당하는 메시지 파일이 존재하지 않으면 언어 구분이 없는 label.properties 파일의 메시지를 사용한다.

스프링 MVC는 웹 브라우저가 전송한 Accept-Language 헤더를 이용해서 Locale을 구한다.

## Spring Validation

일반적으로 데이터 검증 (Validation) 은 여러 계층에 걸쳐서 이루어지게 된다. 동일한 내용의 검증로직이 각 계층별로 구현된다면 중복 작업으로 인한 불필요한 작업이 반복된다.

또 한 각 계층별로 구현된 검증로직간 불일치로 인하여 오류가 발생하기도 쉽다. 비지니스 로직에 검사 로직이 섞여있어 추적이 어렵고, 애플리케이션이 복잡해진다.

이를 해결하기 위해 Spring에서는 사용자가 입력한 값에 대한 유효성을 체크하기 위해 Spring Validator 지원하고, Validator는 Java EE Spec인 Bean Validation의 어노테이션을 이용하여 객체가 제대로 요구사항에 맞추어 생성 됬는지 검증할 수 있다.

### Validator 인터페이스

spring에서는 org.springframework.validation.Validator 인터페이스 제공하고 있다. 

Validator 인터페이스를 상속한 클래스는 두 메서드를 구현해야 한다.

```java

public interface Validator {
    
    boolean supports(Class<?> clazz);
    void validate(Object target, Errors errors);
    
}

```

* supports(Class<?> clazz) : Validator가 검증할 수 있는 타입인지 검사한다. 

* validate(Object target, Errors errors) : 첫 번째 파라미터로 전달받은 객체를 검증하고 오류 결과를 Errors에 담는 기능을 정의한다.

### Validator 인터페이스를 구현한 클래스

```java

public class RegisterRequestValidator implements Validator {
	
	private static final String emailRegExp = 
			"^[_A-Za-z0-9-\\+]+(\\.[_A-Za-z0-9-]+)*@" +
			"[A-Za-z0-9-]+(\\.[A-Za-z0-9]+)*(\\.[A-Za-z]{2,})$";
	
	private Pattern pattern;
	
	public RegisterRequestValidator() {
		pattern = Pattern.compile(emailRegExp);
	}

	@Override
	public boolean supports(Class<?> clazz) {
		return RegisterRequest.class.isAssignableFrom(clazz);
	
	}

	@Override
	public void validate(Object target, Errors errors) {
		RegisterRequest regReq = (RegisterRequest) target;
		if(regReq.getEmail() == null || regReq.getEmail().trim().isEmpty()) {
			errors.rejectValue("email", "required");
		}
		else {
			Matcher matcher = pattern.matcher(regReq.getEmail());
			if ( !matcher.matches()) {
				errors.rejectValue("email", "bad");
			}
		}
		ValidationUtils.rejectIfEmptyOrWhitespace(errors, "name", "required");
		ValidationUtils.rejectIfEmpty(errors, "password", "required");
		ValidationUtils.rejectIfEmpty(errors, "confirmPassword", "required");
		if(!regReq.getPassword().isEmpty()) {
			if(!regReq.isPasswordEqualToConfirmPassword()) {
				errors.rejectValue("confirmPassword", "nomatch");
			}
		}

	}
	
}


```

* supports() 메서드는 파라미터로 전달받은 clazz 객체가 검사 대상 객체 클래스로 타입 변환 가능한지 확인하다.

* supports() 메서드를 직접 실행하진 않지만 스프링 MVC가 자동으로 검증 기능을 수행하도록 설정하려면 supports() 메서드를 올바르게 구현해야한다.

* validate() 메서드는 두 개의 파라미터를 갖는다. 
    * target 파라미터는 검사 대상 객체이다.
    * errors 파라미터는 검사 결과 에러 코드를 설정하기 위한 객체이다.
    
* rejectValue() 메서드는 첫 번째 파라미터로 프로퍼티의 이름을 전달받고, 두 번째 파라미터로 에러 코드를 전달 받는다.

### ValidationUtils 클래스

ValidationUtils 클래스는 객체의 값 검증 코드를 간결하게 작성할 수 있도록 도와준다.

>  ValidationUtils를 사용 하지 않은 경우

```java

String name = regReq.getName();
if(name == null || name.trim().isEmpty()){
    errors.rejectValue("name", "required");
}

```

> ValidationUtils를 사용한 경우

```java

ValidationUtils.rejectIfEmptyOrWhitespace(errors, "name", "required");

```


#### Errors와 ValidationUtils 클래스

* Errors
    * result( ... ) : 특정 프로퍼티에 에러 적용하는게 아닌 객체 자체에 에러를 추가한다.
    * resultValue( ... ) : 특정 프로퍼티에 에러를 적용한다.
    
* ValidationUtils
    * rejectEmpty( ... ) : 필드에 해당하는 프로퍼티 값이 null이거나 빈 문자열("")인 경우 에러코드 추가
    * rejectEmptyOrWhitespace( ... ) : null이거나 빈 문자열인 경우, 공백 문자로만 값이 구성된 경우 에러코드 추가



#### Controller에 유효성 검증 적용 ( 직접 호출 )

> 검증객체를 사용자가 직접호출하여 검증
    
```java

	@PostMapping("/register/step3")
	public String handleStep3(RegisterRequest regReq, Errors errors) {
    
		new RegisterRequestValidator().validate(regReq, errors); // 유효성 검사
                                                                      
		if(errors.hasErrors())
			return "register/step2";
		try {
			
			memberRegisterService.regist(regReq);
			return "register/step3";
			
		} catch (DuplicateMemberException e) {
			errors.rejectValue("email", "duplicate");
			return "register/step2";
		}
	}

```

* 스프링 MVC는 handleStep3() 메서드를 호출할 때 객체와 연결된 Erros 객체를 생성해서 파라미터로 전달한다.
    * Errors 객체는 무조건 검증할 객체 뒤에 위치해야함

* Errors 대신 org.springframework.validation.BindingResult 인터페이스를 파라미터 타입으로 사용해도 된다.

## 글로벌 범위 Validator와 컨트롤러 범위 Vaildtor 

* validate() 메소드를 직접 호출하지 않고 프레임워크에서 호출 하는 방법.
    * 설정 클래스 또는 context 파일에 Validator 구현 객체를 사용할 수 있도록 설정
    * 글로벌 범위 Validator가 검증할 객체에 @Valid 어노테이션 적용
    * 적용할 컨트롤러 범위에 @InitBinder를 이용하여 검증할 객체를 컨트롤러 요청 전 처리

> @Valid 어노태이션 사용을 위한 의존설정 추가

```xml

<dependency>
  <groupId>javax.validation</groupId>
  <artifactId>validation-api</artifactId>
  <version>1.1.0.Final</version>
</dependency>

```

> 설정 클래스에 글로벌 범위 Validator 구현 객체 등록

```java
@Override
public Validator getValidator() {
    return new RegisterRequestValidator();
}

```

#### Controller에 적용 ( 직접 호출 X, 어노태이션 사용 )

> @Valid를 이용한 글로벌 범위 Validator 적용 ( 직접 호출하지 않음 )

```java

	@PostMapping("/register/step3")
	public String handleStep3(@Valid RegisterRequest regReq, Errors errors) { // @Valid 어노테이션 적용
		if (errors.hasErrors())
			return "register/step2";

		try {
			memberRegisterService.regist(regReq);
			return "register/step3";
		} catch (DuplicateMemberException ex) {
			errors.rejectValue("email", "duplicate");
			return "register/step2";
		}
	}

```

> @Initbinder를 이용한 컨트롤러 범위 Validator 적용

``` java

@InitBinder
protected void initBinder(WebDataBinder binder){
    binder.setValidator(new RegisterRequestvalidator());
}


```

@InitBinder가 붙은 메서드는 컨트롤러의 요청 처리 메서드를 실행하기 전에 매번 실행된다.



## Bean Validation을 이용한 유효성 검증

@Valid 애노테이션은 Bean Validation 스펙에 정의되어 있다.

Validator 작성 없이 애노테이션만으로 커맨드 객체의 값 검증을 처리할 수 있다.

> Bean Validation을 적용하기 위한 의존설정 추가

```xml

<dependency>
  <groupId>org.hibernate</groupId>
  <artifactId>hibernate-validator</artifactId>
  <version>5.4.2.Final</version>
</dependency>


```

### 검증할 객체에 Bean Validation 적용

```java

@NotBlank
@Email
private String email;
@Size(min = 6)
private String password;
@NotEmpty
private String confirmPassword;
@NotEmpty
private String name;

```

만약 글로벌 범위의 Validator가 설정 되어 있다면, 해당 설정을 삭제해야 Bean Validation을 사용 할 수 있다.

* 기본 에러 메시지 이외에 원하는 메시지 출력을 원할 경우, 다음 규칙을 따르는 메시지 코드를 메시지 프로퍼티 파일에 추가하면 된다.
    * 애노테이션이름.커맨드객체모델명.프로퍼티명
    * 애노테이션이름.프로퍼티명
    * 애노테이션이름

## 뷰 페이지에서 에러 코드 출력

```jsp

<%@ taglib prefix="spring" uri="http://www.springframework.org/tags" %> // spring 태그 사용을 위한 taglib 추가

<form:error path="프로퍼티"/> // 에러 코드가 저장 된 필드명


```

    


## 참고

* 저자 : 최범균님, 제목 : 초보 웹 개발자를 위한 스프링 5 프로그래밍 입문

* https://engkimbs.tistory.com/728

* https://linuxism.ustd.ip.or.kr/653

* https://medium.com/@gaemi/java-%EC%99%80-spring-%EC%9D%98-validation-b5191a113f5c

* http://wonwoo.ml/index.php/post/1082