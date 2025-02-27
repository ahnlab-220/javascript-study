# Chapter 06 - 데이터 타입

## 0. 데이터 타입은 왜 필요한가?
```javascript
var score = 100;
score = 'Hello';
score = true;
score = null;
...
```
1. 메모리 공간을 확보하고 참조하기 위해
    * 변수에 할당하고자 하는 값의 데이터 타입을 알아야, 값을 저장할 때 **확보해야 하는 메모리 공간의 크기를 결정**할 수 있다.
    * 또한, 변수에 기 할당한 값을 참조할 땐 **한 번에 읽어 들어야 할 메모리 공간의 크기를 결정**할 수 있다.
2. 값을 해석하기 위해
    * 변수에 기 할당한 값을 참조할 때 메모리에서 읽어 들인 **2진수를 어떻게 해석할지** 결정할 수 있다.


## 1. Dynamic Typing: 동적 타이핑
자바스크립트의 변수는 다른 정적 타입 언어(e.i. C언어, Java 등)와는 달리

* 선언이 아닌 **할당에 의해 타입이 결정**된다.(="type inference(타입 추론)된다")
* 그리고 **재할당**에 의해 변수가 참조하는 값의 타입은 언제든지 동적으로 **변할 수 있다**.

데이터 타입을 위와 같이 다루는 언어의 특징을 우리는 "dynamic typing(동적 타이핑)"이라고 부른다


동적 타이핑을 하는 언어, 즉 동적 타입 언어는 개발자가 데이터 타입을 매번 신경 쓰지 않아도 돼 편리한 만큼,
<br>
변수 값을 추적하거나 예측하기 어렵게 만든다는 구조적인 단점 또한 존재한다.


따라서 자바스크립트 변수를 사용할 땐 다음과 같은 **주의사항**을 숙지하자.
1. **변수는 제한적으로 사용한다.** 변수의 개수를 남발할수록 오류가 발생할 확률도 높아진다. 필요한 만큼만 최소한으로 유지한다.
2. **변수의 Scope(유효범위)는 최대한 좁게 만든다.** 변수의 유효 범위가 넓으면 넓을수록 변수를 추적하고 예측하기 어려워 오류가 발생할 확률이 높아진다.
3. **전역 변수는 절대 지양한다.** 전역 변수도 마찬가지로 추적과 예측을 어렵게 하므로 사용을 억제한다.
4. **변수보다는 상수를 사용한다.**
5. **변수명(뿐만 아니라 모든 식별자)는 명확하게 지어 가독성을 높인다.**


## 2. Data Types: 자바스크립트의 테이터 타입
![image](https://github.com/user-attachments/assets/b36ca1be-f49b-4e87-8095-eae1ab68df69)
<br>
자바스크립트(ES6)는 위처럼 primitive type(원시 타입)과 object/reference type(객체 타입)으로 나뉘는 7개의 데이터 타입을 제공한다.
<br>
(ES11부터는 새로운 원시값 BigInt까지 합해서 총 8개의 데이터 타입을 제공한다.)

8개 데이터 타입을 차례대로 살펴보자.
### 1. Number Type: 숫자 타입
자바스크립트는 하나의 숫자 타입만 존재한다. 자바스크립트에서 숫자 타입의 값은 배정밀도 64비트 부동소수점 형식을 따른다.
<br>
**즉, 자바스크립트는 모든 수를 실수로 처리한다.**
```javascript
var integer = 10;   // 10(정수)
var double = 10.12;   // 10.12(실수)
var negative = -20;   // -20(음의 정수)
var binary = 0b01000001; // 65(2진수로 표현했으나 참조 시 10진수로 해석됨)
var octal = 0o101;   // 65(8진수로 표현했으나 참조 시 10진수로 해석됨)
var hex = 0x41;      // 65(16진수로 표현했으나 참조 시 10진수로 해석됨)
```
숫자 타입은 아래처럼 특별한 값도 표현할 수 있다.
```javascript
console.log(10 / 0);  // Infinity(양의 무한대)
console.log(10 / -0); // -Infinity(음의 무한대)
console.log(1 * 'meow'); // NaN(산술 연산 불가(Not-a-Number))
```

### 2. String Type: 문자열 타입
자바스크립트의 문자열은, 배열이나 객체로 표현되는 다른 언어들의 문자열과는 달리
<br>
**primitive(원시) 타입**이며, **immutable value(변경 불가능한 값)** 이다.

### 3. Boolean Type: 불리언 타입
자바스크립트는 논리적 참, 거짓을 나타내는 true, false를 데이터 타입으로 제공한다. 조건문에서 자주 사용한다.

### 4. Undefined Type: undefined 타입
var 키워드로 선언한 변수는 undefined로 암묵적 초기화된다.
<br>
이처럼 undefined는 개발자가 의도적으로 할당하기 위한 데이터라기보다는, **자바스크립트 엔진이 변수를 초기화할 때 사용하는 값**이므로,
<br>
개발자가 굳이 undefined를 할당하는 행동은 하지 않는 게 좋다. (undefined의 본래 취지를 지키며, 혼동을 방지하기 위해)

그렇다면 변수에 값이 없다는 것을 명시하고 싶을 때, 개발자는 무엇을 할당할 수 있는가? 바로 null 값이다.

### 5. Null Type: null 타입
프로그래밍 언어에서 null은 변수에 **값이 없다는 것을 의도적으로 명시**(="intentional absence(의도적 부재)"할 때 사용한다.
<br>
변수에 null을 할당하는 것은, 변수가 이전에 할당되어 있던 값에 대한 **참조를 명시적으로 제거**하겠다고 선언하는 것이다.

### 6. Sympol Type: 심벌 타입
Symbol() 함수를 호출해 생성하는, 다른 값과 절대 중복되지 않는 유일무이한 값. 33장 "7번째 타입 Symbol"에서 자세히 볼 예정.

### 7. BigInt Type: BigInt 타입(책에는 없는 절)
숫자 값을 안정적으로 나타낼 수 있는 최대치인 2<sup>53</sup> - 1보다 큰 정수를 표현할 수 있는, 새로운 원시값이다.
<br>
정수 리터럴의 뒤에 n을 붙이거나, BigInt() 함수를 호출해 생성할 수 있다.
```javascript
var bigInt1 = 10n;
var bigInt2 = BigInt(10);
```

### 8. Objects: 객체 타입
자바스크립트의 객체 타입은 11장 "원시 값과 객체의 비교"에서 자세히 볼 예정.
<br>
중요한 것은 자바스크립트는 객체 기반의 언어이며, **자바스크립트를 이루는 거의 모든 것이 객체**라는 사실이다.

끝.
