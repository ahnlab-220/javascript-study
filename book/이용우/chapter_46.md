### Chapter 46 - 제너레이터와 async/await

ES6에서 도입된 제너레이터(Generator)는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수이다.  
제너레이터와 일반 함수의 차이점은 다음과 같다.

1) 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.  
일반 함수를 호출하면 제어권이 함수에게 넘어가고 일괄 실행한다. 즉, 함수 호출자(Caller)는 함수를 호출한 이후 함수 실행을 제어할 수 없다. 제너레이터 함수는 함수 실행을 함수 호출자가 제어할 수 있다. <font color='orange'>다시 말해, 함수 호출자가 함수 실행을 일시 중지시키거나 재개시킬 수 있다. 이는 함수의 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도(yield)할 수 있다는 것을 의미한다.</font>

2) 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.  
일반 함수를 호출하면 매개변수를 통해 함수 외부에서 값을 주입받고 함수 코드를 일괄 실행하여 결과값을 함수 외부로 반환한다. **즉, 함수가 실행되고 있는 동안에는 함수 외부에서 함수 내부로 값을 전달하여 함수의 상태를 변경할 수 없다.**  제너레이터 함수는 함수 호출자와 양방향으로 함수의 상태를 주고받을 수 있다. 다시 말해, <font color='orange'>제너레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로부터 상태를 전달받을 수도 있다.</font>


3) 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.  
일반 함수를 호출하면 함수 코드를 일괄 실행하고 값을 반환한다. <font color='orange'>제너레이터 함수를 호출하면 함수 호출을 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.</font>

<br><br>

### 제너레이터 함수의 정의
제너레이터 함수는 `function*` 키워드로 선언한다.  
그리고 하나 이상의 `yield` 표현식을 포함한다. 이것을 제외하면 일반 함수를 정의하는 방법과 같다.

``` js
// 제너레이터 함수 선언문
function* genDecFunc() {
    yield 1;
}

// 제너레이터 함수 표현식
const getExpFunc = function* () {
    yield 1;
};

// 제너레이터 메소드
const obj = {
    * genObjMethod() {
        yield 1;
    }
};

// 제너레이터 클래스 메소드
class MyClass {
    * genClsMethond() {
        yield 1;
    }
}
```

애스터리스크(`*`)의 위치는 `function` 키워드의 함수 이름 사이라면 어디든지 상관없다.  
다음 예제의 제너레이터 함수는 모두 유효하다. **하지만 일관성을 유지하기 위해 `function` 키워드 바로 뒤에 붙이는 것을 권장한다.**

<br>

``` js
function* genFunc() {
    yield 1;
}
```


<br>

제너레이터 함수는 화살표 함수로 정의할 수 없다.

``` js
const genArrowFunc = () => {
    yield 1;
};  // SyntaxError: Unexpected token '*'

```

제너레이터 함수는 `new` 연산자와 함께 생성자 함수로 호출할 수 없다.

``` js
function* genFunc() {
    yield 1;
}

new genFunc();  // TypeError: genFunc is not a constructor
```


<br><br>

### 제너레이터 객체
제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다.  

**제너레이터 함수가 반환한 제너레이터 객체는 이터러블(iterable)이면서 동시에 이터레이터(iterator)다.**

<br>

``` js
// 제너레이터 함수
function* getFunc() {
    yield 1;
    yield 2;
    yield 3;
}

// 제너레이터 ㅎ마수를 호출하면 제너레이터 객체를 반환한다.
const generator = genFunc();

// 제너레이터 객체는 이터러블이면서 동시에 이터레이터이다
console.log(Symbol.iterator in generator);  // true

// 제너레이터 객체는 next 메소드를 갖는다.
console.log('next' in generator);  // true
```

<br>

제너레이터 객체는 `next` 메소드를 갖는 이터레이터이지만 이터레이터에는 없는 `return`, `throw` 메소드를 갖는다. 제너레이터 객체의 세 개의 메소드를 호출하면 다음과 같이 동작한다.

- `next` 메소들르 호출하면 제너레이터 함수의 `yield` 표현식까지 코드 블록을 실행하고 `yield`된 값을 `value` 프로퍼티 값으로, `false`를 `done` 프로퍼티 값으로 갖는 이터레이터 result 객체를 반환한다.

- `return` 메소드를 호출하면 인수로 전달받은 값을 `value` 프로퍼티 값으로, `true`를 `done` 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

``` js
function* genFunc() {
    try {
        yield 1;
        yield 2;
        yield 3;
    } catch (e) {
        console.error(e);
    }
}

const generator = genFunc();

console.log(generator.next());  // { value: 1, done: false }
console.log(generator.return('End!'));  // { value: 'End!', done: true }
```


<br>

`throw` 메소드를 호출하면 인수로 전달받은 에러를 발생시키고  
`undefined`를 `value` 프로퍼티 값으로, `true`를 `done` 프로퍼티 값으로 갖는 이터레이터 result 객체를 반환한다.

``` js
function* genFunc() {
    try {
        yield 1;
        yield 2;
        yield 3;
    } catch (e) {
        console.error(e);
    }
}

const generator = genFunc();

console.log(generator.next());  // { value: 1, done: false }
console.log(generator.throw('Error!'));  // { value: undefined, done: true }
```

<br><br>

### 제너레이터의 일시중지와 재개
제너레이터는 `yield` 키워드와 `next` 메소드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수도 있다. 

일반 함수는 호출 이후 제어권을 함수가 독점하지만,  
제너레이터는 함수 호출자에게 제어구너을 양도(yield)하여 필요한 시점에 함수 실행을 재개할 수 있다.

제너레이터 함수를 호출하면 제너레이터 함수의 코드 블록이 실행되는 것이 아니라 제너레이터 객체를 반환한다. <font color='orange'>이터러블이면서 동시에 이터레이터인 제너레이터 객체는 `next` 메소드를 갖는다. 제너레이터 객체의 `next` 메소드를 호출하면 제너레이터 함수의 코드 블록을 실행한다.</font>

단, 일반 함수처럼 한번에 코드 블록의 모든 코드를 일괄 실행하는 것이 아니라, `yield` 표현식 까지만 실행한다. **`yield` 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 `yield` 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다.**

``` js
// 제너레이터 함수
function* genFunc() {
    yield 1;
    yield 2;
    yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
const generator = genFunc();

// 처음 `next` 메소드를 호출하면 첫 번째 `yield` 표현식까지만 실행되고 일시중지된다.
console.log(generator.next());  // { value: 1, done: false }
```

<br>

<font color='orange'>제너레이터 객체의 next 메소드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당된다.</font>

``` js
function* genFunc() {
    // 처음 next 메소드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
    // 이때 yield된 값 1은 next 메소드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
    // x 변수에는 아무것도 할당되지 않는다. x 변수의 값은 next 메소드가 두번째 호출될 때 결정된다.
    const x = yield 1;

    // 두 번째 next 메소드를 호출할 때 전달한 인수 10은 첫 번째 yield 표현식을 할당받는 x 변수에 할당된다.
    const y = yield (x + 10);

    // 세 번째 next 메소드를 호출할 때 전달한 인수 20은 두 번째 yield 표현식을 할당받는 y 변수에 할당된다.
    return x + y;
}


// 제너레이터 객체 반환
const generator = genFunc();

let res = generator.next();
console.log(res);  // { value: 1, done: false }

res = generator.next(10);
console.log(res);  // { value: 20, done: false }


// next 메소드에 인수로 전달한 20은 genFunc 함수의 y 변수에 할당된다.
res = generator.next(20);
console.log(res);  // { value: 30, done: true }

```


<br><br>

### 제너레이터의 활용
#### 이터러블의 구현
제너레이터 함수를 사용하면 이터레이션 프로토콜을 준수해 이터럽르을 생성하는 방식보다 간단히 이터러블을 구현할 수 있다.

``` JS
// 무한 이터러블을 생성
const infiniteFibonacci = (function* () {
    let [pre, cur] = [0, 1];

    while (true) {
        [pre, cur] = [cur, pre + cur];
        yield cur;
    }
}());

// infiniteFibonacci는 무한 이터러블이다.
for (const num of infiniteFibonacci) {
    if (num > 10000) break;
    console.log(num);  // 1 1 2 3 5 8 ... 2584 4181 6765
}
```


<br><br>

#### 비동기 처리
제너레이터 함수는 `next` 메소드와 `yield` 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다.  
이런 특성을 활용하면 <font color='orange'>프로미스를 사용한 비동기 처리를 동기처리처럼 구현할 수 있다.</font>

> 프로미스의 후속 처리 메소드 `then/catch/finally` 없이 비동기 처리 결과를 반환하도록 구현할 수 있다.

``` js
const fetch = require('node-fetch');

// 제너레이터 실행기
const async = generatorFunc => {
    const generator = generatorFunc();  // (2)

    const onResolved = arg => {
        const result = generator.next(arg);   // (5)

        return result.done 
            ? result.value  // (9)
            : result.value.then(res => onResolved(res));   // (7)
    };

    return onResolved();  // (3)
}

(async(function* fetchTodo() {  // (1)
    const url = 'https://jsonplaceholder.typicode.com/todos/1';
    
    const response = yield fetch(url);  // (6)
    const todo = yield response.json();  // (8)
    console.log(todo);
}))();  // (4)
```

위 예제는 아래와 같이 동작한다.

1) `async` 함수가 호출(1)되면 인수로 전달받은 제너레이터 함수 `fetchTodo`를 호출하여 제너레이터를 생성(2)하고 `onResolved` 함수를 반환한다. `async` 함수가 반환한 `onResolved` 함수를 즉시 호출(4)하여 (2)에서 생성한 제너레이터 객체의 `next` 메소드를 처음 호출(5)한다.

2) `next` 메소드가 처음 호출(5)되면 제너레이터 함수 `fetchTodo`의 첫 번째 yield 문(6)까지 실행된다. 이때 제너레이터 함수가 끝까지 실행되지 않았다면 첫 번째 yield된 `fetch` 함수가 반환한 프로미스가 resolve한 Response 객체를 `onResolved` 함수에 인수로 전달하면서 재귀 호출(7)한다.

3) `onResolved` 함수에 인수로 전달된 `Response` 객체를 `next` 메소드에 인수로 전달하면서 `next` 메소드를 두 번째로 호출(5)한다. 이때 `next` 메소드에 인수로 전달한 Response 객체는 제너레이터 함수 fetchTodo의 response 변수(6)에 할당되고 제너레이터 함수 fetchTodo의 두 번째 yield 문(8)까지 실행된다.

4) 제너레이터 함수 `fetchTodo`가 끝까지 시랳ㅇ되지 않았다면 이터레이터 리절트 객체의 `value` 프로퍼티 값, 즉 두 번째 `yield`된 `response.json` 메소드가 반환한 프로미스가 `resolve`한 todo 객체를 onResolved 함수에 인수로 전달하면서 재귀 호출(7)한다.

5) `onResolved` 함수에 인수로 전달한 todo 객체를 `next` 메소드에 인수로 전달하면서 `next` 메소드를 세 번째로 호출(5)한다. 이때 `next` 메소드에 인수로 전달한 todo 객체는 제너레이터 `fetchTodo`의 todo 변수(8)에 할당되고 제너레이터 함수 fetchTodo가 끝까지 실행된다.

6) `next` 메소드가 반환한 이터레이터 리절트 객체의 `done` 프로퍼티 값이 `true`, 즉 제너레이터 함수 `fetchTodo`가 끝까지 실행되었다면 제너레이터 함수 `fetchTodo`의 반환값인 `undefined`를 그대로 반환(9)하고 처리르 종료한다.


<br><br>

위 예제는 제너레이터 실행기인 `async` 함수는 이해를 돕기 위한 예제이다.   
이후에 설명할 `async/await` 키워드를 사용하면  
async 함수와 같은 제너레이터 실행기를 사용할 필요가 없지만, 

혹여나 제너레이터 실행기가 필요하다면 직접 구현하는 것 보다 `co` 라이브러리를 사용하는 것이 나을 수 있다.

``` js
const fetch = require('node-fetch');
const co = require('co');

co(function* () {
    const url = 'https://jsonplaceholder.typicode.com/todos/1';

    const response = yield fetch(url);
    const todo = yield response.json();
    console.log(todo);
})
```


<br><br>

### async/await
위에서 제너레이터를 사용해서 비동기 처리를 동기 처리로 동작하도록 구현했지만,  
코드가 장황해지고 가독성이 나빠진다. 

ES8에서는 제너레이터보다 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있는 `async/await`가 도입되었다.

`async/await`는 프로미스를 기반으로 동작한다.  
<font color='tomato'>`async/await`를 사용하면 프로미스의 `then/catch/finally` 후속 처리 메소드에 콜백 함수를 전달해서  
비동기 처리 결과를 후속 처리 필요 없이 마치 동기 처리처럼 프로미스를 사용할 수 있다.</font>

``` js
const fetch = require('node-fetch');

const fetchTodo = async () => {
    const url = 'https://jsonplaceholder.typicode.com/todos/1';

    const response = await fetch(url);
    const todo = await response.json();
    console.log(todo);
}

fetchTodo();
```


<br><br>


#### async 함수
`await` 키워드는 반드시 `async` 함수 내부에서 사용되어야 한다.  
`async` 함수는 `async` 키워드를 사용해 정의하며, **언제나 프로미스를 반환한다.**

`async` 함수가 명시적으로 프로미스를 반환하지 않더라도  
`async` 함수는 암묵적으로 반환값을 `resolve` 하는 프로미스를 반환한다.

<br>

``` js
// async 함수 선언문
async function foo(n) { return n; }
foo(1).then(v => console.log(v));  // 1

// async 함수 표현식
const bar = async function (n) { return n; }
bar(2).then(v => console.log(v));  // 2

// async 화살표 함수
const bax = async n => n;
bax(3).then(v => console.log(v));  // 3

// async 메소드
const obj = {
    async foo(n) { return n; }
};
obj.foo(4).then(v => console.log(v));  // 4

// async 클래스 메소드
class MyClass {
    async bar(n) { return n; }
}
const myClass = new MyClass();
myClass.bar(5).then(v => console.log(v));  // 5
```

<br><br>

#### await 키워드
`await` 키워드는 프로미스가 `settled` 상태(비동기 처리가 수행된 상태)가 될 때 까지 대기하다가  
`settled` 상태가 되면 프로미스가 `resolve`한 처리 결과를 반환한다.  

`await` 키워드는 반드시 프로미스 앞에서 사용해야 한다.

``` js
const fetch = require('node-fetch');

const getGithubUserName = async id => {
    const res = await fetch(`https://api.github.com/users/${id}`);   // (1)
    const { name } = await res.json();
    console.log(name);
}

getGithubUserName('ungmo2');
```

<br>

`await` 키워드는 프로미스가 `settled` 상태가 될 때 까지 대기한다.  
따라서 (1)의 `fetch` 함수가 수행한 HTTP 요청에 대한 서버의 응답이 도착해서 `fetch` 함수가 반환한 프로미스가 `settled` 상태가 될 대 까지 (1)은 대기하게 된다.  

이후 프로미스가 `settled` 상태가 되면 프로미스가 `resolve`한 처리 결과가 `res` 변수에 할당된다.

이처럼 `await` 키워드는 다음 실행을 일시 중지했다가  
프로미스가 `settled` 상태가 되면 다시 재개한다.



<br><br>

#### 에러 처리
`async/await`에서 에러 처리는 `try ... catch` 문을 사용할 수 있다.  
콜백 함수를 인수로 전달받는 비동기 함수와는 달리 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 대문에 호출자가 명확하다.


``` js
const fetch = require('node-fetch');

const foo = async () => {
    try {
        const wrongUrl = 'https://wrong.url';
        
        const response = await fetch(wrongUrl);
        const data = await response.json();
        console.log(data);
    } catch (err) {
        console.error(err);
    }
}

foo();
```

위 예제에서 `foo` 함수의 `catch`문은 HTTP 통신에서 발생한 네트워크 에러 뿐만 아니라  
`try` 코드 블록 내의 모든 문에서 일반적인 에러까지 모두 캐치할 수 있다.  


`async` 함수 내에서 `catch` 문을 사용해서 에러 처리를 하지 않으면  
`async` 함수는 발생한 에러를 `reject` 하는 프로미스를 반환한다.

**따라서 `async`함수를 호출하고 `Promise.prototype.catch` 후속 처리 메소드를 사용해 에러를 캐치할 수도 있다.**

``` js
const fetch = require('node-fetch');

const foo = async () => {
    try {
        const wrongUrl = 'https://wrong.url';

        const response = await fetch(wrongUrl);
        const data = await response.json();
        console.log(data);
    } catch (err) {
        console.error(err);
    }
}

foo()
    .then(v => console.log(v))
    .catch(err => console.error(err));
```


<br><br>

#### 비동기 함수에서의 에러 처리
