# Chapter 25 - Class(클래스)

## 1. 클래스는 프로토타입의 문법적 설탕(syntactic sugar)인가?
결론: 아니다.

ES6에서 도입한 클래스는 JS 기존의 프로토타입 기반 객체지향 프로그래밍보다, 자바와 같은 **클래스 기반** 객체지향 프로그래밍에 익숙한 개발자들이 더 빠르게 학습할 수 있도록 **새로운 객체 생성 방법**을 제공한다.

그렇다고 ES6의 클래스가 완전히 새로운 모델인 것은 아니다. **사실 클래스는 함수**이며, 기존 프로토타입 기반 패턴을 **클래스 기반 패턴'처럼' 제공**하는 새로운 문법일 뿐이다.

단, 클래스와 생성자 함수는 모두 프로토타입 기반의 인스턴스를 생성하지만 동작하는 방식이 다르다. 차이점은 다음과 같다.

1. 클래스를 **`new` 연산자 없이 호출하면 에러**가 발생한다. (생성자 함수는 `new` 없이 호출하면 일반 함수로 호출된다.)
2. 클래스는 **상속을 지원**하는 `extends`과 `super` 키워드를 제공한다. (생성자 함수는 그런 거 없다.)
3. 클래스는 **호이스팅이 발생하지 않는 것'처럼'** 동작한다.
4. 클래스 내의 모든 코드에는 **암묵적으로 'strict mode'**가 적용된다. (생성자 함수는 그런 거 없다.)
5. 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 `[[Enumerable]]`의 값이 `false`다. 즉, **열거되지 않는다**.

이러한 차이점을 봤을 때, **ES6 클래스**를 단순한 문법적 설탕(syntactic sugar)으로 보기보다는, **새로운 객체 생성 메커니즘**으로 보는 것이 더 합당하다.


## 2. 클래스 정의
클래스는 `class` 키워드를 사용해 정의한다. 파스칼 케이스를 쓰는 것이 일반적이다.
```javascript
// 클래스 선언문
class Person {}

// 익명 클래스 표현식(일반적이진 않은 정의 방식. 참고만 하기)
const Person = class {};

// 기명 클래스 표현식(일반적이진 않은 정의 방식. 참고만 하기)
const Person = class MyClass {};
```

클래스는 일급 객체로서, 다음 같은 특징을 지닌다.
1. 무명의 리터럴로 생성 가능. 즉, **런타임에 생성이 가능**하다.
2. 변수나 자료구죠(**객체, 배열** 등)에 **저장** 가능.
3. 함수의 **매개변수에 전달** 가능.
4. 함수의 **반환값으로 사용** 가능.

클래스와 생성자 함수의 정의 방식을 비교해 보면 다음과 같다.

<img width="60%" src="https://github.com/user-attachments/assets/d556a18a-988b-4cec-8de6-ae01dcf4ad7a" />


## 3. 클래스 호이스팅
클래스는 `let`이나 `const` 키워드로 선언한 변수처럼 호이스팅된다. 즉, 클래스 선언문 이전에 Temporal Dead Zone(TDZ, 일시적 사각지대)에 빠지기 때문에 호이스팅이 발생하지 않는 것'처럼' 동작한다.

## 4. 인스턴스 생성
```javascript
class Person {}

// 인스턴스 생성
const me = new Person();
console.log(me);  // Person {}
```

클래스는 생성자 함수와는 다르게 반드시 `new` 연산자와 호출해야 한다. (오로지 인스턴스를 생성하는 것이 유일한 존재 이유이므로)

## 5. 메서드
클래스 몸체에서 정의할 수 있는 메서드는 constructor(생성자), 프로토타입 메서드, 정적 메서드 이렇게 세 가지가 있다. 차례대로 살펴 보자.

### 5-1. constructor
constructor는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드며, 이름을 변경할 수 없다. constructor는 클래스 내에 최대 한 개만 존재할 수 있다. 만약 생략하면 클래스에 빈 constructor가 암묵적으로 정의된다.
```javascript
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}
```
constructor는 별도의 반환문을 가지면 안 된다. `new` 연산자와 함께 클래스가 호출되면 생성자 함수와 동일하게 암묵적으로 `this`, 즉 인스턴스를 반환하기 때문이다.

### 5-2. prototype method: 프로토타입 메서드
클래스 몸체에서 정의한 메서드는 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도, 기본적으로 프로토타입 메서드가 된다. (명시적으로 프로토타입에 `Person.prototype.sayHi`로 프로토타입 메서드를 생성해야 하는 생성자 함수와는 다름.)
```javascript
class Person {
  constructor(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi, my name is ${this.name}`);
  }
}

const me = new Person('Lee');
me.sayHi();  // Hi, my name is Lee
```

클래스가 생성한 인스턴스는, 생성자 함수와 마찬가지로, 프로토타입 체인의 일원이 된다. 따라서 이 인스턴스는 프로토타입 메서드를 상속받아 사용할 수 있다.

### 5-3. static method: 정적 메서드
정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다. 클래스에서는 메서드 앞에 static 키워드를 붙이면 정적 메서드가 된다.
```javascript
class Person {
  constructor(name) {
    this.name = name;
  }

  // 정적 메서드
  static sayHi() {
    console.log(`Hi!`);
  }
}

Person.sayHi();  // Hi!
```

정적 메서드는 클래스에 바인딩된 메서드가 된다. 정적 메서드는 인스턴스로 호출할 수 없다. (인스턴스의 프로토타입 체인에는 클래스가 존재하지 않기 때문에.)

### 5-4. 정적 메서드와 프로토타입 메서드의 차이
1. 정적 메서드와 프로토타입 메서드는 각자가 속해 있는 프로토타입 체인이 다르다.
2. 정적 메서드는 클래스로 호출, 프로토타입 메서드는 인스턴스로 호출한다.
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 있지만, 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 없다.

### 5-5. 클래스에서 정의한 메서드의 특징
1. 메서드 축약 표현을 사용한다. (즉, `function` 키워드를 생략한다.)
2. 클래스에 메서드를 정의할 때 콤마가 필요 없다.
3. 암묵적으로 strict mode로 실행한다.
4. 열거할 수 없다. 즉 `for ... in` 문이나 `Object.keys` 메서드 사용 불가
5. non-constructor다. 따라서 `new` 연산자와 함께 호출 불가.

## 6. 클래스의 인스턴스 생성 과정
1. 인스턴스 생성과 `this` 바인딩
    * `new` 연산자와 함께 클래스를 호출하면 암묵적으로 빈 객체가 먼저 생성 후, `this`에 바인딩.
2. 인스턴스 초기화
    * constructor 안의 코드가 실행해 `this`에 바인딩된 인스턴스를 초기화.
3. 인스턴스 반환
    * `this`를 암묵적으로 반환.

## 7. 프로퍼티
### 7-1. Instance Properties: 인스턴스 프로퍼티
인스턴스 프로퍼티는 constructor 내부에서 정의하는 속성값들을 뜻한다. constructor 내부에서 정의한 프로퍼티는 언제나 클래스가 생성한 인스턴스의 프로퍼티가 된다.
```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
}
```

### 7-2. Accessor Properties: 접근자 프로퍼티
접근자 프로퍼티는 accessor function(접근자 함수)로 구성된 프로퍼티다. 다음과 같이 클래스에서도 사용할 수 있다.
```javascript
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  get fullName() {  // getter 함수
    return `${this.firstName} ${this.lastName}`;
  }

  set fullName(name) {  // setter 함수
    [this.firstName, this.lastName] = name.split(' ');
  }
}

const me = new Person('Ungmo', 'Lee');
me.fullName = 'Heegun Lee';
console.log(me);  // {firstName: "Heegun", lastName: "Lee"}

console.log(me.fullName);  // Heegun Lee
```
setter는 단 하나의 값만 할당받기 때문에, 단 하나의 매개변수만 선언할 수 있다. 클래스의 접근자 프로퍼티 또한 인스턴스 프로퍼티가 아닌, 프로토타입의 프로퍼티가 된다.

### 7-3. Class Fields: 클래스 필드 정의 제안
클래스 필드란, 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어다. 자바스크립트의 class body(클래스 몸체)에는 메서드만 선언할 수 있다. 그렇다면 클래스 필드는 어떻게 선언할 수 있단 말인가? 자바스크립트의 원래 성격대로라면 아래 코드는 SyntaxError가 발생해야 정상이다.
```javascript
class Person {
  // 클래스 필드 정의
  name = 'Lee';
}
const me = new Person('Lee');
```

최신 브라우저 (Chrome 72 이상) 또는 최신 Node.js(버전 12 이상)에서 실행하면 SyntaxError가 발생하지 않고 정상 동작한다. 그 이유는 인스턴스 프로퍼티를 마치 Java 같은 클래스 기반 객체지향 언어의 클래스 필드처럼 정의할 수 있는 새로운 표준 사양인 "Class field declarations"가 제안되었기 때문이다. -> *2025년 현재는 공식화된 듯함.([참조: [MDN] Public class fields](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Public_class_fields))*

그렇다고 클래스 필드를 정의할 때 아래처럼 `this`에 바인딩하지는 말자. `this`는 클래스의 constructor와 메서드 내에서만 유효하다. 클래스 필드에 함수를 할당하는 짓도 하지 말자. (클래스 필드에 함수를 할당하면 그 함수는 프로토타입 메서드가 아닌 인스턴스 메서드가 되어 버리기 때문에.)
```javascript
// ❌ 틀린 practice!!!!! ❌
class Person {
  this.name = 'Blah';  // SyntaxError: Unexpected token '.'
  name = 'Lee';  // 이건 ㄱㅊ

  // 클래스 필드에 함수를 할당
  getName = function () {
    return this.name;
  }
}

const me = new Person();
console.log(me);  // Person {name: "Lee", getName: f}
console.log(me.getName());  // Lee
```

### 7-4. private 필드 정의 제안
이 책이 쓰인 2021년, 클래스 필드를 `private`하게 정의할 수 있는 새로운 표준 사양이 제안되어 있었다. 이 사양에 따르면 아래 예제처럼 클래식 필드 앞에 `#`을 붙이면 private한 필드를 선언할 수 있다. -> *2025년 현재 공식화된 것으로 보임 ([참조: [MDN] Private properties](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_properties))*
```javascript
class Person {
  // private 필드 정의
  #name = '';

  constructor(name) {
    // private 필드 참조
    this.#name = name;
  }

  get name() {
    return this.#name.trim();
  }
}

const me = new Person('Lee');

// private 필드는 클래스 외부에서 직접 참조할 수 없음.
console.log(me.#name);  // SyntaxError: Private field '#name' must be declared in an enclosing class

// 단, 접근자 프로퍼티를 통해 간접적으로 접근할 수 있음.
console.log(me.name);  // Lee
```

### 7-5. static 필드 정의 제안
static한 필드를 정의하는 문법도 마찬가지로 2025년 현재 공식화된 것으로 보임 ([참조: [MDN] static](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static))
```javascript
class MyMath {
  // static public 필드 정의
  static PI = 22 / 7;

  // static private 필드 정의
  static #num = 10;

  // static 메서드
  static increment() {
    return ++MyMath.#num;
  }
}

console.log(MyMath.PI)  // 3.142857142857143
console.log(MyMath.increment())  // 11
```

## 8. 상속에 의한 클래스 확장
### 8-1. 클래스 상속과 생성자 함수 상속
상속에 의한 클래스 확장은 새로운 클래스를 extend(확장)해 정의하는 방식을 말하며, 지금까지 배워 온 프로토타입 기반 상속과는 다른 개념임을 숙지하자. 클래스는, 생성자 함수와는 다르게, 상속을 통해 기존 클래스를 확장하는 `extends` 문법을 기본적으로 갖고 있다.

<img width="50%" src="https://github.com/user-attachments/assets/6f713e74-ee14-4611-9cb6-257d3b5032c2" />

```javascript
class Animal {
  constructor(age, weight) {
    this.age = age;
    this.weight = weight;
  }

  eat() { return 'eat'; }

  move() { return 'move'; }
}

// 상속으로 Animal 클래스를 확장한 Bird 클래스
class Bird extends Animal {
  fly() { return 'fly'; }
}

const bird = new Bird(12, 5);

console.log(bird);  // Bird {age: 12, weight: 5}
console.log(bird instanceof Bird);  // true
console.log(bird instanceof Animal);  // true

console.log(bird.eat());  // eat
console.log(bird.move());  // move
console.log(bird.fly());  // fly
```

### 8-2. `extends` 키워드
앞서 예제에서 봤듯, `extends` 클래스 간 상속 관계(수퍼클래스-서브클래스)를 설정하는 키워드다. 자바스크립트의 클래스는 이 상속 관계를 구현할 때 프로토타입으로 구현한다.

### 8-3. 동적 상속
생략. 책을 참고합시다!

### 8-4. 서브클래스의 constructor
서브클래스에서 constructor를 생략하면, 서브클래스에서는 다음과 같은 constructor가 암묵적으로 정의된다. 여기서 `super()`는 수퍼클래스의 constructor, 즉 super-constructor를 호출해 인스턴스를 생성한다.
```javascript
constructor(...args) { super(...args); }
```

### 8-5. `super` 키워드
`super` 키워드는 다음 특징을 지닌다.
1. `super`를 호출하면 수퍼클래스의 constructor를 호출한다.
2. `super`를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.

### 8-6. 상속 클래스의 인스턴스 생성 과정
아래와 같은 코드가 있다고 했을 때, 상속 관계에 있는 두 클래스가 어떻게 협력해 인스턴스를 생성하는지 알아 보자.
```javascript
// 수퍼클래스
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }

  toString() {
    return `width = ${this.width}, height = ${this.height}`;
  }
}


// 서브클래스
class ColorRectangle extends Rectangle {
  constructor(width, height, color) {
    super(width, height);
    this.color = color;
  }

  toString() {  // 메서드 오버라이딩
    return super.toString() + `, color = ${this.color}`;
  }
}

const colorRectangle = new ColorRectangle(2, 4, 'red');
console.log(colorRectangle);  // ColorRectangle {width: 2, height: 4, color: 'red'}

// 상속으로 getArea 메서드를 호출할 수 있게 됨.
console.log(colorRectangle.getArea());  // 8

// 상속으로 오버라이딩한 toString 메서드를 호출.
console.log(colorRectangle.toString());  // width = 2, height = 4, color = red
```
1. 서브클래스의 `super` 호출
2. 수퍼클래스의 인스턴스 생성과 `this` 바인딩
3. 수퍼클래스의 인스턴스 초기화
4. 서브클래스 constructor로 복귀 후 `this` 바인딩
5. 서브클래스의 인스턴스 초기화
6. 인스턴스 반환

### 8-7. 표준 빌트인 생성자 함수 확장
`extends` 키워드 뒤에는 꼭 클래스뿐만 아니라, `[[Construct]] 내부 메서드`를 갖는, 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다. `String`, `Number`, `Array` 같은 표준 빌트인 객체도 마찬가지로 `extends` 키워드를 사용해 확장할 수 있다.

```javascript
class MyArray extends Array {
  // ... 생략. 자세한 코드는 책 참고!
}
```


끝.
