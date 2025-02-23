### Chapter 20 - strict mode
#### strict mode란?
``` js
function foo() {
    x = 10;
}

foo();

console.log(x);     // ?
```

foo 함수 내에서 선언하지 않은 x 변수에 값 10을 할당했습니다.  
이때 x 변수를 찾아야 x에 값을 할당할 수 있기 때문에 자바스크립트 엔진은 x 변수가 어디서 선언되었는지 스코프 체인을 통해 검색하게 됩니다.

자바스크립트 엔진은 아래의 순서로 겁색을 시도할 것입니다.
1. `foo` 함수의 스코프에서 `x` 변수의 선언을 검색한다. (`foo` 함수의 스코프에는 `x` 변수의 선언이 없으므로 검색에 실패한다.)
2. `foo` 함수 컨텍스트의 상위 스코프(예제의 경우에는 전역 스코프)에서 `x` 변수의 선언을 검색한다.

전역 스코프에도 `x` 변수의 선언이 존재하지 않기 때문에 `ReferenceError`를 발생시킬 것 같지만  
자바스크립트 엔진은 <font color='orange'>암묵적으로 전역 객체에 `x` 프로퍼티를 생성합니다.</font>
> 마치 전역 변수처럼 `x` 변수를 사용할 수 있게 된다.

**위와 같은 현상을 암묵적 전역(implict global)이라 합니다.**

<br>

개발자 의도와는 상관없이 발생한 암묵적 전역은 오류를 발생시키는 원인이 될 가능성이 큽니다.  
따라서 반드시 `var`, `let`, `const` 키워드를 사용하여 변수를 선언한 다음 사용해야 합니다.

<br>

다만 휴먼 에러로 실수는 언제나 존재할 수 있습니다.  
따라서 오류를 줄여 안정적인 코드를 생성하기 위해 잠재적으로 오류를 발생시키기 어려운 개발 환경을 만들고,  
그 환경에서 개발할 수 있도록 해결 방안을 모색하기 시작했습니다.

<br>

이를 위해 ES5 부터는 `strict mode(엄격 모드)`가 추가되었습니다.  
이 모드는 <font color='orange'>자바스크립트 언어의 문법을 조금 더 엄격하게 적용하여</font> 오류를 발생시킬 가능성이 높거나  
<font color='orange'>자바스크립트 엔진의 최적화 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시키도록 해줍니다.</font>

<br>

`ESLint`와 같은 린트 도구를 사용해도 strict mode와 유사한 효과를 얻을 수 있습니다.  
> 린트 도구는 `정적 분석` 기능을 통해 소스코드를 실행하기 전에 문법적 오류 뿐만 아니라 잠재거인 오류까지 찾아내주는 도구이다.

<br><br>


### strict mode의 적용


자바스크립트에서 strict mode는 더 엄격한 문법과 오류 처리를 적용하여 잠재적인 오류를 방지하고 안전한 코드를 작성할 수 있도록 도와줍니다. 
strict mode를 사용하면 다음과 같은 이점이 있습니다:

- 암묵적 전역 변수 생성을 방지합니다.
- 읽기 전용 속성에 값을 할당하는 등의 실수를 방지합니다.
- `this`가 `undefined`로 설정되는 것을 방지합니다.

strict mode를 활성화하려면 파일 또는 함수의 맨 위에 `"use strict";` 지시어를 추가합니다.  
<font color='orange'>다만, strict mode이 적용될 블록 스코프를 직접 정할 수 있습니다.</font>

``` js
'use strict';

function foo() {
    x = 10; // ReferenceError: x is not defined
}

foo();

console.log(x);
```
위 코드는 전역 스코프에 대해 strict mode가 적용되도록 할 수 있습니다.

<br><br>

``` js


function foo() {
    'use strict';
    x = 10; // ReferenceError: x is not defined
}

foo();

console.log(x);
```

함수 스코프에 추가하면 해당 함수 및 중첩 함수에 strict mode가 적용됩니다.


<br><br>

### 전역에 strict mode를 적용하는 것은 피하자

<font color='orange'>전역에 적용한 strict mode는 스크립트 단위로 적용됩니다.</font>

``` html
<!DOCTYPE html>
<html>
<body>
    <script>
        'use scrict';
    </script>
    <script>
        x = 1;  // 에러가 발생하지 않는다.
        console.log(x);     // 1
    </script>
    <script>
        'use strict';

        y = 1;  // ReferenceError: y is not defined
        console.log(y);
    </script>
</body>
</html>
```


위 예제와 같이 스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않습니다.  
하지만 `strict mode` 스크립트와 `non-strict mode` 스크립트를 혼용하는 것은 오류를 발생시킬 수 있습니다.  
> 외부 서드파티 라이브러리를 사용하는 경우 라이브러리가 `non-strict mode`인 경우가 있기 때문에 전역에 strict mode를 적용하는 것은 바람직하지 않다.

<br><br>

### 함수 단위로 strict mode를 적용하는 것을 피하자
함수 단위로 strict mode를 적용할 수도 있지만  
어떤 함수는 strict mode를 적용하고 어떤 함수는 strict mode를 적용하지 않는 것은 바람직하지 않으며  
모든 함수에 일일이 strict mode를 적용하는 것은 번거롭습니다.  

그리고 strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는다면  
이 또한 문제가 발생할 수 있습니다.


``` js
(function () {
    // non-strict mode
    var let = 10;       // 에러가 발생하지 않음

    function foo() {
        'use strict';

        let = 20;   // SyntaxError: Unexpected strict mode reserved word
    }
    foo();
})
```

따라서 strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직합니다.

``` js
// 즉시 실행 함수의 선두에 strict mode 적용
(function () {
    'use strict';
    // Do something...
}());

```


<br><br>


### strict mode가 발생시키는 에러
아래는 strict mode를 적용시켰을 때 에러가 발생하는 대표적 사례입니다.

<br><br>

#### 암묵적 전역
선언하지 않은 변수를 참조하면 `ReferenceError`가 발생합니다.

``` js
(function () {
    'use strict';

    x = 1;
    console.log(x);     // ReferenceError: x is not defined
}());
```

<br><br>

#### 변수, 함수, 매개변수의 삭제
`delete` 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError가 발생합니다.

``` js
(function () {
    'use strict';

    var x = 1;
    delete x;       // SyntaxError: Delete of an unqualified identifier in strict mode.

    function foo(a){
        delete a;   // SyntaxError: Delete of an unqualified identifier in strict mode.
    }
    delete foo;     // SyntaxError: Delete of an unqualified identifier in strict mode.
}());
```

<br><br>

#### 매개변수 이름의 중복
중복된 매개변수 이름을 사용하면 `SyntaxError`가 발생합니다.

``` js
(function () {
    'use strict';

    // SyntaxError: Duplicate parameter name not allowed in this context
    function foo(x, x) {
        return x + x;
    }
    console.log(foo(1, 2))
}());
```

<br><br>

#### with 문의 사용
`with` 문을 사용하면 `SyntaxError`가 발생합니다.  
`with` 문은 전달된 객체를 스코프 체인에 추가합니다.  

`with` 문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 까지 객체 이름을 생략할 수 있어서 코드가 간단해질 수 있지만  
성능과 가독성이 나빠지는 문제가 있습니다.  따라서 `with` 문은 사용하지 않는 것이 좋습니다.

``` js
(function() {
    'use strict';

    // SyntaxError: Strict mode code may not include a with statement
    with({ x: 1}){
        console.log(x);
    }
}());
```