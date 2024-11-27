# Chapter 15 - `let`, `const` 키워드와 블록 레벨 스코프

## 1. `var` 키워드의 문제점
`var` 키워드로 선언한 변수는 다음과 같은 특징이 있으며, 주의를 기울이지 않으면 심각한 문제를 발생시킬 수 있다.

### 1-1. 변수 중복 선언을 허용
`var` 키워드로는 아래처럼 **중복 선언을 할 수 있다**.<br>
`var` 키워드를 사용할 때 이미 선언된 변수를 모르고 같은 이름으로 중복 선언을 할 경우<br>
의도치 않게 먼저 선언한 변수 값을 변경해 버리는 부작용이 발생할 수 있다.
```javascript
var x = 1;
var y = 1;

// 같은 스코프에서 중복 선언을 할 수 있다.
var x = 100;  // 이렇게 초기화문이 있는 변수 선언문은 마치 var 키워드가 없는 것처럼 동작한다.
var y;  // 이렇게 초기화문이 없으면 그냥 무시됨.

console.log(x);  // 100
console.log(y);  // 1
```

### 1-2. 함수 레벨 스코프
`var`로 선언한 변수는 오직 **함수의 코드 블록만을 지역 스코프로 인정**한다.<br>
따라서 아래처럼 함수 외 코드 블록에서 `var`로 선언한 어떤 다른 전역 변수와 같은 이름으로 변수를 `var`로 선언한다면,<br>
그 변수는 새로 선언한 변수가 아닌 **중복 선언한 전역 변수가 될 뿐**이다.

이렇듯 함수 레벨 스코프를 따르는 `var`를 사용하면 전역 변수를 남발할 수 있다는 문제가 있다.
```javascript
var i = 1;

for (var i = 0; i < 5; i++) {
  // 여기서 i는 이미 위에 선언한 전역 변수 i와 이름이 같으므로, 새로운 변수가 아닌 중복 선언한 전역 변수가 된다.
  console.log(i);  // 0 1 2 3 4
}

console.log(i);  // 5
```

### 1-3. 변수 호이스팅
`var`로 선언한 변수는 **변수 호이스팅**에 의해 변수 선언문이 스코프의 선두로 끌어올려진 것처럼 동작한다.<br>
즉, `var`로 선언한 변수는 **변수 선언문 이전에 참조**할 수 있으며, `undefined`로 참조된다.

이렇게 변수 선언문 이전에 변수를 에러 없이 참조하게 허용하는 `var`의 특징은,<br>
프로그램의 흐름상 맞지 않을뿐더러 **가독성을 떨어뜨리고 오류를 발생시킬 여지**를 남긴다.
```javascript
// 이 시점, 이미 변수 호이스팅에 의해 foo 선언문이 실행돼 undefined로 초기화된다. (1. 선언, 2. 초기화 완료)
console.log(foo);  // undefined

foo = 123;  // (3. 할당 완료)

console.log(foo);  // 123

var foo;
```

## 2. `let` 키워드
앞에서 살펴본 `var`의 단점을 보완하기 위해<br>
자바스크립트는 ES6부터 새로운 변수 선언 키워드 `let`과 `const`를 도입했다.<br>
`var`과 `let`의 차이점을 살펴보자.

### 2-1. 변수 중복 선언을 금지함
`var`과는 달리 `let`으로는 **같은 스코프 안에서 중복 선언을 허용하지 않는다**.<br>
`let`으로 이름이 같은 변수를 중복으로 선언하면 `SyntaxError`가 발생한다.
```javascript
var foo = 123;
var foo = 456;  // 이 선언문은 자바스크립트 엔진에 의해 마치 var 키워드가 없는 것처럼 동작한다. 즉 에러가 발생하지 않는다.

let bar = 123;
let bar = 456;  // SyntaxError: Identifier 'bar' has already been declared
```

### 2-2. 블록 레벨 스코프
`var`과는 달리 `let`으로 선언한 변수는 함수 코드 블록뿐만이 아닌<br>
**모든 코드 블록을 지역 스코프로 인정하는 block-level scope**(블록 레벨 스코프)를 따른다.

아래 예제에서, 코드 블록 밖에서 선언한 `foo` 변수와 안에서 선언한 `foo` 변수는 서로 다른 변수인 것을 볼 수 있다.<br>
그리고 `bar` 변수도 블록 레벨 스코프를 갖는 지역 변수이므로 전역에서는 `bar` 변수를 참조할 수 없다.
```javascript
let foo = 1;

{
  let foo = 2;
  let bar = 3;
}

console.log(foo);  // 1
console.log(bar);  // ReferenceError: bar is not defined
```
아래 그림처럼, 여러 개의 코드 블록이 중첩하면 각 코드 블록마다의 스코프를 지닌다.<br>
<img width="400px" src="https://github.com/user-attachments/assets/144c5a28-56ff-4e9f-ba5e-667cc4a355ac" />

### 2-3. 변수 호이스팅
`var`과는 달리 `let`으로 선언한 변수는 **마치 변수 호이스팅이 발생하지 않는 것처럼 동작**한다.<br>
```javascript
console.log(foo);  // ReferenceError: foo is not defined
let foo;
```

4장에서 배운 바로는 `var`로 선언한 변수는 자바스크립트 엔진에 의해<br>
**런타임 이전에 암묵적으로 "선언 단계"와 "초기화 단계"를 한 번에 실행**한다.

그러나 `let`으로 선언한 변수는 이 두 단계를 분리한다.<br>
즉, 런타임 이전에 암묵적으로 선언 단계를 먼저 실행하지만, **초기화 단계는 변수 선언문에 도달했을 때 실행**한다.

<img width="400px" src="https://github.com/user-attachments/assets/b4a430e9-0b5e-4b26-bb66-58bdcd7a2a9b" />

만약 초기화 단계에 다다르기 전에 변수에 접근하려고 하면 `ReferenceError`가 발생한다.<br>
왜냐하면 `let`으로 선언한 변수는 스코프의 시작 지점부터 변수 선언문까지 변수를 참조할 수 없기 때문이다.<br>
이렇게 스코프의 시작 지점부터 초기화 시작 지점까지, 변수를 참조할 수 없는 구간을 **TDZ**(**Temporal Dead Zone**, 일시적 사각지대)라고 부른다.

그렇다면 `let`으로 선언한 변수는 호이스팅이 발생하지 않는 것일까? **그건 아니다**.<br>
자바스크립트는 모든 선언(`var`, `let`, `const`, `function`, `function*`, `class` 등)을 호이스팅한다.<br>
단, ES6에서 도입한 `let`, `const`, `class`를 사용한 선언문은 **TDZ로 인해 호이스팅이 발생하지 않는 것처럼 동작할 뿐**이다.

### 2-4. 전역 객체와 `let`
`var`로 선언한 전역 변수, 전역 함수, 그리고 선언하지 않는 변수에 값을 할당한 Implicit Globals(암묵적 전역)은,<br>
**전역 객체 `window`의 프로퍼티가 된다**. (`window`의 프로퍼티를 참조할 땐 `window`를 생략할 수 있다.)
```javascript
// 이 예제는 브라우저 환경에서 실행해야 함.

var x = 1;  // 전역 변수
y = 2;  // implicit globals
function foo() {}  // 전역 함수

// var로 선언한 전역 변수는, 전역 객체 window의 프로퍼티가 된다.
console.log(window.x);  // 1
console.log(x);  // 1

// implicit globals는, 전역 객체 windows의 프로퍼티가 된다.
console.log(window.y);  // 2
console.log(y);  // 2

// 함수 선언문으로 정의한 전역 함수는, 전역 객체 windows의 프로퍼티가 된다.
console.log(window.foo);  // f foo() {}
console.log(foo);  // f foo() {}
```

그러나 `let`으로 선언한 전역 변수는 **전역 객체 windows의 프로퍼티가 되지 않는다**.<br>
```javascript
// 이 예제는 브라우저 환경에서 실행해야 함.

let x = 1;

// let, const 키워드로 선언한 전역 변수는, 전역 객체 windows의 프로퍼티가 되지 않는다.
console.log(window.x);  // undefined
console.log(x);  // 1
```

## 3. `const` 키워드
`const` 키워드는 constant(상수) 선언을 위한 키워드지만, 사실 반드시 상수만을 위해 사용하지는 않는다.<br>
`const`의 특징은 `let`의 특징과 대부분 비슷하므로 `let`과의 차이점을 위주로 `const`를 살펴보자.

### 3-1. 선언과 초기화
`const`로 변수를 선언할 땐 **반드시 선언과 동시에 초기화를 해야 한다.** 그렇지 않으면 `SyntaxError`가 발생한다.
```javascript
const foo = 1;
const foo;  // SyntaxError: Missing initializer in const declaration
```

그리고 `const`로 선언한 변수는 `let`과 마찬가지로 **block-level scope**(블록 레벨 스코프)를 가지며,<br>
아래와 같이 마치 변수 호이스팅이 발생하지 않는 것처럼 동작한다.
```javascript
{
  console.log(foo);  // ReferenceError: Cannot access 'foo' before initialization
  const foo = 1;
  console.log(foo);  // 1
}

console.log(foo);  // ReferenceError: foo is not defined
```

### 3-2. 재할당을 금지함
`var`나 `let`으로 선언한 변수와는 다르게, `const`로 선언한 변수는 재할당을 할 수 없다.
```javascript
const foo = 1;
foo = 2;  // TypeError: Assignment to constant variable.
```

### 3-3. 상수
`const`로 선언한 변수에 원시 값을 할당한 경우,<br>
(원시 값은 immutable value이고, const 키워드는 재할당을 허용하지 않으므로) 할당값을 변경하는 방법은 없게 된다.<br>
따라서 상태 유지와 가독성, 유지보수의 편의를 위해 상수를 적극적으로 사용해야 하며, `const`를 사용하면 된다.

### 3-4. `const` 키워드와 객체
`const`로 선언한 변수에 원시 값을 할당한다면 값을 변경할 수 없지만, **객체를 할당한다면 값을 변경할 수 있다**.<br>
```
const person = {
  name: 'Lee'
}
person.name = 'Kim';
console.log(person);  // {name: "Kim"}
```
11장에서 살펴봤듯 `const`는 재할당을 금지할 뿐, "불변"을 의미하진 않는다.<br>
다시 말해 **새로운 값을 재할당하는 것은 불가능하지만 프로퍼티의 동적인 생성, 삭제, 프로퍼티 값의 변경은 가능**하다.<br>
이때 객체 내용이 변하더라도 변수에 할당한 **객체의 참조 값은 변하지 않는다**는 점을 주목하자.

## 4. `var` vs. `let` vs. `const`
변수를 선언할 땐 기본적으로 `const`를 사용하고, 재할당이 필요한 때만 `let`을 제한적으로 사용하는 것이 바람직하다.<br>
이 방식이 의도치 않은 재할당을 방지해 안전하기 때문이다. 우선 `const`로 선언하고, 재할당이 필요해지면 그때 `let`으로 바꿔도 늦지 않다.

`var`, `let` 그리고 `const` 권장 사용 방식을 정리하면 다음과 같다.
1. ES6를 사용한다면 `var`는 아예 사용하지 않는다.
2. 재할당이 필요한 때만 제한적으로 `let`을 사용한다. 이때 변수의 스코프는 최대한 좁게 만든다.
3. 재할당이 필요 없는 상수(변경할 예정이 없거나, 읽기 전용으로 사용할 값들)로 쓰일 원시 값과 객체는 `const`를 사용한다. 

끝.
