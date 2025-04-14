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

https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS

### 1. **Access-Control-Allow-Origin**

- **역할**: 어떤 출처(origin)에서 리소스에 접근할 수 있는지를 명시.
- **값**:
    - `*` : 모든 도메인 허용.
    - 특정 도메인: 예, `https://example.com`.
    - **주의**: `*`은 자격 증명(쿠키, 인증 헤더 등)과 함께 사용 불가.

---

### 2. **Access-Control-Allow-Methods**

- **역할**: 클라이언트가 리소스에 접근할 때 허용되는 HTTP 메서드를 명시.
- **값**:
    - `GET, POST, PUT, DELETE, OPTIONS`, 등.
    - 예: `Access-Control-Allow-Methods: GET, POST`.

---

### 3. **Access-Control-Allow-Headers**

- **역할**: 클라이언트가 요청 시 사용할 수 있는 커스텀 헤더 또는 표준 헤더를 명시.
- **값**:
    - 허용하려는 헤더 이름.
    - 예: `Content-Type, Authorization, X-Custom-Header`.

---

### 4. **Access-Control-Allow-Credentials**

- **역할**: 클라이언트가 자격 증명(쿠키, 인증 헤더 등)을 포함한 요청을 보낼 수 있는지 여부를 명시.
- **값**:
    - `true`: 자격 증명 허용.
    - **주의**: `Access-Control-Allow-Origin`에 `*` 값을 사용할 경우 이 헤더를 설정할 수 없음.

---

### 5. **Access-Control-Max-Age**

- **역할**: 브라우저가 사전 요청(preflight request) 결과를 캐싱할 수 있는 시간(초 단위)을 명시.
- **값**:
    - 정수 값(초).
    - 예: `Access-Control-Max-Age: 3600` (1시간).

---
