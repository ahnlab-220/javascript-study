# Chapter 18 - 함수와 일급 객체
아래의 조건을 만족하는 객체를 일급 객체라고 합니다.
- 무명의 리터럴로 생성이 가능하다. (= 런타임에 생성이 가능하다)
    - 코드 내에서 특정 값을 바로 생성하거나 정의할 수 있는 방식
- 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
- 함수의 매개변수에 전달할 수 있다. (= 매개변수로 함수를 받을 수 있다)
- 함수의 반환값으로 사용할 수 있다. (= 리턴값으로 함수를 리턴할 수 있다)

<br>

자바스크립트의 함수는 위 조건을 모두 만족하므로 일급 객체입니다.


``` js
// 1. 함수는 무명의 리터럴로 생성 가능하다.
// 2. 함수는 변수에 저장할 수 있다.
const increase = function (num) {
    return ++num;
};
const decrease = function (num) {
    return --num;
};

// 2. 함수는 객체에 저장할 수 있다.
const auxs = function (num) {
    return --num;
};

// 3. 함수의 매개변수에 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다.
function makeCounter(aux) {
    let num = 0;
    return function () {
        num = awx(num);
        return num;
    };
}

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const increase = makeCounter(auxs.increase);
console.log(increaser());   // 1
console.log(increaser());   // 2
```


<br>

일급 객체로서 함수가 가지는 가장 큰 특징은  
일반 객체와 같이 함수의 매개변수에 전달할 수 있으며, 함수의 반환값으로도 사용할 수 있다는 것입니다.   
이는 함수형 프로그래밍을 가능하게 하는 자바스크립트의 장점 중 하나입니다.

<br><br>

### 함수 객체의 프로퍼티
함수는 객체입니다. 따라서 함수도 프로퍼티를 가질 수 있습니다.  

``` js
function square(number) {
    return number * number;
}

console.dir(square);
```
<img src="https://github.com/user-attachments/assets/5310582d-f0b8-4dfa-8815-aaa4cc31cb70" width=350>


<br>

`square` 함수의 모든 프로퍼티에 대해  
프로퍼티 어트리뷰트를 `Object.getOwnPropertyDescriptors` 메소드로 확인해보면 다음과 같습니다.

``` js
Object.getOwnPropertyDescriptors(square);
```

<img src="https://github.com/user-attachments/assets/9f3e6d8d-3688-4056-8c35-cc56af9fd00b" width=500>

<br>

``` js
// __proto__는 square 함수의 프로퍼티가 아니다.
Object.getOwnPropertyDescriptor(square, '__proto__');   // undefined

// __proto__는 Object.prototype 객체의 접근자 프로퍼티이다.
// square 함수는 Object.prototype 객체로부터 __proto__ 접근자 프로퍼티를 상속받는다.
Object.getOwnPropertyDescriptor(Object.prototype, '__proto__');
// {enumerable: false, configurable: true, get: ƒ, set: ƒ}
```

<br>

이와 같이 `arguments`, `caller`, `length`, `name`, `prototype` 프로퍼티는 모두 함수 객체의 데이터 프로퍼티이다. <font color='orange'>이 프로퍼티들은 일반 객체에는 없는 함수 객체 고유의 프로퍼티입니다.</font>

다만, `__proto__`는 접근자 프로퍼티이며, 함수 객체 고유의 프로퍼티가 아니라 `Object.prototype` 객체의 프로퍼티를 상속받은 것을 알 수 있습니다.  

`Object.prototype` 객체의 프로퍼티는 모든 객체가 상속받아 사용할 수 있습니다.  
즉, `Object.prototype` 객체의 `__proto__` 접근자 프로퍼티는 모든 객체가 사용할 수 있습니다.
> 상속은 19장 "프로토타입"에서 자세히 다룰 것이다.


<br><br>

#### arguments 프로퍼티
함수 객체의 `arguments` 프로퍼티 값은 `arguments` 객체입니다.  
`arguments` 객체는 함수 호출 시 전달된 인수(`argument`)들의 정보를 가지고 있는 순회 가능한(`iterable`) 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용됩니다.  

자바스크립트는 함수의 매개변수와 인수의 개수가 일치하는지 확인하지 않습니다.  
따라서 함수 호출 시 매개변수 개수만큼 인수를 전달하지 않아도 에러가 발생하지 않습니다.  

``` js
function multiply(x, y) {
    console.log(arguments);
    return x * y;
}

console.log(multiply());        // NaN
console.log(multiply(1));       // NaN
console.log(multiply(1, 2));    // 2
console.log(multiply(1, 2, 3)); // 2

```

<br>

다만 초과된 인수가 그냥 버려지는 것은 아닙니다.  
모든 인수는 암묵적으로 `arguments` 객체의 프로퍼티로 보관됩니다.  

<img src="https://github.com/user-attachments/assets/165a3e1e-0322-49f5-9f40-be9eb2ffe31c" width=400>

<br>

`arguments` 객체는 인수를 프로퍼티 값으로 소유하며 프로퍼티 키는 인수의 순서를 나타냅니다.   
`arguments` 객체의 `callee` 프로퍼티는 호출되어 함수 자기자신을 가리키고  
`arguments` 객체의 `length` 프로퍼티는 인수의 개수를 가리킵니다.


<br>

※ arguments 객체의 Symbol(Symbol.iterator) 프로퍼티  
`arguments` 객체의 `Symbol(Symbol.iterator)` 프로퍼티는 `arguments` 객체를  
순회 가능한 자료구조인 이터러블(`iterable`)로 만들기 위한 프로퍼티입니다.

``` js
function multiply(x, y) {d
    // itertor
    const iterator = arguments[Symbol.iterator]();

    // iterator의 next() 메소드를 호출하여 이터러블 객체 `arguments`를 순회
    console.log(iterator.next());       // {value: 1, done: false}
    console.log(iterator.next());       // {value: 2, done: false}
    console.log(iterator.next());       // {value: 3, done: false}
    console.log(iterator.next());       // {value: undefined, done: true}

    return x * y;
}

multiply(1, 2, 3);
```

<br>

`arguments` 객체는 매개변수 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 특히 유용합니다.  

``` js
function sum() {
    let res = 0;

    for (let i = 0; i < arguments.length; i++) {
        res += arguments[i];
    }
    return res;
}

console.log(sum());         // 0
console.log(sum(1, 2));     // 3
console.log(sum(1, 2, 3));  // 6
```

<br>

arguments 객체는 유사 배열 객체이기 때문에  
배열과 같이 사용하고 싶다면, `Function.prototype.call`, `Function.prototype.apply`와 같이 간접 호출해야 하는 번거로움이 있습니다.

``` js
function sum(){
    // arguments 객체를 배열로 반환
    const array = Array.prototype.slice.call(arguments);
    return array.reduce(function (pre, cur) {
        return pre + cur;
    }, 0);
}

console.log(sum(1, 2));             // 3
console.log(sum(1, 2, 3, 4, 5));    // 15
```

<br>

이런 번거로움을 해결하기 위해 ES6에서는 Rest 파라미터가 도입되었습니다.

``` js
function sum( ...args) {
    return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2));
console.log(sum(1, 2, 3, 4, 5));
```

<br><br>

#### caller 프로퍼티
`caller` 프로퍼티는 ECMAScript 사양에 포함되지 않은 비표준 프로퍼티입니다.  
이후 표준화될 예정이 없는 프로퍼티이므로 사용하지 않고 참고만 하는 것을 추천합니다.

`caller` 프로퍼티는 함수 자신을 호출한 함수를 가리킵니다.

``` js
function foo(func) {
    return func();
}

function bar() {
    return 'caller : ' + bar.caller;
}

console.log(foo(bar));      // caller : function foo(func) {...}
console.log(bar());         // caller : null
```

<br><br>

#### length 프로퍼티
함수 객체의 `length` 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킵니다.

``` js
function foo() {}
console.log(foo.length);        // 0

function bar(x) {
    return x;
}
console.log(bar.length);        // 1

function bar2(x, y) {
    return x * y;
}
console.log(bar2.length);       // 2
```


<br><br>

#### name 프로퍼티
<font color='orange'>함수 객체의 `name` 프로퍼티는 함수 이름을 나타냅니다.</font>  
`name` 프로퍼티는 ES5에서는 익명 함수의 경우 빈 문자열을 값으로 갖지만,  
ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖습니다.

``` js
// 가명 함수 표현식
const namedFunc = function foo() ();
console.log(namedFunc.name);        // foo

// 익명 함수 표현식
const anonymousFunc = function() {};
console.log(anonymousFunc.name);        // anonymousFunc

// 함수 선언문(Function declaration)
function bar() {}
console.log(bar.name);      // bar
```


<br><br>


#### __proto__ 접근자 프로퍼티
모든 객체는 `[[Prototype]]` 이라는 내부 슬롯을 가집니다.  
`__proto__` 프로퍼티는 `[[Prototype]]` 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티입니다.  
내부 슬롯에는 직접 접근할 수 없고 간접적인 접근 방식을 제공하는 경우에 한하여 접근할 수 있습니다.  

``` js
conse obj = { a: 1 };

// 객체 리터럴 방식으로 생성한 객체의 프로토토압 객체는 Object.prototype 이다.
console.log(obj.__proto__ === Object.prototype);        // true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
console.log(obj.hasOwnProperty('a'));           // true
console.log(obj.hasOwnProperty('__proto__'));   // false
```


<br><br>

#### prototype 프로퍼티
`prototype` 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 `constructor`만이 소유하는 프로퍼티입니다.  
일반 객체와 생성자 함수로 호출할 수 없는 `non-constructor`에는 `prototype` 프로퍼티가 없습니다.

``` js
// 함수 객체는 `prototype` 프로퍼티를 소유한다.
(function() {}).hasOwnProperty('prototype');        // true

// 일반 객체는 `prototype` 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype');           // false
```

<br>

`prototype` 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때,  
생성자 함수가 생성할 인스턴스의 프로토타입 객체를 생성합니다.

``` js
function Person(name) {
    this.name = name;
}

const person1 = new Person("Lee");
```