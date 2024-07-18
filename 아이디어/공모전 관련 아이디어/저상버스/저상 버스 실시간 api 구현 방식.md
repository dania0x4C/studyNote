
1. 카카오 맵 자체 코드를 사용 - 정류장 위치에 버스 마커 설정
2. 서울시 버스 도착 정보 공공데이터 활용
3. 원하는 버스가 있는 정류장의 위치, 남은 시간 알려주기


	실시간 데이터 업데이트 반영 방법
	
1. **지도 라이브러리 선택:** [Open Layers](https://marketingaltimetrik.medium.com/real-time-tracking-using-node-js-websockets-redis-and-open-layers-41949d7c979c) 또는 [Google Maps JavaScript API](https://developers.google.com/maps/documentation/javascript/directions) 와 같은 지도 라이브러리를 활용하여 지도를 화면에 표시합니다. 이러한 라이브러리는 지리 데이터를 처리하기 위한 기능을 제공합니다.
    
2. **버스 위치 API와 통합:** 버스 위치 API를 사용하여 실시간 위치 데이터를 가져옵니다. API로부터 지속적으로 업데이트를 수신하려면 HTTP 요청 또는 WebSocket 연결을 구현하세요.
    
3. **실시간 통신을 위한 WebSocket:** [Socket.IO를](https://blog.logrocket.com/building-real-time-location-app-node-js-socket-io/) 활용하여 서버(Node.js)와 클라이언트(브라우저) 간에 WebSocket 연결을 설정합니다. 이를 통해 실시간 통신이 가능하고 지도에서 버스 위치를 업데이트할 수 있습니다.
    
4. **서버 측 구현:** Node.js 서버에서 버스 위치 API로부터 들어오는 데이터를 처리하고 Socket.IO를 사용하여 연결된 클라이언트에 업데이트를 내보냅니다.
    
5. **클라이언트 측 구현:** 클라이언트 측에서는 WebSocket 연결을 통해 업데이트를 수신하고 이에 따라 맵을 업데이트합니다.


web socket io 활용예정