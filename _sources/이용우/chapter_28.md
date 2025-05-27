### Chapter 28 - Number

`Number` 표준 빌트인 객체는 숫자 타입을 다룰 때 유용한 메드ㄹ 제공한다.

<br><br>

### Numbser 프로퍼티
#### Number.EPSILON

`Number.EPSILON`은 1과 1보다 큰 숫자 중 가장 작은 숫자의 차이를 의미한다.

```js
console.log(Number.EPSILON); // 2.220446049250313e-16
```

<br>

정수는 2진법으로 오차 없이 저장 가능하지만,  
부동소수점을 표현하기 위해 널리 쓰이는 표준인 IEEE 754는 2진법으로 변환했을 때  
무한소수가 되어 미세한 오차가 발생할 수 밖에 없다.

``` js
0.1 + 0.2; // 0.30000000000000004
0.1 + 0.2 === 0.3; // false
```

이런 부동소수점으로 발생하는 오차를 해결하기 위해 `Number.EPSILON`을 사용한다.

``` js
function isEqual(a, b) {
  // a와 b를 비교하여 차이가 Number.EPSILON보다 작으면 같은 수로 판단
  return Math.abs(a - b) < Number.EPSILON;
}

isEqual(0.1 + 0.2, 0.3); // true
```


<br><br>


#### Number.MAX_VALUE

`Number.MAX_VALUE`는 자바스크립트에서 표현할 수 있는 가장 큰 숫자를 나타낸다.

``` js
console.log(Number.MAX_VALUE); // 1.7976931348623157e+308
```

> `Number.MAX_VALUE`보다 큰 숫자는 `Infinity`이다.

<br>

#### Number.MIN_VALUE

`Number.MIN_VALUE`는 자바스크립트에서 표현할 수 있는 가장 작은 양수를 나타낸다.

``` js
console.log(Number.MIN_VALUE); // 5e-324
```

> `Number.MIN_VALUE`보다 작은 숫자는 `0`이다.

<br>

#### Number.NaN

`Number.NaN`은 `NaN`을 나타낸다.
> NaN(Not a Number)

``` js
console.log(Number.NaN); // NaN
```


<br><br>

### Number 메소드
#### Number.isFinite
인수로 전달된 숫자가 유한한지 확인하고 그 결과를 불리언 타입으로 반환한다.

``` js
Number.isFinite(0); // true
Number.isFinite(Number.MAX_VALUE); // true
Number.isFinite(Number.MIN_VALUE); // true
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
Number.isFinite(NaN); // false
Number.isFinite(undefined); // false
```

<br>

#### Number.isInteger
인수로 전달된 숫자가 정수인지 확인하고 그 결과를 불리언 타입으로 반환한다.

``` js
Number.isInteger(0); // true
Number.isInteger(123); // true

Number.isInteger('123'); // false
```

<br>

#### Number.isNaN
인수로 전달된 숫자가 `NaN`인지 확인하고 그 결과를 불리언 타입으로 반환한다.

``` js
Number.isNaN(NaN); // true
Number.isNaN(123); // false
Number.isNaN('123'); // false
Number.isNaN(undefined); // false
```

<br>

#### Number.prototype.toExponential
숫자를 **지수 표기법**으로 변환하여 문자열로 반환한다.

> 지수 표기법은 소수점 앞에 10의 거듭제곱을 표기하는 방법이다.
> 예) 1234.5678 -> 1.2345678e+3

``` js
const num = 1234.5678;

console.log(num.toExponential()); // 1.2345678e+3
console.log(num.toExponential(2)); // 1.23e+3
console.log(num.toExponential(4)); // 1.2346e+3
```

<br>

#### Number.prototype.toFixed
숫자를 반올림하여 문자열로 반환한다.

``` js
const num = 1234.5678;

// 인수 생략 시 기본값 0이 지정됨
console.log(num.toFixed()); // 1235

// 소수점 이하 자리수를 지정하여 반올림
console.log(num.toFixed(2)); // 1234.57
console.log(num.toFixed(4)); // 1234.5678
```

<br>

#### Number.prototype.toPrecision
숫자를 지정된 자리수까지 나타내어 문자열로 반환한다.

``` js
const num = 1234.5678;

// 인수 생략 시 기본값 0이 지정됨
console.log(num.toPrecision()); // 1234.5678

// 유효자리수를 지정하여 반올림
console.log(num.toPrecision(2)); // 1.2e+3 (1200)
console.log(num.toPrecision(4)); // 1235
```

<br>

#### Number.prototype.toString
숫자를 문자열로 변환하여 반환한다.

``` js
const num = 1234.5678;

// 10진수 문자열 반환
console.log(num.toString()); // 1234.5678

// 2진수 문자열 반환
console.log(num.toString(2)); // 10011010010.00010100011110101110

// 8진수 문자열 반환
console.log(num.toString(8)); // 2324.446365326547

// 16진수 문자열 반환
console.log(num.toString(16)); // 4d2.91edae147ae
```
