### Chapter 29 - Math

`Math` 표준 빌트인 객체는 수학적인 상수와 함수를 위한 프로퍼티와 메서드를 제공한다.

<br><br>

### Math 프로퍼티

#### Math.PI

`Math.PI`는 원주율을 나타낸다.

``` js
console.log(Math.PI); // 3.141592653589793
```

<br><br>

### Math 메서드

#### Math.abs

`Math.abs`는 인수로 전달된 숫자의 절대값을 반환한다.

``` js
console.log(Math.abs(-1)); // 1
console.log(Math.abs('-1')); // 1
console.log(Math.abs('')); // 0
console.log(Math.abs([])); // 0
console.log(Math.abs(null)); // 0
console.log(Math.abs(undefined)); // NaN
console.log(Math.abs({})); // NaN
console.log(Math.abs('string')); // NaN
console.log(Math.abs()); // NaN
```


<br>

#### Math.round

`Math.round`는 인수로 전달된 숫자의 소수점 이하를 반올림한 정수를 반환한다.

``` js
console.log(Math.round(1.4)); // 1
console.log(Math.round(1.6)); // 2
console.log(Math.round(1)); // 1
console.log(Math.round(0)); // 0
console.log(Math.round(-1.4)); // -1
console.log(Math.round(-1.6)); // -2
console.log(Math.round(-1)); // -1

```

<br>

#### Math.ceil

`Math.ceil`은 인수로 전달된 숫자의 소수점 이하를 올림한 정수를 반환한다.

``` js
console.log(Math.ceil(1.4)); // 2
console.log(Math.ceil(1.6)); // 2
console.log(Math.ceil(1)); // 1
console.log(Math.ceil(0)); // 0
console.log(Math.ceil(-1.4)); // -1

```

<br>

#### Math.floor

`Math.floor`는 인수로 전달된 숫자의 소수점 이하를 내림한 정수를 반환한다.

``` js
console.log(Math.floor(1.9)); // 1
console.log(Math.floor(1.1)); // 1
console.log(Math.floor(1)); // 1
console.log(Math.floor(0)); // 0
console.log(Math.floor(-1.9)); // -2

```

<br>

#### Math.sqrt

`Math.sqrt`는 인수로 전달된 숫자의 제곱근을 반환한다.

``` js
console.log(Math.sqrt(9)); // 3
console.log(Math.sqrt(-9)); // NaN
console.log(Math.sqrt(2)); // 1.4142135623730951
console.log(Math.sqrt(1)); // 1
console.log(Math.sqrt(0)); // 0

```

<br>

#### Math.random

`Math.random`은 0 이상 1 미만의 부동소수점 난수를 반환한다.

``` js
console.log(Math.random()); // 0 이상 1 미만의 부동소수점 난수

// 1부터 10 이하의 정수 난수를 반환
console.log(Math.floor(Math.random() * 10) + 1);
```

<br>

#### Math.max

`Math.max`는 전달받은 인수 중 가장 큰 수를 반환한다.

``` js
console.log(Math.max(1, 2, 3)); // 3
```

배열을 인수로 전달받아 배열의 요소 중 최대값을 구하려면 `Function.prototype.apply` 메소드 또는 스프레드 문법을 사용해야 한다.

``` js
// 배열 요소 중 최대값 획득
Math.max.apply(null, [1, 2, 3]); // 3

// ES6 스프레드 문법
Math.max(...[1, 2, 3]); // 3
```

<br>

#### Math.min

`Math.min`은 전달받은 인수 중 가장 작은 수를 반환한다.

``` js
console.log(Math.min(1, 2, 3)); // 1
```

배열을 인수로 전달받아 배열의 요소 중 최소값을 구하려면 `Function.prototype.apply` 메서드 또는 스프레드 문법을 사용해야 한다.

``` js
// 배열 요소 중 최소값 획득
Math.min.apply(null, [1, 2, 3]); // 1

// ES6 스프레드 문법
Math.min(...[1, 2, 3]); // 1
```




