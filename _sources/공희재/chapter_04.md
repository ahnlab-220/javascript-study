# Chapter 04 - 변수

## 1. Variable: 변수란?
자바스크립트를 비롯한 프로그래밍 언어들은 **메모리 주소에 상징적인 이름을 매핑해 값에 안전하게 접근하고 재사용**하도록 "변수"라는 **메커니즘**을 제공한다
<br><br>

## 2. Variable Declaration: 자바스크립트에서 변수 선언이란?
(ES5의 `var`가 지닌 단점은 우선 차치하고) 자바스크립트의 키워드 `var`로 변수를 선언해보자

```javascript
var score;
```

이렇게 score란 변수를 선언한 순간, 자바스크립트 엔진은 아래처럼 2단계에 걸쳐 변수 선언을 처리한다

1. **선언:** 변수명을 실행 컨텍스트에 등록하고 메모리 공간을 확보한다
2. **초기화:** 변수 선언 이후 확보된 메모리 공간은 암묵적으로 `undefined`라는 값으로 알아서 초기화된다. -> 쓰레기 값으로부터 안전하게 해줌
<br><br>

## 3. Hoisting: 자바스크립트의 호이스팅
![image](https://github.com/user-attachments/assets/6be87788-f164-4ddf-a944-593913e54bcb)

```javascript
console.log(score);  // undefined
var score;
```
자바스크립트에서 위 같은 현상이 가능한 건 자바스크립트의 호이스팅(Hoisting)이라는 특징 때문이다.

호이스팅이란, 소스코드 실행시점(=런타임) 전에 모든 식별자 선언(변수, 함수, 클래스 선언 등)을 먼저 실행시키는 자바스크립트의 동작 방식.

마치 식별자 선언문들이 코드의 선두로 끌어 올려진 것처럼 보여서 Hoisting이라고 표현.
<br><br>

## 4. Assignment of Variables: 값의 할당
```javascript
var score = 80;
```

위와 같이 변수 선언과 값의 할당을 하나의 문으로 표현해도, 자바스크립트 엔진은 변수 선언과 값의 할당을 아래처럼 2개의 문으로 나눠 실행한다

```javascript
var score;  // 1. 변수 선언
score = 80; // 2. 값의 할당
```

중요한 점은 변수 선언과 값의 할당 시점이 다르다는 것이다.
* 변수 선언: 런타임 전에 먼저 실행
* 값의 할당: 런타임에 실행

따라서 아래 같은 현상도 설명이 된다.

```javascript
console.log(score);  // undefined

score = 80;  // 2. 값의 할당(런타임에 실행)
var score;  // 1. 변수 선언(런타임 전에 먼저 실행)

console.log(score);  // 80을 출력한다.
```
<br><br>

## 5. Reassignment of Variables: 값의 재할당
```javascript
var score = 5;
score = 10;
```
위와 같이 값을 재할당하면, 이전 메모리 공간에서 값 `5`를 지우고 같은 공간에 `10`을 새롭게 저장하는 것이 아니라,

새로운 메모리 공간을 확보한 후 그곳에 `10`을 저장하는 식이다. 식별자만 같다.

![image](https://github.com/user-attachments/assets/32f9c62c-2d1f-46c2-be10-1cb284fead08)
_이미지 출처: https://algodaily.com/lessons/intro-to-variables-assignment-js_

그렇다면 이전에 사용했던 값 `5`를 저장한 메모리 공간은 버려진 건데, 이건 어떻게 되느냐!

가비지 콜렉터가 알아서 나중에 해제해준다.
<br><br>

## 6. 식별자 네이밍 규칙

* 식별자는 숫자로 시작할 수 없으며 예약어 사용 불가
* 자바스크립트는 관습적으로 변수와 함수는 camelCase, 생성자와 클래스는 PascalCase를 사용한다.

끝.
