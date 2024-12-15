# Chapter 15 - let, const 키워드와 블록 레벨 스코프

ES5까지 변수를 선언할 수 있는 유일한 방법은 `var` 키워드를 사용하는 것입니다.  
`var` 키워드는 다음과 같은 특징을 가지고 있습니다.

<br><br>

#### var - 변수 중복 선언 허용
`var` 키워드로 선언한 변수는 중복 선언이 가능합니다.  

``` js
var x = 1;
var y = 1;

// var 키워드를 사용하면 같은 스코프 내에서 중복 선언을 허용한다.
var x = 100;

// 초기화 문이 없는 변수 선언문은 무시된다.
var y;

console.log(x);  // 100
console.log(y);  // 1
```

만약 동일한 이름의 변수가 이미 선언되어 있는 것을 모르고 변수를 중복 선언하며 값을 할당했을 경우, 의도치 않게 먼저 선언된 변수 값이 변경되는 부작용이 발생합니다.  


<br><br>

#### var - 함수 레벨 스코프
`var` 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정하게 됩니다.  
따라서 함수 외부에서 `var` 키워드로 선언한 변수는 블록 내에서 선언해도 모두 전역 변수가 됩니다.

``` js
var x = 1;

if (true) {
    // x는 전역 변수이다. 이미 선언된 전역 변수 x가 있으므로 x는 중복 선언된다.
    // 의도치 않게 변수값이 변경되는 부작용이 발생한다.
    var x = 100;
}

console.log(x);     // 100
```

`for` 문의 변수 선언문에서 `var` 키워드로 선언한 변수도 전역 변수가 됩니다.  

<br>

``` js
var i = 10;

// for 문의 변수 선언문에서 var 키워드로 선언한 변수도 전역 변수가 된다.
for (var i = 0; i < 5; i++) {
    console.log(i);     // 0 1 2 3 4
}

// 의도치 않게 i 변수의 값이 변경된다.
console.log(i);     // 5
```

<font color='orange'>함수 레벨 스코프는 전역 변수를 남발할 가능성을 높입니다.  
이로 인해 의도치 않게 전역 변수를 중복 선언하는 경우가 발생합니다.</font>


<br><br>

#### var - 변수 호이스팅
`var` 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어올려진 것처럼 동작합니다.  
> `var` 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다. 

``` js
console.log(foo);       // undefined

// 변수에 값을 할당
foo = 123;
console.log(foo);   // 123

// 변수 선언문은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
var foo;
``` 


<br><br><hr>

### let 키워드
`var` 변수의 단점을 보완하기 위해 ES6에서는 새로운 변수 키워드인 `let`과 `const`를 도입했습니다.

<br>

#### let - 변수 중복 선언 금지
`var` 키워드로 이름이 동일한 변수를 중복 선언하면 아무런 에러가 발생하지 않습니다.  
하지만 `let` 키워드로 이름이 같은 변수를 중복 선언하면 문법 에러(`SyntaxError`)가 발생합니다.

``` js
// var 키워드는 중복 선언을 허용함
var foo = 123;
var foo = 456;

// let 키워드는 중복 선언을 허용하지 않음
let bar = 1234;
let bar = 5678;     // Uncaught SyntaxError: Identifier 'bar' has already been declared
```

<br>

#### let - 블록 레벨 스코프
`var` 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정하는 함수 레벨 스코프를 따르지만,  
`let` 키워드로 선언한 변수는 모든 코드 블록(함수, if, for, while, try/catch 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따릅니다.

``` js
let foo = 1;    // 전역 변수
{
    let foo = 2;    // 지역 변수
    let bar = 3;    // 지역 변수
}

console.log(foo);       // 1
console.log(bar);       // ReferenceError: bar is not defined
```

<br>

#### let - 변수 호이스팅
`var` 키워드로 선언한 변수와 달리 `let` 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작합니다.

``` js
console.log(foo);       // Uncaught ReferenceError: foo is not defined
let foo;
```

`let` 키워드로 선언한 변수는 "선언 단계"와 "초기화 단계"가 분리되어 진행됩니다.  
> 즉, 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행된다.

`let` 키워드로 선언한 변수는 스코프의 시작 지점부터 초기화 시작 지점(변수 선언문)까지 변수를 참조할 수 없습니다.  
이처럼 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 일시적 사각지대(Temporal Dead Zone; TDZ)라고 부릅니다.  

<img width="350" alt="image" src="https://github.com/user-attachments/assets/135c7a43-7a61-4518-9461-0251b894e1a5">

<br>

ES6에서 도입된 `let`, `const`, `class`를 사용한 선언문은 호이스팅이 발생하지 않는 것처럼 동작한다는 것을 기억합시다.


<br><br>

#### 전역 객체와 let
`var` 키워드로 선언한 전역 변수와 전역 함수, 그리고 선언하지 않은 변수에 대한 값을 할당한 암묵적 전역은 전역 객체 `window`의 프로퍼티가 됩니다.

그리고 전역 객체의 프로퍼티를 참조할 때 `window`를 생략할 수 있습니다.

``` js
// 전역 변수
var x = 1;

// 암묵적 전역
y = 2;

// 전역 함수
function foo() {}

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티이다.
console.log(window.x);      // 1
console.log(x);             // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티이다.
console.log(window.y);      // 2
console.log(y);             // 2

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티이다.
console.log(window.foo);    // foo() {}
console.log(foo);           // foo() {}
```

<br>

단, `let`, `const` 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.
> `window.foo`와 같이 접근할 수 없다.

`let` 전역 변수는 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드) 내에 존재하기 때문입니다.  

``` js
let x = 1;

// let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다.
console.log(window.x);      // undefined
console.log(x);             // 1
```


<br><br><hr>

### const 키워드
`const` 키워드는 상수(constant)를 선언하기 위해 사용합니다.  
하지만 반드시 상수만을 위해 사용하지는 않는다.

`const` 키워드의 특징은 `let` 키워드와 대부분 동일하므로  
`let` 키워드와 다른 점을 중점으로 살펴보도록 합시다.

<br>

#### 선언과 초기화
<font color='orange'>const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 합니다. </font>

``` js
const foo = 1;
```
그렇지 않으면 문법 오류(SyntazError)가 발생한다.

`const` 키워드로 선언한 변수는 `let` 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것 처럼 동작합니다.


``` js
{
    // 변수 호이스팅이 발생하지 않는 것 처럼 동작한다.
    console.log(foo);   // ReferenceError: Cannot access 'foo' before initialization
    const foo = 1;
    console.log(foo);   // 1
}

// 블록 레벨 스코프를 가진다.
console.log(foo);   // ReferenceError: foo is not defined
```

<br>

#### 재할당 금지
`var` 또는 `let` 키워드로 선언한 변수는 재할당이 자유로우나  
<font color='orange'>const 키워드로 선언한 변수는 재할당이 금지됩니다. </font>

``` js
const foo = 1;
foo = 2;    // TypeError: Assignment to constant variable
```


<br>

#### 상수
`const` 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없습니다.  
**상수는 재할당이 금지된 변수를 의미합니다.** 

<font color='orange'>상수는 상태 유지와 가독성, 유지보수의 편의를 위해 적극적으로 사용되어야 합니다. </font>

``` js
// 세전 가격
let preTaxPrice = 100;

// 세후 가격
// 0.1의 의미를 모르기 때문에 가독성에 좋지 않다
let afterTaxPrice = preTaxPrice + (preTaxPrice + 0.1)
console.log(afterTaxPrice);     // 110
```

여기서 0.1은 세율을 의미하는 값으로 쉽게 바뀌지 않는 값이며, 프로그램 전체에서 고정적으로 사용되는 값입니다. 이런 경우에 상수로 정의 시, 값의 의미를 쉽게 파악살 수 있고 변경될 수 없는 고정값으로 사용할 수 있습니다.

<br>

일반적으로 상수의 이름은 대문자로 선언해 상수임을 명확히 나타냅니다.  
여러 단어로 이뤄진 경우에는 언더스코어(_)로 구분해서 스네이크 케이스로 표현하는 것이 일반적입니다.  

``` js
const TAX_RATE = 0.1;

let preTaxPrice = 100;
let afterTaxPrice = preTaxPrice + (preTaxPrice + TAX_RATE);

console.log(afterTaxPrice);     // 110
```


<br><br>


#### const 키워드와 객체
`const` 키워드로 선언된 변수에 원시 값을 할당한 경우 값을 변경할 수 없습니다.   
하지만 <font color='orange'>const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있습니다.</font>

``` js
const person = {
    name: 'Lee'
};

// 객체는 변경 가능한 값입니다. 따라서 재할당 없이 변경이 가능합니다.
person.name = 'Kim';
console.log(person);    // {name: "Kim"}
```

**`const` 키워드는 재할당을 금지할 뿐 "불변"을 의미하지 않는다는 것을 기억합시다.**


<br><br><hr>

### var, let, const
`var`와 `let`, `const` 키워드는 다음 상황에서 사용하는 것을 권장합니다.

- ES6을 사용한다면 `var` 키워드는 사용하지 않는다.
- 재할당이 필요한 경우에 한정해 `let` 키워드를 사용합니다. 이때 변수의 스코프는 최대한 좁게 만든다.
- 변경이 발생하지 않고 읽기 전용으로 사용되는 원시 값과 객체에는 `const` 키워드를 사용하는 것을 추천한다.


