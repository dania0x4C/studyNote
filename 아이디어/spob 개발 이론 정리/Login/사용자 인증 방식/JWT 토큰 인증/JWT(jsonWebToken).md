1. 정의: json 객체에 인증이 필요한 정보를 담고 privateKey로 서명한 token이다. 인증 & 권한 허가에 사용되는 방식이다.

2. 프로세스![[Pasted image 20240514005619.png]]

	중요 포인트 
	1. 로그인을 하면 privateKey로 토큰을 만들어 전달
	2. 로그인 후 Client에서는 API 요청시 header에 Token을 지속적으로 넣어서 요청 

	token을 사용하는 이유는  http의 특성 때문이다.

	HTTP의 특성
	1. Conntionless: 한 번 통신이 이루어지면 연결이 바로 끊어진다.
	2. Stateless: 통신에서 이전 상태를 유지하거나 기억하지 않는다.

	즉, 이러한 특성 때문에 api를 요청할 때 신뢰할만한 사용자인지 확인 하는 방식이다. 
	이러한 방식은 귀찮고 통신이 느려지는 문제도 발생한다.

	구조
	header.payload.signature

```
const header = 
{
	"typ" : "JWT",// 타입명
	"alg" : "해싱 알고리즘"
};
```


```
const payload = {
	// registered Claim
    "iss": "토큰 발급자",
    "sub": 토큰 제목,
    "aud": 토큰 대상자,
    "exp": "토큰 만료시간",
    "nbf": "토큰이 활성화 될 날짜(NumericData 형식)",
    "iat": "토큰이 발급된 시간",
    "jti": "jwt  고유 식별자"

	// public Claim
	// 충돌 방지를 위한 이름(URI 형식으로 작성)
	"https://velopert.com/jwt_claims/is_admin": true

	// private Claim
	// 서버와 클라이언트의 협의하에 사용된 클레임 이름
	"username": "velopert"
	
};
```

```
const signature = {
	Hashing(Encoding(header) + "." + Encoding(payload), secretKey)
}
```

```
jwtToken = Encoding(header).Encoding(payload).signature
```