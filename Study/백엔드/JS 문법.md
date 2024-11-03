
# ES6

### 변수, 상수 설정

### var

- var의 경우에는 함수 외부에 선언 하면 전역 변수, 내부에 선언하면 지역 변수가 된다.
- 호이스팅으로 인해 변수 선언을 마지막에 해도 사용할 수 있는 변수가 된다. 
- 호이스팅이란 변수 선언이 코드 상단으로 올려지는 것을 말한다.
- * 주의점* 호이스팅은 변수 생성 만 호이스팅 되며 초기화 되기 때문에 변수에 값이 선언 되지 않은 상태로 사용하면 undefined가 반환이 된다.

### let

- let의 경우에는 변수 선언 범위가 {}로 바인딩 되어있다.
- let은 같은 범위 {}안에서는 재선언이 불가능하다.
- let의 경우 선언 이전에 사용하게 된다면 컴파일 오류가 생겨서 사용이 불가능 하다.

### const

- const는 변수의 범위가 let과 동일하다.
- const는 변수의 값 선언을 다시 할 수 없기 때문에 선언과 동시에 초기화 시켜야 된다.
- const는 개체의 경우에는 업데이트가 가능하다
- 호이스팅의 경우에 let과 동일하다.
```
const car = {
	name: "cs",
	color: "red",
}
car을 const로 선언했다.

car = {
	price:"1w",
	color:"red",
}
car 내부의 개체는 변경이 불가능 하다.
하지만 아래와 값이 car에 들어있는 개체의 속성 값은 변경이 가능하다.

car.name = "pro"
```

### 동등 연산자, 일치 연산자

```
==는 동등 연산자
===는 일치 연산자

동등 연산자는 두 개의 타입을 정리할때 타입을 자동으로 변환 시켜준다
그렇기 때문에 의도하지 않은 결과가 발생 할 수 있다.

일치 연산자의 경우 좌우 정리를 할때 타입까지 확인하는 작업을 한다. 
```
## null, undefined

```
typeof null // 'object'
typeof undefined // 'undefined'
null === undefined // false
null == undefined // true
null === null // true
null == null // true
!null // true
isNaN(1 + null) // false
isNaN(1 + undefined) // true
```
## 함수 선언식, 함수 표현식

```
// 이 방식을 함수 선언식이라고 합니다. 
function helloWorld () { 
	let he = "jihwan";
	console.log(he);
} 

// 이 방식은 함수 표현식이라고 해요! 
const helloWorld = function () { 
	let he = "jihwan";
	console.log(he);
}
```

### 탬플릿 리터럴

```
템플릿 리터럴(template literal) - 템플릿 리터럴 문법은 문자열 사이에 변수를 넣을 때 사용했던 기존 방식보다 훨씬 편하게 변수를 삽입할 수 있게 도와주는 문법입니다. 주의할 점은 이때 문자열을 쓸 때 기존에 쓰던 따옴표 (”” 또는 ‘’)가 아니라 백틱 (``)을 사용한다는 점 눈여겨 봐주세요! 그리고 변수는 ${변수} 형식으로 써주셔야 합니다!
```

```
// 템플릿 리터럴 console.log(`나도 ${umc}가 좋은 동아리라는 의견에 동의해!`);
```

### 화살표 함수

```
const arrFunc = () => {
	const name = "jungmin"
}
```


### Spread 연산자

```
const arr1 = [1, 2, 3]; 
// arr1 안에 있는 걸 꺼내서 새로운 배열에 넣기 

const arr2 = [...arr1, 4, 5]; 
console.log(arr2); // [1, 2, 3, 4, 5]
```
배열이나 객체에 있는 값 전체를 새로운 객체 내부로 넣는 것이다.
### Rest 연산자

```
const obj = { name: 'jihwan', age: 25, city: 'New York' };
const { name, ...rest } = obj;

// name을 제외한 나머지 속성들을 모아서 rest라는 객체로 만들기
console.log(name);// 'Alice' 
console.log(rest);// { age: 25, city: 'New York' }
```
객체나 배열에 선언된 값을 가지고 새로운 객체를 하나 만들어 주는 것이다.

### class, interface
관련 매소드 뭐 있는지 찾아보기


https://github.com/SMUMC-7th/7th-Node

