[[adapter]]
http

1.
user - request
server - response

두 가지 반응으로 작동

2.

stateless (response이후 backend가 기억X)

오직 request에 대한 response 반응만 해줌

webSocket(npm i ws)

1. Connection
handShake와 비슷하게 client가 request시 accept or fail을 sever에서 보낸다

2. 실시간 양방향 연결


Socket - front와 back이 소통 가능하게 하는 역할

back - 연결된 브라우저
front - 서버로의 연결

socket.on - eventListener -> callback 함수를 불러옴

server
	1. connect - message에 대한 정보를 기록함
	2. close - message에 대한 정보를 기록하지 않음
	3. message - message를 보냄?

app
	1. open - sever와 연결
	2. message - message를 보냄??
	3. close - sever disconnection

1.  user에게 input을 받으면 어떤 목적의 input 인지 알 수 없음
2.  그래서 json형식으로 용도에 맞게 data를 기록해둠
3. 그런데 서버 측에서 받을 때 json형식을 사용하기 보다는 원래 형태에서 사용하는 것이 오류 문제가 되지 않기 때문에 parsing 하는 과정이 필요

user가 무엇을 입력했을 때 server로 받아서 보이게 하는 것과 front에서 보이게 하는 방법이 있는데 그럴 때 나를 제외한 나머지에게 data를 주는 방식을 알아야 할 필요도 있다

backend에서 room의 의미 - sever에서 특정 인원만이 소통이 가능한 공간이 필요함


