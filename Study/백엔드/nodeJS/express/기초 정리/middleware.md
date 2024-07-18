
middleware는 함수의 연속이다
middleware추가하는 방법 - use를 사용

```
function logger(req, res , next){// logger이라는 함수를 만들어서 use로 추가한다
	console.log('I am logger'); 
	// middleware는 자기 할 일을 하다면 next함수를 호출 해야한다
	next();
}

function logger2(req, res , next){
	console.log('I am logger2'); 
	next();
}

app.use(logger)// 미들웨어를 추가할 때는 use함수를 사용한다
app.use(logger2)// 추가를 해주어도 next()가 없다면 다음 함수를 불러오지 않음
```

다른 파일의 middleware 받기(npm i morgan)

```
const morgan = require('morgan')

app.use(morgan('dev'))
```

error middleware -> 다시 알아보기

```
function commonmw(req, res, next){
	console.log('commonmv');
	next(new Error('error ouccered'));
}

function errormw(err, req, res , next){
	console.log('err.message');
	// 에러 출력, 결과처리
	next();
	//error를 처리하지 못 하고 넘겨주고 싶다면
	//next(err)
}

```
[[기초 정리]]