# Chapter 18 - 함수와 일급 객체
## 1. 일급 객체
일급 객체란, 자바스크립트에서 다음 같은 조건을 만족하는 객체를 뜻한다.
1. 무명의 리터럴로 생성할 수 있다. 즉, **런타임에 생성**이 가능하다.
2. **변수나 자료구조**(객체, 배열 등)**에 저장**할 수 있다.
3. 함수의 **매개변수에 전달**할 수 있다.
4. 함수의 **반환값으로 사용**할 수 있다.

**자바스크립트의 함수는** 위의 조건을 모두 만족하므로 **일급 객체**다.<br/>
즉, 함수는 **값을 사용할 수 있는 곳**(변수 할당문, 객체의 프로퍼티 값, 배열의 요소, 함수 호출의 인수, 함수 반환문)이라면<br/>
**어디서든지 리터럴로 정의**할 수 있으며, **런타임에 함수 객체로 평가**된다.

이러한 자바스크립트 함수의 특징은, 함수형 프로그래밍을 가능케 하며 자바스크립트의 장점 중 하나다.


## 2. 함수 객체의 프로퍼티
함수는 객체다. 따라서 함수도 프로퍼티를 가진다.<br/>
함수 객체의 프로퍼티를 하나씩 살펴보자.

### 2-1. `arguments` 프로퍼티
함수 객체의 `arguments` 프로퍼티 값은, `arguments` 객체다.<br/>
**`arguments` 객체**는 함수 호출 시 전달된 **인수(arguments)들의 정보**를 담은, **순회 가능한(iterable) 유사 배열 객체**이며,<br/>
함수 내부에서 지역변수처럼 사용한다. 즉, **함수 외부에서는 참조할 수 없다**.

![image](https://github.com/user-attachments/assets/388943be-09ef-42b1-9841-187967fe6595)

위 콘솔 사진에서 빨간 네모 표시된 부분이 인수의 정보가 보여지는 부분이다.<br/>
`arguments` 객체는 다음과 같이 매개변수 개수를 확정할 수 없는, **가변 인자 함수를 구현할 때 유용**하다.

```javascript
function sum() {
  let res = 0;

  // arguments 객체는 length 프로퍼티가 있는 유사 배열 객체이므로, for문으로 순회할 수 있다.
  for (let i = 0; i < arguments.length; i++) {
    res += arguments[i];
  }

  return res;
}

console.log(sum())      // 0
console.log(sum(1, 2))    // 3
console.log(sum(1, 2, 3))  // 6
```

`arguments` 객체는 앞서 말했듯 실제 배열이 아닌, **array-like object(유사 배열 객체)이다**.<br/>
유사 배열 객체란, length 프로퍼티를 지니며 for문으로 순회할 수 있는 객체를 말한다.<br/>
(이터러블이 도입된 ES6부터 `arguments` 객체는 유사 배열 객체이면서 **이터러블**(이후 34장 '이터러블' 참고)이기도 하다.)

### 2-2. `caller` 프로퍼티
`caller` 프로퍼티는 ECMAScript 사양에 포함하지 않는 비표준 프로퍼티이므로 생략한다.

### 2-3. `length` 프로퍼티
함수 객체의 `length` 프로퍼티는 다음 예제와 같이 **함수를 정의할 때 선언한 매개변수의 개수**를 가리킨다.
```javascript
function foo() {}
console.log(foo.length);  // 0

function bar(x) {
  return x;
}
console.log(foo.length);  // 1

function baz(x, y) {
  return x * y
}
console.log(foo.length);  // 2
```
**`arguments` 객체**의 `length` 프로퍼티와, **함수 객체**의 `length` 프로퍼티는 다른 개념인 걸 반드시 숙지하자.<br/>
전자는 **arguments(인자)의 개수**를 가리키고, 후자는 **parameters(매개변수)의 개수**를 가리킨다.

### 2-4. `name` 프로퍼티
함수 객체의 `name` 프로퍼티는 아래 예제처럼 **함수 이름을 나타낸다**. ES6부터 정식 표준이 되었다.<br/>
주의해야 할 점은, `name` 프로퍼티는 **ES5와 ES6에서 동작을 달리한다**는 점이다.<br/>
ES5에서는 익명 함수 표현식의 `name` 프로퍼티의 값이 **빈 문자열**이고, ES6에서는 함수 객체를 가리키는 **식별자**다.

```javascript
// 기명 함수 표현식
var namedFunc = function foo() {};
console.log(namedFunc.name);  // foo

// 익명 함수 표현식
var anonymousFunc = function() {};
// ES5: name 프로퍼티의 값이 빈 문자열이다.
// ES6: name 프로퍼티의 값이 함수 객체를 가리키는 식별자다.
console.log(anonymousFunc.name);  // anonymousFunc

// 함수 선언문
function bar() {};
console.log(bar.name);  // bar
```

### 2-5. `__proto__` 접근자 프로퍼티
모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다. [[Prototype]]은 프로토타입 객체를 가리킨다. (프로토타입에 관한 자세한 내용은 19장 '프로토타입' 참고)<br/>
`__proto__`는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하고자 사용하는 **접근자 프로퍼티**다.

[[Prototype]] 내부 슬롯에는 직접 접근할 수 없고, `__proto__` 접근자 프로퍼티를 통해 **간접적으로** 프로토타입 객체에 접근할 수 있다.

### 2-6. `prototype` 프로퍼티
`prototype` 프로퍼티는 **생성자 함수가 생성할 인스턴스의 프로토타입 객체**를 가리킨다.

`prototype` 프로퍼티는 **constructor만이 소유하는** 특별한 프로퍼티다.<br/>
따라서 일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 `prototype` 프로퍼티가 없다.
```javascript
// 함수 객체는 prototype 프로퍼티를 갖는다.
(function () {}).hasOwnProperty('prototype');  // true

// 일반 객체는 prototype 프로퍼티를 갖지 않는다.
({}).hasOwnProperty('prototype');  // false
```

끝.
