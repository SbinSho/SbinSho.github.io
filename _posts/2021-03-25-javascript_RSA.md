---
title:  "Spring 프로젝트에 javascript RSA를 이용한 Client Side 암호화 처리"
excerpt: "javascript를 이용해, HTTP에서 평문으로 전송되는 개인정보를 암호화 처리하여 서버에 전송 "
categories:
  - SpringFramework
  - RSA

toc: true
toc_sticky: true
toc_label: "목차"

last_modified_at : 2021-04-02

---

## RSA

1978년 로널드 라이베스트(Ron Rivest), 아디 샤미르(Adi Shamir)이 발표한 공개키 암호화 방식으로, 공개키 암호화의 개념을 수학 적으로 구체화 시킨 알고리즘이다.

RSA 암호체계의 안정성은 큰 숫자를 소인수 분해하는 것이 어렵다는 것에 기반을 두고 있다.

RSA는 메시지를 암호화하는데 쓰이는 공개키(public key)와 암호호된 메시지를 복호화하는 개인키(private key) 두 개의 키를 사용한다.

공개키 알고리즘은 누구나 어떤 메시지를 암호화할 수 있지만, 그것을 해독하여 열람할 수 있는 사람은 개인키를 지닌 단 한사람만이 존재한다는 점에서 대칭키 알고리즘과 차이를 가진다.

## RSA를 사용하게 된 계기

WEB 서버에 SSL 인증서 설치 없이 HTTP를 이용한 로그인 또는 회원가입을 처리할때 개인정보를 평문으로 전송 할 경우 중간자 공격(man in the middle attack, MITM)에 취약해지기 때문에 서버에 전송하기 전 평문을 암호화 처리하기 위해서 사용했다.


## javascript에서 RSA 사용

### 동작 순서

* [서버] 서버측에서 RSA 공개키와 개인키(비밀키)를 생성하여, 개인키는 세션에 저장하고 공개키는 자바스크립트 로그인 폼이 있는 페이지로 출력한다.


> code

```java

HttpSession session = request.getSession();
		
KeyPairGenerator generator = KeyPairGenerator.getInstance("RSA");
generator.initialize(1024);
		
KeyPair keyPair = generator.genKeyPair();
KeyFactory keyFactory = KeyFactory.getInstance("RSA");
		
PublicKey publicKey = keyPair.getPublic();
PrivateKey privateKey = keyPair.getPrivate();
 
session.setAttribute("_RSA_WEB_Key_", privateKey);   //세션에 RSA 개인키를 세션에 저장한다.

RSAPublicKeySpec publicSpec = (RSAPublicKeySpec) keyFactory.getKeySpec(publicKey, RSAPublicKeySpec.class);
		
String publicKeyModulus = publicSpec.getModulus().toString(16);
String publicKeyExponent = publicSpec.getPublicExponent().toString(16);
 
request.setAttribute("RSAModulus", publicKeyModulus);  //로그인 폼에 Input Hidden에 값을 셋팅하기위해서
request.setAttribute("RSAExponent", publicKeyExponent);   //로그인 폼에 Input Hidden에 값을 셋팅하기위해서

```

* [클라이언트] 회원가입폼에 양식작성 후 발행(submit)을 누르면 자바스크립트가 이를 가로챈다.
    - 회원가입 폼의 아이디, 비밀번호, 이메일을 RSA로 암호화하여 암호화된 암호문을 회원가입 폼의 값으로 변경한다.

> code

```javascript

// RSA 암호화 생성
var rsa = new RSAKey();
rsa.setPublic($("#RSAModulus").val(), ("#RSAExponent").val());

// 암호화된 비밀번호
var en_pwd = rsa.encrypt(user_pwd);
				
$("#user_pwd").val(en_pwd);

```

* [서버] 서버측에서 세션에 저장된 RSA개인키로 암호화된 암호문을 복호화 한다.

* [서버] 평문이 복호화가 완료되면 유효성 검사를 한번 더 서버에서 실행하고 통과되면 DB에 현재 입력된 회원가입 정보를 저장한다.


## 참고

* https://ko.wikipedia.org/wiki/RSA_%EC%95%94%ED%98%B8
* http://kwon37xi.egloos.com/4427199