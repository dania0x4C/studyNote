
- **res.app**: 똑같이 res 객체를 통해 app 객체에 접근한다. **res.app.get('')**같이 사용 가능.
- **res.set(헤더, 값) / **res.setHeader(헤더, 값)****: **응답의 헤더를 설정**한다. req.get()이 헤더값을 가져오는거라면 이건 헤더 설정
- **res.status(코드) / res.sendStatus(코드)**: 응답 시의 HTTP 상태 코드를 지정한다.
- **res.type(type)** : Contents-Type 헤더를 설정할 수 있는 간단한 메서드.
- **res.cookie(키, 값, 옵션)**: 쿠키를 응답에 설정하는 메서드이다. (cookie-parser 패키지가 필요)
- **res.clearCookie(키, 값, 옵션)**: 쿠키를 응답에서 제거하는 메서드이다.  
      
    ==주의사항 - 각 응답은 페이지 당 하나만 가능==
- **res.end()**: **데이터 없이 응답**을 보낸다.
- **res.json(JSON)**: JSON 형식의 응답을 보낸다.
- **res.redirect(주소)**: 리다이렉트할 주소와 함께 응답을 보낸다.
- **res.locals / res.render(뷰, 데이터)**:   
    res.locals는 뷰를 렌더링하는 기본 콘텍스트를 포함하는 객체다. res.render는 jade와 같은 템플릿 엔진을 사용하여 뷰를 렌더링한다
    
	**아래 두 개는 사용법이 잘 그려지지 않음 추후에 사용해보자**
- **res.send(body), res.send(status, body)** : 클라이언트에 응답을 보냄. 상태 코드는 옵션.   
    기본 콘텐츠 타입은 text/html이므로 text/plain을 보내려면 **res.set(‘Content-Type’, ‘text/plain’)**을 먼저 호출 해야한다. 
- **res.sendFile(경로)**: 경로에 위치한 파일을 응답한다.  
      
    
- **res.attachment([filename]), res.download(path, [filename], [callback])** : 클라이언트에게 파일을 표시하지 말고 다운로드 받으라고 전송함. filename을 주면 파일 이름이 명시되며, res.attachment는 헤더만 설정하므로 다운로드를 위한 node 코드가 따로 필요하다.

[[기초 정리]]