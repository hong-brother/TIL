>
NEST.JS에 JWT Token을 적용하기 위한 이론 알고 가기


# 🔐JWT(JSON Web Token)
- `JWT란` 인증에 필요한 정보들을 암호화시킨 JSON 토큰을 말한다.
- JWT 토큰(Access Token)을 HTTP 헤더내에 Authorization 담아 클라이언트와 서버가 통신한다.
- JWT는 크게 `Header`, `Payload`, `Signature` 으로 점(.)으로 구분하는 구조이다.

![JWT](https://velog.velcdn.com/images/hong-brother/post/35e27902-705d-4804-83ea-4ff472b852ee/image.png)

![](https://velog.velcdn.com/images/hong-brother/post/dab00598-2f8f-40f8-9467-bc4ab88326e3/image.png)


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
- `nbf`: 토큰의 활성 날짜(Not Before)
- `iat`: 토큰이 발급된 시간(issued at)
- `jti`: 고유 식별자로서 중복적인 처리를 방지하기 위해 사용, 일회용 토큰에 사용하면 유용

### public Claim
사용자 정의 클레임으로 공개용 정보를 위해 사용된다. 또한 충돌 방지를 위해 URI 형식으로 이용한다.
```json
{
    "https://velog.io/@hong-brother/name": "hong-brother"
}
```

### private Claim
클라이언트와 서버간에 합의하에 등록하여 사용 할 수 있는 클레임 이다.

### payload 예제
```json
{
  "u": "ggbqmf113013e4c5",
  "r": "fce10bab",
  "t": "a",
  "https://velog.io/@hong-brother/name": "hong-brother"
  "iat": 1671118700,
  "exp": 1671723500
}
```

## Signature
```json
RSASHA256( <-- header의 alg에 따라서 변경
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret
  )
```
signature는 인코딩된 header, payload에 "."을 더한 뒤 비밀키로 해싱하여 생성한다.
Header 및 Payload는 단순 인코딩된 값이기 때문에 해커가 복호화하고 조작할 수 있지만, Signature는 서버 측에서 관리하는 비밀키가 유출되지 않는 이상 복호화할 수 없다.

따라서 Signature는 토큰의 위변조 여부를 확인하는 데 사용된다.
