- **req.app**: req 객체를 통한 app 객체로의 접근이다. 예를 들어 **req.app.get('port')**와 같은 식으로 사용할 수 있다.
- **req.cookies**: cookie-parser 미들웨어가 만드는 요청의 쿠키를 해석한 객체이다.
- **req.signedCookies**: **서명된 쿠키**들은 req.cookies 대신 여기에 담긴다.
- **req.get(헤더 이름)**: 헤더의 값을 가져온다. **req.get('Content-type')  
    **
- **req.body**: body-parser 미들웨어가 만드는 요청의 본문을 해석한 객체이다. POST 방식으로 넘어오는 데이터를 담는다.
- **req.params**: **라우트 매개변수**에 대한 정보가 담긴다.
- **req.query**: GET방식으로 넘어오는 데이터, **쿼리스트링**의 정보가 담긴다.  
      
    
- **req.route** : 현재 라우트에 관한 정보. 디버깅용.
- **req.headers** : HTTP의 Header 정보를 가지고 있다.
- **req.accepts([types])** : 클라이언트가 해당하는 타입을 받을 수 있는지 확인하는 간단한 메서드.
- **req.ip**: 요청의 ip 주소를 담는다.
- **req.path** : 클라이언트가 요청한 경로. 프로토콜, 호스트, 포트, 쿼리스트링을 제외한 순수 요청 경로다.
- **req.host** : 요청 호스트 이름을 반환하는 간단한 메서드. 조작될 수 있으므로 보안 목적으로는 사용하면 안된다.
- **req.xhr** : 요청이 ajax 호출로 시작되었다면 true를 반환하는 프로퍼티
- **req.protocol** : 현재 요청의 프로토콜 (http, https 등)
- **req.secure** : 현재 요청이 보안 요청(SSL?) 이면 true를 반환
- **req.url (req.originalUrl)** : URL 경로와 쿼리 스트링을 반환. 원본 요청을 logging하는 목적으로 많이 쓰임.
- **req.acceptedLanguages** : 클라이언트가 선호하는 자연어 목록을 반환. header에서 파싱하면 다국어를 지원한는 어플리케이션이라면 초기 언어 선택에 도움을 줄 수 있음.
[[기초 정리]]