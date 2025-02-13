# Chapter 22 - `this`

## 1. `this` 키워드
`this`는, 자신이 속한 객체 혹은 자신이 생성할 인스턴스를 가리키는 self-referencing variable(자기 참조 변수)다.<br/>
`this`는 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조할 수 있다.

중요한 점은! `this`가 가리키는 값, 즉 `this` 바인딩은 **함수 호출 방식에 의해 동적으로 결정**된다.<br/>
예제와 함께 살펴 보자.

```javascript
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  }
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}
const me = new Person('Lee');
```
(참고로 strict mode가 적용된 일반 함수 내부의 `this`에서는 undefined가 바인딩된다. 엄밀히 말하면 일반 함수 내부에선 `this`를 사용할 이유가 없기 때문이다.)

함수 호출 방식에 따라 달라지는 `this` 바인딩은 다음 절에서 더 자세히 살펴 보자.

## 2. 함수 호출 방식과 `this` 바인딩
동일한 함수여도 다음과 같이 어떤 방식으로 호출하느냐에 따라 `this`에 바인딩되는 값이 달라진다. 예제와 함께 보자.
1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

```javascript
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo = function () {
  console.dir(this);
};

// 동일한 함수도 다양한 방식으로 호출할 수 있다.

// 1. 일반 함수 호출
// foo 함수를 일반적인 방식으로 호출
// foo 함수 내부의 this는 전역 객체 window를 가리킨다.
foo(); // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = { foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo(); // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this는 인수에 의해 결정된다.
const bar = { name: 'bar' };
foo.call(bar); // bar
foo.apply(bar); // bar
foo.bind(bar)(); // bar
```

### 2-1. 일반 함수 호출
함수를 일반 함수로 호출하면, 함수 내부의 `this`에는 Global object가 바인딩된다. (e.g., `window`)<br/>
그 어떤 함수라도(**중첩 함수, 콜백 함수 포함**), 일반 함수로 호출된다면 `this`에는 Global object가 바인딩된다.<br/>
(단, strict mode가 적용된 일반 함수 내부의 `this`에는 `undefined`가 바인딩된다.)

이런 자바스크립트의 특징은 보통의 경우 문제가 되기 때문에,<br/>
중첩 함수나 콜백 함수 내부의 `this` 바인딩을 메서드의 `this`와 일치시키기 위한 여러 방법이 존재한다.
* Function.prototype.apply/call/bind 메서드를 활용해 명시적으로 `this`를 바인딩하거나,
* 화살표 함수를 이용해 `this` 바인딩을 일치시킬 수도 있다.

### 2-2. 메서드 호출
메서드 내부의 `this`에는 그 메서드를 호출한 객체가 바인딩된다.<br/>
메서드 내부의 `this`는 그 메서드 객체를 가리킬 것처럼 보일 수도 있겠지만 아니다. (메서드란 것은 프로퍼티에 바인딩된, 엄연히 독립적인 함수이자 객체이기 때문에 그렇게 오해할만함)<br/>
메서드 내부의 `this`는 메서드를 가리키는 객체와는 관계가 없고, **그 메서드를 호출한 객체에 바인딩된다는 사실을 유념하자.**

### 2-3. 생성자 함수 호출
생성자 함수 내부의 `this`에는 생성자 함수가 (미래에) 생성**할** 인스턴스가 바인딩된다.

### 2-4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
(apply, call 메서드 내용은 생략)<br/>
**bind 메서드**는 메서드의 `this`와 메서드 내부의 중첩 함수 또는 콜백 함수의 `this`가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.<br/>
예제와 함께 살펴 보자.

```javascript
const person = {
  name: 'Lee',
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100);
  }
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee.
});
```


**⭐최종 정리⭐** <br/>
<img src="https://github.com/user-attachments/assets/54e5612d-ca38-4045-95b8-d3a9631e0a55" width="60%" />

끝.
