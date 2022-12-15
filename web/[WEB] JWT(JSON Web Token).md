>
NEST.JS에 JWT Token을 적용하기 위한 이론 알고 가기


# 🔐JWT(JSON Web Token)
- `JWT란` 인증에 필요한 정보들을 암호화시킨 JSON 토큰을 말한다.
- JWT 토큰(Access Token)을 HTTP 헤더내에 Authorization 담아 클라이언트와 서버가 통신한다.
- JWT는 크게 `Header`, `Payload`, `Signature` 으로 점(.)으로 구분하는 구조이다.

![JWT](https://velog.velcdn.com/images/hong-brother/post/35e27902-705d-4804-83ea-4ff472b852ee/image.png)

![](https://velog.velcdn.com/images/hong-brother/post/e66316fb-ea0c-4afc-ab6d-c39f437155f0/image.png)


## Header
header는 `typ`,`alg` 정보를 가지고 있다.
`typ`은 토큰의 타입을 지정하며, `alg`은 해싱 알고리즘을 지정한다. 
해싱 알고리즘은 HMAC, SHA256, RSA등이 있으며 지정한 알고리즘은 토큰 검증 시 signature 부분에 사용 된다.

![](https://velog.velcdn.com/images/hong-brother/post/f1e94014-3574-43b3-b673-a37ea21b8600/image.png)

## Payload
payload는 토큰에 담을 정보가 담겨 있다. `key-value` 형식으로 이루어진 정보를 `클레임 셋 (Claim Set)` 이라 부른다.
Claim에는 크게 세가지 `registered Claim`, `public Claim`, `private Claim`을 토큰에 담을 수 있다.
### registered Claim
토큰에 대한 정보들을 담기 위한 클레임 셋이다. 아래 클레임 셋은 모두 Optional이다.

- `iss`: 토큰 발급자(issuer)
- `sub`: 토큰 제목(subject)
- `aud`: 토큰 대상자(audience)
- `exp`: 토큰 만료시간(expiration)
- `nbf`: Not Before를 의미 토큰의 활성 날짜와 비슷한 개념
- `iat`: 토큰이 발급된 시간
- `jti`: 고유 식별자로서 중복적인 처리를 방지하기 위해 사용.

## Verify Signature
