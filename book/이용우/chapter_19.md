### Chapter 19 - 프로토타입

자바스크립트는 명령형(imperative), 함수형(functional), 프로토타입 기반(prototype-based),  
객체 지향 프로그램이(OOP; Object Oriented Programming)을 지원하는 멀티 페러다임 프로그래밍 언어입니다.

자바스크립트를 이루고 있는 거의 "모든 것"이 객체입니다.
원시 타입(primitive type)을 제외한 나머지 값들(함수, 배열, 정규 표현식 등)은 모두 객체입니다.

<br><br>

### 객체 지향 프로그래밍
객체 지향 프로그래밍은 실세계의 실체(사물이나 개념)을 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작합니다.  
실체는 특징이나 성질을 나타내는 **속성(attribute/property)**을 가지고 있고,  
이를 통해 실체를 인식하거나 구별할 수 있습니다.

사람의 "이름"과 "주소"라는 속성을 갖는 `person`이라는 객체를 자바스크립트로 표현하면 아래와 같을 것입니다.

``` js
const person = {
    name: 'Lee',
    address: 'Seoul'
};

console.log(person);
```

위와 같이 <font color='orange'>속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조를 객체라고 합니다.</font>

<br><br>

조금 더 나아가서 원(Circle)이라는 개념을 객체로 구현해보도록 합시다.  
원에는 반지름이라는 속성이 있고, 이 반지름을 이용해 지름, 둘레, 넓이 등을 구할 수 있습니다.  
이때 반지름은 원의 상태를 나타내는 데이터이고, 원의 지름, 둘레, 넓이를 구하는 것은 동작으로 정의내릴 수 있습니다.

``` js
const circle = {
    radius: 5,
    
    // 원의 지름: 2r
    getDiameter() {
        return 2 * this.radius;
    },

    // 원의 둘레: 2πr
    getPerimeter() {
        return 2 * Math.PI * this.radius;
    },

    // 원의 넓이: πrr
    getArea() {
        return Math.PI * this.radius ** 2;
    }
};
```

이처럼 객체 지향 프로그래밍은 객체의 상태(state)를 나타내는 데이터와  
상태 데이터를 조작할 수 있는 동작(behavior)를 하나의 논리적인 단위로 묶어 생각할 수 있습니다.

각 객체는 고유의 기능을 갖는 독립적인 부품으로 볼 수도 있지만  
자신의 고유한 기능을 수행하면서 다른 객체와 관계성을 가질 수도 있다.  

<br><br>

### 상속과 프로토타입
상속은 객체지향 프로그래밍의 핵심 개념이며,  
<font color='orange'>어떤 객체의 프로퍼티 또는 메소드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말합니다. </font>

<font color='orange'>자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거합니다.</font>  
중복을 제거하는 방법은 기존의 코드를 적극적으로 재사용하는 것입니다.

``` js
// 생성자 함수
function Circle(radius) {
    this.radius = radius;
    this.getArea = function () {
        return Math.PI * this.radius ** 2;
    };
}

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);

// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는 
// getArea 메소드를 중복 생성하고 모든 인스턴스가 중복 소유합니다.
// getArea 메소드는 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직합니다.
console.log(circle.getArea === circle2.getArea);    // false

console.log(circle1.getArea());
console.log(circle2.getArea());
```

<br>

`Circle` 생성자 함수가 생성하는 모든 객체(인스턴스)는 `radius` 프로퍼티와 `getArea` 메소드를 갖습니다.  
`radius` 프로퍼티 값은 일반적으로 인스턴스마다 다르지만 `getArea` 메소드는 모든 인스턴스가 동일한 내용의 메소드를 사용하므로  
단 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직합니다.  

<font color='orange'>그런데 `Circle` 생성자 함수는 인스턴스를 생성할 때마다 `getArea` 메소드를 중복해서 생성하고 모든 인스턴스가 중복으로 소유하고 있습니다. </font>

<img width="350" alt="image" src="https://github.com/user-attachments/assets/c7c9bec0-54ec-45c5-8326-0907bb9951ee" />

<br>

이처럼 동일한 생성자 함수에 의해 생성된 모든 인스턴스가 동일한 메소드를 중복 소유하는 것은 메모리를 불필요하게 낭비합니다.  
또한 인스턴스를 생성할 때마다 메소드를 생성하므로 퍼포먼스에도 악영향을 줍니다.  
만약 10개의 인스턴스를 생성하면 내용이 동일한 메소드로 10개 성상됩니다.  

<br>

상속을 통해 불필요한 중복을 제거할 수 있습니다.  
**자바스크립트는 프로토타입(prototype)을 기반으로 상속을 구현합니다.**

``` js
// 생성자 함수
function Circle(radius) {
    this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메소드를
// 공유해서 사용할 수 있도록 프로토토압에 추가합니다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로토타입에 바인딩되어 있습니다.
Circle.prototype.getArea = function() {
    return Math.PI * this.radius ** 2;
};

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);

// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는 
// getArea 메소드를 중복 생성하고 모든 인스턴스가 중복 소유합니다.
// getArea 메소드는 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직합니다.
console.log(circle.getArea === circle2.getArea);    // false

console.log(circle1.getArea());
console.log(circle2.getArea());
```

<img width="550" alt="image" src="https://github.com/user-attachments/assets/b21682ae-e183-40ad-8871-ceb0f324f1d9" />

<br>

`Circle` 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입,  
즉 상위(부모) 객체 역할을 하는 `Circle.prototype`의 모든 프로퍼티와 메소드를 상속받습니다. 

`getArea` 메소드는 단 하나만 생성되어 프로토타입인 `Circle.prototype`의 메소드로 할당되어 있습니다.  
따라서 `Circle` 생성자 함수가 생성되는 모든 인스턴스는 `getArea` 메소드를 상속받아 사용할 수 있습니다.  

즉, 자신의 상태를 나타내는 `radius` 프로퍼티만 개별적으로 소유하고 내용이 동일한 메소드는 상속을 통해 공유하여 사용하는 것입니다.   
상속은 코드의 재사용이란 관점에서 매우 유용합니다.  
<font color='orange'>메소드를 프로토타입에 미리 구현해두면 모든 인스턴스는 별도의 구현 없이 상위(부모) 객체인 프로토토압의 자산을 공유하여 사용할 수 있습니다. </font>

<br><br>

#### 프로토타입 객체
프로토타입 객체(또는 줄여서 프로토타입)란 객체지향 프로그래밍의 근간을 이루는 객체 간 상속 을 구현하기 위해 사용됩니다.  
프로토타입은 어떤 객체의 상위(부모) 객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티(메서드 포함)를 제공합니다.  
프로토타입을 상속받은 하위(자식) 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있습니다.

모든 객체는 [[Prototype]] 이라는 내부 슬롯을 가지며, [[Prototype]]에 저장되는 프로토타입은 객체 생성 방식에 의해 결정됩니다.  
즉, 객체가 생성될 때 객체 생성 방식에 따라 프로토타입이 결정되고 [[Prototype]]에 저장됩니다.

> 예를 들어, 객체 리터럴에 의해 생성된 객체의 프로토타입은 `Object.prototype`이고  
> 생성자 함수에 의해 생 성된 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다. 

모든 객체는 하나의 프로토타입을 갖습니다. 그리고 모든 프로토타입은 생성자 함수와 연결되어 있습니다.  
<font color='orange'>즉, 객체와 프로토타입과 생성자 함수는 다음 그림과 같이 서로 연결되어 있습니다.</font>

<br><br>

<img width="400" alt="image" src="https://github.com/user-attachments/assets/61ffaf59-7522-4244-bf04-87c0edcca57f" />

[[Prototype]] 내부 슬롯에는 직접 접근할 수 없지만, 위와 같이 `__proto__` 접근자 프로퍼티를 통해  
자신의 프로토타입, 즉 자신의 [[Prototype]] 내부 슬롯이 가리키는 프로토타입에 간접적으로 접근할 수 있습니다.  

그리고 프로토타입은 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근할 수 있고,  
생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근할 수 있습니다.

<br><br>

##### __proto__ 접근자 프로퍼티
모든 객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입,  
즉 [[Prototype]] 내부 슬롯에 **간접적으로 접근할 수 있습니다.**

<img width="300" alt="image" src="https://github.com/user-attachments/assets/bb50a2ca-1454-4d59-bb06-ef967dcad068" />


<br><br>

<font color='tomato'>`__proto__`는 접근자 프로퍼티이다.</font>  

16.1절 "내부 슬롯과 내부 메서드"에서 살펴보았듯이 내부 슬롯은 프로퍼티가 아닙니다.  
따라서 자바스크립트는 원칙적으로 내부 슬롯과 내부 메서드에 직접적으로 접근하거나 호출할 수 있는 방법을 제공하지 않습니다.  

단, 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단을 제공합니다. 

[[Prototype]] 내부 슬롯이 _proto__ 접근자 프로퍼티를 통해 간접적으로 [[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근할 수 있습니다.

16.3.2절 "접근자 프로퍼티"에서 살펴본 것처럼  
접근자 프로퍼티는 자체적으로는 값([[Value]] 프로퍼티 어트리뷰트)을 갖지 않고  
다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수(accessor function),  
즉 [[Get]], [[Set]] 프로퍼티 어트리뷰트로 구성된 프로퍼티입니다.

<br>

<img width="400" alt="image" src="https://github.com/user-attachments/assets/76852c15-7d07-4078-8319-00ff322a8e14" />


``` js
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__ 가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;

// setter 함수인 set __proto__ 가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x);     // 1
```

<br><br>

<font color='tomato'>`__proto__` 접근자 프로퍼티는 상속을 통해 사용된다.</font>

`__proto__` 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 `Object.prototype`의 프로퍼티입니다.  
모든 객체는 상속을 통해 `Object.prototype.__proto__` 접근자 프로퍼티를 사용할 수 있습니다.

``` js
const person = { name: 'Lee' };

// person 객체는 __proto__ 프로퍼티를 소유하지 않습니다.
console.log(person.hasOwnProperty('__proto__'));        // false

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티입니다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));
// {get: f, set: f, enumerable: false, configurable: true}

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있습니다.
console.log({}.__proto__ === Object.prototype);     // true
```

<br>

> ※ `Object.prototype`  
> 모든 객체는 프로토타입의 계층 구조인 프로토타입 체인에 묶여 있다.  
> 자바스크립트 엔진은 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때  
> 해당 객체에 접근하려는 프로퍼티가 없다면 `__proto__` 접근자 프로퍼티가 가리키는 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 
> 
> 프로토타입 체인의 종점, 즉 프로토타입 체인의 최상위 객체는 `0bject.prototype`이며,  
> 이 객체의 프로퍼티와 메서드는 모든 객체에 상속된다. 

<br><br>

<font color='tomato'>`__proto__` 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유</font>   
[[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근하기 위해 접근자 프로퍼티를 사용하는 이유는  
상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서입니다.

``` js
const parent = {};
const child = {};

// child의 프로토타입을 parent로 설정
child.__proto__ == parent;

// parent의 프로토타입을 child로 설정
parent.__proto__ = child;       // TypeError: Cyclic __proto__ value
``` 

<br>

위 예제에서는 `parent` 객체를 `child` 객체의 프로토타입으로 설정한 후,  
`child` 객체를 `parent` 객체의 프로토타입으로 설정했습니다.

이러한 코드가 에러 업싱 정상적으로 처리되면  
서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인이 만들어지기 때문에  
`__proto__` 접근자 프로퍼티는 에러를 발생시킵니다.

<img width="250" alt="image" src="https://github.com/user-attachments/assets/418e3e23-f40f-4329-a10a-4ff71e0b6fd4" />

> 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인

<br><br>

이런 문제 때문에 프로토타입 체인은 단방향 Linked List로 구현되어야 합니다. 
따라서 아무런 체크 없이 무조건적으로 프로토타입을 교체하는 로직이 발생하지 않도록  
`__proto__` 접근자 프로퍼티를 통해 프로토타입에 접근하고 교체하도록 구현되어 있습니다.



<br><br>

<font color='tomato'>`__proto__` 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.</font>     
코드 내에서 `__proto__` 접근자 프로퍼티를 직접 사용하는 것은 권장하지 않습니다.  
모든 객체가 `__proto__` 접근자 프로퍼티를 사용할 수 있는 것은 아니기 때문입니다. 

직접 상속을 통해 아래와 같이 `Object.prototype`을 상속받지 않는 객체를 생성할 수도 있기 때문에  
`__proto__` 접근자 프로퍼티를 사용할 수 없는 경우가 있습니다.

``` js
// obj는 프로토타입 체인의 종점이다. 따라서 Object.__proto__를 상속받을 수 없습니다.
const obj = Object.create(null);

// obj는 Object.__proto__를 상속받을 수 없습니다.
console.log(obj.__proto__);     // undefined

// 따라서 __proto__보다 Object.getPrototypeOf 메소드를 사용하는 것이 바람직하다.
console.log(Object.getPrototypeOf(obj));        // null
```

따라서 프로토타입의 참조를 얻고 싶은 경우에는, `Obejct.getPrototypeOf` 메소드를 사용하고,  
프로토타입을 교체하고 싶은 경우에는 `Object.setPrototypeO ㅈf` 메소드를 사용할 것을 권장합니다.

``` js
const obj = {};
const parent = { x: 1 };

// obj 객체의 프로토타입을 취득
Object.getPrototypeOf(obj);     // obj.__proto__;

// obj 객체의 프로토타입을 교체
Object.setPrototypeOf(obj, parent);     // obj.__proto__ = parent;

console.log(obj.x);     // 1
```

<br><br>

#### 함수 객체의 prototype 프로퍼티
함수 객체가 소유한 `prototype` 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킵니다.

``` js
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function() {}).hasOwnProperty('prototype');    // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype');   // false
```

<br>

`prototype` 프로퍼티는 생성자 함수가 생성할 객체(인스턴스)의 프로토타입을 가리킵니다.  

따라서 생성자 함수로서 호출할 수 없는 함수, 즉 non-constructor인 화살표 함수와 ES6 메소드 축약 표현으로 정의한 메소드는  
`prototype` 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않습니다.

``` js
// 화살표 함수는 non-constructor 이다.
const Person = name => {
    this.name = name;
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(Person.hasOwnProperty('prototype'));    // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(Person.prototype);  // undefined

// ES6의 메소드 축약 표현으로 정의한 메소드는 non-constructor다.
const obj = {
    foo() {}
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(obj.foo.hasOwnProperty('prototype'));   // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(obj.foo.prototype);     // undefined
```

<br>

모든 객체가 가지고 있는 `__proto__` 접근자 프로퍼티와  
함수 객체만이 가지고 있는 `prototype` 프로퍼티는 결국 동일한 프로토타입을 가리킵니다.  
하지만 이들 프로퍼티를 사용하는 주체가 다릅니다.

|구분|소유|값|사용 주체|사용 목적|
|--|--|--|--|--|
|`__proto__` 접근자 프로퍼티|모든 객체|프로토타입의 참조|모든 객체|객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용|
|`prototype` 프로퍼티|constructor|프로토타입의 참조|생성자 함수|생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용|

<br><br>

``` js
// 생성자 함수
function Person(name) {
    this.name = name;
}

const me = new Person('Lee');

// Person.prototype와 me.__proto__는 동일한 프로토타입을 가리킨다.
console.log(Person.prototype === me.__proto__);     // true
```

<br>

<img width="500" alt="image" src="https://github.com/user-attachments/assets/2170f869-879a-4b14-8229-87b0132a8484" />

<br>

<font color='tomato'>모든 인스턴스는 결국 같은 prototype을 사용한다.</font>


<br><br>

#### 프로토타입의 constructor 프로퍼티와 생성자 함수
모든 프로토타입은 `constructor` 프로퍼티를 갖습니다.  
이 `constructor` 프로퍼티는 `prototype` 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킵니다.

``` js
// 생성자 함수
function Person(name) {
    this.name = name;
}

const me = new Person('Lee');

// me 객체의 생성자 함수는 Person
console.log(me.constructor === Person);     // true
```

<img width="400" alt="image" src="https://github.com/user-attachments/assets/de4a6966-f0af-4dce-913a-6d310d5e02c5" />



<br><br>

### 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입
생성자 함수에 의해 생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결됩니다.  
> constructor 프로퍼티가 가리키는 생성자 함수는 인스턴스를 생성한 생성자 함수다.

``` js
// obj 객체를 생성한 생성자 함수는 Object이다.
const obj = new Object();
console.log(obj.constructor === Object);        // true

// add 함수 객체를 생성한 생성자 함수는 Function이다.
const add = new Function('a', 'b', 'return a + b');
console.log(add.constructor === Function);  // true

// 생성자 함수
function Person(name) {
    this.name = name;
}

// me 객체를 생성한 생성자 함수는 Person이다.
const me = new Person('Lee');
console.log(me.constructor === Person);     // true
```

<br>

하지만 리터럴 표기법에 의한 객체 생성 방식과 같이 명시적으로 `new` 연산자와 함께 생성자 함수를 호출하여  
인스턴스를 생성하지 않는 객체 생성 방식도 있습니다.

``` js
// 객체 리터럴
const obj = {};

// 함수 리터럴
const add = function (a, b) {
    return a + b;
}

// 배열 리터럴
const arr = [1, 2, 3];

// 정규 표현식 리터럴
const regexp = /is/ig;
```

<br>

리터럴 표기법에 의해 생성된 객체도 프로토타입이 존재합니다.  

<font color='orange'>하지만 리터럴 표기법에 의해 생성된 객체의 경우  
프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수는 없습니다.</font>

``` js
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성했다.
const obj = {};

// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수이다.
console.log(obj.constructor === Obejct);    // true
```

위 예제의 obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴에 의해 생성된 객체입니다.  
하지만 obj 객체는 Object 생성자 함수와 constructor 프로퍼티로 연결되어 있습니다.  

그렇다면 객체 리터럴에 의해 생성된 객체는 사실 Object 생성자 함수로 생성되는 것은 아닐까?  
ECMAScript 사양을 살펴봅시다.

> Object 생성자 함수에 인수를 전달하지 않거나 `undefined` 또는 `null`을 인수로 전달하면서 호출하면  
> 내부적으로는 추상 연산 `OrdinaryObjectCreate`를 호출하여 `Object.prototype`을 프로토타입으로 갖는 빈 객체를 생성합니다.

이는 <font color='tomato'>"빈 객체를 생성할 때, 전역 prototype (`Object.prototype`)을 가지는 객체가 생성된다."</font>와 같습니다.

<br><br>

※ 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입  
|리터럴 표기법|생성자 함수|프로토타입|
|---|---|---|
|객체 리터럴|`Object`|`Object.prototype`|
|함수 리터럴|`Function`|`Function.prototype`|
|배열 리터럴|`Array`|`Array.prototype`|
|정규표현식 리터럴|`RegExp`|`ExgExp.prototype`|


<br><br>

### 프로토타입의 생성 시점
객체는 리터럴 표기법 또는 생성자 함수에 의해 생성되므로 결국 모든 객체는 생성자 함수와 연결되어 있습니다.  

<font color='orange'>프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성됩니다.</font>  
프로토타입과 생성자 함수는 단독으로 존재하지 않고 언제나 쌍으로 존재하기 때문입니다.  

생성자 함수는 사용자가 직접 정의한 사용자 정의 생성자 함수와 자바스크립트가 기본 제공하는 빌트인 생성자 함수로 구분할 수 있습니다.  
사용자 정의 생성자 함수와 빌트인 생성자 함수를 구분하여 프로토타입 생성 시점에 대해 살펴볼 것입니다.

<br><br>

#### 사용자 정의 생성자 함수와 프로토타입 생성 시점
내부 메소드 `[[Construct]]`를 갖는 함수 객체,  
즉 화살표 함수나 ES6의 메소드 축약 표현으로 정의하지 않고   
일반 함수(함수 선언문, 함수 표현식)로 정의한 함수 객체는 `new` 연산자와 함께 생성자 함수로서 호출할 수 있습니다.

``` js
// 함수 정의(constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성됩니다.
console.log(Person.prototype);      // {constructor: f}

// 생성자 함수
function Person(name) {
    this.name = name;
}
```

생성자 함수로서 호출할 수 없는 함수인 `non-constructor`는 프로토타입이 생성되지 않습니다.

``` js
// 화살표 함수는 non-constructor 입니다.
const Person = name => {
    this.name = name;
};

// non-constructor는 프로토타입이 생성되지 않습니다.
console.log(Person.prototype);      // undefined


// 메소드 축약 표현 객체 리터럴은 non-constructor 입니다.
const person = {
    name: 'Lee',
    setName(name) {
        this.name = name;
    }
}

// non-constructor는 프로토타입이 생성되지 않습니다.
console.log(person.setName.prototype);      // undefined
```

<br>

"12장 함수 생성 시점과 함수 호이스팅"에서 보았듯이,  
함수 선언문은 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행됩니다.  

따라서 함수 선언문으로 정의된 `Person` 생성자 함수는 어떤 코드보다 먼저 평가되어 함수 객체가 됩니다.  
이때 프로토타입도 더불어 생성됩니다.  

생성된 프로토타입은 `Person` 생성자 함수의 `prototype` 프로퍼티에 바인딩됩니다.

<img width="350" alt="image" src="https://github.com/user-attachments/assets/9c4bd199-9585-4c6e-8f03-6b80df79e628" />

생성된 프로토타입은 오직 `constructor` 프로퍼티만 갖는 객체로 보입니다.  
<font color='orange'>프로토타입도 객체이고, 모든 객체는 프로토타입을 가지므로 프로토타입도 자신의 프로토타입을 가집니다.</font>

> 생성된 프로토타입의 프로토타입은 `Object.prototype` 입니다.

<img width="500" alt="image" src="https://github.com/user-attachments/assets/ddc9c3d7-0098-4c79-afe2-09a4aae74067" />

<br>

정리하자면, **사용자 정의 생성자 함수는 자신이 평가되어 함수 객체로 생성되는 시점에 프로토타입도 더불어 생성되며,**  
**생성된 프로토타입의 프로토타입은 언제나 `Object.prototype`입니다.**

<br><br>

#### 빌트인 생성자 함수와 프로토타입 생성 시점
`Object`, `String`, `Number`, `Function`, `Array`, `RegExp`, `Date`, `Promise` 등과 같은 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성됩니다.  

<font color='orange'>모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성됩니다.</font>

생성된 프로토타입은 빌트인 생성자 함수의 `prototype` 프로퍼티에 바인딩됩니다.

<img width="500" alt="image" src="https://github.com/user-attachments/assets/bcedbf64-7a03-4aaa-8738-ceb0cf270acf" />

<br>

이처럼 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재합니다.  
이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면  
프로토타입은 생성된 객체의 `[[Prototype]]` 내부 슬롯에 할당됩니다. 이후 생성된 객체는 프로토타입을 상속받습니다.

<br><br><br>

### 객체 생성 방식과 프로토타입의 결정
객체는 아래와 같이 다양한 생성 방법이 있습니다.
- 객체 리터럴
- `Object` 생성자 함수
- 생성자 함수
- `Object.create` 메소드
- 클래스(ES6)

<br><br>

이처럼 다양한 방식으로 생성된 모든 객체는 각 방식마다 세부적인 객체 생성 방식의 차이는 있으나  
추상 연산 `ordinaryobjectCreate`에 의해 생성된다는 공통점이 있습니다.

이는 즉, 프로토타입은 추상 연산 `OrdinaryObjectCreate`에 전달되는 인수에 의해 결정됩니다.  
이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정됩니다.  

<br><br>

#### 객체 리터럴에 의해 생성된 객체의 프로토타입
자바스크립트 엔진은 <font color='orange'>리터럴을 평가하여 객체를 생성할 때 추상 연산 `OrdinaryObjectCreate`를 호출합니다.</font>

이때 추상 연산 `OrdinaryObjectCreate`에 전달되는 프로토타입은 `Object.prototype`입니다.  
즉, 객체 리터럴에 의해 생성되는 객체의 프로토타입은 `Object.prototype`입니다.

``` js
const obj = {
    x: 1
};
```

위 객체 리터럴이 평가되면 추상 연산 `OrdinaryObjectCreate`에 의해  
다음과 같이 `Object` 생성자 함수와 `Object.prototype`과 생성된 객체 사이에 연결이 만들어집니다.  

<img width="450" alt="image" src="https://github.com/user-attachments/assets/8ddb387b-b878-4c44-91ae-ef769509e6e3" />


<br>

이와 같이 객체 리터럴에 의해 생성된 `obj` 객체는 `Object.prototype`을 프로토타입으로 갖게 되며,  
`Object.prototype`을 상속받습니다.  

`obj` 객체는 `constructor` 프로퍼티와 `hasOwnProperty` 메소드 등을 소유하지는 않지만  
자신의 프로토타입인 `Object.prototype`의 `constructor` 프로퍼티와 `hasOwnProperty` 메소드를  
자신의 자산인 것처럼 사용 가능합니다.

> `obj` 객체가 자신의 프로토타입인 `Object.prototype` 객체를 상속받았기 때문이다.

``` js
const obj = {
    x: 1
};


// 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속받습니다.
console.log(obj.constructor === Object);    // true
console.log(obj.hasOwnProperty('x'));       // true
```

<br><br>

#### Object 생성자 함수에 의해 생성된 객체의 프로토타입
`Object` 생성자 함수를 인수 없이 호출하면 빈 객체가 생성됩니다.  
`Object` 생성자 함수를 호출하면 객체 리터럴과 마찬가지로 추상 연산 `OrdinaryObjectCreate`가 호출됩니다.  

이때 추상 연산 `OrdinaryObjectCreate`에 전달되는 프로토타입은 `Object.prototype`입니다. 
즉, `Object` 생성자 함수에 의해 생성되는 객체의 프로토타입은 `Object.prototype`입니다.  

``` js
const obj = new Object();
obj.x = 1;
```

<br><br>

위 코드가 실행되면 추상 연산 `OrdinaryObjectCreate`에 의해  
아래와 같이 `Object` 생성자 함수와 `Object.prototype`과 생성된 객체 사이에 연결이 만들어집니다.  
> 객체 리터럴에 의해 생성된 객체와 동일한 구조를 갖게 된다.

<img width="450" alt="image" src="https://github.com/user-attachments/assets/1bae91c4-6ddd-4b06-a2a5-d2772d38e735" />

<br>

``` js
const obj = new Object();
obj.x = 1;

// Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 상속받습니다.
console.log(obj.constructor === Object);        // true
console.log(obj.hasOwnProperty('x'));           // true
```

<br><br>

#### 생성자 함수에 의해 생성된 객체의 프로토타입
`new` 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하면  
다른 객체 생성 방식과 마찬가지로 추상 연산 `OrdinaryObjectCreate`가 호출됩니다.  

즉, 생성자 함수에 의해 생성되는 객체의 프로토타입은  
생성자 함수의 `prototype` 프로퍼티에 바인딩되어 있는 객체입니다.


``` js
function Person(name) {
    this.name = name;
}

const me = new Person('Lee');
```

위 코드가 실행되면 추상 연산 `OrdinaryObjectCreate`에 의해  
다음과 같이 생성자 함수와 생성자 함수의 `prototype` 프로퍼티에 바인딩되어 있는 객체와 생성된 객체 사이에 연결이 만들어집니다.  

<img width="500" alt="image" src="https://github.com/user-attachments/assets/dab4aca2-1dcd-419c-a2af-ef88f453bd62" />

<br>

표준 빌트인 객체인 `Object` 생성자 함수와 더불어 생성된 프로토타입 `Object.prototype`은  
다양한 빌트인 메소드(`hasOwnProperty`, `propertyIsEnumerable` 등)를 가지고 있습니다.  

하지만 사용자 정의 생성자 함수와 더불어 생성된 프로토타입 `Person.prototype`의 프로퍼티는 `constructor` 뿐입니다.

<br>

``` js
function Person(name) {
    this.name = name;
}

// 프로토타입 메소드
Person.prototype.sayHello = function() {
    console.log(`Hi! my name is ${this.name}`);
};

const me = new Person('Lee');
const you = new Person('Kim');

me.sayHello();
you.sayHello();
``` 

`Person` 생성자 함수를 통해 생성된 모든 객체는 프로토타입에 추가된 `sayHello` 메소드를 상속받아  
자신의 메소드처럼 사용할 수 있습니다.

<img width="500" alt="image" src="https://github.com/user-attachments/assets/047b165f-d89d-4f48-a52e-ba53b0191c85" />

<br><br>

또한 자바스크립트는 프로토타입 체인을 통해 빌트인 메소드를 사용할 수 있도록 지원합니다.

<br><br>

### 프로토타입 체인

``` js
function Person(name) {
    this.name = name;
}

// 프로토타입 메소드
Person.prototype.sayHello = function() {
    console.log(`Hi! my name is ${this.name}`);
};

const me = new Person('Lee');

// hasOwnPeropery는 Object.prototype의 메소드입니다.
console.log(me.hasOwnProperty('name'));     // true
```

`Person` 생성자 함수에 의해 생성된 me 객체는 `Object.prototype`의 메서드인 hasownProperty를 호출할 수 있습니다.  
이것은 me 객체가 `Person.prototype`뿐만 아니라 `Object.prototype`도 상속받았다는 것을 의미합니다.

<br>

``` js
Object.getPrototypeOf(me) === Person.prototype;     // true
```

`Person.prototype`의 프로토타입은 `Object.prototype`입니다.  
프로토타입의 프로토타입은 언제나 `Object.prototype`인 것입니다.

``` js
Object.getPrototypeOf(Person.prototype) === Object.prototype;       // true
```
<img width="600" alt="image" src="https://github.com/user-attachments/assets/96c2ba78-f20b-401c-b52b-b545502a6d0a" />

<br>

<font color='orange'>자바스크립트는 객체의 프로퍼티(메소드 포함)에 접근하려 할 때  
해당 객체에 접근하려는 프로퍼티가 없다면 `[[Prototype]]` 내부 슬롯의 참조를 따라   
자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색하는데, 이것을 프로토타입 체인이라고 합니다.</font>


``` js
// me 객체는 프로토타입 체인을 따라 hasOwnProperty 메소드를 검색하여 사용합니다.
me.hasOwnProperty('name');
```

<br><br><br>

### 오버라이딩과 프로퍼티 섀도잉
``` js
const Person = (function () {
    // 생성자 함수
    function Person(name) {
        this.name = name;
    }

    // 프로토타입 메소드
    Person.prototype.sayHello = function() {
        console.log(`Hi, My name is ${this.name}`);
    };

    // 생성자 함수를 반환
    return Person;
}());

const me = new Person('Lee');

// 인스턴스 메소드
me.sayHello = function() {
    console.log(`Hey! My name is ${this.name}`);
};

// 인스턴스 메소드가 호출됩니다.
// 프로토타입 메소드는 인스턴스 메소드에 의해 오버라이딩 됩니다.
me.sayHello();      // Hey! My name is Lee
```

생성자 함수로 객체(인스턴스)를 생성한 다음, 인스턴스에 동일 이름의 메소드를 새로 선언하면  
새로이 생성한 메소드로 **오버라이딩** 됩니다.

그리고 이 현상에 의해 가려진 원래의 메소드가 사라지는 현상을 **프로퍼티 섀도잉**이라 부릅니다.

<br><br>

### 직접 상속
#### Object.create에 의한 직접 상속
`Object.create` 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성합니다.  
`Object.create` 메서드도 다른 객체 생성 방식과 마찬가지로 추상 연산 `OrdinaryObjectCreate`를 호출합니다.  

`Object.create` 메서드의 **첫 번째 매개변수에는 생성할 객체의 프로토타입으로 지정할 객체를 전달합니다.**  
**두 번째 매개변수에는 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체를 전달합니다.**  

이 객체의 형식은 `Object.defineProperties` 메서드의 두 번째 인수와 동일합니다.  
두 번째 인수는 옵션이므로 생략 가능합니다.

``` js
/**
 * 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체를 생성하여 반환
 * @param {Object} prototype - 생성할 객체의 프로토타입으로 지정할 객체
 * @param {Object} [propertiesObject] - 생성할 객체의 프로퍼티를 갖는 객체
 * @returns {Object} 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체
 */
Object.create(prototype[, propertiesObject])
```

<br>

``` js
// 프로토타입이 null인 객체를 생성합니다. 생성된 객체는 프로토타입 체인 종점에 위치합니다.
// obj → null
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null);   // true

// Object.prototype을 상속받지 못합니다.
console.log(obj.toString());    // TypeError: obj.toString is not a function

// obj → Object.prototype → null
// obj = {};와 동일합니다.
obj = Object.create(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype);   // true

// obj → Object.prototype → null
// obj = { x: 1 };와 동일합니다.
obj = Object.create(Object.prototype, {
    x: { value: 1, writable: true, enumerable: true, configurable: true}
});

console.log(obj.x);     // 1
console.log(Object.getPrototypeOf(obj) === Object.prototype);   // true

const myProto = { x: 10 };
// 임의의 객체를 직접 상속받는다.
// obj → myProto → Object.prototype → null
obj = Object.create(myProto);
console.log(obj.x);     // 10
console.log(Object.getPrototypeOf(obj) === myProto);    // true

// 생성자 함수
function Person(name) {
    this.name = name;
}

// obj → Person.prototype → Object.prototype → null
// obj = new Person('Lee')와 동일합니다.
obj = Object.create(Person.prototype);
obj.name = 'Lee';
console.log(obj.name);
console.log(Object.getPrototypeOf(obj) === Person.prototype);   // true
```

위와 같이 `Object.create` 메소드는 첫 번째 매개변수에 전달한 객체의 프로토타입 체인에 속하는 객체를 생성합니다.  
즉, 객체를 생서앟면서 직접적으로 상속을 구현하는 것입니다.

장점은 아래와 같습니다.
- `new` 연산자 없이도 객체 생성이 가능합니다.
- 프로토타입을 지정하면서 객체를 생성할 수 있습니다.
- 객체 리터럴에 의해 생성된 객체도 상속받을 수 있습니다.


<br>

단 빌트인 메소드를 객체가 직접 호출하는 것은 권장하지 않습니다.
``` js
// 프로토타입이 null인 객체, 즉 프로토타입 체인의 종점에 위치하는 객체를 생성합니다.
const obj = Object.create(null);
obj.a = 1;

console.log(Object.getPrototypeOf(obj) === null);   // true

// obj는 Object.prototype의 빌트인 메소드를 사용할 수 없다.
console.log(obj.hasOwnProperty('a'));   // TypeError: obj.hasOwnProperty is not a function
```

<br>

따라서 Object.prototype의 빌트인 메소드는 아래와 같이 간접적으로 호출하는 것이 좋습니다.

``` js
// 프로토타입이 null인 객체를 생성
const obj = Object.create(null);
obj.a = 1;

// Object.prototype의 빌트인 메소드는 객체로 직접 호출하지 않습니다.
console.log(Object.prototype.hasOwnProperty.call(obj, 'a'));    // true
```

<br><br>

#### 객체 리터럴 내부에서 __proto__에 의한 직접 상속
`Object.create` 메소드에 의한 직접 상속은 여러 장점들이 있지만,  
**두 번째 인자로 프로퍼티를 정의하는 것이 번거롭습니다.**

객체를 일단 생성한 다음에 프로퍼티를 추가하는 방법도 있지만, 이 또한 깔끔하지는 않습니다.
ES6에서는 객체 리터럴 내부에서 `__proto__` 접근자 프로퍼티를 사용하여 직접 상속을 구현할 수 있습니다.

``` js
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있습니다.
const obj = {
    y: 20,
    // 객체를 직접 상속받는다.
    // obj → myProto → Object.prototype → null
    __proto__: myProto
};

/* 위 코드는 아래와 동일합니다.
const obj = Object.create(myProto, {
    y: { value: 20, writable: true, enumerable: true, configurable: true }
});
*/

console.log(obj.x, obj.y);  // 10, 20
console.log(Object.getPrototypeOf(obj) === myProto);    // true
```

<br><br>

### 프로퍼티 존재 확인

#### in 연산자
`in` 연산자는 객체 내 특정 프로퍼티가 존재하는지 여부를 확인합니다. 
``` js
/**
 * key: 프로퍼티 키를 나타내는 문자열
 * object: 객체로 평가되는 표현식 
 */
key in object
```

``` js
const person = {
    name: 'Lee',
    address: 'Seoul'
};

// person 객체에 name 프로퍼티가 존재합니다.
console.log('name' in person);  // true

// person 객체에 age 프로퍼티가 존재하지 않습니다.
console.log('age' in person);  // false
```

`in` 연산자는 확인 대상 객체의 프로퍼티 뿐만 아니라  
확인 대상 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인하므로 주의가 필요합니다.

``` js
console.log('toString' in person);  // true
```

`person` 객체에는 `toString`이라는 프로퍼티가 없지만 아래의 실행결과는 true 입니다.
이는 `in` 연산자가 `person` 객체가 속한 프로토타입 체인 상 존재하는  
모든 프로토타입에서 `toString` 프로퍼티를 검색했기 때문입니다.

<br><br>

`in` 연산자 대신 ES6에서 도입된 `Reflict.has` 메소드를 사용할 수 있습니다.

``` js
const perosn = {
    name: 'Lee'
};

console.log(Reflict.has(person, 'name'));   // true
console.log(Reflict.has(person, 'toString'));   // true
```

<br><br>

#### Object.prototype.hasOwnProperty 메소드
`Object.prototype.hasOwnProperty` 메소드를 사용해도 객체에 특정 프로퍼티가 존재하는지 확인할 수 있습니다.

```js
console.log(person.hasOwnProperty('name'));     // true
console.log(person.hasOwnProperty('age'));      // false
```

<br>

`Object.prototype.hasOwnProperty` 메소드는 이름에서 알 수 있듯이   
인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 `true`를 반환하고  
상속받은 프로토타입의 프로퍼티 키인 경우 `false`를 반환합니다.

``` js
console.log(person.hasOwnProperty('toString'));     // false
```

<br><br>


### 프로토타입 열거

#### for ... in 문
객체의 모든 프로퍼티를 순회하며 열거하려면 `for ... in` 문을 사용하면 됩니다.

``` js
const perosn = {
    name: 'Lee', 
    address: 'Seoul'
};

// for ... in 문의 변수 prop에 person 객체의 프로퍼티 키가 할당됩니다.
for (const key in person) {
    console.log(key + ': ' + person[key]);
}
```

<br><br>

#### Object.keys/values/entries 메소드
`for ... in` 문은 상속받은 프로퍼티 까지 열거합니다.

객체 자신의 고유 프로퍼티만 열거하기 위해서는  
**`for ... in` 보다는 `Object.keys/values/entries` 메소드를 사용하는 것이 권장됩니다.**

``` js
const person = {
    name: 'Lee',
    address: 'Seoul',
    __proto__: { age: 20 }
};

console.log(Object.keys(person));       // ["name", "address"]
```

<br>

ES8에서 도입된 `Object.values` 메소드는 객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환합니다.

``` js
console.log(Object.values(person));     // ["Lee", "Seoul"]
```

<br>

ES8에서 도입된 `Object.entries` 메소드는 객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환합니다.

``` js
console.log(Object.entries(person));       // [["name", "Lee"], ["address", "Seoul"]]

Object.entires(person).forEach(([key, value]) => console.log(key, value))
/*
name Lee
address Seoul
*/
```

