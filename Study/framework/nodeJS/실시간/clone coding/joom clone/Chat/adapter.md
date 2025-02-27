[[webSocket]]
1. 기본적으로 database에 저장하지 않으면 연결이 끊기면 데이터가 사라짐
2. 그리고 다른 server간의 data가 실시간의 전송되지 않음
3. 그래서 adapter를 이용해서 다른 서버 간의 정보를 실시간으로 반영해줌

코드 이해 해보자
```
function publicRooms() {

  const {

    socket: {

      adapter: { sids, rooms },

    },

  } = wsServer;

  const publicRooms = [];

  rooms.forEach((_, key) => {

    if (sids.get(key) === undefined) {

      publicRooms.push(key);

    }

  });

  return publicRooms;

}
```

이 코드는 publicRoom을 찾아주는 코드 privateRoom이 있는지 없는지 확인

```
socket.on("room_change", room => {

  const roomList = welcome.querySelector("ul");

  room.innerHTML = "";

  if (roomList.length === 0) {

    return;

  }
   room.foreach(room => {

    const li = document.createElement("li");

    li.innerText = room;

    roomList.append(li);

  });

});

```
이거는 방이 생기고 사라질 때 상태를 현재 방 유무 알려주는 코드
  

 방에서 private와 public이 만들어지는 차이점을 이해하지 못 함


그리고 nickname 저장시 오류 발생 왜 그런지 코드 확인 필요
