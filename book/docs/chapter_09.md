# Chapter 09 - 타입 변환과 단축 평가
### 타입 변환
값의 타입은 개발자의 의도에 따라 다른 타입으로 변환할 수 있습니다.  
개발자가 의도적으로 값의 타입을 변환하는 것을   
명시적 타입 변환(explict coercion) 또는 타입 캐스팅(type casting)이라 합니다.

``` js
let x = 100;

// 명시적 타입 변환
let str = x.toString();
console.log(typeof str, str);   // string 100

// x 변수의 값이 변경된 것은 아님
console.log(typeof x, x);   // number 100
```

<br>

개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해   
암묵적으로 타입이 자동 변환되도록 하는 것을  
암묵적 타입 변환(implict coercion) 또는 타입 강제 변환(type coercion)이라 합니다.  

``` js
let x = 100;

// 암묵적 타입 변환
let str = x + '';
console.log(typeof str, str);   // string 100

// x 변수의 값이 변경된 것은 아님
console.log(typeof x, x);  // number 100
```

<br><br>

암묵적 타입 변환은 기존 변수 값을 재할당하여 변경하는 것이 아닙니다.  
자바스크립트 엔진은 표현식을 에러 없이 평가하기 위해  
피연산자의 값을 암묵적 타입 변환해 새로운 타입의 값을 만들어 단 한 번 사용하고 버립니다.

암묵적 타입 변환만을 사용할 수도 있지만 가끔 명시적 타입 변환을 사용하는 경우가 있다.  
**이는 타입을 변경하겠다는 개발자의 의지가 그대로 드러나는 것**입니다.  
따라서 명시적 타입 변환 코드가 보였을 경우 해당 문은 코드의 흐름에 중요한 역할을 할 가능성이 높습니다.  

<br><br>

### 암묵적 타입 변환
자바스크립트 엔진은 표현식을 평가할 때 개발자의 의도와는 상관없이  
코드의 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환할 때가 있습니다.

``` js
'10' + 2  // '102'

5 * '10'  // '50'

!0  // true
```

이 처럼 표현식을 평가할 때 코드의 문맥에 부합하지 않는 다양한 상황이 발생할 수 있는데   
이때 프로그래밍 언어에 따라 에러를 발생시키기도 하지만  
자바스크립트는 가급적 에러를 발생시키지 않도록 암묵적 타입 변환을 통해 표현식을 평가합니다.

암묵적 타입 변환이 발생하면 문자열, 숫자, 불리언과 같은 원시 타입 중 하나로 자동 변환됩니다.  
타입 별로 암묵적 타입 변환이 어떻게 이루어지는지 살펴보도록 하자.

<br><br>

#### 문자열 타입으로 변환
``` js
1 + '2'  //   '12'
```

위 예제의 `+` 연산자는 피연산자 중 하나 이상이 문자열이므로 문자열 연결 연산자로 동작합니다.  
문자열 연결 연산자의 역할은 문자열 값을 만드는 것입니다.  
따라서 문자열 연결 연산자의 모든 피연산자는 코드의 문맥상 모두 문자열 타입이어야 합니다.  

자바스크립트 엔진은 문자열 연결 연산자 표현식을 평가하기 위해 문자열 연결 연산자의 피연산자 중에서  
문자열 타입이 아닌 피연산자를 문자열 타입으로 암묵적 타입 변환합니다.  

<br>

자바스크립트 엔진은 문자열 타입 아닌 값을 문자열 타입으로   
암묵적 타입 변환을 수행할 때 아래와 같이 동작합니다.

``` js
// 숫자 타입
0 + ''              // '0'
-0 + ''             // '0'
1 + ''              // '1'
-1 + ''             // '-1'
NaN + ''            // 'NaN'
Infinity + ''       // 'Infinity'
-Infinity + ''      // '-Infinity'

// 불리언 타입
true + ''           // "true"
false + ''          // "false"

// null 타입
null + ''           // "null"

// undefined 타입
undefined + ''      // "undefined"

// Symbol 타입
(Symbol()) + ''     // TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + ''           // "[object Object]"
Math + ''           // "[object Math]"
[] + ''             // ""
[10, 20] + ''       // "10,20"
(function(){}) + '' // "function(){}"
Array + ''          // "function Array() { [native code] }"
```

<br><br>

#### 숫자 타입으로 변환
``` js
1 - '1'     // 0
1 * '10'    // 10
1 / 'one'   // NaN
```

위 예제에서 사용한 연산자는 모두 산술 연산자입니다.    
산술 연산자의 역할은 숫자 값을 만드는 것입니다.   
따라서 산술 연산자의 모든 피연산자는 코드 문맥상 모두 숫자 타입이어야 합니다.  

자바 스크립트 엔진은 산술 연산자 표현식을 평가하기 위해   
산술 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환합니다.  
이때 피연산자를 숫자 타입으로 변환할 수 없는 경우는 산술 연산을 수행할 수 없으므로 평가 결과는 `NaN`이 됩니다.

<br>

피연산자를 숫자 타입으로 변환해야 할 문맥은 산술 연산자 뿐만이 아닙니다.  

``` js
'1' > 0     // true
```

비교 연산자의 역할은 boolean 값을 만드는 것입니다.  
자바스크립트 엔진은 비교 연산자 표현식을 평가하기 위해  
비교 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환합니다.  

<br>

자바스크립트 엔진은 숫자 타입이 아닌 값을 숫자 타입으로   
암묵적 타입 변환을 수행할 때 아래와 같이 동작합니다.
> `+` 단항 연산자의 경우 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환한다.

``` js
// 문자열 타입
+''         // 0
+'0'        // 0
+'1'        // 1
+'string'   // NaN

// boolean 타입
+true       // 1
+false      // 0

// null 타입
+null       // 0

// undefined 타입
+undefined  // NaN

// 심볼 타입
+Symbol()   // TypeError: Cannot convert a Symbol value to a number

// 객체 타입
+{}         // NaN
+[]         // 0
+[10, 20]   // NaN
+(function(){})     // NaN
```

빈 문자열(`''`), 빈 배열(`[]`), `null`, `false`는 0으로, `true`는 1로 반환됩니다.  
객체, 빈 배열이 아닌 배열, `undefined`는 변환되지 않아 `NaN`이 된다는 것을 주의해야 합니다.

<br>

#### boolean 타입으로 변환
자바스크립트 엔진은 boolean 타입이 아닌 값을 True 또는 False로 구분합니다.  
아래의 값들은 `false`로 평가되는 값들입니다.
- `false`
- `undefined`
- `null`
- `0`, `-0`
- `NaN`
- `''` (빈 문자열)

<br>

위를 제외한 나머지는 모두 `true`로 평가되는 값들입니다.


<br><br>

### 명시적 타입 변환
개발자의 의도에 따라 명시적으로 타입을 변경하는 방법은 다양합니다.  
표준 Bulit-In 생성자 함수(`String`, `Number`, `Boolean`)를 `new` 연산자 없이 호출하는 방법과  
Built-In 메서드를 활용하는 방법, 그리고 암묵적 타입 변환을 이용하는 방법도 있습니다.  

<br><br>

#### 문자열 타입으로 변환
문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법은 아래와 같습니다.  
- `String` 생성자 함수를 `new` 연산자 없이 호출하는 방법
- `Object.prototype.toString` 메소드를 이용하는 방법
- 문자열 연결 연산자를 이용하는 방법


``` js
// 1. `String` 생성자 함수를 `new` 연산자 없이 호출하는 방법
String(1);          // "1"
String(NaN);        // "NaN"
String(Infinity);   // "Infinity"
String(true);       // "true"
String(false);       // "false"

// 2. `Object.prototype.toString` 메소드를 이용하는 방법
(1).toString();         // "1"
(NaN).toString();       // "NaN"
(Infinity).toString();  // "Infinity"
(true).toString();      // "true"
(false).toString();     // "false"

// 3. 문자열 연결 연산자를 이용하는 방법
1 + '';                 // "1"
NaN + '';               // "NaN"
Infinity + '';          // "Infinity"
true + '';              // "true"
false + '';             // "false"
```


<br><br>

#### 숫자 타입으로 변환
숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법은 아래와 같습니다.  
- `Number` 생성자 함수를 `new` 연산자 없이 호출하는 방법
- `parseInt`, `parseFloat` 함수를 사용하는 방법 (문자열만 숫자 타입으로 변환 가능)
- `+` 단항 산술 연산자를 이용하는 방법
- `*` 산술 연산자를 이용하는 방법

``` js
// 1. `Number` 생성자 함수를 `new` 연산자 없이 호출하는 방법
Number('0');            // 0
Number(true);           // 1
Number(false);          // 0

// 2. `parseInt`, `parseFloat` 함수를 사용하는 방법 (문자열만 숫자 타입으로 변환 가능)
parseInt('0');          // 0

// 3. `+` 단항 산술 연산자를 이용하는 방법
+'0';                   // 0
+true;                  // 1
+false;                 // 0

// 4. `*` 산술 연산자를 이용하는 방법
true * 1;               // 1
false * 1;              // 0
```


<br><br>

#### boolean 타입으로 변환
boolean 타입이 아닌 값들을 boolean 타입으로 변환하는 방법은 다음과 같습니다.
- `Boolean` 생성자 함수를 `new` 연산자 없이 호출하는 방법
- `!` 부정 논리 연산자를 두 번 사용하는 방법

``` js
// 1. `Boolean` 생성자 함수를 `new` 연산자 없이 호출하는 방법
// 문자열 타입 > 불리언 타입
Boolean('x');           // true
Boolean('');            // false
Boolean('false');       // true

// 숫자 타입 > 불리언 타입
Boolean(0);             // false
Boolean(1);             // true
Boolean(NaN);           // false
Boolean(Infinity);      // true

// null 타입 > 불리언 타입
Boolean(null);          // false

// undefined 타입 > 불리언 타입
Boolean(undefined);     // false

// 객체 타입 > 불리언 타입
Boolean({});            // true
Boolean([]);            // true

// 2. `!` 부정 논리 연산자를 두 번 사용하는 방법
// 문자열 타입 > 불리언 타입
!!'x';           // true
!!'';            // false
!!'false';       // true

// 숫자 타입 > 불리언 타입
!!0;             // false
!!1;             // true
!!NaN;           // false
!!Infinity;      // true

// null 타입 > 불리언 타입
!!null;          // false

// undefined 타입 > 불리언 타입
!!undefined;     // false

// 객체 타입 > 불리언 타입
!!{};            // true
!![];            // true
```

<br><br>

### 단축 평가
#### 논리 연산자를 사용한 단축 평가
``` js
'Cat' && 'Dog'  // "Dog"
```

논리곱(`&&`) 연산자는 두 개의 피연산자가 모두 `true`로 평가될 때 `true`를 반환합니다.  
논리곱 연산자는 좌항에서 우항으로 평가가 진행됩니다.  

첫 번째 피연산자 'Cat'은 `true`로 평가됩니다.  
하지만 이 시점까지는 위 표현식을 평가할 수 없습니다.  
두 번째 피연산자까지 평가해 보아야 위 표현식을 평가할 수 있습니다.  
즉, 두 번째 피연산자가 위 논리곱 연산자 표현식의 평가 결과를 결정합니다. 

<font color='orange'>논리곱 연산자는 결과를 결정하는 두 번째 피연산자, 즉 문자열 'Dog'를 그대로 반환합니다. </font>  
논리합(`||`) 연산자도 논리곱(`&&`) 연산자와 동일하게 동작합니다.  

<br>

``` js
'Cat' || 'Dog'      // 'Cat'
```

논리합(`||`) 연산자는 2개의 피연산자 중 하나만 `true`로 평가되어도 `true`를 반환합니다.  

<br>

이처럼 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환합니다.  
이를 단축 평가(short-circuit evaluation)이라 합니다.  

> 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 의미합니다.

<br>

단축 평가는 다음 규칙을 따릅니다.

|단축 평가 표현식|평가 결과|
|:--|:--|
|true \|\| anything|true|
|false \|\| anything|anything|
|true && anything| anything|
|false && anything|false|


<br><br>

단축 평가를 사용하면 `if` 문을 대체할 수 있습니다.  
<font color='orange'>어떤 조건이 참으로 평가되는 값일 때 무언가를 해야 한다면 논리곱(`&&`) 연산자 표현식으로 `if` 문을 대체할 수 있습니다.</font>  

``` js
let done = true;
let message = '';

// 주어진 조건이 false일 때
if (done) message = '완료';

// if 문은 단축 평가로 대체 가능
// done이 true라면 message에 '완료'를 할당
message = done && '완료';
console.log(message);       // 완료

```

<br>

<font color='orange'>조건이 거짓으로 평가되는 값일 때 무언가를 해야 한다면 논리합(`||`) 연산자 표현식으로 `if` 문을 대체할 수 있습니다.</font>

``` js
let done = false;
let message = '';

// 주어진 조건이 false일 때
if (!done) message = '미완료';

// if 문은 단축 평가로 대체 가능
// done이 false라면 message에 '미완료'를 할당
message = done || '미완료';
console.log(message);       // 미완료
```


<br><br>

단축 평가는 다음과 같은 상황에서 유용하게 사용될 수 있습니다.
- 객체를 가리키기를 기대하는 변수가 `null` 혹은 `undefined`가 아닌지 확인하고 프로퍼티를 참조할 때
- 함수 매개변수에 기본값을 설정할 때

<br>

##### 객체를 가리키기를 기대하는 변수가 `null` 혹은 `undefined`가 아닌지 확인하고 프로퍼티를 참조할 때
객체를 가리키리르 기대하는 변수의 값이 객체가 아니라 `null` 또는 `undefined`일 경우  
객체의 프로퍼티를 참조하면 타입 에러가 발생합니다.  

``` js
let elem = null;
let value = elem.value;         // TypeError: Cannot read property 'value' of null
```

이때 단축 평가를 사용하면 에러를 발생시키지 않는다.

``` js
let elem = null;
let value = elem && elem.value;     // null
```

<br>

##### 함수 매개변수에 기본값을 설정할 때
<font color='orange'>함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 `undefined`가 할당됩니다.</font>  
이때 단축 평가를 사용해 매개변수의 기본값을 설정하면 `undefined`로 인해 발생할 수 있는 에러를 방지할 수 있다.

``` js
// 단축 평가를 위한 매개변수의 기본값 설정
function getStringLength(str) {
    str = str || '';
    return str.length;
}

// ES6의 매개변수 기본값 설정
function getSTringLength(str = '') {
    return str.legnth;
}
```

<br><br>

#### 옵셔널 체이닝 연산자
ES11(ECMAScript2020)에서 도입된 옵셔널 체이닝(optional chaining) 연산자 `?.`는   
좌항의 피연산자가 `null` 또는 `undefined`인 경우 `undefined`를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어갑니다.

``` js
let elem = null;

// elem이 null 혹은 undefined이면 undefined를 반환, 그렇지 않으면 우항의 프로퍼티 참조를 이어감.
let value = elem?.value;
console.log(value);     // undefined
```

옵셔널 체이닝 연산자 `?.`는 객체를 가리키기를 기대하는 변수가 `null` 또는 `undefined`가 아닌지 확인하고 프로퍼티를 참조할 때 유용합니다.  
옵셔널 체이닝 연산자가 도입되기 이전에는   
아래와 같이 논리 연산자 `&&`를 사용한 단축 평가를 통해 변수가 `null` 또는 `undefined`인지 확인했습니다.

``` js
let elem = null;

let value = elem && elem.value;
console.log(length);
```

<br>

하지만 아래와 같은 경우에는 빈 문자열의 경우에는 객체로 판단하지 않는다는 문제점이 있다.
``` js
let string = '';

let length = string && string.length;
console.log(length);    // ''   
```

<br>

하지만 옵셔널 체이닝 연산자는 좌항 피연산자가 false로 평가되는 값 (`false`, `undefined`, `null`, `0`, `-0`, `NaN`, '')이라도  
`null` 또는 `undefined`가 아니면 우항의 프로퍼티 참조를 이어갑니다.  

``` js
let str = '';
let length = str?.length;
console.log(length);        // 0
```


<br><br>

#### null 병합 연산자
ES11(ECMAScript2020)에서 도입된 <font color='orange'>null 병합 연산자 `??`는  
좌항의 피연산자가 `null` 또는 `undefined`인 경우 우항의 피연산자를 반환하고,  
그렇지 않으면 좌항의 피연산자를 반환합니다.</font>  
null 병합 연산자 `??`는 변수에 기본값을 설정할 때 유용합니다.  

``` js

// 좌항의 피연산자가 null 또는 undefined이면 우항의 피연산자를 반한,
// 그렇지 않으면 좌항의 피연산자를 반환한다.
let foo = null ?? 'default string';
console.log(foo);       // "default string"
```

null 병합 연산자 `??`가 도입되기 이전에는 논리 연산자 `||`를 사용한 단축 평가를 통해 변수에 기본값을 설정했습니다.  
논리 연산자 `||`를 사용한 단축 평가의 경우  
좌항의 피연산자가 `false`로 평가되는 값 (`false`, `undefined`, `null`, `0`, `-0`, `NaN`, '')이면  
우항의 피연산자를 반환합니다.  

만약 `0`이나 ''도 기본값으로서 유효할 경우 예기치 않은 오류가 발생할 수도 있습니다.

``` js
// `0`이나 ''도 기본값으로서 유효할 경우 예기치 않은 오류가 발생할 수도 있습니다.
let foo = '' || 'default string';
console.log(foo);       // "default string"
```

<br>

하지만 null 병합 연산자 `??`는 좌항의 피연산자가 `false`로 평가되는 값 (`false`, `undefined`, `null`, `0`, `-0`, `NaN`, '')이라도 `null` 또는 `undefined`가 아니면 좌항의 피연산자를 그대로 반환합니다.

``` js

// 좌항의 피연산자가 null 또는 undefined이면 우항의 피연산자를 반한,
// 그렇지 않으면 좌항의 피연산자를 반환한다.
let foo = '' ?? 'default string';
console.log(foo);       // ""
```