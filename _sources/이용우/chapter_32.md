### Chapter 32 - String

표준 빌트인 객체인 String은 원시 타입인 문자열을 다룰 때 유용한 프로퍼티와 메소드를 제공한다.

#### String 생성자 함수
String 객체는 생성자 함수 객체이다.  
따라서 `new` 연산자와 함께 호출하여 `String` 인스턴스를 생성할 수 있다.

`String` 생성자 함수에 인수를 전달하지 않고 `new` 연산자와 함께 호출하면 `[[StringData]]` 내부 슬롯에  
빈 문자열을 할당한 `String` 래퍼 객체를 생성한다.

``` js
const strObj = new String();
console.log(strObj); // String { length: 0, [[PrimitiveValue]]: '' }

const str = new String('Hello');
console.log(str); // String {0: "H", 1: "e", 2: "l", 3: "l", 4: "o", length: 5, [[PrimitiveValue]]: "Hello"}
```

<br>

### length 프로퍼티

length 프로퍼티는 문자열의 길이를 나타내는 0 이상의 정수를 반환한다.

``` js
const str = 'Hello';
console.log(str.length); // 5
```

<br><br>

### String 메소드

문자열은 변경 불가능(immutable)한 값이다.  
따라서 문자열의 일부를 변경하면 새로운 문자열을 생성하고 반환한다.

#### String.prototype.indexOf

indexOf 메소드는 주어진 문자열에서 찾고자 하는 문자열을 검색하여 찾은 위치를 반환한다.  

``` js
const str = 'Hello';
console.log(str.indexOf('e')); // 1
```

<br>

indexOf 메소드는 대상 문자열에 특정 문자열이 존재하는지 확인하는 데에도 사용할 수 있다.

``` js
const str = 'Hello';

console.log(str.indexOf('Hello') !== -1); // true

// ES6
console.log(str.includes('Hello')); // true
```

<br>

#### String.prototype.search

search 메소드는 대상 문자열에서 주어진 문자열을 검색하여 검색된 문자열의 인덱스를 반환한다.  

``` js
const str = 'Hello';
console.log(str.search('e')); // 1
```

<br>

정규식을 사용하여 검색할 수도 있다.

``` js
const str = 'Hello';
console.log(str.search(/e/)); // 1
```

<br>

#### String.prototype.includes

includes 메소드는 대상 문자열에 주어진 문자열이 포함되어 있는지 확인하여 그 결과를 불리언 값으로 반환한다.  

``` js
const str = 'Hello';
console.log(str.includes('e')); // true
```

<br>

#### String.prototype.startsWith

startsWith 메소드는 대상 문자열이 주어진 문자열로 시작하는지 확인하여 그 결과를 불리언 값으로 반환한다.  

``` js
const str = 'Hello';
console.log(str.startsWith('H')); // true
```

2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

``` js
const str = 'Hello';
console.log(str.startsWith('H', 1)); // false
```

<br>

#### String.prototype.endsWith

endsWith 메소드는 대상 문자열이 주어진 문자열로 끝나는지 확인하여 그 결과를 불리언 값으로 반환한다.  

``` js
const str = 'Hello';
console.log(str.endsWith('o')); // true
```

2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

``` js
const str = 'Hello';

// 문자열 str의 처음 5글자를 대상으로 검색하여 문자열 'Hello'로 끝나는지 확인한다.
console.log(str.endsWith('o', 5)); // true
```


<br>

#### String.prototype.charAt

charAt 메소드는 대상 문자열에서 인덱스에 해당하는 문자를 검색하여 반환한다.  

``` js
const str = 'Hello';
console.log(str.charAt(1)); // 'e'
```

범위를 벗어날 경우 빈 문자열을 반환한다.

<br>

#### String.prototype.substring

substring 메소드는 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에서  
두 번째 인수로 전달받은 인덱스 이전의 부분 문자열을 반환한다.  

``` js
const str = 'Hello';
console.log(str.substring(1, 4)); // 'ell'
```

<br>

#### String.prototype.slice

slice 메소드는 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에서  
두 번째 인수로 전달받은 인덱스 이전의 부분 문자열을 반환한다.  

``` js
const str = 'Hello';
console.log(str.slice(1, 4)); // 'ell'
```

`substring` 메소드와 동일하게 동작하지만  
`slice` 메소드에는 음수인 인수를 전달할 수 있다.

<br>

#### String.prototype.trim

trim 메소드는 대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다.  

``` js
const str = '   foo   ';
console.log(str.trim()); // 'foo'
```

<br>

#### String.prototype.replace

replace 메소드는 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규식을  
두 번째 인수로 전달받은 문자열로 치환한 문자열을 반환한다.  

``` js
const str = 'Hello';
console.log(str.replace('H', 'h')); // 'hello'
```

<br>

#### String.prototype.repeat

repeat 메소드는 대상 문자열을 인수로 전달받은 횟수만큼 반복 연결한 새로운 문자열을 반환한다.  

``` js
const str = 'Hello';
console.log(str.repeat(2)); // 'HelloHello'
```

<br>


#### String.prototype.split

split 메소드는 대상 문자열을 인수로 전달받은 구분자를 기준으로 잘라 배열을 반환한다.  

``` js
const str = 'hello world';

// 문자열 str을 공백 문자를 기준으로 잘라 배열을 반환한다.
console.log(str.split(' ')); // ['hello', 'world']
```



끝.