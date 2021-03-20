---
title:  "Git 원격 저장소 push 작업시 자동 로그인"
excerpt: "github push하게 되면 인증정보를 입력해야하는데 인증정보를 묻지 않고 바로 push하는 작업"
categories:
  - Git

toc: true
toc_sticky: true
toc_label: "목차"

last_modified_at : 2021-03-20

---

## Credential 저장소

SSH 프로토콜을 사용하여 리모트 저장소에 접근할 때 Passphase 없이 생성한 SSH Key를 사용하면 사용자이름과 암호를 입력하지 않고도 안전하게 데이터를 주고받을 수 있다. 반면 HTTP 프로토콜을 사용하는 경우는 매번 사용자이름과 암호를 입력해야 한다.

> 출처 : <https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Credential-%EC%A0%80%EC%9E%A5%EC%86%8C>


### Git Credential / cache

* 일정 시간 동안 메모리에 인증정보를 기억
* Disk에 저장하지 않음
* timeout 옵션으로 시간 지정 가능, 기본은 15분

```
# git config 업데이트
git config --global credential.helper cache
# git config --global --list
# ☞ credential.helper=cache
```


### Git Credential / store    

* 인증정보를 Disk의 텍스트 파일로 저장하며 계속 유지
* 인증정보가 암호화 되지 않고 그대로 저장됨
* 로그인 정보는 ~/.git-credentials에 저장되게 된다.

```
# git config 업데이트
git config --global credential.helper store
# git config --global --list
# ☞ credential.helper=store
```

### Git Credential / Keychain

* credential.helper를 store로 지정했을 때 암호화 하지 않고 파일 저장하는 문제 발생
* 안전하게 사용하기 위한 OS 자체의 Keychain 시스템 이용

> windows

```
# for windows
git config --global credential.helper wincred
# git config --global --list
# ☞ credential.helper=wincred
```

> mac

```
# for Mac
git config --global credential.helper osxkeychain
```

* remote repo의 비밀번호가 수정되면 "자격 증명 관리자"에서 해당 정보를 삭제해야함

### 설정 상태 확인

```
# local 설정
git config  --list
# Global 설정
git config --global --list
```

### 옵션 초기화 하기

```
git config --global --unset credential.helper
## system 옵션을 주면, 현재 시스템의 모든 사용자에게 적용
git config --system --unset credential.helper
```

만약, 초기화가 안될 경우 윈도우 기준 '자격 증명 관리자'에서 github.com와 관련된 증명 제거


### 참고
* https://pinedance.github.io/blog/2019/05/29/Git-Credential