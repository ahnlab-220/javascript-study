### Chapter 25 - 클래스

자바스크립트는 프로토타입 기반 객체지향 언어이다.  

<font color='orange'>프로토타입 기반 객체지향 언어는 클래스가 필요없는 객체지향 프로그래밍 언어이다.</font>  
ES5에서는 클래스 없이도 아래와 같이 생성자 함수와 프로토타입을 통해 객체지향 언어의 상속을 구현할 수 있다.

``` js
// ES5 생성자 함수
var Person = (function () {
    // 생성자 함수
    function Person(name) {
        this.name = name;
    }

    // 프로토타입 메소드
    Person.prototype.sayHi = function () {
        console.log('Hi! My name is' + this.name);
    };

    // 생성자 함수 반환
    return Person;
}());


// 인스턴스 생성
var me = new Person('Lee');
me.sayHi();     // Hi! My name is Lee
```

하지만 클래스 언어 기반 언어에 익숙한 프로그래머들은  
프로토타입 기반 프로그래밍 방식에 혼란을 느낄 수 있다.

ES6에서 도입된 클래스는 기존 프로토타입 기반 객체지향 프로그래밍보다  
자바나 C#같은 클래스 기반 객체지향 프로그래밍에 익숙한 프로그래머가 더욱 빠르게 학습할 수 있도록  
클래스 기반 객체지향 프로그래밍 언어와 매우 흡사한 새로운 객체 생성 매커니즘을 제시하였다.  

> ES6의 클래스가 기존의 프로토타입 기반 객체지향 모델을 폐지하고 새롭게 클래스 기반 객제지향 모델을 제공하는 것은 아니다. 사실 클래스도 함수이며, 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 것이다.

<br>

다만, 클래스와 생성자 함수는 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않는다.  
클래스는 생성자 함수보다 엄격하며, 생성자 함수에서는 제공하지 않는 기능도 제공한다.

클래스와 생성자 함수는 아래와 같은 차이점을 가진다.

1. 클래스는 `new` 연산자 없이 호출하면 에러가 발생한다.
2. 클래서는 상속을 지원하는 `extends`와 `super` 키워드를 제공한다.
3. 클래스는 호이스팅이 발생하지 않는 것 처럼 동작한다.
4. <font color='orange'>클래스 내의 모든 코드에는 암묵적으로 strict mode가 지정된다.</font>
5. 클래스의 `constructor`, 프로토타입 메소드, 정적 메소드는 모두 프로퍼티 어트리뷰트 `[[Enumerable]]`의 값이 `false`이다. 따라서 `for...in` 문으로 열거할 수 없다.



<br><br>

### 클래스 정의
클래스는 `class` 키워드를 사용하여 정의한다.  

``` js
class Person {
    ...
}
```

일반적이지는 않지만 함수와 마찬가지로 표현식으로 클래스를 정의할 수도 있다.  
이때 클래스는 함수와 마찬가지로 이름을 가질 수도 있고, 갖지 않을 수도 있다.

``` js
// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```

클래스를 표현식으로 정의할 수 있다는 것은 클래스를 값으로 사용할 수 있는 일급 객체라는 것이다.  
즉, 클래스는 일급객체로써 다음과 같은 특징을 갖는다.

- 무명의 리터럴로 생성할 수 있다. (런타임에 생성이 가능하다)
- 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
- 함수의 매개변수에게 전달할 수 있다.
- 함수의 반환값으로 사용할 수 있다.

> 자바스크립트에서 클래스는 함수이다. 따라서 클래스는 값처럼 사용할 수 있는 일급 객체이다.

<br><br>

클래스 몸체에는 0개 이상의 메소드만 정의할 수 있다.  
클래스 몸체에서 정의할 수 있는 메소드는 constructor(생성자), 프로토타입 메소드, 정적 메소드 3가지가 있다.

``` js
// 클래스 선언문
class Person {
    // 생성자
    constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;   // public
    }

    // 프로토타입 메소드
    sayHi() {
        console.log(`Hi! My name is ${this.name}`);
    }

    // 정적 메소드
    static sayHello() {
        console.log('Hello!');
    }
}

// 인스턴스 생성
const me = new Person('Lee');

// 인스턴스의 프로퍼티 참조
console.log(me.name); // Lee

// 프로토타입 메소드 호출
me.sayHi(); // Hi! My name is Lee

// 정적 메소드 호출
Person.sayHello(); // Hello!
```

인스턴스를 생성하지 않고 사용할 수 있음

<br><br>

### 클래스 호이스팅
클래스는 함수로 평가된다.

``` js
// 클래스 선언문
class Person {}

console.log(typeof Person); // function
```

클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가 과정,  
즉 런타임 이전에 먼저 평가되어 함수 객체를 생성한다.  

이때 클래스가 평가되어 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수, 즉 `constructor` 이다.  
생성자 함수로서 호출할 수 있는 함수는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.  

> 프로토타입과 생성자 함수는 단독으로 존재할 수 없고, 언제나 쌍으로 존재하기 때문이다.

<br>

``` js
console.log(Person);    // ReferenceError: Cannot access 'Person' before initialization

// 클래스 선언
class Person {}
```

클래스 선언문은 마치 호이스팅이 발생하지 않는 것처럼 보이나 그렇지 않다.  
``` js
const Person = '';  // 전역 스코프의 Person

{
    // 여기서 블록 스코프의 Person 클래스가 호이스팅됨
    // 하지만 초기화되기 전이라 TDZ에 있음
    console.log(Person);  // TDZ로 인한 ReferenceError 발생
    
    class Person {}      // 이 시점에서 Person 클래스가 초기화됨
}
```

클래스 선언문도 함수 선언, 함수 정의와 마찬가지로 호이스팅이 발생한다.  
단, 클래스는 `let`, `const` 키워드로 선언한 변수처럼 호이스팅된다.  

따라서 클래스 선언문 이전에 일시적 사각지대(TDZ)에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 동작한다.


<br><br>

### 인스턴스 생성
클래스는 생성자 함수이며 `new` 연산자와 함게 호출되어 인스턴스를 생성한다.  

``` js
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me);    // Person {}
```

<br>

함수는 `new` 연산자의 사용 여부에 따라 일반 함수로 호출되거나  
인스턴스 생성을 위한 생성자 함수로 호출되지만  
<font color='orange'>클래스는 인스턴스를 생성하는 것이 유일한 존재 이유이므로 반드시 `new` 연산자와 함께 호출해야 한다.</font>

만약 `new` 연산자 없이 호출하면 아래와 같이 타입 에러가 발생한다.
``` js
const Person {}

// 클래스를 new 연산자 없이 호출하면 타입 에러가 발생한다.
const me = Person();
// TypeError: Class constructor Person cannot be invoked without 'new'
```

<br>

클래스 표현식으로 정의돈 클래스의 경우  
아래와 같이 클래스를 가리키는 식별자(Person)를 사용해 인스턴스를 생성하지 않고  
기명 클래스 표현식의 클래스 이름(`MyClass`)을 사용해 인스턴스를 생성하면 에러가 발생한다.

``` js
const Person = class MyClass {};

// 함수 표현식과 마찬가지로 클래스를 가리키는 식별자로 인스턴스를 생성해야 한다.
const me = new Person();

// 클래스 이름 MyClass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자이다.
// MyClass라는 이름은 클래스 내부에서만 사용할 수 있고 클래스 외부에서는 사용할 수 없다.
console.log(MyClass);   // ReferenceError: MyClass is not defined

const you = new MyClass();      // ReferenceError: MyClass is not defined
```

<br>

결국 클래스를 만드는 방법은 아래와 같이 2가지로 정리할 수 있다.

``` js
// 방법 1: 이름이 없는 클래스를 변수에 할당
// 가장 일반적인 방식이라고 함
const Person = class {};

// 방법 2: 이름이 있는 클래스를 변수에 할당
const Person = class MyClass {};
```


<br><br>


### 메소드
클래스 내에서는 0개 이상의 메소드를 생성할 수 있다.  
클래스 내에서 정의할 수 있는 메소드는 아래와 같이 3가지가 있다.

1. constructor(생성자)
2. 프로토타입 메소드
3. 정적 메소드

<br>

#### constructor
<font color='orange'>constructor는 인스턴스를 생성하고 초기화하기 위한 특수 메소드이다.</font>  
`constructor`는 이름을 변경할 수 없다.


``` js
class Person {
    // 생성자
    constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
    }
}
```

초기화 시 인자를 전달받을 수도 있다.

``` js
class Person {
    constructor(name, address) {
        // 인수로 인스턴스를 초기화
        this.name = name;
        this.address = address;
    }
}

// 인수로 초기값을 전달.
const me = new Person('Lee', 'Seoul');
console.log(me);        // Person {name: "Lee", address: "Seoul"}
```

<br><br>

`constructor`는 별도의 반환문을 가지지 말아야 한다.  
"17.2.3절 생성자 함수의 인스턴스 생성 과정"에서와 같이  
`new` 연산자와 함께 클래스가 호출되면 생성자 함수와 동일하게 암묵적으로 `this`, 즉 인스턴스를 반환하기 때문이다.  

만약 `this`가 아닌 다른 객체를 명시적으로 반환하면 `this`,  
즉 인스턴스가 반환되지 못하고 `return` 문에 명시한 객체가 반환된다.  

``` js
class Person {
    constructor(name) {
        this.name = name;

        // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다
        return {};
    }
}

const me = new Person('Lee');
console.log(me);  // {}
```

하지만 명시적으로 원시값을 반환하면 원시값 반환은 무시되고  
암묵적으로 this가 반환된다.

``` js
class Person {
    constructor(name) {
        this.name = name;  
        return 100;
    }
}

const me = new Person('Lee');
console.log(me);    // Person { name: "Lee" }
```

<br><br>

#### 프로토타입 메소드
클래스 몸체에서 정의한 메소드는  
새엇ㅇ자 함수에 의한 객체 생성 방식과는 다르게  
클래스의 `prototype` 프로퍼티에 메소드를 추가하지 않아도 기본적으로 프로토타입 메소드가 된다.

``` js
class Person {
    // 생성자
    constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
    }

    // 프로토타입 메솓,
    sayHi() {
        console.log(`Hi! my name is ${this.name}`);
    }
}

const me = new Person('Lee');
me.sayHi();     // Hi! My name is Lee
```

<br><br>

#### 정적 메소드
정적 메소드는 인스턴스를 생성하지 않아도 호출할 수 있는 메소드를 의미한다.
클래스에서는 메소드에 `static` 키워드를 붙이면 정적 메소드가 된다.

``` js
class Person {
    // 생성자
    constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
    }

    // 정적 메소드
    static sayHi() {
        console.log('Hi!');
    }
}
```

<br><br>

### 상속에 의한 클래스 확장
상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장(`extends`) 정의하는 것이다.  
생성자 함수에 반해 <font color='orange'>클래스는 상속을 통해 기존 클래스를 확장할 수 있는 문법이 기본적으로 제공된다.</font>


상속을 통해 `Animal` 클래스를 확장한 `Bird` 클래스를 구현하면 아래와 같다.
``` js
class Animal {
    constructor(age, weight) {
        this.age = age;
        this.weight = weight;
    }

    eat() { retrun 'eat'; }
    move() { return 'move'; }
}

// 상속을 통해 Animal 클래스를 확장한 Bird 클래스
class Bird extands Animal {
    fly() { return 'fly'; }
}
```

<br><br>

#### extends 키워드
상속을 통해 클래스를 확장하려면 `extends` 키워드를 사용하여 상속받을 클래스를 정의한다.

``` js
// super 클래스
class Base {}

// sub 클래스
class Derived extends Base {}
```

상속을 통해 확장된 클래스를 서브 클래스(sub-class)라 부르고,  
서브 클래스에게 상속된 클래스를 슈퍼 클래스(super-class)라 부른다.

`extends` 키워드의 역할은 슈퍼클래스와 서브클래스 간의 상속 관계를 설정하는 것이다.  


<br><br>

#### 동적 상속
`extends` 키워드는 클래스 뿐만 아니라 생성자 함수를 상속받아 클래스를 확장할 수도 있다.  
단, `extends` 키워드 앞에는 반드시 클래스가 와야 한다.

``` js
// 생성자 함수
function Base(a) {
    this.a = a;
}

// 생성자 함수를 상속받는 서브클래스
class Derived extends Base {}

const derived = new Derived(1);
console.log(derived);       // Derived {a: 1}
```

<br>

`extends` 키워드 다음에는 클래스 뿐만 아니라 `[[Construct]]` 내부 메소드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다.
이를 통해 동적으로 상속받을 대상을 결정할 수 있다.

``` js
function Base1() {}
class Base2 {}

let condition = true;

// 조건에 따라 동적으로 상속 대상을 결정하는 서브클래스
class Derived extends (condition ? Base1 : Base2) {}

const derived = new Derived();
console.log(derived);  // Derived {}

console.log(derived instanceof Base1);  // true
console.log(derived instanceof Base2);  // false
```


<br><br>

#### 서브클래스의 constructor
클래스에서 `constructor`를 생략하면 클래스에 다음과 같이 비어있는 `constructor`가 암묵적으로 정의된다.

``` js
constructor() {}
```

그리고 서브클래스에서 `constructor`를 생략하면 클래스에 다음과 같은 `constructor`가 암묵적으로 정의된다.  
`args`는 `new` 연산자와 함께 클래스를 호출할 때 전달한 인수의 리스트이다. 

```
constructor(...args) { super(...args); }
```

> *Rest 파라미터*  
> 매개변수에 `...`를 붙이면 Rest 파라미터가 된다.  
> Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.


<br>

아래 예제를 살펴보자.

``` js
// 슈퍼 클래스
class Base {}

// 서브 클래스
class Derived extends Base {}
```

위 예제의 클래스에는 아래와 같이 암묵적으로 `constructor`가 정의된다.

``` js
// 슈퍼클래스
class Base {
    constructor() {}
}

// 서브클래스
class Derived extends Base {
    constructor(...args) { super(...args); }
}

const derived = new Derived();
console.log(derived);   // Derived {}
```

<br><br>

#### super 키워드
`super` 키워드는 함수처럼 호출할 수도 있고 `this`와 같이 식별자처럼 참조할 수도 있는 특수한 키워드이다.  
`super` 키워드는 다음과 같이 동작한다.

- `super`를 호출하면 슈퍼클래스의 constructor를 호출한다.
- `super`를 참조하면 슈퍼클래스의 메소드를 호출할 수 있다.

<br>

##### super 호출
<font color='orange'>super를 호출하면 슈퍼클래스의 constructor를 호출한다.</font>  

``` js
// 슈퍼 클래스
class Base {
    constructor(a, b) {
        this.a = a;
        this.b = b;
    }
}

// 서브 클래스
class Derived extends Base {
    constructor(a, b, c) {
        super(a, b);
        this.c = c;
    }
}

const derived = new Derived(1, 2, 3);
console.log(derived);  // Derived {a: 1, b: 2, c: 3}
```

<br>

super를 호출할 때 주의사항은 아래와 같다.
1. 서브 클래스에서 `constructor`를 생략하지 않을 경우에는 반드시 `super`를 호출해야 한다.
2. 서브 클래스의 `constructor`에서 `super`를 호출하기 전에는 `this`를 참조할 수 없다.
3. `super`는 반드시 서브클래스의 `constructor`에서만 호출한다.

<br><br>

##### super 참조
<font color='orange'>메소드 내에서 super를 참조하면 슈퍼클래스의 메소드를 호출할 수 있다.</font>

1. 서브클래스의 프로토타입 내에서 `super.sayHi`는 슈퍼클래스의 프로토타입 메소드인 `sayHi`를 가리킨다.
    ``` js
    // 슈퍼클래스
    class Base {
        constructor(name) {
            this.name = name;
        }

        sayHi() {
            return `Hi! ${this.name}`;
        }
    }

    // 서브클래스
    class Derived extends Base {
        sayHi() {
            return `${super,sayHi()}. How are you doing?`;
        }
    }

    const derived = new Derived('Lee');
    console.log(derived.sayHi());   // Hi! Lee. How are you doing?
    ```

2. 서브클래스의 정적 메소드 내에서 `super.sayHi`는 슈퍼클래스의 정적 메소드 `sayHi`를 가리킨다.
``` js
// 슈퍼클래스
class Base {
    static sayHi() {
        return 'Hi!';
    }
}

// 서브클래스
class Derived extends Base {
    static sayHi() {
        // super.sayHi는 슈퍼클래스의 정적 메소드를 가리킨다.
        return `${super.sayHi()} how are you doing?`;
    }
}

console.log(Derived.sayHi());   // Hi! how are you doing?
```