```
const express = require('express');
const app = express();// app은 express instance(독립적 객체)이고 application이라고 함

app.listen(portNum, function(){
	console.log("server is running")
})
```


app.use를 이용해서 라우팅을 할 수 있음
```
app.use('/', indexRouter);
```


[[기초 정리]]