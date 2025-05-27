### Chapter 22 - this

### this 키워드

객체는 상태(state)를 나타내는 프로퍼티와 동작(behavior)을 나타내는 메서드를 하나의 묶음으로 관리한다.

동작을 나타내는 메소드는 자신이 속한 객체의 상태,  
즉 프로퍼티를 참조하고 변경할 수 있어야 한다.  

이때 메소드가 자신이 속한 객체의 프로퍼티를 참조하려면  
먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.

객체 리터럴 방식으로 생성한 객체의 경우 메소드 내부에서 메소드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다.  

``` js
const circle = {
    // 프로퍼티: 객체 고유의 상태 데이터
    radius: 5,

    // 메소드: 상태 데이터를 참조하고 조작하는 동작
    getDiameter() {
        // 이 메소드가 자신이 속한 객체의 프로퍼티를 참조하려면 
        // 먼저 자신이 속한 객체를 가리키는 식별자를 참조해야 한다.
        return 2 * circle.radius;
    }
}

console.log(circle.getDiameter()); // 10
```

<br>

`getDiameter` 메소드 내에소 메소드 자신이 속한 객체를 가리키는 식별자 `circle`을 참조하고 있다.

이 참조 표현식이 평가되는 시점은 `getDiameter` 메소드가 호출되어  
함수 몸체가 실행되는 시점이다.

<br>

위 예제의 객체 리터럴은 `circle` 변수가 할당되기 직전에 평가된다.  
따라서 `etDiameter` 메소드가 호출되는 시점에는 이미 객체 리터럴의 평가가 완료되어 객체가 생성되었고 `circle` 식별자에 생성된 객체가 할당된 이후다.  

따라서 메소드 내부에서 circle 식별자를 참조할 수 있다.

<font color='orange'>하지만 자기 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지 않으며 바람직하지 않다.</font>

<br><br>


``` js
function Circle(radius) {
    // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
    ????.radius = radius;
}

Circle.prototype.getDiameter = function 90 {
    // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
    return s2 * ????.radius;
};

// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 한다.
const circle = new Circle(5);
```

생성자 함수 내부에서는 프로퍼티 또는 메소드를 추가하기 위해  
자신이 생성할 인스턴스를 참조할 수 있어야 한다.

하지만 생성자 함수에 의한 객체 생성 방식은  
먼저 생성자 함수를 정의한 이후 `new` 연산자와 함께 생성자 함수를 호출하는 단계가 추가로 필요하다.  

다시 말해, 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수가 존재해야 한다.  

생성자 함수를 정의하는 시점에는 아직 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다.

> 이 문제를 해결하기 위해 자바스크립트는 `this` 식별자를 제공한다.


<br>

<font color='orange'>`this`는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.</font>

`this`를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메소드를 참조할 수 있다.

단, `this`가 가리키는 값, 즉 `this` 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.

<br><br>

``` js
// 객체 리터럴
const circle = {
    radius: 5,
    getDiameter() {
        // this는 메소드를 호출한 객체를 가리킨다.
        return 2 * this.radius;
    }
};

console.log(circle.getDiameter()); // 10
```

객체 리터럴의 메소드 내부에서의 `this`는 메소드를 호출한 객체, 즉 `circle` 객체를 가리킨다.

<br>

``` js
// 생성자 함수
function Circle(radius) {
   // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
   this.radius = radius; 
}

Circle.prototype.getDiameter = function () {
    // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

생성자 함수 내부의 `this`는 생성자 함수가 생성할 인스턴스를 가리킨다.


<br><br>

### 함수 호출 방식과 this 바인딩
this 바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.

주의 할 점은 동일한 함수도 아래와 같이 다양한 방식으로 호출이 가능하다는 것이다.
1. 일반 함수 호출
2. 메소드 호출
3. 생성자 함수 호출
4. Function.prototype.apply/call/bind 메소드에 의한 간접 호출

<br>

``` js
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다
const foo = function () {
    console.dir(this);
};

// 1. 일반 함수 호출
// 일반 함수로 호출된 경우 this는 전역 객체 window를 가리킨다.
foo();  // window

// 2. 메소드 호출
// 메소드 내부의 this는 메소드를 호출한 객체를 가리킨다.
const obj = { foo };
obj.foo();  // obj

// 3. 생성자 함수 호출
// 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
new foo();  // foo {}

// 4. Function.prototype.apply/call/bind 메소드에 의한 간접 호출
// Function.prototype.apply/call/bind 메소드에 의한 간접 호출한 경우 this는 인수에 의해 결정된다.
const bar = { name: 'bar' };

// foo 함수를 호출하면서 인수로 전달한 객체를 가리킨다.
foo.call(bar);  // bar
foo.apply(bar);  // bar
foo.bind(bar)();  // bar
```

<br><br>

#### 일반 함수 호출
기본적으로 `this`에는 전역 객체가 바인딩된다.

``` js
function foo() {
    console.log('foo의 this: ', this);  // window
    function bar() {
        console.log('bar의 this: ', this);  // window
    }
    bar();
}
foo();
```

<br>

전역 함수나 중첩 함수를 일반 함수로 호출하면 함수 내부의 `this`에는 전역 객체가 바인딩된다.  
다만 `this`는 객체의 프로퍼티나 메소드를 참조하기 위한 자기 참조 변수이므로  
객체를 생성하지 않는 일반 함수에서 `this`는 의미가 없다.

따라서 아래와 같이 `strict mode`가 적용된 일반 함수의 this에는 `undefined`가 바인딩된다.  

``` js
function foo() {
    'use strict';

    console.log('foo의 this: ', this);  // undefined
    function bar() {
        console.log('bar의 this: ', this);  // undefined
    }
    bar();
}

foo();
```


<br><br>

하지만 메소드 내에서 정의한 중첩 함수 또는 메소드에게 전달한 콜백 함수가 일반 함수로 호출되는 경우  
메소드 내의 중첩 함수 또는 콜백 함수의 `this`가 전역 객체를 바인딩하는 것은 문제가 있다.

> 중첩 함수나 콜백 함수는 외부 함수를 돕는 헬퍼 함수의 역할을 하므로  
> 외부 함수의 일부 로직을 대신하는 경우가 대부분이다.

<br>

때문에 메소드 내부에서 중첩 함수나 콜백 함수의 `this` 바인딩을   
메소드의 `this` 바인딩과 일치시키기 위한 방법은 다음과 같다.

``` js
var value = 1;

const obj = {
    value: 100,
    foo() {
        // this 바인딩(obj)을 변수 that에 할당
        const that = this;

        // 콜백 함수 내부에서 this 대신 that을 참조
        setTimeout(function () {
            console.log(that.value);  // 100
        }, 100);
    }
};

obj.foo();
```

위 방법 이외에도 자바스크립트는 this를 병시적으로 바인딩할 수 있는  
`Function.prototype.bind`, `Function.prototype.call`, `Function.prototype.apply` 메소드를 제공한다.

<br>

``` js
var value = 1;

const obj = {
    value: 100;
    foo() {
        // 콜백 함수에 명시적으로 this를 바인딩
        setTimeout(function (){
            console.log(this.value);  // 100
        }.bind(this), 100);
    }
};

obj.foo();
```

<br>

또는 화살표 함수를 사용해 this 바인딩을 일치시킬 수도 있다.

``` js
var value = 1;

const obj = {
    value: 100,
    foo() {
        // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
        setTimeout(() => console.log(this.value), 100);
    }
}

```

<br><br>

#### 메소드 호출
메소드 내부의 `this`에는 메소드를 호출한 객체,  
<font color='orange'>즉 메소드를 호출할 때 메소드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩된다.</font>  

주의할 것은 메소드 내부의 `this`는 메소드를 소유한 객체가 아닌  
메소드를 호출한 객체에 바인딩된다는 것이다.

``` js
const person = {
    name: 'Lee',
    getName() {
        // 메소드 내부이 this는 메소드를 호출한 객체에 바인딩된다.
        return this.name;
    }
};

// 메소드 getName을 호출한 객체는 person이다.
console.log(person.getName());  // Lee
```

<br>

위 예제의 `getName` 메소드는 `person` 객체의 메소드로 정의되었다.  
메소드는 프로퍼티에 바인딩된 함수이다.  

즉 `person` 객체의 `getName` 프로퍼티가 가리키는 함수 객체는 `person` 객체에 포함된 것이 아니라  
독립적으로 존재하는 별도의 개겣이다.  

<font color='orange'>`getName` 프로퍼티가 함수 객체를 가리키고 있을 뿐이다.</font>

<img src="https://github.com/user-attachments/assets/37c4ccf3-33f6-4137-9cc7-d9c98efc4128" width="450">

<br>

따라서 `getName` 프로퍼티가 가리키는 함수 객체,  
즉 `getName` 메소드는 <font color='orange'>다른 객체의 프로퍼티에 할당하는 것으로  
다른 객체의 메소드가 될 수도 있고 일반 변수에 할당하여 일반 함수로 호출될 수도 있다.</font>

<br>

``` js
const anotherPerson = {
    name: 'Lee'
};

// getName 메소드를 anotherPerson 객체의 메소드로 할당
anotherPerson.getName = person.getName;

// getName 메소드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName());  // Lee

// getName 메소드를 변수에 할당
const getName = person.getName;

// getName 메소드를 일반 함수로 호출
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
console.log(getName());  // ''
```

<br><br>

프로토타입 메소드 내부에서 사용된 `this`도 일반 메소드와 마찬가지로   
해당 메소드를 호출한 객체에 바인딩된다.

``` js
function Person(name) {
    this.name = name;
}

Person.prototype.getName = function () {
    return this.name;
};

const me = new Person('Lee');

// getName 메소드를 호출한 객체는 me다.
console.log(me.getName());  // Lee
```

<br><br>

#### 생성자 함수 호출
생성자 함수 내부의 `this`에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.

``` js
// 생성자 함수
function Circle(radius) {
    // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

// 반지름이 5인 Circle 인스턴스를 생성
const circle = new Circle(5);

// 인스턴스 circle의 프로퍼티를 참조
console.log(circle.getDiameter());  // 10
```

<br>

만약 `new` 연산자와 함께 생성자 함수를 호출하지 않으면  
생성자 함수가 아니라 일반 함수로 동작한다.

``` js
const circle = Circle(15);

// 일반 함수로 호출된 Circle에는 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle);  // undefined
```

<br><br>




#### Function.prototype.apply/call/bind 메소드에 의한 간접 호출
`apply`, `call`, `bind` 메소드는 `Function.prototype`의 메소드다.  
즉, 이들 메소드는 모든 함수가 상속받아 사용할 수 있다.

<br>

``` js
function getThisBinding() {
    return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding());  // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg));  // { a: 1 }
console.log(getThisBinding.call(thisArg));  // { a: 1 }
```

`apply`와 `call` 메소드의 본질적인 기능은 함수를 호출하는 것이다.  
`apply`와 `call` 메소드는 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 `this`에 바인딩한다.

<br><br>

`bind` 메소드는 메소드의 `this`와   
메소드 내부의 중첩 함수 또는 콜백 함수의 `this`가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.

``` js
const person = {
    name: 'Lee',
    foo(callback) {
        // 1) 콜백 함수 호출
        setTimeout(callback, 100);
    }
};

person.foo(function () {
    console.log(`Hi! my name is ${this.name}`);
});

// 일반 함수로 호출된 콜백 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
// Hi! my name is undefined
```

`person.foo`의 콜백 함수가 호출되기 이전인 1) 시점에서 `this`는 `foo` 메소드를 호출한 객체,  
즉 `person` 객체를 가리키고 있다.  

하지만 콜백 함수는 일반 함수로 호출되므로 콜백 함수의 `this`는 전역 객체를 참조한다.    
따라서 콜백 함수를 일반 함수로 호출하면 `this.name`이 아니라 `window.name`을 참조하게 된다.

<br>

이때 위 예제에서 `person.foo`의 콜백 함수는 외부 함수 `person.foo`를 돕는 헬퍼 함수(보조 함수) 역할을 하기 때문에 외부 함수 `person.foo` 내부의 `this`와 콜백 함수 내부의 `this`가 상이하면 문맥상 문제가 발생한다.  

따라서 콜백 함수 내부의 `this`를 외부 함수 내부의 `this`와 일치시켜야 한다.  

이때 `bind` 메소드를 사용하여 `this`를 일치시킬 수 있다.

``` js
const person = {
    name: 'Lee',
    foo(callback) {
        // bind 메소드로 callback 함수 내부의 this 바인딩을 전달
        setTimeout(callback.bind(this), 100);
    }
};

person.foo(function () {
    console.log(`Hi! my name is ${this.name}`);
});
```









