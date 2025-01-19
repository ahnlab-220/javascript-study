# Chapter 10 - 객체 리터럴

### 객체 
자바스크립트를 구성하는 거의 모든 것은 객체입니다.  
원시 값을 제외한 나머지 값(함수, 배열, 정규 표현식 등)은 모두 객체입니다.  

객체 타입(object/reference type)은 다양한 타입의 값(원시 값 또는 다른 객체)을 하나의 단위로 구성한 자료구조입니다.  
또한 <font color='orange'>원시 값은 변경 불가능한 값(immutable value)이지만 객체는 변경 가능한 값(mutable value)입니다.</font>

객체는 0개 이상의 프로퍼티로 구성된 집합이며,  
프로퍼티는 key와 value로 구성됩니다.  

``` js
let person = {
    name: 'Lee',
    age: 27
};
```

<img src="https://github.com/user-attachments/assets/e1e8eb94-d30a-4583-b793-756ea1c2cd3e" width="400px" />
<img src="https://github.com/user-attachments/assets/a4ef049b-1a7d-40b0-8b89-d7de590863f9" width="220px" />

_이미지 출처: https://blog.vietnamlife.info/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-object-keys-%EC%99%80-values-%EB%A5%BC-%EC%98%88%EC%A0%9C%EC%99%80-%ED%95%A8%EA%BB%98-%EC%89%BD%EA%B2%8C-%EB%B0%B0%EC%9B%8C%EB%B3%B4/_
<br><br>


<br><br>


자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값이 될 수 있습니다.  
따라서 함수도 프로퍼티 값으로 사용할 수 있습니다. 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 메소드(method)라 부릅니다.

``` js
let counter = {
    num: 0,
    increase: function (){
        this.num++;
    }
};
```

객체는 객체의 상태를 나타내는 값(프로퍼티)와 프로퍼티를 참조하고 조작할 수 있는 메소드를 모두 포함할 수 있기 때문에  
상태와 동작을 하나의 단위로 구조화할 수 있습니다.  

<br><br>

### 객체 리터럴에 의한 객체 생성
C++나 자바와 같이 클래스 기반 객체지향 언어는 클래스를 사전에 정의하고   
필요한 시점에 `new` 연산자와 함께 생성자(Constructor)를 호출하여  
인스턴스를 생성하는 방식으로 객체를 생성합니다.  

자바스크립트는 프로토타입 기반 객체지향 언어로써 클래스 기반 객체지향 언어와는 달리  
**다양한 객체 생성 방법을 지원합니다.**

- 객체 리터럴
- `Object` 생성자 함수
- 생성자 함수
- `Object`, `create` 메소드
- 클래스(ES6)


<br>

일반적으로 사용하는 방법은 '객체 리터럴'을 사용하는 자바스크립트의 유연함을 대표하는 방식입니다.  
객체 리터럴은 중괄호(`{...}`) 내에 0개 이상의 프로퍼티를 정의하는 방식입니다.  

``` js
const person = {
    name: 'Kong',
    sayHello: function () {
        console.log(`Hello! My name is ${this.name}`);
    }
};

console.log(typeof person);  // object
console.log(person);  // {name: "Kong", sayHello: f}
person.sayHello();  // Hello! My name is Kong
```

**참고:**   
우리가 객체 리터럴로 객체를 표기하면, 자바스크립트 엔진은 변수에 할당되는 시점에 객체 리터럴을 해석해 우리의 객체를 생성해 줍니다.

> 객체 리터럴은 코드 블록을 의미하지 않는다는 것에 주의하도록 합시다. (세미콜론을 붙이도록 하자)


<br><br>

객체 리터럴은 자바스크립트의 **유연함과 강력함을 대표**하는 객체 생성 방식입니다.  
마치 숫자나 문자열을 만드는 것처럼 리터럴로 간편하게 객체를 생성할 수 있을 뿐더러,  
객체 리터럴에 프로퍼티를 포함해 객체를 생성함과 동시에 프로퍼티를 만들거나,  
**객체 생성 이후 프로퍼티를 동적으로 추가**할 수도 있습니다.

<br><br>

### 프로퍼티
객체는 프로퍼티의 집합이며, 프로퍼티는 key와 value로 구성됩니다. 

``` js
let person = {
    name: 'Lee',
    age: 27
};
```
<img src="https://github.com/user-attachments/assets/e1e8eb94-d30a-4583-b793-756ea1c2cd3e" width="400px" />


여기서 프로퍼티 키는 문자열이므로 `''`나 `""`로 묶어야 하지만,  
식별자 네이밍 규칙을 준수하는 이름의 경우에는 이를 생략할 수 있습니다.  

``` js
let person = {
    firstName: 'Hong',          // 식별자 네이밍 규칙을 준수하는 프로퍼티 키
    'last-name': 'Gil Dong'     // 식별자 네이밍 규칙을 준수하지 않는 프로퍼티 키
};

// 따옴표를 붙이지 않으면 SyntaxError가 발생한다.
const person = {
    firstName: 'Ung-mo',
    last-name: 'Lee' // SyntaxError: Unexpected token -
};
```

> 자바스크립트는 key에 있는 `-` 문구를 연산자가 있는 표현식으로 해석해버린다.

<br><br>

문자열 또는 문자열로 평가할 수 있는 표현식을 사용해 프로퍼티 key를 동적으로 생성할 수도 있습니다.
이 경우에는 대괄호(`[...]`)로 묶어주어야 합니다.
``` js
let obj = {};
let key = 'hello';

// ES5: 프로퍼티 키 동적 생성
obj[key] = 'world';

// ES6: 계산된 프로퍼티 이름
// const obj = { [key]: 'world' };
console.log(obj); // {hello: "world"}
```

<br><br>

이미 존재하는 key를 중복으로 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티 키를 덮어씁니다.  
때문에 에러가 발생하지 않으니 이를 주의해야 합니다.

``` js
let sample = {
    alphabet: 'A',
    alphabet: 'B'
};

console.log(sample);        // {alphabet: "B"}
```

<br><br>

프로퍼티 key에 문자열이나 심벌 값 외의 값을 사용하면 **암묵적 타입 변환**을 통해 문자열이 됩니다.

``` js
var foo = {
    0: 1,
    1: 2,
    2: 3
};

console.log(foo); // {0: 1, 1: 2, 2: 3}
```


<br><br>

### 메소드

프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메소드(method)라고 부릅니다.  
객체에 묶여 있는 함수를 지칭하는 말입니다.

``` js
let person = {
    name: 'Lee',
    printHello: function () {
        console.log(`Hello! My name is ${this.name}.`);
    }
};
```

메소드 내부에서 사용한 `this` 키워드는 객체 자신을 참조하는 참조 변수입니다.


<br><br>

### 프로퍼티 접근
프로퍼티에 접근하는 방법은 아래와 같이 2가지입니다.
- 마침표 표기법(dot notation)
- 대괄호 표기법(bracket notation)

``` js
let person = {
    name: 'Lee',
    printHello: function () {
        console.log(`Hello! My name is ${this.name}.`);
    }
};

console.log(person.name);       // 'Lee' (마침표 표기법)
console.log(person['name']);    // 'Lee' (대괄호 표기법)
```

<br><br>

### 프로퍼티 값 갱신
이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신(update)됩니다.

``` js
let person = {
    name: 'Lee'
};

person.name = 'Kim';
console.log(person);       // {name: "Kim"}
```

<br><br>

### 프로퍼티 동적 생성
존재하지 않는 프로퍼티에 값을 할당하면 동적으로 프로퍼티가 생성되고 프로퍼티 값이 할당됩니다.

``` js
let person = {
    name: 'Lee'
};

person.age = 27;
console.log(person);       // {name: "Lee", age: 27}
```


<br><br>

### 프로퍼티 삭제
`delete` 연산자는 객체의 프로퍼티를 삭제합니다.  
이때 `delete` 연산자의 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 합니다.  
존재하지 않는 프로퍼티를 삭제하면 아무 에러가 발생하지 않으며, 무시됩니다.

``` js
let person = {
    name: 'Lee'
};

person.age = 27;
delete person.age;  // age 프로퍼티를 삭제
```

<br><br>

### 객체 리터럴 확장 기능 (ES6)
ES6에서는 간편하고 표현력 있는 객체 리터럴의 확장 기능을 제공합니다.

<br>

#### 프로퍼티 축약 표현
ES6에서는 프로퍼티의 value로 변수를 사용하는 경우, 
<font color='orange'>변수 이름과 프로퍼티 key가 동일한 이름일 때 프로퍼티 key를 생략할 수 있습니다.</font>    
이때 프로퍼티 키는 변수 이름으로 자동 생성됩니다.

``` js
let x = 1, y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };
console.log(obj);       // {x: 1, y: 2}
```

<br>

#### 계산된 프로퍼티 이름 (Computed property name)
표현식을 사용해 프로퍼티 key를 동적으로 생성할 수 있습니다.  
이 방식을 사용하려면 대괄호로 묶어 사용해야 합니다.  

ES5에서는 아래와 같이 사용되었습니다.
``` js
let prefix = 'prop';
let i = 0;

let obj = {};

// 프로퍼티 키를 동적으로 생성
obj[perfix + '-' + ++i] = i;
obj[perfix + '-' + ++i] = i;
obj[perfix + '-' + ++i] = i;

console.log(obj);       // {prop-1: 1, prop-2: 2, prop-3: 3}
``` 

<br>

ES6에서는 객체 리터럴 내부에서도 프로퍼티 키를 동적으로 생성할 수 있습니다.

``` js
let prefix = 'prop';
let i = 0;

// 객체 리터럴 내부에서 프로퍼티 키를 동적으로 생성
const obj = {
    [`${prefix}-${++i}`]: i,
    [`${prefix}-${++i}`]: i,
    [`${prefix}-${++i}`]: i
};

console.log(obj);       // {prop-1: 1, prop-2: 2, prop-3: 3}
```


<br><br>

#### 메소드 축약 표현
ES5에서 객체의 메소드를 선언하려면 프로피터 value로 함수를 할당해야 합니다.

``` js
let person = {
    name: 'Lee',
    printHello: function () {
        console.log('Hello ' + this.name);
    }
};

person.printHello();    // "Hello Lee"
``` 

ES6에서는 메소드를 정의할 때 `function` 키워드를 생략한 축약 표현을 사용할 수 있습니다.

``` js
let person = {
    name: 'Lee',
    printHello() {
        console.log('Hello ' + this.name);
    }
};

person.printHello();    // "Hello Lee"
```

단, 메소드 축약 표현으로 정의한 메소드는 프로퍼티에 할당한 함수와는 다르게 동작합니다.  
이에 대해서는 26절에서 자세히 다룹니다.