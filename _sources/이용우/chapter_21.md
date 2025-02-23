### Chapter 21 - 빌트인 객체

빌트인 객체는 자바스크립트 엔진에 내장되어 있는 객체를 말한다. 자바스크립트 프로그램은 이 빌트인 객체를 포함하여 실행된다.

빌트인 객체는 아래와 같이 크게 세 가지로 분류된다.

1. 표준 빌트인 객체
2. 환경에 따라 조금씩 다르게 동작하는 호스트 객체
3. 사용자 정의 객체


<br><br>

### 표준 빌트인 객체

자바 스크립트는 `Object`, `String`, `Number`, `Boolean`, `Symbol`, `Date`, `RegExp`, `Error` 등 40여 개의 표준 빌트인 객체를 제공한다.

<br>

`Math`, `Reflect`, `JSON`을 제외한 표준 빌트인 객체는  
아래와 같이 `Object.prototype`를 상속받는다. (인스턴스를 생성할 수 있는 생성자 함수)

```js
const obj = new Object();
console.log(obj.toString()); // [object Object]
```

> `Math`, `Reflect`, `JSON`은 인스턴스를 생성할 수 없는 빌트인 객체이다.


<br>

표준 빌트인 객체인 `String`, `Number`, `Boolean`,   
`Function`, `Array`, `Date`는 생성자 함수로 호출하여 인스턴스를 생성할 수 있다.

``` js
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee');
console.log(typeof strObj); // object
console.log(strObj); // String { "Lee" }

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123);
console.log(typeof numObj); // object
console.log(numObj); // Number { 123 }      

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj = new Boolean(true);
console.log(typeof boolObj); // object
console.log(boolObj); // Boolean { true }

// Function 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function('x', 'return x * x');
console.log(typeof func); // function
console.log(func(2)); // 4

// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3);
console.log(typeof arr); // object
console.log(arr); // [ 1, 2, 3 ]

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date();
console.log(typeof date); // object
console.log(date); // Mon Feb 10 2025 15:30:00 GMT+0900 (한국 표준시)
```

<br><br>

**생성자 함수인 표준 빌트인 객체**가 생성한 인스턴스의 프로토타입은  
표준 빌트인 객체의 `prototype` 프로퍼티에 바인딩되어 있는 객체이다.

예를 들어, 표준 빌트인 객체인 `String`의 경우,  
`String.prototype`에 바인딩되어 있는 객체가 인스턴스의 프로토타입이 된다.

``` js
console.log(Object.getPrototypeOf(new String('Lee')) === String.prototype); // true
```

<br><br>

표준 빌트인 객체의 `prototype` 프로퍼티에 바인딩되어 있는 객체는   
다양한 기능의 빌트인 프로토타입 메소드를 제공한다.

그리고 표준 빌트인 객체는 인스턴스 없이도 호출 가능한 빌트인 정적 메소드를 제공한다.

<br>

예를 들어, 표준 빌트인 객체인 `Number`의 `prototype` 프로퍼티에 바인딩된 객체,  
`Number.prototype`에 바인딩되어 있는 객체는 아래와 같은 빌트인 프로토타입 메소드를 제공한다.

``` js
// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123.456);

// Number.prototype.toFixed
console.log(numObj.toFixed()); // 123

// Number.prototype.toString
console.log(numObj.toString()); // 123.456
```

<br><br>

### 원시값과 래퍼 객체
**원시값인 문자열, 숫자, 불리언 값의 경우**  


``` js
const str = 'Hello';

// 문자열 원시값에 대해 마침표 표기법으로 접근하면 자바스크립트 엔진은 암묵적으로 래퍼 객체를 생성한다.
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```

원시값은 객체가 아니므로 프로퍼티나 메소드를 가질 수 없지만,  
원시값인 문자열이 위와 같이 마치 객체처럼 동작한다.

이들 원시값에 대해 마치 객체처럼 마침표 표기법(또는 대괄호 표기법)으로 접근하면  
자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해주기 때문이다.  

<br>

즉, 원시값을 객체처럼 사용하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여  
객체로 프로퍼티에 접근하거나 메소드를 호출하고 다시 원시값으로 되돌린다.

<br>

이와 같이 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체라고 한다.

<br><br>

문자열에 대해 마침표 표기법으로 접근하면  
자바스크립트 엔진은 문자열을 객체로 인식하여 임시로 생성한 래퍼 객체에 접근한다.

``` js
const str = 'Hello';

console.log(str.length); // 5
```

<br><br>

### 전역 객체
전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해   
어떤 객체보다도 먼저 생성되는 특수 객체이며, 어떤 객체에도 속하지 않은 최상위 객체이다.  

<br>

전역 객체는 자바스크립트 환경에 따라 지칭하는 이름이 제각각이다.  
예를 들어, 브라우저 환경에서는 `window`, `global` 등으로 불리고,  
Node.js 환경에서는 `global` 객체를 의미한다.

<br>

전역 객체는 계층적 구조상 어떤 객체오도 속하지 않은 모든 빌트인 객체의 최상위 객체이다.  
> 전역 객체 자신은 어떤 객체의 프로퍼티도 아니며  
> 객체의 계층적 구조상 표준 빌트인 객체와 호스트 객체를 프로퍼티로 소유한다는 것을 의미한다.

<br>

전역 객체의 특징은 아래와 같다.
1. 전역 객체는 개발자가 의도적으로 생성할 수 없다. (전역 객체를 생성할 수 있는 생성자 함수가 제공되지 않는다)
2. 전역 객체의 프로퍼티를 참조할 때 window(또는 global)을 생략할 수 있다.
    ``` js
    // 브라우저 환경
    window.parseInt('18'); // 18
    parseInt('18'); // 18
    ```
3. 전역 객체는 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
4. 자바스크립트 실행 환경에 따라 추가적으로 프로퍼티와 메소드를 갖는다.  
5. `var` 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 된다.
    ``` js
    var foo = 123;
    function bar() { return 3; }

    // 전역 객체의 프로퍼티는 전역 변수와 전역 함수를 참조할 수 있다.
    console.log(window.foo); // 123
    console.log(window.bar()); // 3
    ```
6. `let`이나 `const` 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.
    ``` js
    let foo = 123;
    console.log(window.foo); // undefined
    ```
7. 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 `window`를 공유한다. 이는 분리되어 있는 자바스크립트 코드가 하나의 전역을 공유한다는 의미와 같다.


<br><br>

### 빌트인 전역 프로퍼티
빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 의미한다.  

<br><br>

#### Infinity
`Infinity` 프로퍼티는 무한대를 나타내는 숫자값 `Infinity`를 갖는다.

``` js
console.log(window.Infinity === Infinity); // true

// 양의 무한대
console.log(3/0); // Infinity

// 음의 무한대
console.log(-3/0); // -Infinity
```

<br><br>

#### NaN
`NaN` 프로퍼티는 숫자가 아님을 나타내는 숫자값 `NaN`을 갖는다.

``` js
console.log(window.NaN); // NaN

console.log(Number('xyz')); // NaN
console.log(1 * 'string'); // NaN
console.log(typeof NaN); // number
```

<br><br>

#### undefined
`undefined` 프로퍼티는 원시값 `undefined`를 갖는다.

``` js
console.log(window.undefined); // undefined

var foo;
console.log(foo); // undefined
console.log(undefined); // undefined
```

<br><br>

### 빌트인 전역 함수
빌트인 전역 함수는 전역 객체의 메소드다.  

<br><br>

#### eval
`eval` 함수는 자바스크립트 코드를 나타내는 문자열을 인자로 전달받는다.  

``` js
/**
 * 문자열 자체를 자바스크립트 코드로 평가한다.
 * @param {string} code - 자바스크립트 코드
 * @returns {*} - 자바스크립트 코드의 평가 결과 (코드의 실행 결과)
 */
eval('1 + 2'); // 3
```

<br><br>

#### isFinite
`isFinite` 함수는 인자로 전달된 값이 정상적인 유한수인지 검사하여 유한수이면 `true`,  
무한수이면 `false`를 반환한다.

``` js
/**
 * 전달받은 인수가 유한수인지 확인하고 그 결과를 반환한다.
 * @param {number} testValue - 검사 대상 값
 * @returns {boolean} - 유한수이면 true, 무한수이면 false
 */
isFinite(testValue);
```

``` js
isFinite(0); // true
isFinite(2e64); // true
isFinite(100); // true
isFinite(null); // true: null은 0으로 평가된다.
``` 

<br><br>

#### isNaN
`isNaN` 함수는 인자로 전달된 값이 `NaN`인지 검사하여  
`NaN`이면 `true`, `NaN`이 아니면 `false`를 반환한다.

``` js
/**
 * 전달받은 인수가 NaN인지 확인하고 그 결과를 반환한다.
 * @param {number} testValue - 검사 대상 값
 * @returns {boolean} - NaN이면 true, NaN이 아니면 false
 */
isNaN(testValue);
```

``` js
// 숫자
isNaN(NaN); // true
isNaN(10); // false
isNaN(1 + undefined); // true

// 문자열
```

<br><br>

#### parseFloat
전달받은 문자열 인수를 부동 소수점 숫자(floating point number)로 변환하여 반환한다.

``` js
/**
 * 전달받은 문자열 인수를 부동 소수점 숫자로 변환하여 반환한다.
 * @param {string} stringValue - 변환할 문자열
 * @returns {number} - 변환된 부동 소수점 숫자
 */
parseFloat(stringValue);
```

``` js
parseFloat('100'); // 100
parseFloat('100.99'); // 100.99

// 공백으로 구분된 문자열은 첫 번째 문자열만 변환한다.
parseFloat('100 200'); // 100

// 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN을 반환한다.
parseFloat('xyz abc'); // NaN

// 부호를 붙일 수 있다.
parseFloat('-100'); // -100
parseFloat('+100'); // 100
```

<br><br>

#### parseInt
전달받은 문자열 인수를 정수로 변환하여 반환한다.

``` js
/**
 * 전달받은 문자열 인수를 정수로 변환하여 반환한다.
 * @param {string} stringValue - 변환할 문자열
 * @param {number} [radix] - 진수(진법을 나타내는 기수)
 * @returns {number} - 변환된 정수
 */
parseInt(stringValue, [radix]);
```

``` js
parseInt('100'); // 100

parseInt('100', 10); // 100
parseInt('100', 2); // 4: 2진수
parseInt('100', 8); // 64: 8진수
```

<br><br>

#### encodeURI / decodeURI
`encodeURI` 함수는 완전한 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다.  
`decodeURI` 함수는 인코딩된 URI를 인수로 전달받아 이스케이프 처리를 위해 디코딩한다.

``` js
/**
 * 완전한 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다.
 * @param {string} uri - 인코딩할 URI
 * @returns {string} - 인코딩된 URI
 */
encodeURI(uri);
```

인코딩이란, URI의 문자들을 이스케이프 처리하는 것을 의미한다.  
이스케이프 처리는 네트워크를 통해 정보를 공유할 때  
어떤 시스템에서도 읽을 수 있는 **아스키 문자 셋으로 변환**하는 것이다.

예를 들어, 특수 문자인 공백 문자는 `%20`, 한글 '가'는 `%EA%B0%80`로 인코딩된다.

``` js
// 완전한 URI
const uri = 'https://practice.com?name=이용우?job=programmer&teacher';

// encodeURI 함수는 완전한 URI를 인수로 전달받아 이스케이프 처리를 위해 인코딩한다.
const encoded = encodeURI(uri);
console.log(encoded); // https://practice.com?name=%EC%9D%B4%EC%9A%A9%EC%9A%B0?job=programmer&teacher

// decodeURI 함수는 인코딩된 URI를 인수로 전달받아 이스케이프 처리를 위해 디코딩한다.
const decoded = decodeURI(encoded);
console.log(decoded); // https://practice.com?name=이용우?job=programmer&teacher
```

<br><br>

#### encodeURIComponent / decodeURIComponent
`encodeURIComponent` 함수는 인수로 전달받은 문자열을   
URI의 구성요소인 쿼리 문자열의 일부로 간주하여 인코딩한다.  

`decodeURIComponent` 함수는 인코딩된 문자열을 인수로 전달받아 이스케이프 처리를 위해 디코딩한다.

``` js
/**
 * 인수로 전달받은 문자열을 URI의 구성요소인 쿼리 문자열의 일부로 간주하여 인코딩한다.
 * @param {string} uriComponent - 인코딩할 URI 구성요소
 * @returns {string} - 인코딩된 URI 구성요소
 */
encodeURIComponent(uriComponent);

/**
 * 인코딩된 문자열을 인수로 전달받아 이스케이프 처리를 위해 디코딩한다.
 * @param {string} encodedURIComponent - 디코딩할 인코딩된 URI 구성요소
 * @returns {string} - 디코딩된 URI 구성요소
 */
decodeURIComponent(encodedURIComponent);
```

<br>

`encodeURIComponent` 함수는 인수로 전달받은 문자열을  
URI의 구성요소인 쿼리 문자열의 일부로 간주하여 인코딩한다.  

따라서 쿼리 스트링 구분자로 사용되는 `=`, `&`, `?` 까지 인코딩한다.
반면 `encodeURI` 함수는 매개변수로 전달된 문자열을 완전한 URI 전체라고 간주한다.  
따라서 쿼리 스트리이 구분자로 사용되는 `=`, `&`, `?` 는 인코딩하지 않는다.

``` js
// URI의 쿼리 스트링
const uriComp = 'name=이용우&job=programmer&teacher';

// encodeURIComponent 함수는 인수로 전달받은 문자열을 URI의 구성요소인 쿼리 문자열의 일부로 간주하여 인코딩한다.
const encoded = encodeURIComponent(uriComp);
console.log(encoded); // name=%EC%9D%B4%EC%9A%A9%EC%9A%B0&job=programmer&teacher

// decodeURIComponent 함수는 인코딩된 문자열을 인수로 전달받아 이스케이프 처리를 위해 디코딩한다.
const decoded = decodeURIComponent(encoded);
console.log(decoded); // name=이용우&job=programmer&teacher
```

<br><br>

### 암묵적 전역
``` js
var x = 10;

function foo() {
    // 선언하지 않은 식별자에 값을 할당
    y = 20;
}   

foo();

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(y); // 20
```

foo 함수 내의 y는 선언하지 않은 식별자이다.  
따라서 `y = 20`이 실행되면 참조 에러가 발생할 것처럼 보인다.

하지만 선언하지 않은 식별자 y는 마치 전역 변수처럼 동작한다.  
이는 선언하지 않은 식별자에 값을 할당하면 전역 객체의 프로퍼티가 되기 때문이다.  

<br>

`foo` 함수가 호출되면 자바스크립트 엔진은 y 변수에 값을 할당하기 위해  
먼저 스코프 체인을 통해 선언된 변수인지를 확인한다.  

이때 foo 함수의 스코프와 전역 스코프 어디에도 y 변수의 선언을 찾을 수 없으므로 참조 에러가 발생한다.  
하지만 자바스크립트 엔진은 `y = 20`을 `window.y = 20`으로 해석하여 전역 객체에 프로퍼티를 동적으로 생성한다.  

결국 y는 전역 객체의 프로퍼티가 되어 마치 전역 변수처럼 동작하는데,  
이를 암묵적 전역이라고 한다.

> 다만 `y`는 변수 선언 없이 단지 전역 객체의 프로퍼티로 추가되었을 뿐이다.  
> 따라서 `y`는 변수가 아니다. (변수가 아니므로 변수 호이스팅이 발생하지 않는다)


<br>

``` js
// 전역 변수 x는 호이스팅이 발생한다.
console.log(x);     // undefined

// 전역 변수가 아니라 단지 전역 객체의 프로퍼티인 y는 호이스팅이 발생하지 않는다.
console.log(y);     // ReferenceError: y is not defined

var x = 10;

function foo() {
    // 선언하지 않은 식별자에 값을 할당
    y = 20; // window.y = 20
}

foo();

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x + y); // 30  
```

<br><br>

또한 변수가 아니라 단지 프로퍼티인 `y`는 `delete` 연산자로 삭제가 가능하다.   
전역 변수는 프로퍼티이지만 `delete` 연산자로 삭제할 수 없다.

``` js
var x = 10;     // 전역 변수

function foo() {
    // 선언하지 않은 식별자에 값을 할당
    y = 20; // window.y = 20
    console.log(x + y);
}

foo();      // 30

console.log(window.x); // 10
console.log(window.y); // 20

delete x; // 전역 변수는 삭제되지 않는다.
delete y; // 프로퍼티는 삭제된다.

console.log(window.x); // 10
console.log(window.y); // undefined

```