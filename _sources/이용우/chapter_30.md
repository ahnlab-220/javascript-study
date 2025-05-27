### Chapter 30 - Date

`Date` 객체는 날짜와 시간을 위한 정보를 제공한다.

<br>

#### Date 생성자 함수

`Date` 생성자 함수는 현재 날짜와 시간을 가지는 `Date` 객체를 반환한다.

``` js
const today = new Date();
console.log(today);     // Thu May 15 2025 22:58:35 GMT+0900 (한국 표준시)

```

<br>

#### new Date()

Date 생성자 함수를 인수 없이 `new` 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 `Date` 객체를 반환한다.

``` js
const today = new Date();
console.log(today);     // Thu May 15 2025 22:58:35 GMT+0900 (한국 표준시)

```

<br>

Date 생성자 함수를 new 연산자 없이 호출하면 인수를 전달하지 않아도 현재 날짜와 시간을 나타내는 `Date` 객체를 반환한다.

``` js
const today = Date();
console.log(today);     // Thu May 15 2025 22:58:54 GMT+0900 (한국 표준시)

```

<br>

#### new Date(milliseconds)

`Date` 생성자 함수에 숫자 타입의 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 가지는 `Date` 객체를 반환한다.

``` js
// 한국 표준시 KST는 협정 세계시 UTC에 9시간 더한 시간이다.
const date = new Date(0);
console.log(date);     // Thu Jan 01 1970 09:00:00 GMT+0900 (한국 표준시)

/*
86400000은 1일을 나타내는 밀리초이다.
1s = 1000ms
1m = 60s * 1000ms = 60000ms
1h = 60m * 60000ms = 3600000ms
1d = 24h * 3600000ms = 86400000ms
*/
new Date(86400000);
console.log(date);     // Fri Jan 02 1970 09:00:00 GMT+0900 (한국 표준시)
```

<br>


#### new Date(dateString)

`Date` 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 가지는 `Date` 객체를 반환한다.

``` js
const date = new Date('May 15, 2025 22:58:35');
console.log(date);     // Thu May 15 2025 22:58:35 GMT+0900 (한국 표준시)
```

<br>

#### new Date(year, month, day, hour, minute, second, millisecond)

`Date` 생성자 함수에 연도, 월, 일, 시, 분, 초, 밀리초를 나타내는 숫자 타입의 인수를 전달하면 지정된 날짜와 시간을 가지는 `Date` 객체를 반환한다.  
지정하지 않은 인수는 0으로 초기화된다.

``` js
const date = new Date(2025, 4, 15, 22, 58, 35, 0);
console.log(date);     // Thu May 15 2025 22:58:35 GMT+0900 (한국 표준시)
```

<br><br>

### Date 메서드

#### Date.now

`Date.now` 메서드는 1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 **밀리초**를 숫자 타입의 반환한다.

``` js
const now = Date.now();
console.log(now);     // 1715779115000
```

<br>

#### Date.parse

`Date.parse` 메서드는 인수로 전달된 날짜와 시간을 나타내는 문자열을 분석하여  
1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

``` js
// UTC
Date.parse('Jan 2, 1970 00:00:00 UTC'); // 86400000

// KST
Date.parse('Jan 2, 1970 09:00:00'); // 86400000

// KST
Date.parse('1970/01/02/09:00:00'); // 86400000
```

<br>

#### Date.UTC

`Date.UTC` 메서드는 인수로 전달된 연도, 월, 일, 시, 분, 초, 밀리초를 1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

``` js
// UTC
Date.UTC(1970, 0, 1); // 86400000

// KST
Date.UTC(1970, 0, 1, 9); // 86400000

Date.UTC('1970/1/2'); // NaN
```

<br>

#### Date.prototype.getFullYear

`getFullYear` 메서드는 년도를 반환한다.

``` js
const date = new Date('2025/05/15');
console.log(date.getFullYear()); // 2025
```

<br>

#### Date.prototype.setFullYear

`setFullYear` 메서드는 `Date` 객체에 연도를 나타내는 정수를 설정한다.  
연도 이외에 옵션으로 월, 일도 설정 가능하다.

``` js
const today = new Date();

// 연도 설정
today.setFullYear(2000);
console.log(today);     // Thu May 15 2000 22:58:35 GMT+0900 (한국 표준시)

// 연도, 월 설정
today.setFullYear(2000, 0, 1);
console.log(today);     // Sat Jan 01 2000 22:58:35 GMT+0900 (한국 표준시)

// 연도, 월, 일, 시, 분, 초, 밀리초 설정
today.setFullYear(2000, 0, 1, 9, 0, 0, 0);
console.log(today);     // Sat Jan 01 2000 09:00:00 GMT+0900 (한국 표준시)
```

<br>

#### Date.prototype.getMonth

`getMonth` 메서드는 월을 나타내는 0 ~ 11의 정수를 반환한다.

``` js
const date = new Date('2025/05/15');
console.log(date.getMonth());  // 4
```

<br>

#### Date.prototype.setMonth

`setMonth` 메서드는 `Date` 객체에 월을 나타내는 정수를 설정한다.  
월 이외에 옵션으로 일, 시, 분, 초, 밀리초도 설정 가능하다.

``` js
const today = new Date();

// 월 설정
today.setMonth(0);
console.log(today);     // Thu Jan 15 2025 22:58:35 GMT+0900 (한국 표준시)
```

<br>

#### Date.prototype.getTime

`getTime` 메서드는 1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

``` js
const date = new Date();
console.log(date.getTime()); // 1715779115000
```

<br>

#### Date.prototype.setTime

`setTime` 메서드는 `Date` 객체에 1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 설정한다.

``` js
const date = new Date();

date.setTime(86400000);  // 86400000은 1일을 나타내는 밀리초이다.
console.log(date);     // Thu Jan 02 1970 09:00:00 GMT+0900 (한국 표준시)
```

<br>

#### Date.prototype.getTimezoneOffset

`getTimezoneOffset` 메서드는 현재 시간과 UTC의 차이를 분 단위로 반환한다.  

KST는 UTC에 9시간을 더한 시간이다. (UTC = KST - 9시간)

``` js
const date = new Date();
console.log(date.getTimezoneOffset() / 60); // -9
```

<br>

#### Date.prototype.toDateString

`toDateString` 메서드는 년, 월, 일을 문자열로 반환한다.

``` js
const date = new Date();
console.log(date.toDateString()); // Thu May 15 2025
```

<br>

#### Date.prototype.toTimeString

`toTimeString` 메서드는 사람이 읽을 수 있는 형식으로 시, 분, 초를 문자열로 반환한다.

``` js
const date = new Date();
console.log(date.toTimeString()); // 22:58:35 GMT+0900 (한국 표준시)
```

<br>

#### Date.prototype.toISOString

`toISOString` 메서드는 날짜를 ISO 8601 형식의 문자열로 반환한다.

``` js
const date = new Date();
console.log(date.toISOString()); // 2025-05-15T13:58:35.000Z

// 활용
today.toISOString().slice(0, 10); // 2025-05-15
today.toISOString().slice(0, 10).replace(/-/g, ''); // 20250515
```

<br>

#### Date.prototype.toLocaleString

`toLocaleString` 메서드는 인수로 전달한 Locale을 기준으로 `Date` 객체의 **날짜와 시간**을 표현한 문자열을 반환한다.  
인수를 생략한 경우 브라우저가 동작 중인 시스템으 locale을 적용한다.

``` js
const date = new Date();
console.log(date.toLocaleString()); // 2025/5/15 오후 11:58:35
console.log(date.toLocaleString('ko-KR')); // 2025/5/15 오후 11:58:35
console.log(date.toLocaleString('en-US')); // 5/15/2025 11:58:35 AM
```

<br>

#### Date.prototype.toLocaleTimeString

`toLocaleTimeString` 메서드는 인수로 전달한 Locale을 기준으로 `Date` 객체의 **시간**을 표현한 문자열을 반환한다.  
인수를 생략한 경우 브라우저가 동작 중인 시스템의 locale을 적용한다.


``` js
const date = new Date();
console.log(date.toLocaleTimeString()); // 22:58:35 GMT+0900 (한국 표준시)
console.log(date.toLocaleTimeString('ko-KR')); // 오후 11:58:35
console.log(date.toLocaleTimeString('en-US')); // 11:58:35 PM
```

<br>


끝.


