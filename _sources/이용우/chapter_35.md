### chapter 35 - 스프레드 문법
스프레드 문법은 하나로 뭉쳐 있는 여러 값들을 펼쳐서 개별 값의 목록으로 만든다.

``` js
// ...[1, 2, 3]은 [1, 2, 3]의 개별 요소로 분리한다 (1, 2, 3)
console.log(...[1, 2, 3]);    // 1 2 3

// ...{ a: 1, b: 2 }은 { a: 1, b: 2 }의 개별 프로퍼티로 분리한다 (a, b)
console.log(...{ a: 1, b: 2 });    // a b

// ...'Hello'은 'Hello'의 개별 문자로 분리한다 (H, e, l, l, o)
console.log(...'Hello');    // H e l l o
```

<br><br>

### 함수 호출문의 인수 목록에서 사용하는 경우
요소들의 집합인 배열을 펼쳐서 개별적인 값들의 목록으로 만든 후,  
이를 함수의 인수로 전달해야 하는 경우가 있다.

이때 스프레드 문법을 사용하면 배열을 펼쳐서 개별적인 값들의 목록으로 만든 후,  
이를 함수의 인수로 전달할 수 있다.

``` js
function foo(x, y, z) {
    console.log(x, y, z);
}

foo(...[1, 2, 3]);    // 1 2 3
```

<br><br>

### 배열 리터럴 내부에서 사용하는 경우
#### concat
기존에는 2개의 배열을 하나의 배열로 병합하고 싶을 때 `concat` 메소드를 사용했다.

``` js
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

const arr = arr1.concat(arr2);
console.log(arr);    // [1, 2, 3, 4, 5, 6]
```

<br>

스프레드 문법을 사용하면 간결하게 병합할 수 있다.

``` js
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

const arr = [...arr1, ...arr2];
console.log(arr);    // [1, 2, 3, 4, 5, 6]
```

<br><br>

#### splice
배열의 중간에 다른 배열의 요소를 추가하거나 제거하려면 `splice` 메소드를 사용했다.  
이때 splice 메소드의 세 번째 인수로 배열을 전달하면 배열 자체가 추가된다.


``` js
var arr1 = [1, 4];
var arr2 = [2, 3];

arr1.splice(1, 0, arr2);
console.log(arr1);    // [1, [2, 3], 4]
```

<br>

스프레드 문법을 사용하면 더 간결하게 작성 가능하다.

``` js
var arr1 = [1, 4];
var arr2 = [2, 3];

arr1.splice(1, 0, ...arr2);
console.log(arr1);    // [1, 2, 3, 4]
```


<br><br>

### 배열 복사
기존에는 배열 복사를 하려면 `slice` 메소드를 사용했다.

``` js
const arr = [1, 2, 3];

const copy = arr.slice();
console.log(copy);    // [1, 2, 3]
```

<br>

스프레드 문법을 사용하면 더 간결하게 작성 가능하다.

``` js
const arr = [1, 2, 3];

const copy = [...arr];
console.log(copy);    // [1, 2, 3]
```


<br><br>

### 객체 리터럴 내부에서 사용하는 경우

스프레드 문법은 일반 객체를 대상으로도 사용을 허용한다.

``` js
const obj = { a: 1, b: 2 };
const copy = { ...obj };
console.log(copy);    // { a: 1, b: 2 }


const merged = { ...obj, c: 3 };
console.log(merged);    // { a: 1, b: 2, c: 3 }
```

<br>

일반 객체를 대상으로도 사용 가능해서 활용성이 좋다.

``` js
// 객체 병합 (프로퍼티 중복 시, 뒤에 위치한 프로퍼티가 우선권을 갖는다)
const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };
console.log(merged);    // { x: 1, y: 10, z: 3 }

// 특정 프로퍼티 추가
const changed = { ...{ x: 1, y: 2 }, y: 100 };
console.log(changed);    // { x: 1, y: 100 }
```




