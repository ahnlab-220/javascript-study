# Chapter 21 - 빌트인 객체

## 1. 자바스크립트 객체의 분류
자바스크립트 객체는 다음과 같이 크게 3개로 분류할 수 있다.
1. Standard built-in objects 또는 Native objects 또는 Global objects (표준 빌트인 객체)
2. Host objects (호스트 객체)
3. User-defined objects (사용자 정의 객체)
차례대로 살펴 보자.

## 2. Standard built-in objects: 표준 빌트인 객체
자바스크립트는 다음과 같이 40여 개의 표준 빌트인 객체를 제공한다.
* Object
* String
* Number
* Boolean
* Symbol
* Date
* Math
* RegExp
* Array
* Map/Set
* ...

이러한 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체는 다양한 기능의 메서드를 제공한다.

## 3. Wrapper object: 원시값과 래퍼 객체
문자열, 숫자, 불리언 객체를 생성하는 String, Number, Boolean 표준 빌트인 생성자 함수가 존재하긴 하나,<br/>
자바스크립트가 암묵적으로 생성하는 래퍼 객체에 의해 해당 표준 빌트인 객체들의 프로토타입 메서드 또는 프로퍼티를 참조할 수 있게 된다.

따라서 굳이 String, Number, Boolean 표준 빌트인 생성자 함수를 new 연산자와 함께 호출해 문자열, 숫자, 불리언 인스턴스를 생성할 필요가 없으며 권장하지 않는다.

## 4. Global object: 전역 객체
Global object는 코드가 실행되기 전, 자바스크립트 엔진에 의해 그 어떤 객체보다도 먼저 생성되는 특수한, 최상위 객체다.<br/>
브라우저 환경에서는 window가 되고, Node.js 환경에서는 global이 된다.

Global object의 특징은 다음과 같다.
* 개발자가 의도적으로 생성할 수 없음. 즉, Global object를 생성하는 생성자 함수가 제공되지 않는다.
* Global object의 프로퍼티를 참조할 때, 식별자명(e.g., `window`, `global`)을 생략할 수 있다.

### 4-1. Built-in global property: 빌트인 전역 프로퍼티
Built-in global property는 Global object의 프로퍼티를 의미한다. 다음과 같이 주로 애플리케이션 전역에서 사용하는 값을 제공한다.
* `Infinity`
* `NaN`
* `undefined`

### 4-2. Built-in global function: 빌트인 전역 함수
Built-in global function은 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서, Global object의 메서드다.
* `eval()`
* `isFinite()`
* `isNaN()`
* `parseFloat()`
* `parseInt()`
* `encodeURI() / decodeURI()`
* `encodeURIComponent() / decodeURIComponent()`

### 4-3. Implicit global: 암묵적 전역
선언하지 않은 식별자에 값을 할당하는 코드를 작성하게 되면 마치 참조 에러가 발생할 것 같지만,<br/>
선언하지 않은 그 식별자는 Global object의 프로퍼티가 되기 때문에 마치 전역 변수처럼 동작한다.<br/>
(하지만 전역 '변수'는 아니라 변수 호이스팅이 발생하지 않음을 유의)

이러한 현상을 우리는 Implicit global라 부른다.

끝.
