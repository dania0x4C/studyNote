app.get()
```
// routes/index.js

const express = require('express');

const router = express.Router();

router.get('/in', (req, res) => {
    res.send('Hello, World !');
});

module.exports = router;

// routes/user.js

const express = require('express');

const router = express.Router();

router.get('/iam', (req, res) => {
    res.send('Hello, User');
});

module.exports = router;
```

```
const express = require('express');

const app = express();

const indexRouter = require('./routes');// routes/index.js 불러오기
const userRouter = require('./routes/user');// routes/user.js 불러오기

app.set('port', process.env.PORT || 3000); // portNum 선언

// localhost:3000/
// localhost:3000/user

app.use('/', indexRouter); // app에 /로 indexRouter 미들웨어 장착
app.use('/user', userRouter); // app에 /user로 userRouter 미들웨어 장착

app.use((req, res, next) => { // 기본경로나 /user말고 다른곳 진입했을경우 실행
    res.status(404).send('Not Found');
});

app.listen(app.get('port'), () => {
   console.log(app.get('port'), '번 포트에서 대기 중');
});

// localhost:3000/in -> index.js에서 get에 접근
// localhost:3000/user/iam -> user.js에서 get에 접근

```

[[기초 정리]]