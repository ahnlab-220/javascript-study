# Chapter 12 - 함수

## 1. 함수란? 그리고 왜 쓰는가?
**함수**는 입력을 받아 출력을 내보내는 일련의 과정을 statement(문)으로 구현하고,<br>
코드 블록으로 감싸 하나의 재사용 가능한 실행단위로 정의한 것이다.

![image](https://github.com/user-attachments/assets/7912ee5f-0108-49d9-8ee0-23f75c5165af)
* **parameter(매개변수):** 함수 내부로 입력을 전달받는 변수.
* **argument(인수):** 함수의 입력.
* **return value(반환값):** 함수의 출력, 함수의 실행 결과.
* **function definition(함수 정의):** 함수를 생성하는 행위.
* **function call/invoke(함수 호출):** argument를 parameter를 통해 함수에 전달하며 함수의 실행을 명시적으로 지시하는 행위.

함수를 사용하는 이유는 대표적으로 **코드의 재사용**을 위함임.<br>
코드를 재사용할 수 있도록 돕는 함수는 프로그램의 **유지보수**를 편리하게 하고,<br>
개발자의 실수를 줄여 코드의 **신뢰성**을 높인다. 또한 함수 이름으로 **가독성**을 향상시킨다.

## 2. Function Literals: 함수 리터럴
```javascript
// 변수에 함수 리터럴을 할당
var f = function add(x, y) {
  return x + y;
}
```
**자바스크립트의 함수는 객체**다.<br>
따라서 마치 숫자 값을 숫자 리터럴로 생성하고, 객체를 객체 리터럴로 생성하는 것처럼<br>
함수도 **함수 리터럴**로 생성할 수 있다. 함수 리터럴도 다른 리터럴들처럼 **평가되어 값을 생성**한다.<br>
함수 리터럴의 구성요소는 다음과 같다.
* **함수 이름**
  * 함수 몸체 내에서만 참조할 수 있는 식별자.
  * 생략 가능함. 이에 따라 이름이 있는 함수를 named function(기명 함수), 이름이 없는 함수를 anonymous function(익명 함수)라고 부른다.
* **매개변수 목록**
  * 0개 이상의 매개변수를 소괄호로 감싸고 쉼표로 구분함.
* **함수 몸체**
  * 함수 호출에 의해 실행되는 코드 블록

## 3. Defining Functions: 함수 정의
함수 정의란, 함수 호출 이전에 인수를 전달받을 매개변수와 실행할 statements, 그리고 반환할 값을 지정하는 행위다.<br>
함수를 정의하는 방법에는 아래처럼 4가지가 있다. 차례대로 살펴 보자.

![image](https://github.com/user-attachments/assets/2b92b95e-8c50-4fa6-920f-eb734a1a7de3)

### 3-1. Function Declaration: 함수 선언문
**함수 선언문**은 표현식이 아닌 문이다.<br>
함수 선언문은 함수 리터럴과 형태가 동일하나, **함수 이름을 생략할 수 없다**는 특징이 있다.<br>
함수 선언문을 사용해 함수를 정의하는 방식은 다음과 같다.
```javascript
// 함수 선언문
function add(x, y) {
  return x + y;
}
```
함수는 함수 이름으로 호출하는 것이 아니라 **함수 객체를 가리키는 식별자로 호출**한다.<br>
그런데 자바스크립트의 특이한 점은, 위 코드 예제처럼 함수 선언문만 작성한 경우에도<br>
`add`라는 함수 이름으로(여기서 `add`는 함수 객체를 가리키는 '식별자'가 아닌, 함수 선언문의 '함수 이름'일 뿐인데도)<br>
아래처럼 `add` 함수를 호출할 수 있다.
```javascript
// 위에서 선언한 add 함수를 호출하기
console.log(add(2, 7));  // 7
```
어떻게 이게 가능할까?<br>
함수 선언문으로 선언만 했을 뿐인 `add`라는 함수 이름으로, `add` 함수를 호출까지 할 수 있었던 이유는<br>
바로 자바스크립트 엔진이 **암묵적으로** `add` 함수의 식별자를 생성했기 때문이다.

자바스크립트 엔진은 함수 선언문을 해석해 함수 객체를 생성한다.<br>
이때, 만약 함수 객체를 가리키는 식별자가 없다면 생성된 함수 객체를 참조할 수 없으므로 호출할 수도 없다.<br>
따라서 자바스크립트 엔진은 생성된 함수를 호출하기 위해 함수 이름과 동일한 이름의 식별자를 **암묵적으로** 생성하고, 거기에 함수 객체를 할당한다.

즉, 수도 코드로 표현하자면 함수 선언문을 작성한 경우, 내부에서는 다음과 같이 작동한다는 의미다.
```javascript
var add = function add(x, y) {
  return x + y;
}

console.log(add(2, 7));  // 7
```
![image](https://github.com/user-attachments/assets/3c57e685-a58d-4661-a547-f00508ff2461)

자바스크립트 엔진은 **함수 선언문을 함수 표현식으로 자동 변환**해 함수 객체를 생성한다고도 할 수 있다.<br>
그러나 함수를 정의하는 이 두 방식이, 자바스크립트 엔진에서 완전히 동일하게 동작하지는 않는다.<br>
둘 간의 미묘한 차이점이 무엇인지 알아보기 전에 함수 표현식부터 우선 알아보자.

### 3-2. Function Expression: 함수 표현식
자바스크립트에서 함수는, **마치 값처럼** 자유자재로 사용할 수 있는 일급 객체다.<br>
따라서 개발자는 함수를 생성해 그 객체를 **변수에 할당** 또한 할 수 있다. 이러한 방식을 우리는 **함수 표현식**이라고 부른다.

함수를 호출할 때는 함수 이름이 아닌 함수 객체를 가리키는 식별자를 사용해야만 하며,<br>
함수 이름은 그 함수의 몸체 내부에서만 유효하므로 함수 표현식의 함수 리터럴은 함수 이름을 생략하는 것이 일반적이다.
```javascript
var add = function foo (x, y) {
  return x + y;
}

// 함수는 그 함수 객체를 가리키는 식별자로만 호출할 수 있다.
console.log(add(2, 5));  // 7

// 함수 이름으로 호출할 수 없다. 함수 이름은 함수 몸체 내부에서만 유효하다.
console.log(foo(2, 5));  // ReferenceError: foo is not defined

// 따라서 함수 표현식의 함수 리터럴은 다음처럼 함수 이름을 생략하는 것이 일반적이다.
var add = function (x, y) {
  return x + y;
}
```

### 3-3. 함수 선언문 vs. 함수 표현식(함수 생성 시점과 함수 호이스팅)
다음 예제를 보며 함수 선언문과 함수 표현식 간 차이점을 이해해 보자.

![image](https://github.com/user-attachments/assets/6e9d8c43-8b6f-49f1-9e18-69e127cb3d07)
![image](https://github.com/user-attachments/assets/373bf8f7-fa37-48d0-8bd2-061e93579416)

**함수 선언문으로 정의한 함수:**
* 런타임 **이전에** 함수 객체가 생성됨 
  * 다시 말해, 자바스크립트 엔진이 함수 이름과 동일한 식별자를 암묵적으로 생성 후, 함수 객체를 **할당까지 함.**
  * 따라서 함수 정의 **이전** 위치에서도 함수를 **참조/호출**할 수 있게 됨(=**Function Hoisting(함수 호이스팅)**)

**함수 표현식으로 정의한 함수:**
* 함수 표현식은 변수 선언문 및 할당문과 다를 게 없음
  * 다시 말해, 런타임 **이전에** 실행돼 함수 식별자가 `undefined`로 초기화되며, 런타임 **중에** 할당문이 평가돼 함수 객체가 뒤늦게 생성됨.
  * 다시 말해, **변수 호이스팅**이 발생할지언정 함수 호이스팅은 발생하지 않음.
  * 따라서 함수 표현식으로 정의한 함수는, 함수를 정의하기 전 위치에서는 함수를 참조/호출할 수 **없음**
![image](https://github.com/user-attachments/assets/9d146808-ee4e-4f67-bea4-9261867c7342)

함수 선언문이 선사하는 **함수 호이스팅**은 '반드시 함수를 호출하기 전에 함수를 선언해야 한다'는 **상식을 파괴**한다.<br>
따라서 일반적으로 함수 선언문 대신 **함수 표현식을 사용할 것을 권장한다.**

### 3-4. Function Constructor: Function 생성자 함수
생략! (자세한 내용은 책을 참고)

### 3-5. Arrow Function Expression: 화살표 함수
생략! (26장 'ES6 함수의 추가 기능'에서 자세히 배울 예정)

## 4. Calling Functions: 함수 호출


## 5. 참조에 의한 전달과 외부 상태의 변경

## 6. 다양한 함수의 형태
### 6-1. IIFE (Immediately Invoked Function Expression): 즉시 실행 함수
![image](https://github.com/user-attachments/assets/c6742e36-db3e-4a50-992b-b4fdc8136497)
### 6-2. Recursive Function: 재귀 함수
![image](https://github.com/user-attachments/assets/63eb1519-32da-4ee7-b99f-41e2f2f33fc6)
![image](https://github.com/user-attachments/assets/3627d7c7-ad9a-49f6-933a-0d502720a8e8)
### 6-3. Nested/Inner Function: 중첩 함수
![image](https://github.com/user-attachments/assets/184732a3-b0b6-4e6a-af81-8bbd0cb3d0f1)
### 6-4. Callback Function: 콜백 함수
![image](https://github.com/user-attachments/assets/e8ecbe13-acb0-4c73-a722-2a76758a9bc7)
### 6-5. Pure Function vs. Impure Function: 순수 함수와 비순수 함수
![image](https://github.com/user-attachments/assets/9d6a68bf-5efb-4fb8-b573-58646db13619)
![image](https://github.com/user-attachments/assets/514b3601-094e-4439-ae4c-7345ba306aa1)

끝.
