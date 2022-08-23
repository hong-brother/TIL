# OAuth2란?
OAuth2(Open Authorization 2.0)은 인증을 위한 개방형 표준 프로토콜이다.
OAuth 프로토콜은 써드파티 어플리케이션에게 인증을 위한 접근 권한을 위임하는 방식을 제공한다.

대표적으로 구글, 카카오, 네이버, 메타 등에서 제공하는 `간편 로그인` 기능도 OAuth2 프로토콜 기반의 사용자 인증 기능을 제공하고 있습니다.

# OAuth2를 사용하는 이유?
만약 구글을 이용하여 구글드라이브 API를 통한 서드파티 어플리케이션을 개발한다고 가정하다. 써드파티 어플리케이션을 사용하는 사용자의 구글 아이디와 패스워드를 알고 있어야만 써드파티에서도 쉽게 서비스르를 이용할 수 가 있다. 하지만 이 방법은 처음보는 써드파티 어플리케이션을 신뢰 할 수 없기 때문에 상당히 위험하고 무서운 방법이다. 
그래서 OAuth2 프로토콜을 이용하여 사용자의 아이디 패스워드가 아닌 `accessToken`을 이용하여 접근 할 수 있으며 `accessToken`을 통하여 제공하는 서비스의 접근을 제한 할 수 있다.




# OAuth2 주요 용어
|용어|내용|
|--|--|
|Authentication|인증, 접근 자격이 있는지 검증하는 단계|
|Authorization|인가, 자원에 접근할 권한을 부여한다. 인가가 완료되면 리소스 접근 권한이 담긴 Access Token이 클라이언트에게 전달된다.|
|Access Token|리소스 서버에게서 리소스 소유자의 보호된 자원을 획득할 때 사용되는 토큰
|Refresh Token|Access Token 만료시 이를 갱신하기 위한 용도로 사용하는 Token, 일반적으로 Access Token보다 Refresh Token이 만료 시간이 더 길다.|

# Authorization
## Authorization Code Grant(권한 부여 승인 코드 방식)
![](https://velog.velcdn.com/images/hong-brother/post/4e9d637b-2a1d-416e-9108-78a17df98a42/image.png)

- OAuth2 에서 가장 많이 사용되는 방식이다.
- 간편 로그인 기능에서 사용되는 방식으로 클라이언트가 사용자를 대신하여 특정 자원에 접근을 요청할 때 사용되는 방식이다. 

https://yonghyunlee.gitlab.io/temp_post/oauth-authorization-code/



# Reference
[생활코딩-OAuth2](https://opentutorials.org/course/3405)
