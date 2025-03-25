### Chapter 26 - ES6 함수의 추가 기능

ES6 이전의 함수는 동일한 함수라도 다양한 형태로 호출할 수 있다.

``` js
var foo = function () {
    return 1;
};

// 일반적인 함수로서 호출
foo();

// 생성자 함수로서 호출
new foo();

// 메소드로서 호출
var obj = { foo: foo };
obj.foo();
```

위와 같이 ES6 이전의 함수는 사용 목적에 따라 명확히 구분되지 않는다.  
> ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서도 호출할 수 있다.  
> 즉, `callable`이면서 `constructor` 이다.

<br>

이런 방식은 사용 목적에 따라 명확한 구분이 없으므로  
호출 방식에 특별한 제약이 없고 생성자 함수로 호출되지 않아도 프로토타입 객체를 생성한다.  

이 문제를 해결하기 위해 ES6에서는 함수를 사용 목적에 따라 세 가지 종류로 명확히 구분하였다.

|ES6 함수 구분|`constructor`|`prototype`|`super`|`arguments`|
|:--|:--|:--|:--|:--|
|일반 함수(Normal)|O|O|X|O|
|메소드(Method)|X|X|O|O|
|화살표 함수(Arrow)|X|X|X|X|

일반 함수는 함수 선언문이나 함수 표현식으로 정의한 함수를 의미한다.  
단, ES6에서 메소드와 화살표 함수는 ES6 이전의 함수와 명확한 차이가 있다.

일반 함수는 `constructor` 이지만 ES6의 메소드와 화살표 함수는 `non-constructor`이다.

- `constructor`: `new` 키워드를 사용하여 인스턴스 생성 가능
- `non-constructor`: `new` 키워드로 인스턴스를 생성할 수 없음

<br><br>

### 메소드
**ES6 사양에서 메소드는 메소드 축약 표현으로 정의된 함수만을 의미한다.**

``` js
const obj = {
    x: 1
    // foo는 메소드이다.
    foo() {return this.x;},

    // bar에 바인딩된 함수는 메소드가 아닌 일반 함수이다.
    bar: function() { return this.x; }
}
```

ES6 사양에서 정의된 메소드는 인스턴스를 생성할 수 없는 `non-constructor`이다.  
따라서 ES6 메소드는 생성자 함수로서 호출할 수 없다.

``` js
new obj.foo();      // TypeError: obj.foo is not a constructor
new obj.bar();      // bar {}
```

ES6 메소드는 인스턴스를 생성할 수 없으므로 `prototype` 프로퍼티가 없고 프로토타입도 생성하지 않는다.

``` js
// obj.foo는 constructor가 아닌 ES6 메소드이므로 prototype 프로퍼티가 없다.
obj.foo.hasOwnProperty('prototype'); // false

// obj.bar는 constructor이므로 prototype 프로퍼티가 있다.
obj.bar.hasOwnProperty('prototype'); // true
```

<br><br>

ES6 메소드는 자신읠 바인딩한 객체를 가리키는 내부 슬롯 `[[HomeObject]]`를 갖는다.  
`super` 참조는 내부 슬롯 `[[HomeObject]]`를 참조하여 수퍼클래스의 메소드를 참조하므로  
ES6 메소드는 `super` 키워드를 사용할 수 있다.

<br>

``` js
const base = {
    name: 'Lee',
    sayHi() {
        return `Hi ${this.name}`;
    }
}

const derived = {
    __proto__: base,
    sayHi() {
        return `${super.sayHi()}. how are you doing?`;
    }
}

console.log(derived.sayHi()); // Hi Lee. how are you doing?
```

<br>

반대로 ES6 메소드가 아닌 함수는 `super` 키워드를 사용할 수 없다.

``` js
const derived = {
    __proto__: base,
    sayHi: function() {
        // SyntaxError: 'super' keyword unexpected here
        return `${super.sayHi()}. how are you doing?`;  
    }
}
```

<br>

ES6 메소드는 본연의 기능(super)를 추가하고  
의미적으로 맞지 않는 기능(constructor)은 제거했다.  

따라서 메소드를 정의할 때 프로퍼티 값으로 익명 함수 표현식을 할당하는 ES6 이전의 방식은 사용하지 않는 것이 좋다.

``` js
// 1. ES6 이전의 방식 (권장하지 않음)
const person = {
    name: 'Lee',
    sayHi: function() {        // 익명 함수 표현식을 할당
        console.log(`Hi, ${this.name}`);
    }
};

// 2. ES6의 메소드 정의 (권장되는 방식)
const person2 = {
    name: 'Lee',
    sayHi() {                  // 메소드 축약 표현
        console.log(`Hi, ${this.name}`);
    }
};
```

<br><br>

### 화살표 함수
화살표 함수는 function 키워드 대신 화살표(=>)를 사용하여 선언한다.  
<font color='orange'>화살표 함수는 콜백 함수 내부에서 `this`가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 주로 사용된다.</font>

<br>

``` js
// 예제 1
const arrow = (x, y) => x + y;

// 예제 2
const arrow = x => x * x;

// 예제 3
const arrow = () => 10;

// 예제 4
const arrow = x => {
    return x * x;
};

// 예제 5: 객체 리터럴 반환 시 소괄호 () 로 감싸주어야 함
const create = (name, age) => ({ name, age });

// 예제 6
const create = (name, age) => {
    return { name, age };
};
```

<br>

화살표 함수도 일급 객체이기 때문에 `Array.prototype.map`, `Array.prototype.filter`, `Array.prototype.reduce` 등의 고차 함수에 인수로 전달할 수 있다.

``` js
[1, 2, 3].map(v => v * v);
```

<br><br>

#### 화살표 함수와 일반 함수와의 차이
화살표 함수와 일반 함수의 차이는 다음과 같다.

1. 화살표 함수는 인스턴스를 생성할 수 없는 `non-constructor`이다.
2. 중복된 매개변수 이름을 선언할 수 없다.
3. 화살표 함수는 함수 자체의 `this`, `arguments`, `super`, `new.target`을 바인딩하지 않는다.


<br><br>

#### this
화살표 함수가 일반 함수와 구별되는 가장 큰 특징은 바로 `this`이다.  
<font color='orange'>그리고 화살표 함수는 다른 함수의 인수로 전달되어 콜백 함수로 사용되는 경우가 많다.</font>

화살표 함수의 `this`는 일반 함수의 `this`와 다르게 동작한다.  
> `this`에 바인딩될 객체는 정적으로 결정되는 것이 아닌,  
> 함수가 호출될 때 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.


<br>

화살표 함수는 함수 자체의 `this` 바인딩을 가지지 않는다.  
따라서 화살표 함수 내부에서 `this`를 참조함녀 상위 스코프의 `this`를 참조한다.  
이를 `lexical this`라 한다.

``` js
class Prefixer {
    constructor(prefix) {
        this.prefix = prefix;
    }

    add(arr) {
        return arr.map(item => this.prefix + item);
    }
}

const prefixer = new Prefixer('Hello');
console.log(prefixer.add(['Lee', 'Kim']));  // ['Hello Lee', 'Hello Kim']
```

이는 마치 렉시컬 스코프와 같이  
<font color='orange'>화살표 함수의 `this`가 함수가 정의된 위치에 의해 결정된다는 것을 의미한다.</font>

<br><br>

클래스 필드에 할당된 화살표 함수는 프로토타입 메소드가 아니니 인스턴스 메소드가 된다.  
따라서 메소드를 정의할 때 ES6 메소드 축약 표현으로 정의한 ES6 메소드를 사용하는 것이 좋다.

``` js
class Person {
    // 클래스 필드 정의
    name = 'Lee';

    sayHi() {
        console.log(`Hi ${this.name}`);
    }
}

const me = new Person();
me.sayHi();  // Hi Lee
```

<br><br>


### Rest 파라미터
Rest 파라미터는 매개변수 이름 앞에 세개의 점(`...`)을 붙여서 정의한 매개변수를 의미한다.  
<font color='orange'>`Rest` 파라미터는 함수에 전달된 인수의 목록을 배열로 전달받는다.</font>

> 파이썬으로 치면 `*args` 이다.

```js
function foo(...rest) {
    console.log(rest);
}

foo(1, 2, 3, 4, 5);  // [1, 2, 3, 4, 5]
```

<br>

일반 매개변수와 Rest 파라미터는 함께 사용할 수 있다.  
이때 함수에 전달된 인수들은 매개변수와 Rest 파라미터에 순차적으로 할당된다.

``` js
function foo(param, ...rest) {
    console.log(param);  // 1
    console.log(rest);   // [2, 3, 4, 5]
}

foo(1, 2, 3, 4, 5);
```

<br>

Rest 파라미터는 반드시 마지막 파라미터이어야 한다.  
그렇지 않으면 문법 에러가 발생한다.




<br><br>

### 매개변수 기본값
함수를 호출할 때 매개변수의 개수만큼 인수를 전달하는 것이 바람직하지만  
그렇지 않은 경우에도 에러를 발생하지 않는다.   
이는 자바스크립트 엔진이 매개변수의 개수와 인수의 개수를 체크하지 않기 때문이다.  

인수가 전달되지 않은 매개변수의 값은 `undefined`이다.  

ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있다.

``` js
function sum(x = 0, y = 0) {
    return x + y;
}

console.log(sum(1, 2));  // 3
console.log(sum(1));    // 1
console.log(sum());     // 0
```



