# Chapter 26 - ES6 함수의 추가 기능
## 1. 함수의 구분
ES6 이전까지 자바스크립트의 함수는 별다른 구분 없이 다양한 목적으로 사용되었다.<br/>
이는 편리한 것 같지만 실수를 유발시킬 수 있으며 성능 면에서도 손해다. 예제를 살펴 보자.
```javascript
var foo = function() {
  return 1;
};

// 일반적인 함수로 호출
foo();  // -> 1

// 생성자 함수로 호출
new foo();  // -> foo {}

// 메서드로 호출
var obj = { foo: foo };
obj.foo();  // -> 1
```

ES6 이전의 모든 함수는 사용 목적에 따라 명확한 구분이 없으므로<br/>
호출 방식에 특별한 제약이 없고 생성자 함수로 호출되지 않아도 프로토타입 객체를 생성한다.<br/>
자바스크립트 함수의 이러한 특징은 이해하기 혼란스럽고, 프로그램 성능에도 좋지 않다.

이러한 문제를 해결하기 위해, **ES6에서는 함수를 사용 목적에 따라 세 가지 종류로 구분**해 정의했다.

|**ES6 함수의 구분**|`constructor`|`prototype`|`super`|`arguments`|
|-|-|-|-|-|
|**일반 함수(Normal)**|O|O|X|O|
|**메서드(Method)**|X|X|O|O|
|**화살표 함수(Arrow)**|X|X|X|X|

일반 함수는 constructor이지만, ES6의 메서드와 화살표 함수는 non-constructor이다. 다음 절에서 더 자세히 살펴 보자.

## 2. ES6 Methods: 메서드
ES6 사양에서는 메서드에 대한 정의가 명확하게 규정되었다: **메서드 축약 표현으로 정의된 함수만이 '메서드'다.**
```javascript
const obj = {
  x: 1,
  foo() { return this.x; },  // ✅ foo는 '메서드'다.
  bar: function() { return this.x; }  // ❌ bar는 '메서드'가 아닌 일반 함수다.
};
```

ES6에서 정의한 '**메서드**'는, 인스턴스를 생성할 수 없는 non-constructor이다. 따라서 생성자 함수로서 호출할 수 없게 된다.<br/>
ES6 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.<br/>
또한, ES6 메서드는 자신을 바인딩한 객체를 가리키는 슬롯 `[[HomeObject]]`를 갖기 때문에, `super` 키워드를 사용할 수 있다.

이처럼 ES6 메서드는 본연의 기능인 `super`를 추가하고, 의미적으로 맞지 않는 기능인 constructor는 제거했다.

## 3. Arrow Functions: 화살표 함수
화살표 함수는, 기존의 함수와 비교해 표현만 간략한 게 아니라 **내부 동작도 간략**하다.<br/>
특히, 화살표 함수는 콜백 함수 내부에서 **`this`가 전역 객체를 가리키는 문제를 해결**한다.

### 3-1. 화살표 함수 정의
생략. 책을 참고합시다~

### 3-2. 화살표 함수와 일반 함수의 차이
1. 화살표 함수는 인스턴스를 생성할 수 없는 **non-constructor**이다.
2. 화살표 함수는 중복된 매개변수 이름을 선언할 수 없다.
3. 화살표 함수는 함수 자체의 `this`, `arguments`, `super`, `new.target` 바인딩을 **갖지 않는다**.

### 3-3. `this`
화살표 함수의 `this`는 일반 함수의 그것과 다르게 동작한다. 이건 의도된 설계다:<br/>
**콜백 함수 내부의 `this`가, 외부 함수의 `this`와 달라 혼란을 발생시키는 문제를 해결하기 위함**이다.

22장에서 배웠듯이 `this`의 값은 함수가 어떻게 호출됐는지에 따라 다르게 바인딩된다.<br/>
자바스크립트의 이러한 특징은, **콜백 함수를 일반 함수 형태로 호출할 때 골치 아파진다**.<br/>
일반 함수로 호출하는 모든 함수의 내부에는 `this`가 전역 객체를 가리키지만,<br/>
그 함수가 만약 클래스 내부라면 `undefined`를 바인딩하게 된다.<br/>
클래스 내부의 모든 코드에는 strict mode가 적용되는 영향을 받기 때문이다.
```javascript
// ❌ Wrong Practice!! ❌
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    return arr.map(function (item) {
      return this.prefix + item;  // TypeError: Cannot read property 'prefix' of undefined
    });
  }
}
```

이런 문제를 우리는 **콜백 함수 내부의 `this` 문제**라고 부른다.<br/>
ES6에서는, 콜백 함수 내부의 `this` 문제를 해결하기 위해 화살표 함수를 제안한다.

```javascript
// ✅ 위 예제를 화살표 함수로 해결 ✅
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    return arr.map(item => this.prefix + item);
  }
}
```

화살표 함수는 함수 자체의 `this` 바인딩을 갖지 않는다.<br/>
따라서 **화살표 함수 내부에서 `this`를 참조하면 상위 스코프의 `this`를 그대로 참조**한다.

우리는 이를 **Lexical This**라고 부른다.<br/>
Lexical this는 마치 렉시컬 스코프처럼, 화살표 함수의 `this`는 **함수가 정의된 위치에 의해 결정**되는 `this`임을 의미한다.

한 가지 유의할 점은 객체 내 메서드를 정의할 땐 화살표 함수를 사용하지 않아야 한다는 점이다.
```javascript
// ❌ Wrong Practice!! ❌
const person = {
  name: 'Lee',
  sayHi: () => console.log(`hey ${this.name}`)
};
```
위 예제가 틀린 이유는, sayHi 프로퍼티에 할당한 화살표 함수의 `this`는,<br/>
메서드를 호출한 객체인 person을 가리키는 게 아니라, 상위 스코프인 전역 객체를 가리키기 때문이다.

따라서 **객체의 메서드를 정의할 땐** 아래처럼 ES6 메서드 축약 표현으로 정의한 **ES6 메서드**를 사용하는 것이 좋다.
```javascript
// ✅ 위 예제를 해결 ✅
const person = {
  name: 'Lee',
  sayHi() {
    console.log(`hey ${this.name}`);
  }
};
```

### 3-4. `super`
화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다.<br/>
따라서 화살표 함수 안에서 `super`를 참조하면, `this`가 동작하는 것과 마찬가지로 상위 스코프의 `super`를 참조한다.
```javascript
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi! ${this.name}`;
  }
}

class Derived extends Base {
  sayHi = () => `${super.sayHi()} how are you doing?`;
}

const derived = new Derived('Lee');
console.log(derived.sayHi());  // Hi! Lee how are you doing?
```

### 3-5. `arguments`
마찬가지로 화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다.<br/>
따라서 화살표 함수 안에서 `arguments`를 참조하면, `this`가 동작하는 것과 마찬가지로 상위 스코프의 `arguments`를 참조한다.

이처럼 **화살표 함수에서 가변 인자 함수**를 구현해야 할 때는<br/>
(`arguments`를 쓰지 못하므로) 반드시 **Rest 파라미터**를 사용해야 한다.

## 4. Rest 파라미터
### 4-1. 기본 문법
Rest 파라미터는 아래처럼 함수에 전달된 인수들의 목록을 **배열**로 받는다.<br/>
Rest 파라미터는 단 하나만 선언할 수 있으며, 다른 일반 매개변수와 섞어 쓸 수 있다.
```javascript
function foo(param, ...rest) {
  console.log(param);  // 1
  console.log(rest);  // [2, 3, 4, 5]
}

foo(1, 2, 3, 4, 5);
```

### 4-2. Rest 파라미터와 `arguments` 객체
Rest 파라미터와 `arguments` 객체 간 가장 큰 차이점은 바로 배열이냐 아니냐에 있다.<br/>
`arguments` 객체는 유사 배열 객체이므로 반드시 배열로 변환해야만 배열 메서드를 사용할 수 있는 반면,<br/>
**Rest 파라미터는 배열**이므로 `map`이나 `filter` 같은 배열 메서드를 곧바로 사용할 수 있다.

## 5. 매개변수 기본값
생략. 책을 참고합시다~!

끝.
