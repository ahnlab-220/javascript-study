### Chapter 45 - 프로피스

<font color='orange'>자바스크립트는 비동기 처리를 위한 하나의 패턴으로 콜백 함수를 사용한다.</font>

하지만 전통적인 콜백 패턴은 콜백 지옥으로 인해 가독성이 나쁘고  
비동기 처리 중 발생한 에러의 처리가 곤란하며 여러 개의 비동기 처리르 한번에 처리하는 데도 한계가 있다.

ES6에서는 비동기 처리를 위한 또 다른 패턴으로 프로미스(Promise)를 도입했다.  
<font color='orange'>프로미스는 전통적인 콜백 패턴이 가진 단점을 보완혀며 비동기 처리 시점을 명확하게 표현할 수 있다는 장점이 있다.</font>

<br><br>

### 비동기 처리를 위한 콜백 패턴의 단점
#### 콜백 헬

GET 요청을 위한 함수를 작성해보자.

``` js
// GET 요청을 위한 비동기 함수
const get = url => {
    // 1. XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();
    
    // 2. HTTP 요청 초기화
    xhr.open('GET', url);

    // 3. HTTP 요청 전송
    xhr.send();

    // 4. HTTP 응답 처리
    xhr.onload = () => {
        if (xhr.status === 200) {
            console.log(JSON.parse(xhr.response));
        } else {
            const.error(`${xhr.status} ${xhr.statusText}`);
        };
    };
};

// id가 1인 post를 취득
get('https://jsonplaceholder.typicode.com/todos/1');
```

<br>

위 예제의 get 함수는 서버의 응답 결과를 콘솔에 출력한다.  
get 함수가 서버의 응답 결과를 반환하게 하려면 어떻게 해야 할까?

<font color='orange'>비동기 함수란, 함수 내부에 비동기로 동작하는 코드를 포함한 함수를 의미한다.</font>  
비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않았다 해도 기다리지 않고 즉시 종료한다.  
즉, 비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료된다.  

따라서 비동기 함수 내부의 비동기로 동작하는 코드에서  
처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.

<br>

get 함수도 비동기 함수이다.  
get 함수가 비동기 함수인 이유는 get 함수 내부의 `onload` 이벤트 핸들러가 비동기로 동작하기 때문이다.  

get 함수를 호출하면 GET 요청을 전송하고 onload 이벤트 핸들러를 등록한 다음 `undefined`를 반환하고 즉시 종료된다.  

즉, 비동기 함수인 `onload` 이벤트 핸들러는 get 함수가 종료된 이후에 실행된다.  
<font color='orange'>따라서 get 함수의 `onload` 이벤트 핸들러에서 서버의 응답 결과를 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.</font>

<br>

`get` 함수가 서버의 응답 결과를 반환하도록 수정해보자.

``` js
const get = url => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();

    xhr.onload = () => {
        if (xhr.status === 200) {
            // 서버의 응답을 반환한다.
            return JSON.parse(xhr.response);
        } else {
            const.error(`${xhr.status} ${xhr.statusText}`);
        };
    };
};

// id가 1인 post를 취득
const response = get('https://jsonplaceholder.typicode.com/todos/1');
console.log(response);  // undefined
```

<br>

xhr.onload 이벤트 핸들러 프로퍼티에 바인딩한 이벤트 핸들러의 반환문은 get 함수의 반환문이 아니다.  
get 함수는 반환문이 생략되었으므로 암묵적으로 undefined를 반환한다.  

`onload` 이벤트 핸들러는 get 함수가 호출하지 않기 때문에 암묵적으로 `undefined`를 반환한다.  

<font color='tomato'>따라서 onload 이벤트 핸들러의 반환값은 비동기 함수인 get 함수에서 참조할 수 없다.</font>

> 이벤트 핸들러에서의 반환(return)은 의미가 없다.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <input type="text">
    <script>
        document.querySelector('input').oninput = function() {
            console.log(this.value);

            // 이벤트 핸들러에서의 반환(return)은 의미가 없다.
            return this.value;
        }
    </script>
</body>
</html>
```

<br>

서버의 응답을 상위 스코프의 변수에 할당하면 어떻게 될까?

``` js
let todos;

// GET 요청을 위한 비동기 함수
const get = url => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();

    xhr.onload = () => {
        if (xhr.status === 200) {
            // 서버의 응답을 상위 스코프의 변수에 할당한다
            todos = JSON.parse(xhr.response);
        } else {
            const.error(`${xhr.status} ${xhr.statusText}`);
        };
    };
};

// id가 1인 post를 취득
get('https://jsonplaceholder.typicode.com/todos/1');
console.log(todos);  // undefined
```

이것 또한 기대한 대로 결과가 나오지 않는다.  
`xhr.onload` 이벤트 핸들러 프로퍼티에 바인딩한 이벤트 핸들러는  
언제나 `console.log`가 종료한 이후에 호출된다.  

따라서 `console.log`가 호출되는 시점에서는 아직 전역 변수 `todos`에 서버의 응답 결과가 할당되지 않았기 때문에 `undefined`가 출력된다.  

<br>

> 비동기 함수는 비동기 처리 결과를 외부에 반환할 수 없고, 상위 스코프의 변수에 할당할 수도 없다.  
> 따라서 <font color='orange'>비동기 함수의 처리 결과(서버 응답 등)에 대한 후속 처리는  
> 반드시 비동기 함수 내부에서 수행되어야 한다.</font>  
> 이때 비동기 함수에 비동기 처리 결과에 대한 후속 처리를 수행하는 콜백 함수를 전달하는 것이 일반적이다.
> (콜백 함수는 비동기 함수가 종료된 이후에 반드시 호출된다는 특성을 이용한 것이다)

``` js
// GET 요청을 위한 비동기 함수
const get = (url, successCallback, failureCallback) => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();

    xhr.onload = () => {
        if (xhr.status === 200) {
            // 서버의 응답을 콜백 함수에 전달하면서 호출하여 응답에 대한 후속 처리 수행
            successCallback(JSON.parse(xhr.response));
        } else {
            // 에러 정보를 콜백 함수의 인수에 전달하면서 호출하여 에러 처리
            failureCallback(`${xhr.status} ${xhr.statusText}`);
        };
    };
};

// id가 1인 post를 취득
get('https://jsonplaceholder.typicode.com/todos/1', console.log, console.error);
```


<br><br>

#### 에러 처리의 한계
비동기 처리를 위한 콜백 패턴의 문제점 중 가장 심각한 것이 바로 에러 처리이다.

``` js
try { 
    setTimeout(() => { throw new Error('Error!');}, 1000);
} catch (error) {
    // 에러를 캐치하지 못함
    console.error(error);
}
```

`try`코드 블록 내에서 호출한 `setTimeout` 함수는 1초 후에 콜백 함수가 실행되도록 타이머를 설정하고, 이후 콜백 함수는 에러를 발생시킨다.  
하지만 이 에러는 `catch` 코드 블록에서 캐치되지 않는다.  

비동기 함수인 `setTimeout`이 호출되면 `setTimeout` 함수의 실행 컨텍스트가 생성되어 콜 스택에 푸쉬되어 실행된다.   
`setTimeout`은 비동기 함수이므로 콜백 함수가 호출되는 것을 기다리지 않고 즉시 종료되어 콜 스택에서 제거된다.  

이후 타이머가 만료되면 `setTimeout` 함수의 콜백 함수는 테스크 큐로 Push되고 콜 스택이 비어졌을 때 이벤트 루프에 의해 콜 스택으로 Push되어 실행된다.  

`setTimeout` 함수의 콜백 함수가 실행될 때, `setTimeout` 함수는 이미 콜 스택에서 제거된 상태이다.  

이것은 `setTimeout` 함수의 콜백 함수를 호출한 것이 `setTimeout` 함수가 아니라는 것을 의미한다.  

<br><br>

> **테스크 큐(Task Queue)**  
> 비동기 함수의 콜백 함수 또는 이벤트 핸들러가 일시적으로 보관되는 영역

> **이벤트 루프(Event Loop)**  
> 콜 스택에 현재 실행 중인 실행 컨텍스트가 있는지, 테스크 큐에 대기중인 함수(콜백 함수, 이벤트 핸들러 등)가 있는지 반복해서 확인한다.


<br>

에러는 호출자(Caller) 방향으로 전파된다.
> 콜 스택의 아래 방향(실행 중인 실행 컨텍스트가 Push되기 직전에 Push된 실행 컨텍스트 방향)으로 전파된다.

<br>

하지만 `setTimeout` 함수의 콜백 함수를 호출한 것은 `setTimeout` 함수가 아니므로  
`setTimeout` 함수의 콜백 함수가 발생시킨 에러는 `catch` 블록에서 catch되지 않는다.


<br>

이를 극복하기 위해 ES6에서 프로미스(Promise)가 도입되었다.

<br><br>

### 프로미스의 생성
`Promise` 생성자 함수를 `new` 연산자와 함께 호출하면 프로미스(Promise) 객체를 생성한다.

`Promise` 생성자 함수는 비동기 처리를 수행하는 콜백함수를 인수로 전달받는데  
이 콜백 함수는 `resolve`와 `reject` 함수를 인수로 전달받는다.

<br>

``` js
// 프로미스 생성
const promise = new Promise((resolve, reject) => {
    // Promise 함수의 콜백 함수 내부에서 비동기 처리를 수행한다.
    if (/* 비동기 처리 성공 */) {
        resolve(/* 비동기 처리 결과 */);
    } else {
        reject(/* 에러 메시지(정보) */);
    }
});
```

<br>

`Promise` 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 비동기 처리를 수행한다.  
이때 비동기 처리가 성공하면 `resolve` 함수를 호출하고 실패하면 `reject` 함수를 호출한다.  

<br>

``` js
// GET 요청을 위한 비동기 함수
const promiseGet = url => {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open('GET', url);
        xhr.send();

        xhr.onload = () => {
            if (xhr.status === 200) {
                resolve(JSON.parse(xhr.response));
            } else {
                reject(new Error(xhr.status));
            }
        };
    });
};

// id가 1인 post를 취득
promiseGet('https://jsonplaceholder.typicode.com/todos/1');
```

<br>

비동기 함수인 `promiseGet`은 함수 내부에서 프로미스를 생성하고 반환한다.  
비동기 처리는 `Promise` 생성자 함수의 인수로 전달받은 콜백 함수 내부에서 수행한다.  

만약 비동기 처리가 성공하면 비동기 처리 결과를 `resolve` 함수에 인수로 전달하면서 호출하고,  
비동기 처리가 실패하면 에러를 `reject` 함수에 인수로 전달하면서 호출한다.

<br>

프로미스는 다음과 같이 현재 비동기 처리가 어떻게 진행되고 있는지를 나타내는 상태(state) 정보를 갖는다.

|상태 정보|의미|상태 변경 조건|
|:---:|:---:|:---:|
|pending|비동기 처리 진행중|`resolve` 또는 `reject` 함수가 호출되기 전|
|fulfilled|비동기 처리 성공|`resolve` 함수가 호출된 상태|
|rejected|비동기 처리 실패|`reject` 함수가 호출된 상태|

<br>

`fullfilled` 또는 `rejected` 상태를 `settled` 상태라고 부른다.
> `settled` 상태는 비동기 처리가 수행된 상태를 의미한다.

<br>

즉,비동기 처리가 진행 중이라면 `pending` 상태이고, 비동기 처리가 수행되었다면 `settled` 상태(fulfilled 또는 rejected)가 된다.

<br>

<font color='tomato'>프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체이다.</font>

<br><br>

### 프로미스의 후속 처리 메소드
프로미스의 비동기 처리 상태가 변하면 이에 따른 후속 처리를 해야 한다.  
- 프로미스가 fulfilled 상태가 되면 프로미스의 처리 결과를 가지고 무언가를 해야한다.
- 프로미스가 rejected 상태가 되면 프로미스의 처리 결과를 가지고 무언가를 해야한다.

<font color='orange'>이를 위해 프로미스는 후속 메소드 `then`, `catch`, `finally`를 제공한다.</font>

<br>

#### Promise.prototype.then
`then` 메소드는 2개의 콜백 함수를 인수로 전달받는다.

- 첫 번째 콜백 함수는 프로미스가 fulfilled 상태가 되면 호출한다. 
    이때 콜백함수는 프로미스의 비동기 처리 결과를 인수로 전달받는다.
- 두 번째 콜백 함수는 프로미스가 rejected 상태가 되면 호출한다.
    이때 콜백함수는 프로미스의 에러를 인수로 전달받는다.

<br>

``` js
// fulfilled
new Promise(resolve => resolve('fulfilled'))
    .then(v => console.log(v), e => console.error(e));

// rejected
new Promise((_, reject) => reject(new Error('rejected')))
    .then(v => console.log(v), e => console.error(e));
```

`then` 메소드는 언제나 프로미스를 반환한다.  
만약 `then` 메소드의 콜백 함수가 프로미스를 반환하면 그 프로미스를 그대로 반환하고,  
콜백 함수가 프로미스가 아닌 값을 반환하면 그 값을 암묵적으로 `resolve` 또는 `reject` 하여 프로미스를 생성해 반환한다.


<br><br>

#### Promise.prototype.catch
`catch` 메소드는 한 개의 콜백 함수를 인수로 전달받는다.  

- 콜백 함수는 프로미스가 rejected 상태인 경우만 호출한다.
    이때 콜백함수는 프로미스의 에러를 인수로 전달받는다.

<br>

``` js
new Promise((_, reject) => reject(new Error('rejected')))
    .catch(e => console.error(e));
```

<br>

`catch` 메소드는 `then`과 마찬가지로 언제나 프로미스를 반환한다.  


<br><br>

#### Promise.prototype.finally
`finally` 메소드는 한 개의 콜백 함수를 인수로 전달받는다.  
`finally` 메소드는 프로미스의 성공(fulfilled) 또는 실패(rejected)와 상관없이 무조건 한 번 호출된다.  

`finally` 메소드는 프로미스의 상태와 상관없이 공통적으로 수행해야 할 처리 내용이 있을 때 유용하다.


<br>

``` js
new Promise(() => {})
    .finally(() => console.log('finally'));
```

<br><br>

프로미스로 구현한 비동기 함수 `get`을 사용해 후속 처리를 구현해보자.

``` js
const promiseGet = url => {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open('GET', url);
        xhr.send();

        xhr.onload = () => {
            if (xhr.status === 200) {
                resolve(JSON.parse(xhr.response));
            } else {
                reject(new Error(xhr.status));
            }
        };
    });
};

// promiseGet 함수는 프로미스를 반환한다.
promiseGet('https://jsonplaceholder.typicode.com/todos/1')
    .then(res => console.log(res))
    .catch(err => console.error(err))
    .finally(() => console.log('end'));
```


<br><br>


### 프로미스 체이닝

``` js
const url = 'https://jsonplaceholder.typicode.com';

promiseGet(`${url}/todos/1`)
    .then(({ userId }) => promiseGet(`${url}/users/${userId}`))
    .then(user => console.log(user))
    .catch(err => console.error(err));
```

위 예제에서 `then` → `then` → `catch` 순서로 후속 처리 메소드를 호출했다.   
`then`, `catch`, `finally` 후속 처리 메소드는 언제나 프로미스를 반환하므로 연속적으로 호출 가능하다.
> 이를 프로미스 체이닝이라 한다.

<br>

만약 프로미스가 아닌 값을 반환하도록 코드를 작성해도 그 값을 암묵적으로 `resolve` 또는 `reject` 하여 프로미스를 생성해 반환한다.  

<br>

다만 프로미스도 콜백 패턴을 사용하므로 콜백 함수를 완전히 사용하지 않는 것은 아니다.  
콜백 패턴은 가독성이 좋지 않다. 이 문제는 ES8에서 도입된 `async/await`로 개선할 수 있다.

<br>

`await`를 사용하면 프로미스의 후속처리 메소드 없이  
마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다.

``` js
const url = 'https://jsonplaceholder.typicode.com';

(async () => {
    // id가 1인 post의 userId를 취득
    const { userId } = await promiseGet(`${url}/posts/1`);

    // 취득한 post의 userId로 user 정보를 취득
    const userInfo = await promiseGet(`${url}/users/${userId}`);

    console.log(userInfo);
})();

```

`async/await`도 프로미스를 기반으로 동작하므로 프로미스를 잘 이해하고 있어야 한다.


<br><br>

### Promise.resolve / Promise.reject
`Promise.resolve`와 `Promise.reject` 메소드는 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용한다.  

`Promise.resolve` 메소드는 인수로 전달받은 값을 `resolve`하는 프로미스를 생성한다.
``` js
// 배열을 resolve 하는 프로미스를 생성
const resolvedPromise = Promise.resolve([1, 2, 3]);
resolvedPromise.then(console.log);

// 위 코드는 아래 코드와 동일하다.
const resolvedPromise = new Promise(resolve => resolve([1, 2, 3]));
resolvedPromise.then(console.log);
```

<br>

`Promise.rejec` 메소드는 인수로 전달받은 값을 `reject`하는 프로미스를 생성한다.
``` js
// 에러 객체를 reject 하는 프로미스를 생성
const rejectedPromise = Promise.reject(new Error('Error!'));
rejectedPromise.catch(console.error);

// 위 코드는 아래 코드와 동일하다.
const rejectedPromise = new Promise((_, reject) => reject(new Error('Error!')));
rejectedPromise.catch(console.error);
```



<br><br>

### 마이크로태스크 큐
``` js
setTimeout(() => console.log(1), 0);

Promise.resolve()
    .then(() => console.log(2))
    .then(() => console.log(3));
```

프로미스 후속 처리 메소드도 비동기로 동작하므로 1 → 2 → 3 순으로 출력될 것 같지만 그렇지 않다.  
> 실제로는 2 → 3 → 1 순으로 출력된다.

그 이유는 <font color='orange'>후속 처리 메소드의 콜백 함수는 태스크 큐가 아니라 마이크로태스크 큐에 저장되기 때문이다.</font>

<br>

마이크로태스크 큐는 태스크 큐와는 별도의 큐이다.  
<font color='orange'>마이크로태스크 큐에는 프로미스의 후속 처리 메소드의 콜백 함수가 일시적으로 저장된다.</font>

그 외의 콜백 함수나 이벤트 핸들러는 태스크 큐에 일시 저장된다.

<br>

마이크로태스크 큐는 태스크 큐보다 우선순위가 높다.  
<font color='tomato'>즉, 이벤트 루프는 콜 스택이 비면 먼저 마이크로태스크 큐에서 대기하고 있는 함수를 가지고 와서 실행한다.</font>



<br><br>


### fetch
`fetch` 함수는 `XMLHttpRequest` 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API이다.  
`fetch` 함수는 `XMLHttpRequest` 객체보다 사용법이 간단하고, 프로미스를 지원하기 때문에 비동기 처리를 위한 콜백 패턴의 단점에서 자유롭다.

`fetch` 함수에는 HTTP 요청을 전송할 URL과 HTTP 요청 메소드, HTTP 요청 헤더, 페이로드 등을 설정한 객체를 전달한다.

``` js
const promise = fetch(url [, options]);
```

<br>

`fetch` 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환한다.  
fetch 함수로 GET 요청을 전송해보자.

fetch 함수의 첫 번째 인자로 URL만 전달하면 GET 요청을 전달한다.

``` js
fetch('https://jsonplaceholder.typicode.com/todos/1'
    .then(response => console.log(response));
)
```

`fetch` 함수는 HTTP 응답을 나타내는 `Response` 객체를 래핑한 프로미스를 반환하므로   
후속 처리 메소드 `then`을 통해 프로미스가 `resolve`한 `Response` 객체를 전달받을 수 있다.

<br>

`Response.prototype`에는 `Response` 객체에 포함되어 있는 HTTP 응답 몸체를 위한 다양한 메소드를 제공한다.  
예를 들어 `fetch` 함수가 반환한 프로미스가 래핑하고 있는 MIME 타입이 `application/json`인 HTTP 응답 몸체를 취득하려면  
`Response.prototype.json` 메소드를 사용한다.

``` js
fetch('https://jsonplaceholder.typicode.com/todos/1')
    .then(response => response.json())
    .then(json => console.log(json));
```

<br>

`fetch` 함수를 사용할 때는 에러 처리에 유의해야 한다.

``` js
const wrongUrl = 'https://jsonplaceholder.typicode.com/wrong/1';

fetch(wrongUrl)
    .then(response => console.log(response))
    .catch(err => console.error(err));
```

<br>

`fetch` 함수에 전달한 URL이 잘못되었거나 서버가 응답하지 않는 경우 프로미스는 실패한다.  
부적절한 URL이 지정되었기 때문에 404 Not Found 에러가 발생하고  
catch 후속 처리 메소드에 의해 `error`가 출력될 것처럼 보이지만 `ok`가 출력된다.

`fetch` 함수가 반환하는 프로미스는 기본적으로 HTTP 에러가 발생해도 에러를 `reject`하지 않고  
불리언 타입의 ok 상태를 `false`로 설정한 `Response` 객체를 `resolve`한다.
> 오프라인 등의 네트워크 장애나 CORS 에러에 의해 요청이 완료되지 못한 경우에만 프로미스를 `reject`한다.

<br>

따라서 `fetch` 함수를 사용할 때는 아래와 같이  
프로미스가 resolve한 불리언 타입의 `ok` 상태를 확인해 명시적으로 에러를 처리할 필요가 있다.

``` js
const wrongUrl = 'https://jsonplaceholder.typicode.com/wrong/1';

// 부적절한 URL이 지정되었기 때문에 404 Not Found 에러가 발생한다
fetch(wrongUrl
    .then(response => {
        if (!response.ok) {
            throw new Error(response.statusText);
        }
        return response.json();
    })
    .then(json => console.log(json))
    .catch(err => console.error(err));
)
```

<br>

참고로 `axios`는 모든 HTTP 에러를 reject하는 프로미스를 반환한다.  
따라서 모든에러를 `catch`에서 처리할 수 있어 편리하다.

모든 `axios`는 인터셉터, 요청 설정 등 `fetch`보다 다양한 기능을 지원한다.

<br>

`fetch` 함수를 통해 HTTP 요청을 보내보자.  
- 첫 번째 인수로 URL을 전달한다.
- 두 번째 인수로 요청 메소드, 요청 헤더, 페이로드 등을 설정한 객체를 전달한다.

``` js
const request = {
    get(url) {
        return fetch(url);
    },
    post(url, payload) {
        return fetch(url, {
            method: 'POST',
            headers: { 'content-type': 'application/json' },
            body: JSON.stringify(payload)
        })
    },
    patch(url, payload) {
        return fetch(url, {
            method: 'PATCH', 
            headers: { 'content-type': 'application/json' },
            body: JSON.stringify(payload)
        });
    },
    delete(url) {
        return fetch(url, { method: 'DELETE' });
    }
};
```


<br>

#### GET 요청
``` js
request.get('https://jsonplaceholder.typicode.com/todos/1')
    .then(response => {
        if (!response.ok) {
            throw new Error(response.statusText);
        }
        return response.json();
    })
    .then(todos => console.log(todos))
    .catch(err => console.error(err));
```

<br>

#### POST 요청
``` js
request.post('https://jsonplaceholder.typicode.com/todos', {
    userId: 1,
    title: 'JavaScript',
    completed: false
})
    .then(response => {
        if (!response.ok) {
            throw new Error(response.statusText);
        }
        return response.json();
    })
    .then(todos => console.log(todos))
    .catch(err => console.error(err));
```


<br>

#### PATCH 요청
``` js
request.patch('https://jsonplaceholder.typicode.com/todos/1', {
    completed: true
})
    .then(response => {
        if (!response.ok) {
            throw new Error(response.statusText);
        }
        return response.json();
    })
    .then(todos => console.log(todos))
    .catch(err => console.error(err));
```

<br>


#### DELETE 요청
``` js
request.delete('https://jsonplaceholder.typicode.com/todos/1')
    .then(response => {
        if (!response.ok) {
            throw new Error(response.statusText);
        }
        return response.json();
    })
    .then(todos => console.log(todos))
    .catch(err => console.error(err));
```


끝.