### Chapter 24 - 클로저

클로저는 자바스크립트 고유의 개념이 아니다.  
함수를 일급 객체로 취급하는 함수형 프로그래밍 언어에서 자주 사용되는 중요한 특성이다.  
> 하스켈(Haskell), 리스프(Lisp), 얼랭(Erlang) 스칼라(Scala) 

MDN에서는 클로저에 대해 아래와 같이 정의내린다.
> 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.


<font color='tomato'>렉시컬 스코프 = 정적 스코프</font>

<br>




<br>

핵심 키워드는 <font color='orange'>"함수가 선언된 렉시컬 환경"</font>이다.

``` js
const x = 1;

function outerFunc() {
    const x = 10;

    function innerFunc() {
        console.log(x);
    }

    innerFunc();
}

outerFunc();
```

<br>

`outerFunc` 내부에서 중첩 함수 `innerFunc`가 정의되고 호출되었다.  
이때 중첩 함수 innerFunc의 상위 스코프는 외부 함수 `outerFunc`의 스코프이다.  

따라서 중첩 함수 `innerFunc` 내부에서는 자신을 포함하고 있는  
외부 함수 `outerFunc`의 x 변수에 접근할 수 있다.

> 이는 자바스크립트가 렉시컬 스코프를 따르는 프로그래밍 언어이기 때문이다.

<br><br>

### 렉시컬 스코프

렉시컬 스코프는 함수를 어디서 호출하는지가 아니라  
<font color='orange'>함수를 어디에 정의하는지에 따라 상위 스코프를 결정한다.</font>

``` js
const x = 1;

function foo() {
    const x = 10;
    bar();
}

function bar() {
    console.log(x);
}

foo();
bar();
```

<br>

위 예제의 `foo` 함수와 `bar` 함수는 모두 전역에서 정의된 전역 함수이다.  
함수의 상위 스코프는 함수를 어디서 정의했느냐에 따라 결정되므로  
`foo` 함수와 `bar` 함수의 상위 스코프는 전역이다.  

> 함수를 어디서 호출하는지는 함수의 상위 스코프 결정에 어떠한 영향도 주지 않는다.  

<br>

"실행 컨텍스트"에서 확인한 내용과 같이,  
스코프의 실체는 실행 컨텍스트의 렉시컬 환경이다.  

<font color='orange'>이 렉시컬 환경은 "외부 렉시컬 환경에 대한 참조"를 통해 상위 렉시컬 환경과 연결된다.</font>  
이것이 스코프 체인이다.  

<br>

따라서 "함수의 상위 스코프를 결정한다"는 것은 "렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값을 결정한다"는 것과 같다. 이것이 상위 스코프이기 때문이다.  

이 개념으로 다시 한번 "렉시컬 스코프"를 정의하면 다음과 같다.  
> 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장할 참조값, 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정된다.

<br><br>

### 함수 객체의 내부 슬롯 [[Environment]]

함수 객체는 자신의 내부 슬롯 [[Environment]]에 자신이 정의된 환경,  
즉 상위 스코프의 참조를 저장한다.  

``` js
const x = 1;

function foo() {
    const x = 10;

    // 상위 스코프는 함수 정의 환경(위치)에 따라 결정된다.
    // 함수 호출 위치와 상위 스코프는 관계가 없다.
    bar();
}

// 함수 bar는 자신의 상위 스코프, 즉 전역 렉시컬 환경을 [[Environment]]에 저장한다.
function bar() {
    console.log(x);
}

foo();
bar();
```

<br><br>

### 클로저와 렉시컬 환경
``` js
const x = 1;

function outer() {
    const x = 10;
    const inner = function () {
        console.log(x);
    };
    return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환한다.
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 pop되어 제거된다.
const innerFunc = outer();
innerFunc();    // 10
```

<br>

`outer` 함수를 호출하면 `outer` 함수는 중첩함수 `inner`를 반환하고 생명 주기를 마감한다.
> `outer` 함수의 실행이 종료되면 `outer` 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 pop된다.

이때 `outer` 함수의 지역 변수 `x`와 변수 10을 저장하고 있던 `outer` 함수의 실행 컨텍스트가 제거되었으므로 `outer` 함수의 지역 변수 `x` 또한 생명 주기를 마감한다.

따라서 `outer` 함수의 지역 변수 `x`는 더이상 유효하지 않아 보인다.

<br>

하지만 위 코드의 실행 결과는 `outer` 함수의 지역 변수 `x` 값인 10이다.  
이미 생명 주기가 종료되어 실행 컨텍스트 스택에서 제거된 `outer` 함수의 지역 변수 `x`가 부활한 것 처럼 동작하고 있다.

<br>

<font color='orange'>이와 같이 외부 함수보다 중첩 함수가 더 오래 유지되는 경우,  
중첩 함수는 이미 종료한 외부 함수의 변수를 참조할 수 있다.  </font>

<font color='tomato'>이러한 중첩 함수를 클로저(closure)라고 부른다.</font>

<br><br>

### 클로저의 활용
클로저는 상태(state)를 안전하게 변경하고 유지하기 위해 사용한다.  
> 상태가 의도적으로 변경되지 않도록 상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용한다.

<br>

``` js
let num = 0;

// 카운트 상태 변경 함수
const increase = function () {
    // 카운트 상태를 1만큼 증가시킨다.
    return ++num;
};

console.log(increase());    // 1
console.log(increase());    // 2
console.log(increase());    // 3
```
위 코드는 잘 동작하지만 오류를 발생시킬 가능성을 내포하고 있는 좋지 않은 코드이다.  
위 예제가 잘 동작하려면 아래의 전제 조건이 지켜져야 하기 때문이다.

1. 카운트 상태(num 변수의 값)는 `increase` 함수가 호출되기 전까지 변경되지 않고 유지되어야 한다.
2. 이를 위해 카운트 상태(num 변수의 값)는 `increase` 함수만 변경할 수 있어야 한다.

<br>

하지만 카운트 상태는 전역에서 관리되고 있기 때문에  
누구든지 접근할 수 있고 변경할 수 있다.  

> 의도치 않게 상태를 변경시킬 수 있어 오류를 발생시킬 가능성이 있다.

<br>

따라서 카운트 상태(num 변수의 값)를 안전하게 변경하고 유지하기 위해서는  
`increase` 함수만이 `num` 변수를 참조하고 변경할 수 있게 하는 것이 바람직하다.

이를 위해 변수 `num`을 `increase` 함수의 지역 변수로 바꾸어 의도치 않은 상태 변경을 방지해보도록 하자.

``` js
const increase = function () {
    // 카운트 상태 변수
    let num = 0;

    // 카운트 상태를 1만큼 증가시킴
    return ++num;
};

console.log(increase());    // 1
console.log(increase());    // 2
console.log(increase());    // 3
```

카운트 상태를 안전하게 변경하고 유지하기 위한 전역 변수 `num`을 `increase` 함수의 지역 변수로 변경하여  
의도치 않은 상태 변경을 방지했다.    
이제 `num` 변수의 상태는 `increase` 함수만이 변경할 수 있다.

하지만 `increase` 함수가 호출될 때마다 지역 변수 `num`은 다시 선언되고 0으로 초기화되기 때문에  
출력 결과는 언제나 1이다. 

다시 말해, 상태가 변경되기 이전의 상태를 유지하지 못한다.   
이전 상태를 유지할 수 있도록 클로저를 사용해보자.

``` js
// 카운트 상태 변경 함수
const increase  = (function () {
    // 카운트 상태 변수
    let num = 0;

    // 클로저
    return function () {
        return ++num;
    };
}());

console.log(increase());    // 1
console.log(increase());    // 2
console.log(increase());    // 3
```

위 코드가 실행되면 즉시 실행 함수가 호출되고,  
즉시 실행 함수가 반환한 함수가 `increase` 변수에 할당된다.  

`increase` 변수에 할당된 함수는 자신이 정의된 위치에 의해 결정된   
<font color='orange'>상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하는 클로저이다.</font>

<br><br>

즉시 실행 함수는 호출 이후에 소멸되지만,  
즉시 실행 함수가 반환한 클로저는 `increase` 변수에 할당되어 호출된다.  

따라서 즉시 실행함수가 반환한 클로저는  
카운트 상태를 유지하기 위한 <font color='tomato'>자유 변수</font> `num`을 어디서든 참조하고 변경할 수 있다.

<br>

``` js
function outer() {
    let count = 0;  // 외부 함수의 변수
    
    return function inner() {  // 내부 함수
        return ++count;  // 외부 함수의 변수를 참조
    }
}

const counter = outer();  // outer는 실행 종료되지만
counter();  // 1 - inner 함수는 여전히 count 변수에 접근 가능
counter();  // 2
```

위 예시는 클로저를 활용한 또 다른 예제이다.

<br><br>

변수 값은 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본적인 원인이 될 수 있다.  
외부 상태 변경이나 가변(mutable) 데이터를 피하고 불변성(immutability)을 지향하는 함수형 프로그래밍에서 부수 효과를 최대한 억제하여  
오류를 피하고 프로그램의 안정성을 높이기 위해 클로저는 적극적으로 사용될 수 있다.

아래는 함수형 프로그래밍에서 클로저를 활용한 간단한 예시이다.

``` js
// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
function makeCounter(aux) {
    // 자유 변수
    let counter = 0;

    // 클로저로 반환
    return function () {
        // 인수로 전달받은 보조 함수에 상태 변경을 위임
        counter = aux(counter);
        return counter;
    };
};

// 보조 함수
function increase(n) {
    return ++n;
}

// 보조 함수
function decrease(n) {
    return --n;
}

const increaser = makeCounter(increase);
console.log(increaser());  // 1
console.log(increaser());  // 2

const decreaser = makeCounter(decrease);
console.log(decreaser());  // -1
console.log(decreaser());  // -2
```

위 예제는 외부 함수의 인자도 클로저에 전달되어 클로저가 외부 함수의 변수에 접근할 수 있도록 하여  
카운트 상태를 안전하게 변경하고 유지할 수 있도록 하는 예이다.


<br><br>

### 캡슐화와 정보 은닉 (얘가 핵심)
대부분의 객체 지향 프로그래밍 언어는 클래스를 정의하고 그 클래스를 구성하는 맴버에 대해  
`public`, `private`, `protected` 등의 접근 제한자를 사용하여 공개 범위를 제한할 수 있다.

다만 자바스크립트는 접근 제한자를 제공하지 않는다.  
따라서 자바스크립트 객체의 모든 프로퍼티와 메소드는 기본적으로 외부에 공개되어 있다.  

즉, 객체의 모든 프로퍼티와 메소드는 기본적으로 `public`이다.

``` js
function Person(name, age) {
    this.name = name;   // public
    let _age = age;    // private

    // 인스턴스 메소드
    this.sayHi = function () {
        console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
    };
}

const me = new Person('Lee', 20);
me.sayHi();    // Hi! My name is Lee. I am 20.

console.log(me.name);   // Lee
console.log(me._age);   // undefined
```

위 예제의 `name` 프로퍼티는 외부에 공개되어 있어서 자유롭게 참조하거나 변경할 수 있다.  
하지만 `_age` 변수는 Person 생성자 함수의 지역 변수이므로 `Person` 생성자 함수 외부에서 참조하거나 변경할 수 없다.  
즉, `_age` 변수는 `private` 하다.

위 예제의 `sayHi` 메소드는 인스턴스 메소드이기 때문에 `Person` 객체가 생성될 때마다 중복 생성된다.  
`sayHi` 메소드를 프로토타입 메소드로 변경하여 `sayHi` 메소드의 중복 생성을 방지해보도록 하자.

``` js
function Person(name, age) {
    this.name = name;   // public
    let _age = age;    // private

}

// 프로토타입 메소드
Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
};
```

이 경우에 `Person.prototype.sayHi` 메소드 내에서 `Person` 생성자 함수의 지역 변수 `_age`에 참조할 수 없는 문제가 발생한다.  

따라서 이를 해결하기 위해 즉시 실행 함수를 사용하여   
`Person` 생성자 함수의 `Person.prototype.sayHi` 메소드를 하나의 함수 내에 모아보도록 하자.

``` js
const Person = (function () {
    let _age = 0;    // private

    // 생성자 함수
    function Person(name, age) {
        this.name = name;   // public
        _age = age;
    }

    // 프로토타입 메소드
    Person.prototype.sayHi = function () {
        console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
    };

    // 생성자 함수를 반환
    return Person;
}());

const me = new Person('Lee', 20);
me.sayHi();    // Hi! My name is Lee. I am 20.

console.log(me.name);   // Lee
console.log(me._age);   // undefined

```

위와 같은 패턴을 사용하면 접근 제한자를 제공하지 않는 자바스크립트에서도 정보 은닉이 가능한 것처럼 만들 수 있다.  

즉시 실행 함수가 반환하는 `Person` 생성자 함수와 `Person` 생성자 함수의 인스턴스가 상속받아 호출할  
`Person.prototype.sayHi` 메소드는 즉시 실행 함수가 종료된 이후에 호출된다.  

하지만 `Person` 생성자 함수와 `sayHi` 메소드는 이미 종료되어 소멸한  
즉시 실행 함수의 지역 변수 `_age`를 참조할 수 있는 클로저이다. 

<br>

하지만 위 코드도 문제는 있다.  
`Person` 생성자 함수가 여러 개의 인스턴스를 생성할 경우  
아래와 같이 `_age` 변수의 상태가 유지되지 않는다는 것이다.


``` js
const me = new Person('Lee', 20);
me.sayHi();    // Hi! My name is Lee. I am 20.

const you = new Person('Kim', 30);
you.sayHi();   // Hi! My name is Kim. I am 30.

// _age 값이 변경되는 것을 볼 수 있음
me.sayHi();     // Hi! My name is Lee. I am 30.
```

이는 `Person.prototype.sayHi` 메소드가 단 한번 생성되는 클로저이기 때문에 발생하는 현상이다.    
`_age`는 즉시실행함수(IIFE)의 렉시컬 스코프에 있는 단일 변수이다.  
이는 모든 `Person` 인스턴스가 동일한 `_age` 변수를 참조한다는 것과 같다.  

이와 같이 정보 은닉을 완벽하게 지원하지는 않아,  
ES6의 `Symbol` 또는 `WeakMap`을 사용하여 `private`한 프로퍼티를 흉내낼 수 있긴 하지만  
이는 정보 은닉을 완벽하게 지원하는 것은 아니다.

> 다만, 최신 브라우저와 최신 Node.js에서는 이런 문제를 해결하기 위한 방안이 마련되어 있다.  
> 이는 25장의 "private 필드 정의 제안" 파트에서 다룬다.