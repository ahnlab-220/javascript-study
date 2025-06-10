### Chapter 43 - Ajax

### Ajax란?
Ajax(Asynchronous JavaScript and XML)는 자바스크립트를 사용하여 <font color='orange'>브라우저가 서버에게   
비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식</font>이다.  

Ajax는 브라우저에서 제공하는 Web API인 `XMLHttpRequest` 객체를 기반으로 동작한다.  

<br><br>

### JSON
JSON(JavaScript Object Notation)은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다.  
자바스크립트에 종속되지 않는 독립형 데이터 포멧으로, 대부분의 프로그래밍 언어에서 사용 가능하다.

<br>

#### JSON 표기법
자바스크립트의 객체 리터럴과 유사하게 키와 값으로 구성된 순수 텍스트이다.

``` json
{
    "name": "Lee",
    "age": 20,
    "alive": true,
    "hobby": ["traveling", "tennis"]
}
```

JSON의 키는 반드시 큰따옴표(작은 따옴표 사용 불가)로 묶어야 한다.  
값은 그대로 사용 가능하나, 문자열의 경우에는 큰따옴표로 묶어야 한다.

<br>

#### JSON.stringify
JSON.stringify 메서드는 객체를 JSON 문자열로 변환한다.  
> 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데 이를 직렬화(Serialization)라고 한다.

``` js
const obj = {
    name: 'Lee',
    age: 20,
    alive: true,
    hobby: ['traveling', 'tennis']
}

// obj 객체를 JSON 문자열로 변환
const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name":"Lee","age":20,"alive":true,"hobby":["traveling","tennis"]}

// 객체 원본은 변경되지 않는다.
console.log(obj);
// {name: 'Lee', age: 20, alive: true, hobby: Array(2)}
```

<br>

배열도 JSON 포맷의 문자열로 변환할 수 있다.

```js
const todo = [
    { id: 1, content: 'HTML', completed: true },
    { id: 2, content: 'CSS', completed: true },
    { id: 3, content: 'JavaScript', completed: false }
];

// todo 배열을 JSON 문자열로 변환
const json = JSON.stringify(todo);
console.log(typeof json, json);
// string [{"id":1,"content":"HTML","completed":true},{"id":2,"content":"CSS","completed":true},{"id":3,"content":"JavaScript","completed":false}]
```


<br>

#### JSON.parse
JSON.parse 메서드는 JSON 문자열을 객체로 변환한다.  
> 서버가 전송한 JSON 문자열을 객체로 변환하여 사용하려면 이를 역직렬화(Deserialization)해야 하는데 이를 JSON.parse 메서드로 가능하다.

``` js
const json = '{"name":"Lee","age":20,"alive":true,"hobby":["traveling","tennis"]}';

// JSON 문자열을 객체로 변환
const obj = JSON.parse(json);
console.log(typeof obj, obj);
// object {name: 'Lee', age: 20, alive: true, hobby: Array(2)}
```

<br>

배열이 JSON 포맷의 문자열로 변환되어 있는 경우, JSON.parse 메소드는 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.

``` js
const todos = [
    { id: 1, content: 'HTML', completed: true },
    { id: 2, content: 'CSS', completed: true },
    { id: 3, content: 'JavaScript', completed: false }
];

// todos 배열을 JSON 문자열로 변환
const json = JSON.stringify(todos);
console.log(typeof json, json);
// string [{"id":1,"content":"HTML","completed":true},{"id":2,"content":"CSS","completed":true},{"id":3,"content":"JavaScript","completed":false}]

// JSON 문자열을 객체로 변환
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
// object [{id: 1, content: 'HTML', completed: true}, {id: 2, content: 'CSS', completed: true}, {id: 3, content: 'JavaScript', completed: false}]
```

<br><br>


### XMLHttpRequest
자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용해야 한다.   
Web API인 `XMLHttpRequest` 객체는 HTTP 요청을 전송하는 역할을 한다.  

<br>

#### XMLHttpRequest 객체의 생성
`XMLHttpRequest` 객체를 생성하는 방법은 다음과 같다.

``` js
const xhr = new XMLHttpRequest();
```

<br>

#### XMLHttpRequest 객체의 메서드
대표적인 프로퍼티와 메소드는 아래와 같다.

| 프로토타입 프로퍼티 | 설명 |
|:---:|:---:|
|`readyState`|HTTP 요청의 현재 상태를 나타내는 정수 <br> 0: UNSENT, 1: OPENED, 2: HEADERS_RECEIVED, 3: LOADING, 4: DONE|
|`status`|HTTP 요청의 현재 상태를 나타내는 정수 (예: 200)|
|`statusText`|HTTP 요청에 대한 응답 메시지를 나타내는 문자열 (예: OK)|
|`responseType`|HTTP 응답 타입 (예: document, json, text, blob, arraybuffer)|
|`response`|서버의 응답 몸체(response body)를 반환|
|`responseText`|서버의 응답 몸체(response body)를 문자열로 반환|

<br>

##### XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티

| 이벤트 핸들러 프로퍼티 | 설명 |
|:---:|:---:|
|`onreadystatechange`|readyState 프로퍼티 값이 변경된 경우|
|`onloadstart`|HTTP 요청에 대한 응답을 받기 시작한 경우|
|`onprogress`|HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생|
|`onabort`|`abort` 메서드에 의해 HTTP 요청이 중단된 경우|
|`onerror`|HTTP 요청에 에러가 발생한 경우|
|`onload`|HTTP 요청이 성공적으로 완료된 경우|
|`ontimeout`|HTTP 요청이 초과한 경우|
|`onloadend`|HTTP 요청이 완료된 경우, HTTP 요청 성공 또는 실패 시 발생|

<br>

##### XMLHttpRequest 객체의 메서드

| 메서드 | 설명 |
|:---:|:---:|
|`open`|HTTP 요청 초기화|
|`send`|HTTP 요청 전송|
|`abort`|HTTP 요청 중단|
|`setRequestHeader`|특정 HTTP 요청 헤더 설정|
|`getResponseHeader`|특정 HTTP 응답 헤더 조회|

<br>

##### XMLHttpRequest 객체의 정적 프로퍼티

| 정적 프로퍼티 | 값 | 설명 |
|:---:|:---:|:---:|
|`UNSENT`|0|HTTP 요청을 초기화하기 전 (open 메서드 호출 전)|
|`OPENED`|1|open 메서드 호출 후 (open 메서드 호출 후)|
|`HEADERS_RECEIVED`|2|send 메서드 호출 후, 서버가 응답 헤더를 받은 후 (send 메서드 호출 후)|
|`LOADING`|3|서버가 응답 몸체를 받는 중 (send 메서드 호출 후, 응답 데이터 미완성 상태₩)|
|`DONE`|4|응답 완료|

<br>

#### HTTP 요청 전송
HTTP 요청을 전송하는 경우 아래의 순서를 따른다.

1) XMLHttpRequest.prototype.open 메서드로 HTTP 요청을 초기화한다.
2) 필요에 따라 XMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청 헤더를 설정한다.
3) XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송한다.

``` js
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

// 특정 HTTP 요청 헤더를 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입을 json으로 지정
xhr.setRequestHeader('Content-type', 'application/json');

// HTTP 요청 전송
xhr.send();
```

<br>

여기서 주의할 점은 클라이언트가 서버로 전송할 데이터의 MIME 타입을 지정해야 한다.  
이를 content-type 헤더 필드에 지정한다.  

|MIME 타입|서브 타입|
|:---:|:---:|
|`text`| `text/plain`, `text/html`, `text/css`, `text/javascript`|
|`application`| `application/json`, `application/x-www-form-urlencoded`|
|`multipart`| `multipart/formed-data`|

<br>

반대로 서버가 응답할 데이터의 MIME 타입을 Accept 헤더 필드로 지정할 수 있다.

``` js
// 서버가 응답할 데이터의 MIME 타입을 json으로 지정
xhr.setRequestHeader('Accept', 'application/json');
```

<br>

#### HTTP 요청 응답 처리

서버가 전송한 응답을 처리하려면 XMLHttpRequset가 발생시키는 이벤트를 캐치해야 한다.  
XMLHttpRequest 객체는 onreadystatechange, onload, onerror와 같은 이벤트 핸들러 프로퍼티를 갖는데,  
이 이벤트 핸들러 프로퍼티 중에서 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티 값이 변경된 경우 발생하는 `readystatechange` 이벤트를 캐치해야 한다.

``` js
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

// HTTP 요청 전송
xhr.send();

// HTTP 요청 응답 처리
// readystatechange 이벤트는 HTTP 요청의 현재 상태가 변경될 때마다 발생한다.
xhr.onreadystatechange = () => {
    // 서버 응답이 완료되지 않은 경우 아무런 처리를 하지 않음
    if (xhr.readyState !== XMLHttpRequest.DONE) return;

    // 서버 응답이 완료된 경우
    // status 프로퍼티 값을 확인하여 정상 처리 여부를 확인
    if (xhr.status === 200) {
        console.log(JSON.parse(xhr.response));
    } else {
        console.log('Error', xhr.status, xhr.statusText);
    }
}
```

<br>


`readystatechange` 이벤트 대신 `load` 이벤트를 캐치해도 좋다.  
`load` 이벤트는 HTTP 요청이 성공적으로 완료된 경우에 발생한다.

따라서 `load` 이벤트를 캐치한 경우 xhr.readyState 프로퍼티 값이 XMLHttpRequest.DONE인지 확인할 필요가 없다.

``` js
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

// HTTP 요청 전송
xhr.send();

// HTTP 요청 응답 처리
// load 이벤트는 HTTP 요청이 성공적으로 완료된 경우에 발생한다.
xhr.onload = () => {
    if (xhr.status === 200) {
        console.log(JSON.parse(xhr.response));
    } else {
        console.log('Error', xhr.status, xhr.statusText);
    }
}
```

<br>

끝.