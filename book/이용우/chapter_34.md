### chapter 34 - 이터러블
#### 이터레이션 프로토콜
이터러블 프로토콜(iterable protocol)은 순회 가능한(iterable) 자료구조로 만들기 위한 규약이다.

ES6에서는 순회 가능한 자료 구조를 이터레이션 프로토콜을 준수하는 이터러블로 통일하여  
`for...of` 문, `spread 문법` 등 다양한 문법에서 사용할 수 있도록 정의했다.

<br>

이터레이션 프로토콜에는 이터러블(iterable) 프로토콜과 이터레이터(iterator) 프로토콜이 있다.

<img src="https://github.com/user-attachments/assets/d64c12a3-c500-4233-a8a5-186b8bd01922"/>

- iterable: 순회 가능한 자료구조를 의미
- iterator: iterable의 요소를 탐색하기 위한 포인터

<br>

#### 이터러블
이터러블 프로토콜을 준수한 객체를 이터러블이라 한다.  
> 이터러블은 `Symbol.iterator`를 프로퍼티 키로 사용한 메소드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 의미

예를 들어, 배열은 `Array.prototype`의 `Symbol.iterator` 메소드를 상속받는 이터러블이다.  
이터러블은 `for...of` 문으로 순회할 수 있고, 스프레드 문법과 배열 디스트럭처링(destructuring) 문법에서 사용할 수 있다.

``` js
const array = [1, 2, 3];

// 이터러블인 배열은 for...of 문으로 순회할 수 있다.
for (const item of array) {
    console.log(item);
}

// 이터러블인 배열은 스프레드 문법의 대상으로 사용 가능하다.
console.log([...array]);    // [1, 2, 3]


// 이터러블인 배열은 배열 디스트럭처링 문법에서 사용 가능하다.
const [a, ...rest] = array;
console.log(a, rest);    // 1, [2, 3]
```

<br>

<font color='orange'>※ 2021년 부터 일반 객체의 스프레드 문법 사용을 허용한다.</font>

``` js
const obj = { a: 1, b: 2 };
console.log({ ...obj });    // { a: 1, b: 2 }
```

<br><br>

#### 이터레이트
이터러블의 `Symbol.iterator` 메소드를 호출하면 이터레이터를 반환한다.  
이터러블의 `Symbol.iterator` 메소드가 반환한 이터레이터는 `next` 메소드를 갖는다.

``` js
const array = [1, 2, 3];
const iterator = array[Symbol.iterator]();

console.log(iterator.next());    // { value: 1, done: false }
console.log(iterator.next());    // { value: 2, done: false }
console.log(iterator.next());    // { value: 3, done: false }
console.log(iterator.next());    // { value: undefined, done: true }
```

`done` 프로퍼티는 이터러블의 순회 완료 여부를 의미한다.


<br><br>

### for...of 문

`for...of` 문은 이터러블을 순회하며 이터레이트를 반환한다.

``` js
const array = [1, 2, 3];

for (const item of array) {
    console.log(item);
}
```

`for...of` 문은 `for...in` 문과 유사하지만  
객체의 프로퍼티 이름(key)이 아니라 객체의 값(value)을 순회한다는 차이가 있다.

``` js
const obj = { a: 1, b: 2 };

// for...in 문은 객체의 프로퍼티 이름(key)을 순회한다.
for (const key in obj) {
    console.log(key);    // a, b
}

// for...of 문은 객체의 값(value)을 순회한다.
for (const value of obj) {
    console.log(value);    // 1, 2
}

```

<br><br>

### 유사 배열 객체와 이터러블
유사 배열 객체는 배열처럼 인덱스로 프로퍼티 값에 접근 가능하며  
`length` 프로퍼티 객체를 갖는다.

<font color='orange'>`length` 프로퍼티를 가지고 있기 때문에 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.</font>

```js
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
};

// 유사 배열 객체는 length 프로퍼티를 가지고 있기 때문에 인덱스로 프로퍼티 값에 접근할 수 있다.
for (let i = 0; i < arrayLike.length; i++) {
    // 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.
    console.log(arrayLike[i]);    // 1, 2, 3
}
```

단, 유사 배열 객체는 이터러블이 아닌 일반 객체이다.
> 유사 배열 객체는 `Symbol.iterator` 메소드가 없음

때문에 `for...of` 문으로 순회할 수 없다.

``` js
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
};

for (const item of arrayLike) {
    console.log(item);
}    // TypeError: arrayLike is not iterable


// 순회하려면 이터러블로 만들어야 한다.
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
};

// Array.from 메소드를 사용하면 유사 배열 객체를 배열로 변환할 수 있다.
const arr = Array.from(arrayLike);
console.log(arr);    // [1, 2, 3]
```




끝.