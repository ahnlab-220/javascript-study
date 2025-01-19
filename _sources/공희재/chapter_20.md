# Chapter 20 - strict mode

## 1. strict mode란?

자바스크립트는 오타나 문법 지식의 미비로 인한 실수를 저지르기 쉬운 언어다. (e.g., implicit global(암묵적 전역) 등)<br/>
개발자의 의도와는 다르게 오류가 발생하는 것을 막기 위해 ES5부터 **strict mode**(엄격 모드)가 추가되었다.

ESLint 같은 린트 도구를 사용해도 strict mode와 유사한 효과를 얻을 수 있다.<br/>
더군다나 린트 도구는 static analysis(정적 분석) 기능으로 소스코드를 실행하기 전에 미리 스캔해 문법적 오류뿐만 아니라<br/>
잠재적 오류까지 찾아내고 리포팅해주는 유용한 도구다.<br/>
(따라서 책에서는 strict mode보다는 **ESLint 같은 린트 도구를 더 선호**한다고 밝히기는 함)

참고로 ES6부터 도입된 **클래스와 모듈은 기본적으로 strict mode**가 적용된다.

## 2. strict mode의 적용
strict mode를 적용하려면 **전역의 선두**, 또는 **함수 몸체의 선두**에 **`'use strict';`** 한 줄만 추가하면 된다.<br/>
전역의 선두에 추가하면 스크립트 전체에 strict mode가 적용되고, 함수 몸체의 선두에 추가하면 해당 함수와 중첩 함수에 strict mode가 적용된다.
```javascript
// 전역 선두에 추가한 예제
'use strict';

function foo() {
  x = 10;  // ReferenceError: x is not defined
}
foo();
```

```javascript
// 코드의 선두에 추가하지 않으면 제대로 동작하지 않는다
function foo() {
  x = 10;  // 에러가 발생하지 않음.
  'use strict';
}
foo();
```

## 3. 전역에 strict mode를 적용하는 것은 피하자
전역에 적용한 strict mode는 아래 예제처럼 스크립트 단위로 적용된다.
```html
<!DOCTYPE html>
<html>
<body>
  <script>
    x = 1;  // 에러가 발생하지 않음.
    console.log(x);  // 1
  </script>
  <script>
    'use strict';
    y = 1;  // ReferenceError: y is not defined
    console.log(y);
  </script>
</body>
</html>
```
그러나, strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것은 오류를 발생시킬 수 있다.<br/>
특히 외부 서드파티 라이브러리를 사용하는 경우, 라이브러리가 non-strict mode인 경우도 있기 때문에<br/>
전역에 strict mode를 적용하는 것은 어쨌든 **바람직한 행위가 아니다**.

## 4. 함수 단위로 strict mode를 적용하는 것도 피하자.
함수 안에서 strict mode를 사용하는 것도 권장할만한 행위는 아니다.<br/>
어떤 함수는 strict mode이고 어떤 함수는 non-strict mode인 것 자체가 바람직하지 않으며 모든 함수에 일일이 strict mode를 적용하는 것은 번거로운 일이다.

대신, strict mode는 **즉시 실행 함수로 감싼 스크립트 단위로 적용**하는 것이 바람직하다.

## 5. strict mode가 발생시키는 에러
다음은 strict mode를 적용했을 때, 에러가 발생하는 대표적인 사례다.

### 5-1. 암묵적 전역
애초에 var, let, const 등의 키워드로 **선언하지 않은 변수를 참조하면 `ReferenceError`를 발생**시킨다.
```javascript
(function () {
  'use strict';
  x = 1;
  console.log(x);  // ReferenceError: x is not defined
}());
```

### 5-2. 변수, 함수, 매개변수의 삭제
delete 연산자로 변수, 함수, 매개변수를 삭제하면 `SyntaxError`를 발생시킨다.

### 5-3. 매개변수 이름의 중복
**중복하는 매개변수 이름을 사용하면 `SyntaxError`를 발생**시킨다.
```javascript
(function () {
  'use strict';

  // SyntaxError: Duplicate parameter name not allowed in this context
  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
}());
```
### 5-4. with 문의 사용
with 문을 사용하면 `SyntaxError`를 발생시킨다. (사실 with 문은 애초에 사용을 지양해야 한다고 책은 언급한다.)

## 6. strict mode 적용에 의한 변화
### 6-1. 일반 함수의 this
strict mode에서 함수를 **일반 함수**로서 호출하면, **`this`에 `undefined`가 바인딩**된다.<br/>
일반 함수 내부에서는 this를 사용할 필요가 없기 때문이다. 단, 이때 에러가 발생하지는 않는다.
```javascript
(function () {
  'use strict';

  function foo() {
    console.log(this);  // undefined
  }
  foo();

  function Foo() {
    console.log(this);  // Foo
  }
  new Foo();
}());
```

### 6-2. arguments 객체
strict mode에서는 매개변수에 전달된 인수를 재할당해 변경해도, **arguments 객체에 반영되지 않는다**.
```javascript
(function (a) {
  'use strict';

  // 매개변수에 전달된 인수를 재할당해 변경해도,
  a = 2;

  // 변경된 인수가 arguments 객체에 반영되지 않는다.
  console.log(arguments);  // { 0: 1, length: 1 }
}(1));
```

끝.
