# Chapter 10 - 객체 리터럴

## 1. Object: 객체란?
자바스크립트에서 원시 값을 제외한 나머지 값(**함수, 배열, 정규 표현식** 등)은 **모두** 객체입니다.

**객체 타입(Object/Reference Type)** 이란?
* 객체 타입은 원시 값 또는 다른 객체를 하나의 단위로 구성한 **복합적인 자료구조**입니다.
* 객체 타입의 값은 변경 가능한 값(**mutable value**)입니다. 이 특징은 변경 불가능한 값을 지닌다는 원시 타입의 특징과 대비됩니다. (11장 '원시 값과 객체의 비교'에서 자세히 볼 예정)

그래서 **객체**란?
* 자바스크립트의 객체는 0개 이상의 **1. 프로퍼티** 또는 **2. 메서드**로 구성된 집합체입니다.
    1. **프로퍼티**는 객체의 **상태**를 나타내는 값이며, key와 value로 구성합니다.
    2. **메서드**는 객체 내 **프로퍼티 값으로 사용하는 함수**를 뜻하며, _(그렇습니다. 자바스크립트에선 무엇이든 객체의 프로퍼티 값이 될 수 있으며, 함수도 예외가 아닙니다.)_ 프로퍼티를 참조하거나 조작하는 동작(behavior)을 수행합니다.

<img src="https://github.com/user-attachments/assets/e1e8eb94-d30a-4583-b793-756ea1c2cd3e" width="400px" />
<img src="https://github.com/user-attachments/assets/a4ef049b-1a7d-40b0-8b89-d7de590863f9" width="220px" />

_이미지 출처: https://blog.vietnamlife.info/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-object-keys-%EC%99%80-values-%EB%A5%BC-%EC%98%88%EC%A0%9C%EC%99%80-%ED%95%A8%EA%BB%98-%EC%89%BD%EA%B2%8C-%EB%B0%B0%EC%9B%8C%EB%B3%B4/_
<br><br>

객체가 무엇인지 알아봤으니, 이제 객체를 어떻게 생성하는지 알아봅시다.


## 2. 객체 리터럴에 의한 객체 생성
자바스크립트는 C++나 자바 같은 클래스 기반 객체지향 언어와는 다르게, 객체를 생성할 때 반드시 클래스를 사전에 정의하거나 new 연산자로 생성자를 호출할 필요는 없습니다.

대신 자바스크립트는 **프로토타입 기반 객체지향 언어**이기 때문에, 다음처럼 다양한 객체 생성 방법을 지원합니다.
* 객체 리터럴
* Object 생성자 함수
* 생성자 함수
* Object.create 메서드
* 클래스(ES6)

이 중에서 가장 일반적인 방법은 **객체 리터럴** 입니다.<br>
객체 리터럴은 아래처럼 중괄호`{...}` 내에 0개 이상의 프로퍼티를 정의하며 객체를 생성하는 표기법입니다.
```javascript
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
참고: 우리가 객체 리터럴로 객체를 표기하면, 자바스크립트 엔진은 변수에 할당되는 시점에 객체 리터럴을 해석해 우리의 객체를 생성해 줍니다.
<br><br>

객체 리터럴은 자바스크립트의 **유연함과 강력함을 대표**하는 객체 생성 방식입니다. 마치 숫자나 문자열을 만드는 것처럼 리터럴로 간편하게 객체를 생성할 수 있을 뿐더러, 객체 리터럴에 프로퍼티를 포함해 객체를 생성함과 동시에 프로퍼티를 만들거나, **객체 생성 이후 프로퍼티를 동적으로 추가**할 수도 있습니다.

지금까지 객체를 생성하는 가장 일반적인 객체 리터럴 방식을 알아 보았습니다. 나머지 객체 생성 방식은 12장 '함수'에서 알아보는 것으로 하고, 이제는 프로퍼티를 자세히 살펴봅시다.


## 3. 프로퍼티
앞서 말했듯, 자바스크립트의 객체는 프로퍼티의 집합이며, 프로퍼티는 key와 value로 이루어집니다.

<img src="https://github.com/user-attachments/assets/e1e8eb94-d30a-4583-b793-756ea1c2cd3e" width="400px" />

프로퍼티는 각각 쉼표(`,`)로 구분하며, key와 value는 각각 다음과 같은 특징이 있습니다.
* **프로퍼티 key의 특징:**
    1. 프로퍼티 key로 사용할 수 있는 값은 **문자열** 또는 심벌 값입니다. 일반적으로 문자열을 이용합니다.
    2. 프로퍼티 key는 프로퍼티 value에 접근하는 식별자로서 기능하기 때문에, **식별자 네이밍 규칙을 사용하는 것을 권장**합니다. (식별자 네이밍 규칙을 따르지 않으면 반드시 따옴표를 붙여야 하는 등 번거롭기 때문)
        * e.g.,
            ```javascript
            const person = {
                firstName: 'Ung-mo', // 식별자 네이밍 규칙을 준수하는 프로퍼티 키
                'last-name': 'Lee'   // 식별자 네이밍 규칙을 준수하지 않는 프로퍼티 키
            };
            console.log(person); // {firstName: "Ung-mo", last-name: "Lee"}

            // 따옴표를 붙이지 않으면 SyntaxError가 발생한다.
            const person = {
                firstName: 'Ung-mo',
                last-name: 'Lee' // SyntaxError: Unexpected token -
            };
            ```
    3. 문자열 또는 문자열로 평가되는 표현식을 사용해 **동적으로 프로퍼티 key를 생성**할 수도 있습니다. 이 경우엔 표현식을 대괄호`[]`로 묶어야 합니다.
        * e.g.,
            ```javascript
            const obj = {};
            const key = 'hello';

            // ES5: 프로퍼티 키 동적 생성
            obj[key] = 'world';
            // ES6: 계산된 프로퍼티 이름
            // const obj = { [key]: 'world' };

            console.log(obj); // {hello: "world"}
            ```
    4. 프로퍼티 key에 문자열이나 심벌 값 외의 값을 사용하면 **암묵적 타입 변환**을 통해 문자열이 됩니다.
        * e.g.,
            ```javascript
            var foo = {
                0: 1,
                1: 2,
                2: 3
            };

            console.log(foo); // {0: 1, 1: 2, 2: 3}
            ```
    5. 이미 존재하는 프로퍼티 key를 중복 선언하면 나중에 선언한 프로퍼티가 **먼저 선언한 프로퍼티를 덮어쓰며**, 이때 에러가 발생하지 않는다는 점을 주의해야 합니다.
        * e.g.,
            ```javascript
            var foo = {
                name: 'Lee',
                name: 'Kim'
            };

            console.log(foo); // {name: "Kim"}
            ```


* **프로퍼티 value의 특징:**
    1. 프로퍼티 value로 사용할 수 있는 값은 자바스크립트에서 사용할 수 있는 **모든 값**입니다.
