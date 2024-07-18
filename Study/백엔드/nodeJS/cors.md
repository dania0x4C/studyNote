[[aws연결]]

cross-Origin Resource Sharing

domain, protocol, port가 다른 server의 자원을 요청하는 매커니즘

Cors정책
client, server가 동일한 ip(server)에서 동작한다면 resource를 제약없이 공유 가능

만약 ip가 다르면 원칙적으로 data를 주고 받는 것이 불가능
이럴 경우 server에서 header에 Access-Control-Allow-Origin를 추가하여 response를 보냄
그럼 client는 ip에 대해 승인 했음을 확인하고 data를 server에서 받아 올 수 있게 됨


```
app.use(cors());

//모든 cross-origin 요청에 응답, 특정 요청에만 응답 -> 그의 따른 옵션 설정 필요

추후에 cors 옵션에 대해서 작성하기로 하겠다
```