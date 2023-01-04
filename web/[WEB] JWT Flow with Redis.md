>
JWT(JSON Web Token) 이용하여 로그인시 Token을 발행하고 관리하는 Flow 대한 정리.

## Access Token & Refresh Token
- Access Token과 Refresh Toekn은 똑같이 JWT이다 둘 차이는 Refresh Token이 Access Token보다 유효기간이 상대적으로 길다.
- 사용자 로그인시 Access Toeken과 Refresh Token을 함께 발행하여 사용자에게 전달해주며 Access Token 만료시 Refresh Token을 전송하여 새로운 Access Token을 갱신 할 수 있다.
- 또한 서버측에서는 Refresh Token을 안전한 장소에 저장하고 사용자의 Access Token이 탈취당할 경우를 대비하여 탈취당했다고 판단 시, 서버측 Refresh Token을 삭제하여 해당 사용자를 강제 로그아웃을 시킬 수 있다.
- 이로써 JWT의 단점인 발급한 후 삭제가 불가능한 문제를 Access Token, Refresh Token으로 관리하므로써 어느정도 해소 시킬 수 있다.


## JWT Token Flow
- Access Token, Refresh Token을 이용하여 JWT Token Flow을 생각해볼 것이다. 크게 Client, Server, DB, Redis를 사용하도록 구성 하였다.
- Redis를 사용하는 이유는 Refresh Token을 저장하기 위함이 가장 크다.
Redis는 key-value 쌍으로 데이터를 관리하는 스토리지 이며 key에 TTL을 적용하여 Expire을 설정 할 수 있다.
- Refresh Token의 저장하기 위한 Redis를 선택한 이유도 Redis의 장점을 이용한 빠른 엑세스 속도로 로그인시 병목 현상을 방지하기 위함이다. 또한 Refresh Token을 DB에 저장 할 경우 세부적으로 서버단에서 스케줄러에 의해서 또는 트리거에 의해서 Refresh Token을 직접 컨트롤 해야 한다. 하지만 Redis의 TTL을 사용하여 적용한다면 서버단에서 핸들링 할 부담은 더 줄어들 것이다.


### 로그인 Flow
![](https://velog.velcdn.com/images/hong-brother/post/682dc8ee-eb75-41a3-bbd8-fee4ebcec880/image.png)
- `Client`가 로그인 시 `Server`는 `DB`의 사용자 정보를 조회 하여 회원 가입여부에 따라 **AccessToken, Refresh Token** 을 발급한다. 
- 발급한 Refresh Token은 Redis에 저장 한다.
- `Server`는 요청한 `Client`에게 Access Token, Refresh Token을 응답한다.
- `Client`는 매 요청 API 마다 Header에 Access Token을 담아 전송 하고 서버는 전송받은 Access Token을 검증하여 `Client`요청에 응답한다.


### Access Token 만료시 Flow
![](https://velog.velcdn.com/images/hong-brother/post/1b419352-cc75-4984-b4f4-82741b0d6e2f/image.png)
- `client`에서 Access Token 만료 시 재발급을 위한 /reissue API를 요청한다. 해당 API의 Heder내에 Authorization header내에는 Refresh Token을 전송 한다.
- `Server`에서는 Refresh Token의 payload내에 Access Token 인지, Refresh Token 인지 확인한다.
- `Server`에서는 확인된 Token이 Refresh Token이면 Payload 내에 사용자 정보를 얻을 수 있는(ex. email, id ...)key로 `Redis`에 조회 하여 Refresh Token이 존재하는지 확인한다.
- Refresh Token이 존재한다면 `Server`에서는 다시 Access Token, Refresh Token을 발급하여 `Client`에게 전달한다.
- 새로 발급된 Refresh Token은 다시 Redis에 저장한다.

### 로그아웃된 사용자 Flow
![](https://velog.velcdn.com/images/hong-brother/post/afa2c856-0adb-4040-ac65-1e2701f49387/image.png)
- `Client`에서는 Access Token 만료 시 재발급을 위한 /reissue API를 요청 한다.
만약 해당 사용자가 로그아웃이 되어 있다면 Refresh Token은 Redis에서 삭제 되었을 것이다.
- Refresh Token을 받은 `Server` 에서는 Redis에서 Refresh Token이 존재 여부를 확인하고 존재 하지 않을 경우 `Client` 응답으로 HTTP UnAuthorized(401) 상태를 응답하게 된다.

### Refresh Token을 이용한 보안 강화
- 만약 정상적인 사용자 Access Token이 탈취 당하거나, 원치 않은 해외 IP가 사용된 사용자인 경우 Refresh Token을 서버내에(Redis)삭제 하므로써 해당 사용자를 강제 로그아웃하여 보안을 강화 할 수 있을 것 이다.
