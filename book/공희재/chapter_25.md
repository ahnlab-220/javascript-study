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
### 7-1. 인스턴스 프로퍼티
인스턴스 프로퍼티는 constructor 내부에서 정의하는 속성값들을 뜻한다. constructor 내부에서 정의한 프로퍼티는 언제나 클래스가 생성한 인스턴스의 프로퍼티가 된다.
```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
}
```

### 7-2. Accessor property: 접근자 프로퍼티
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

### 7-3. 클래스 필드 정의 제안


### 7-4. private 필드 정의 제안
### 7-5. static 필드 정의 제안

## 8. 상속에 의한 클래스 확장
### 8-1. 클래스 상속과 생성자 함수 상속
### 8-2. `extends` 키워드
### 8-3. 동적 상속
### 8-4. 서브클래스의 constructor
### 8-5. `super` 키워드
### 8-6. 상속 클래스의 인스턴스 생성 과정
### 8-7. 표준 빌트인 생성자 함수 확장
