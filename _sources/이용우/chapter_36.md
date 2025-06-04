### Chapter 36 - 디스트럭처링 할당

디스트럭처링 할당(Destructuring assignment)은 이터러블 또는 객체를 비구조화(Destructuring)하여  
한개 이상의 변수에 개별적으로 할당하는 것을 의미한다.

> 이터러블 또는 객체 리터럴에서 필요한 값을 추출하여 변수에 할당할 때 주로 사용된다.

<br>

### 배열 디스트럭처링 할당
배열 디스트럭처링 할당은 배열의 각 요소를 배열로부터 추출하여 하나 이상의 변수에 할당한다.  
할당 대상은 이터러블해야하고, 할당 기준은 배열의 index이다.

``` js
const arr = [1, 2, 3];

// 디스트럭처링 할당
const [one, two, three] = arr;
console.log(one, two, three); // 1 2 3
```

<br>

변수의 개수와 이터러블의 요소 개수가 반드시 일치할 필요는 없다.

``` js
const [a, b] = [1, 2];
console.log(a, b); // 1 2

const [c, d] = [1];
console.log(c, d); // 1 undefined

const [e, f] = [1, 2, 3];
console.log(e, f); // 1 2
```

<br>

배열 디스트럭처링 할당은 배열과 같은 이터러블에서 필요한 요소만 추출하여 변수에 할당하고 싶을 때 주로 사용한다.  
``` js
function parseURL(url = '') {
    const parsedURL = url.match(/^(w+):\/\/([^/]+)(.*)$/);
    if (!parsedURL) return {};

    // 배열 디스트럭처링 할당을 사용하여 이터러블에서 필요한 요소만 추출한다.
    const [, protocol, host, path] = parsedURL;
    return { protocol, host, path };
}

const parsedURL = parseURL('https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array');
console.log(parsedURL);
// { protocol: 'https', host: 'developer.mozilla.org', path: '/ko/docs/Web/JavaScript/Reference/Global_Objects/Array' }
```

<br><br>

### 객체 디스트럭처링 할당
객체의 각 프로퍼티를 객체로부터 추출하여 1개 이상의 변수에 할당한다.  
이때 객체 디스트럭처링 할당의 대상은 객체이며, **할당 기준은 프로퍼티의 키이다.**

``` js
const user = { firstName: 'firstName', lastName: 'lastName' };

// *프로퍼티 키를 기준*으로 디스트럭처링 할당이 이루어진다. 순서는 의미가 없다.
const { lastName, firstName } = user;
console.log(lastName, firstName);  // lastName firstName
```

<br>

만약 객체의 프로퍼티 키와 다른 변수 이름으로 프로퍼티 키를 할당하고 싶다면 아래와 같이 변수를 선언하면 된다.

``` js
const user = { firstName: 'firstName', lastName: 'lastName' };

// 프로퍼티 키와 다른 변수 이름으로 프로퍼티 키를 할당하고 싶다면 아래와 같이 변수를 선언하면 된다.
const { lastName: lastName2, firstName: firstName2 } = user;
console.log(lastName2, firstName2);  // lastName firstName
```


<br>

객체 디스트럭처이 할당은 객체에서 프로퍼티 키로  
필요한 프로퍼티 값만 추출하여 변수에 할당하고 싶을 때 유용하다.

``` js
const str = 'Hello';

const { length } = str;
console.log(length); // 5

const todo = { id: 1, content: 'HTML', completed: true };

// todo 객체에서 id만 추출한다.
const { id } = todo;
console.log(id); // 1
```

<br>

객체 디스트럭처링 할당은 함수의 매개변수에서도 활용할 수 있다.

``` js
function printTodo({ content, completed }) {
    console.log(`${content} is ${completed ? 'completed' : 'not completed'}`);
}

printTodo({ id: 1, content: 'HTML', completed: true });
// HTML is completed
```

<br><br>

배열의 요소가 객체인 경우 배열 디스트럭처링 할당과 객체 디스트럭처링 할당을 혼용할 수 있다.

``` js
const todos = [
    { id: 1, content: 'HTML', completed: true },
    { id: 2, content: 'CSS', completed: false },
    { id: 3, content: 'JavaScript', completed: false }
];


// todos 배열의 두 번째 요소인 객체로부터 id 프로퍼티만 추출한다.
const [, { id }] = todos;
console.log(id); // 2
```

<br>

중처버 객체의 경우에는 아래와 같이 사용 가능하다.

``` js
const user = {
    name: 'Lee',
    address: {
        city: 'Seoul',
        country: 'Korea'
    }
};

// address 프로퍼티 키로 객체를 추출하고 이 객체의 city 프로퍼티 키로 값을 추출한다.
const { address: { city } } = user;
console.log(city); // Seoul
```

<br><br>

객체 디스트럭처링 할당을 위한 변수에 Rest 프로퍼티 `...`를 사용할 수 있다.  

``` js
// Rest 프로퍼티
const { x, ...rest } = { x: 1, y: 2, z: 3 };
console.log(x, rest); // 1, {y: 2, z: 3}
```


끝.