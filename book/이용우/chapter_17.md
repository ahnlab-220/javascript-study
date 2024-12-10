# Chapter 17 - 생성자 함수에 의한 객체 생성
자바 스크립트에서는 객체 리터럴에 의한 다양한 객체 생성 방식이 있습니다.  
그중에서 객체 리터럴은 가장 일반적이고 간단한 객체 생성 방식입니다.  
하지만 객체 리터럴 이외에도 다양한 방법으로 생성할 수 있습니다.  

<br><br>

### Object 생성자 함수
`new` 연산자와 함께 `Object` 생성자 함수를 호출하면 빈 객체를 생성하여 반환합니다.  
빈 객체를 생성한 이후 프로퍼티 또는 메소드를 추가하여 객체를 완성할 수 있습니다.

``` js
// 빈 객체 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'Lee';
person.sayHello = function () {
    console.log('My name is ' + this.name);
};

console.log(person);    // {name: "Lee", sayHello: f}
person.sayHello();      // Hi! My name is Lee
```

<br>

> **※ 생성자 함수**  
> `new` 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수를 의미한다.  
> 생성자 함수에 의해 생성된 객체를 **인스턴스(instance)**라고 한다.  
> `Object` 생성자 함수 이외에도 `String`, `Number`, `Boolean`, `Function`, `Array`, `Date`, `RegExp`, `Promise` 등의 빌트인 생성자 함수를 제공한다.

<br>

반드시 `Object` 생성자 함수를 사용해 빈 객체를 생성할 필요는 없습니다.  
객체를 생성하는 방법은 객체 리터럴을 사용하는 것이 더 간편합니다.

<br><br>

### 생성자 함수
#### 객체 리터럴에 의한 객체 생성 방식의 문제점
객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성한다는 문제점이 있습니다.  
따라서 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적입니다.  

<br>

``` js
const circle1 = {
    radius: 5,
    getDiameter() {
        return 2 * this.radius
    }
};

console.log(circle1.getDiameter());     // 10

const circle2 = {
    radius: 10,
    getDiameter() {
        return 2 * this.radius;
    }
};

console.log(circle2.getDiameter());     // 20
```

**객체는 프로퍼티를 통해 객체 고유의 상태(state)를 표현**합니다.  
그리고 메소드를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작(behavior)을 표현합니다.  
따라서 프로퍼티는 객체마다 프로퍼티 값이 다를 수 있지만 메소드는 동일한 경우가 일반적입니다.

여기서 circle1 객체와 circle2 객체는 프로퍼티 구조가 동일합니다.  
객체 고유의 상태 데이터인 `radius` 프로퍼티 값은 객체마다 다르지만,  
`getDiameter()` 메소드는 완전히 동일합니다.

다만 객체 리터럴에 의해 객체를 생성하는 경우,  
프로퍼티 구조가 동일함에도 매번 같은 프로퍼티와 메소드를 기술해야 하는 번거로움이 있습니다.

<br>

#### 생성자 함수에 의한 객체 생성 방식의 장점
생성자 함수에 의한 객체 생성 방식은  
객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 생성자 함수를 사용해서  
프로퍼티 구조가 동일한 여러 개의 객체를 간편히 만들 수 있습니다.


``` js
// 생성자 함수
function Circle(radius) {
    // 생성자 함수 내부이 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

// 인스턴스의 생성
const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter());
console.log(circle2.getDiameter());
```

<br>

**※ this**  
자기 자신의 프로퍼티나 메소드를 참조하기 위한 자기 참조 변수(self-referencing variable)이다.

``` js
function foo(){
    console.log(this);
}

// 일반적인 함수로써 호출
// 전역 객체는 브라우저 환겨엥서는 window, Node.js 환경에서는 global을 가리킨다.
foo();

const obj = { foo };        // ES6 프로퍼티 축약 표현

// 메소드로서 호출
obj.foo();      // obj

// 생성자 함수로써 호출
const inst = new foo();     // inst
```

<br>

생성자 함수는 이름 그대로 객체(인스턴스)를 생성하는 함수입니다.  
하지만 자바와 같은 클래스 기반 객체지향언어의 생성자와는 다르게 그 형식이 정해져 있는 것이 아니라  

일반 함수와 동일한 방법으로 생성자 함수를 정의하고 `new` 연산자와 함께 호출하면   
해당 함수는 생성자 함수로 동작합니다.

``` js
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다.
const circle3 = Circle(15);
```

> 객체로써 동작하게 하려면 new 연산자를 사용해야 함


<br>

#### 생성자 함수의 인스턴스 생성 과정
<font color='orange'>생성자 함수의 역할은 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿으로서 동작하여 인스턴스를 생성하는 것과 생성된 인스턴스를 초기화 하는 것입니다.</font>

``` js
// 생성자 함수
function Circle(radius) {
    // 인스턴스 초기화
    this.radius = radius;
    this.getDiameter = function (){
        retrun 2 * this.radius;
    };
}

// 인스턴스 생성
const circle1 = new Circle(5);      // 반지름이 5인 Circle 객체를 생성
```

자바스크립트는 아래의 과정을 통해 암묵적으로 인스턴스를 생성하고  
인스턴스를 초기화한 후 암묵적으로 인스턴스를 반환합니다.

<br>

##### 1. 인스턴스 생성과 this 바인딩
먼저 암묵적으로 빈 객체가 생성됩니다.  
이 빈 객체가 생성자 함수가 생성한 인스턴스입니다.  

그리고 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩됩니다.   
이 처리는 런타임 이전에 동작합니다.  

``` js
function Circle(radius) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩
    console.log(this);      // Circle {}

    this.radius = radius;
    this.getDiameter = function (){
        retrun 2 * this.radius;
    };
}
```


<br>

##### 2. 인스턴스 초기화
생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 this에 바인딩되어 있는 인스턴스가 초기화됩니다.  
즉, this에 바인딩되어 있는 인스턴스에 프로퍼티나 메소드를 추가하고 생성자 인수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당합니다.

``` js
function Circle(radius) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩
   
    // 2. this에 바인딩되어 있는 인스턴스를 초기화
    this.radius = radius;
    this.getDiameter = function (){
        retrun 2 * this.radius;
    };
}
```


<br>

##### 3. 인스턴스 반환
생성자 함수 내부에서 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환합니다.

``` js
function Circle(radius) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩
   
    // 2. this에 바인딩되어 있는 인스턴스를 초기화
    this.radius = radius;
    this.getDiameter = function (){
        retrun 2 * this.radius;
    };

    // 3. 완성된 인스턴스가 바인딩된 this를 반환
}


// 인스턴스 생성, Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
```


<br>

만약 this가 아닌 다른 객체를 명시적으로 반환하면  
this가 아닌 return 문에 명시한 객체가 반환됩니다.

``` js
function Circle(radius) {
    this.radius = radius;
    this.getDiameter = function (){
        retrun 2 * this.radius;
    };

    return {};
}

// 인스턴스 생성, Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle);    // {}
```

<br><br>

#### 내부 메소드 [[Call]]과 [[Construct]]
함수 선언문 또는 함수 표현식으로 정의한 함수는  
일반적인 함수로서 호출할 수 있으면서, 생성자 함수로도 호출이 가능합니다.

함수는 객체이지만 일반 객체와는 다릅니다.  
**일반 객체는 호출할 수 없지만 함수는 호출할 수 있습니다.**

따라서 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메소드는 물론,  
함수 객체만을 위한 [[Environment]], [[FormalParameters]] 등의 내부 슬롯과  
[[Call]], [[Construct]] 같은 내부 메소드를 추가로 가지고 있습니다.  

> 파이썬으로 치면 클래스의 매직 메소드 같은 느낌

<br>

``` js
function foo() {}

// 일반적인 함수로서 호출: [[Call]] 이 호출
foo();

// 생성자 함수로서 호출: [[Construct]]가 호출
new foo();
```

<br>

다만, 모든 함수 객체는 내부 메소드 [[Call]]을 가지고 있지만,  
[[Construct]]는 그렇지 않다는 특이사항이 있습니다.


<br><br>

#### constructor와 non-constructor의 구분

자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때   
함수 정의 방식에 따라 constructor와 non-constructor로 구분합니다.
- constructor: 함수 선언문, 함수 표현식, 클래스(클래스도 함수)
- non-constructor: 메소드(ES6 메소드 축약 표현), 화살표 함수

<br>

``` js
// 일반 함수 정의: 함수 선언문, 함수 표현식
function foo() {}
const bar = function () {};

// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수이다. 이는 메소드로 인정되지 않는다.
const baz = {
    x: function () {}
};

// 일반 함수로 정의된 함수만이 constructor 이다.
new foo();      // foo {}
new bar();      // bar {}   
new baz.x();    // x {}

// 화살표 함수 정의
const arrow = () => {};
new arrow();        // TypeError: arrow is not a constructor

// 메소드 정의: ES6의 메소드 축약 표현만 메소드로 인정함.
const obj = {
    x() {}
};

new obj.x();    // TypeError: obj.x is not a constructor
``` 

<font color='orange'>함수의 프로퍼티 값으로 사용하면 일반적으로 메소드로 통칭합니다.  
하지만 ESMAScript 사양에서 메소드란, ES6 메소드 축약 표현만을 메소드로 칭합니다.</font>

일반함수, 즉 함수 선언문과 함수 표현식으로 정의된 함수만이 constructor이고  
ES6의 화살표 함수와 메소드 축약 표현으로 정의된 함수는 non-constructor입니다.

<br><br>

#### new 연산자
new 연산자와 함께 호출하는 함수는 반드시 constructor여야 합니다.
new 연산자 없이 생성자 함수를 호출하면 일반 함수로서 호출됩니다.

``` js
// 생성자 함수
function Circle(radius) {
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
    };
}

// new 연산자 없이 생성자 함수를 호출하면 일반함수로서 호출됩니다.
const circle1 = Circle(5);
console.log(circle1);    // undefined

const circle2 = new Circle(5);
console.log(circle2);    // Circle { radius: 5, getDiameter: [Function (anonymous)] }
```


<br><br>

#### new.target
생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해  
ES6에서는 new.target을 지원합니다.

`new.target`은 `this`와 유사하게 constructor인 모든 함수 내부에서  
암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라고도 부릅니다. 

함수 내부에서 `new.target`을 사용하면 `new` 연산자와 함께 생성자 함수로서 호출되었는지 확인할 수 있습니다.  
`new` 연산자와 함께 생성자 함수로서 호출하면 함수 내부의 `new.target`은 함수 자신을 가리킵니다.  
`new` 연산자 없이 일반 함수로서 호출된 함수 내부의 `new.target`은 `undefined`입니다.

따라서 함수 내부에서 `new.target`을 사용하여 
생성자 함수로서 호출되었는지 여부를 확인하여  
생성자 함수로서 호출되지 않았을 경우, 생성자 함수 호출을 강제할 수 있습니다.

``` js
// 생성자 함수
function Circle(radius) {
    // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined이다.
    if (!new.target) {
        // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
        return new Circle(radiius);
    }

    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}
```

<br><br>

**※ 스코프 세이프 생성자 패턴 (scope-safe constructor)**  
new.target은 ES6에서 도입되었기 때문에 new.target을 사용할 수 없는 상황이라면  
스코프 세이프 생성자 패턴을 사용할 수 있습니다.

``` js
function Circle(radius) {
    if (!(this instanceof Circle)) {
        return new Circle(radius);
    }

    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}
```

> `instanceof` 연산자는 '19. 프로토타입'에서 다룰 것이다.




