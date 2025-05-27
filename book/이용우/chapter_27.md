### Chapter 27 - 배열

배열은 여러 개의 값을 순차적으로 나열한 자료구조이다.

``` js
const arr = ['apple', 'banana', 'orange'];
```

- 요소(element): 배열이 가지고 있는 값
- 인덱스(index): 요소의 위치

<br>

배열은 일반 객체와는 구별되는 특징이 있다.

|구분|객체|배열|
|:--|:--|:--|
|구조|프로퍼티 키와 프로퍼티 값|인덱스와 요소|
|값의 참조|프로퍼티 키|인덱스|
|값의 순서|X|O|
|length 프로퍼티|X|O|

<br>

배열은 처음부터 순차적으로, 또는 역순으로 요소에 접근할 수 있다.

``` js
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);
}
```

<br>

#### 자바스크립트의 배열은 배열이 아님
자료구조에서 말하는 배열의 요소는 하나의 데이터 타입으로 통일되어 있으며,  
서로 연속적으로 인접한다. (= 밀집 배열, dense array)

<img width="350" src="https://github.com/user-attachments/assets/0f74cc09-1114-4a20-b0aa-33f25be944ca" />

인덱스를 통해 O(1) 시간복잡도로 빠르게 특정 요소에 접근할 수 있다.

<br>

자바스크립트의 배열은 배열의 요소가 연속적으로 이뤄지지 않는다. (= 희소 배열, sparse array)  
> 자바스크립트의 배열은 배열의 동작을 흉내낸 특수 객체이다.

``` js
console.log(Object.getOwnPropertyDescriptors([1, 2, 3]));
```

<img width="492" src="https://github.com/user-attachments/assets/7eefdb1e-5254-4f20-b251-6ddc450e22b9" />

- 인덱스를 나타내는 문자열을 프로퍼티 키로 가진다.
- length 프로퍼티를 가지는 특수한 객체이다.

즉, 자바스크립트에서 사용할 수 있는 모든 값은 객체의 프로퍼티 값이 될 수 있기 때문에  
타입에 상관없이 배열의 요소가 될 수 있다.

<br>

일반 배열과 자바스크립트 배열 간 차이점은 다음과 같다.
- 일반적인 배열: 인덱스로 요소에 빠르게 접근 가능. 단, 요소를 삽입 또는 삭제하는 경우 효율적이지 않음
- 자바스크립트 배열: 해시 테이블로 구현되어 있어 인덱스 접근은 일반적인 배열보다 성능적인 면에서 느릴 수 있음. 단, 요소를 삽입 또는 삭제하는 경우에는 빠름.


<br><br>

### 배열 생성
가장 일반적이고 간편한 배열 생성 방식은 배열 리터럴을 사용하는 것이다.

``` js
const arr = [1, 2, 3];
console.log(arr.length);    // 3
```

<br>

배열 리터럴에 요소를 생략하면 희소 배열이 생성된다.

``` js
const arr = [1, , 3];   //  [1, empty, 3]
console.log(arr[1]);    // undefined
```

`arr[1]`에 프로퍼티가 존재하지 않기 때문에 `undefined` 출력됨


<br>

`Array` 생성자 함수를 사용해서도 배열을 만들 수 있다.  
생성자 함수의 인자로 어떤 값을 주느냐에 따라 생성되는 배열이 달라진다.

전달된 인수가 1개이고 숫자인 경우 넘겨준 인수의 크기를 가진 배열 생성
``` js
const arr = new Array(10);

console.log(arr);   // [empty * 10]
console.log(arr.length);    // 10
```

<br>

전달된 인수가 없는 경우 빈 배열 생성
``` js
new Array();    // []
```

<br>

전달된 인수가 2개 이상이거나 숫자가 아닌 경우 인수를 요소로 갖는 배열을 생성
``` js
// 전달된 인수가 2개 이상
new Array(1, 2, 3); // [1, 2, 3]

// 전달된 인수가 1개지만 숫자가 아닐 경우
new Array('a');     // ['a']
```

<br>

ES6부터 도입된 `Array.of` 메소드는 전달된 인수를 요소로 갖는 배열을 생성한다.

``` js
Array.of(1);            // [1]
Array.of(1, 2, 3);      // [1, 2, 3]
Array.of('a')           // ['a']
```

<br>

ES6에서 도입된 `Array.from` 메소드는 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아  
배열로 변환하여 리턴한다.

``` js
Array.from({length: 2, 0: 'a', 1: 'b'});        // ['a', 'b']
Array.from('Hello');        // ['H', 'e', 'l', 'l', 'o']
```

<br><br>

### 배열 요소의 추가와 갱신
배열에 요소를 동적으로 추가할 수 있다.  

존재하지 않은 인덱스를 사용해 값을 할당하면 새로운 요소가 추가된다.
``` js
const arr = [0];

arr[1] = 1;
console.log(arr);       // [0, 1]
```

<br>

현재 배열의 length 프로퍼티 값보다 큰 인덱스로 새로운 요소를 추가하면 희소 배열이 된다.

``` js
const arr = [0];
arr[100] = 100;

console.log(arr);       // [0, empty × 99, 100]
```

<br>

이미 요소가 존재할 경우 요소에 값을 재할당하여 갱신 가능하다.
``` js
const arr = [0];
arr[0] = 5;

console.log(arr);       // [5]
```

<br>

만약 정수 이외의 값을 인덱스로 사용할 시 프로퍼티가 된다.

``` js
const arr = [0];

// 배열 요소 추가
arr[0] = 1;
arr['1'] = 2;

// 프로퍼티 추가
arr['foo'] = 3;
arr[1.1] = 4;

console.log(arr);       // [1, 2, foo: 3, 1.1: 4]

```

<br><br>

### 배열 요소의 삭제
배열의 특정 요소 삭제를 위해 `delete` 연산자를 쓸 수 있다.  
> 단지 empty로 바꿔주는 것이기 때문에 `length`에 영향을 주지 않는다.

``` js
const arr = [1, 2, 3];

delete arr[1];
console.log(arr);       // [1, empty, 3]

console.log(arr.length);    // 3
```

이 방식은 length에 영향을 주지 않기 때문에 비추천한다.  
희소 배열을 만들지 않으면서 특정 요소를 완전히 삭제하려면 `Array.prototype.splice` 메소드를 사용하면 된다.

``` js
const arr = [1, 2, 3];

// arr[1] 부터 '1'개의 요소를 제거
arr.splice(1, 1);
console.log(arr);   // [1, 3]

console.log(arr.length);    // 2
```

<br><br>

### 배열 메소드
배열 메소드는 결과 반환 패턴이 2가지이다.
- 원본 배열(배열 메소드를 호출한 배열)을 직접 변경하는 메소드
- 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 리턴하는 메소드

원본 배열을 직접 변경하는 메소드는 외부 상태를 직접 변경하는 Side Effect가 있으므로 주의해야 한다. 
> 가급적 새로운 배열을 생성하여 리턴하는 메소드 사용을 추천한다.

<br>

#### Array.isArray
전달된 인수가 배열일 경우 `true`를 반환한다.
``` js
Array.isArray([]);  // true
```

<br>

#### Array.prototype.indexOf
`indexOf` 메소드는 원본 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환한다.

``` js
const arr = [1, 2, 2, 3];

// arr에서 요소 2를 검색한 뒤 가장 첫 번째로 검색된 요소의 index를 반환
arr.indexOf(2);     // 1

// 요소가 없을 경우 -1을 반환
arr.indexOf(4);     // -1

// 두 번째 인수 지정 시 시작 index를 지정할 수 있다.
arr.indexOf(2, 2);  // 2
```

<br>

`indexOf` 메소드를 배열에 특정 요소가 존재하는지 확인할 때 사용할 수는 있으나  
이 경우에는 ES7의 `Array.prototype.includes` 메소드를 사용하는 것이 가독성이 더 좋다.

``` js
const foods = ['apple', 'banana', 'orange'];

// ES7 이전
if (foods.indexOf('orange') === -1) {
    foods.push('orange');
}

// ES7
if (!foods.includes('orange')) {
    // foods 배열에 'orange'가 존재하지 않을 시 'orange' 요소를 추가한다.
    foods.push('orange');
}
```

<br>

#### Array.prototype.push
인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가한다.  
원본 배열을 직접 변경하며, 원본 배열을 리턴한다.

``` js
const arr = [1, 2];

// 마지막 요소 추가
arr.push(3);
console.log(arr);   // [1, 2, 3]

// 여러 개의 요소 추가
arr.push(4, 5);
console.log(arr);   // [1, 2, 3, 4, 5]
```

<br>

`push` 메소드는 성능이 좋지 않다.  
→ `length` 프로퍼티를 사용하는 것이 더 빠르다고 함

``` js
const arr = [1, 2];

arr[arr.length] = 3;
console.log(arr);   // [1, 2, 3]
```


<br>

`push` 메소드는 원본 배열을 직접 변경한다.  
→ ES6의 **스프레드 문법**을 사용하는 것이 좋다고 함

``` js
const arr = [1, 2];
const newArr = [...arr, 3];

console.log(newArr);   // [1, 2, 3]
```


<br>

#### Array.prototype.pop
원본 배열의 마지막 요소를 제거한다.  
원본 배열을 직접 변경하며, 제거한 요소를 리턴한다.

``` js
const arr = [1, 2, 3];  

// 마지막 요소 제거
arr.pop();
console.log(arr);   // [1, 2]

// 제거한 요소 리턴
arr.pop();
console.log(arr);   // [1]
```

<br>

#### Array.prototype.unshift
인수로 전달받은 모든 값을 원본 배열의 선두에 추가하고  
`length` 프로퍼티 값을 반환한다.  
원본 배열을 직접 변경한다.

``` js
const arr = [1, 2];

// 선두에 요소 추가
let result = arr.unshift(0);
console.log(result);   // 3
console.log(arr);   // [0, 1, 2]

// 여러 개의 요소 추가
result = arr.unshift(-1, -2);
console.log(result);   // 5
console.log(arr);   // [-1, -2, 0, 1, 2]
```

<br>

이 또한 원본 배열을 직접 변경하기 때문에 부수 효과가 있다.  
→ ES6의 **스프레드 문법**을 사용하는 것이 좋다고 함

<br>

#### Array.prototype.shift
원본 배열의 첫 번째 요소를 제거하고 제거한 요소를 리턴한다.  
원본 배열을 직접 변경한다.

``` js
const arr = [1, 2];

// 첫 번째 요소 제거
let result = arr.shift();
console.log(result);   // 1
console.log(arr);   // [2]
```

> `shift` 메소드와 `push` 메소드를 활용하면 스택을 쉽게 구현할 수 있다.


<br>

#### Array.prototype.concat
인수로 전달된 값을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환한다.  
원본 배열을 직접 변경하지 않는다.

``` js
const arr = [1, 2];
const newArr = arr.concat(3, 4);

console.log(newArr);   // [1, 2, 3, 4]
console.log(arr);   // [1, 2]
```

<br>

`concat` 메소드는 ES6의 스프레드 문법으로 대체 가능하다.

``` js
let result = [1, 2].concat([3, 4]);
console.log(result);   // [1, 2, 3, 4]

result = [...[1, 2], ...[3, 4]];
console.log(result);   // [1, 2, 3, 4]
```


<br>

#### Array.prototype.splice
원본 배열의 특정 요소를 삭제하거나 교체하거나 추가할 수 있다.  
원본 배열을 직접 변경한다.

``` js
const arr = [1, 2, 3, 4];

// 인덱스 1의 요소부터 2개의 요소를 제거
arr.splice(1, 2);
console.log(arr);   // [1, 4]
```

<br>

``` js
const arr = [1, 2, 3, 4];

// 인덱스 1의 요소부터 2개의 요소를 제거하고 그 자리에 새로운 요소 추가
arr.splice(1, 2, 'a', 'b');
console.log(arr);   // [1, 'a', 'b', 4]
```

<br>

#### Array.prototype.slice
원본 배열의 일부를 복사하여 반환한다.  
원본 배열을 직접 변경하지 않는다.

``` js
const arr = [1, 2, 3, 4];

// 인덱스 1부터 인덱스 3 이전까지 복사
const result1 = arr.slice(1, 3);
console.log(result1);   // [2, 3]

// 인덱스 1부터 끝까지 복사
const result2 = arr.slice(1);
console.log(result2);   // [2, 3, 4]
```

<br>

#### Array.prototype.join
원본 배열의 모든 요소를 문자열로 변환한 후, 인수로 전달받은 문자열 또는 디폴트 값을 사이에 두어 결합한다.  
원본 배열을 직접 변경하지 않는다.

``` js
const arr = [1, 2, 3, 4];

// 기본 구분자는 쉼표임
let result = arr.join();
console.log(result);   // 1,2,3,4

// 구분자를 지정하면 지정된 구분자를 사이에 두어 결합함
result = arr.join(':');
console.log(result);   // 1:2:3:4
```

<br>

#### Array.prototype.reverse
원본 배열의 순서를 반대로 뒤집는다.  
원본 배열을 직접 변경한다.

``` js
const arr = [1, 2, 3, 4];

// 원본 배열의 순서를 반대로 뒤집는다.
arr.reverse();
console.log(arr);   // [4, 3, 2, 1]
```

<br>

#### Array.prototype.fill
인수로 전달받은 값을 원본 배열의 처음부터 끝까지 채워 넣는다.  
원본 배열을 직접 변경한다.

``` js
const arr = [1, 2, 3, 4];

// 인덱스 1부터 인덱스 3 이전까지 요소를 0으로 채움
arr.fill(0, 1, 3);
console.log(arr);   // [1, 0, 0, 4]

// 모두 0으로 채움
arr.fill(0);
console.log(arr);   // [0, 0, 0, 0]
```

<br>

#### Array.prototype.includes
배열 내에 특정 요소가 존재하는지 확인하여 그 결과를 불리언 값으로 리턴한다.  

``` js
const arr = [1, 2, 3, 4];

// 배열에 요소 2가 존재하는지 확인
arr.includes(2);
console.log(result);   // true

// 배열에 요소 2가 존재하는지 인덱스 1부터 확인
arr.includes(2, 1);
console.log(result);   // true
```

<br>

#### Array.prototype.flat
중첩 배열을 하나의 배열로 평탄화하여 반환한다.  
원본 배열을 직접 변경하지 않는다.

``` js
const arr = [1, [2, 3, 4], 5];

// 중첩 배열을 평탄화하여 반환
const result = arr.flat();
console.log(result);   // [1, 2, 3, 4, 5]
```


<br><br>

### 배열 고차 함수
#### Array.prototype.sort
배열의 요소를 정렬한다.  
원본 배열을 직접 변경한다.

``` js
const fruits = ['banana', 'apple', 'orange', 'orange', 'apple'];

// 문자열 배열을 정렬할 때는 기본적으로 유니코드 순서로 정렬된다.
fruits.sort();
console.log(fruits);   // ['apple', 'apple', 'banana', 'orange', 'orange']

// 내림차순 정렬
fruits.sort().reverse();
console.log(fruits);   // ['orange', 'orange', 'banana', 'apple', 'apple']

// 숫자 배열을 정렬할 때는 기본적으로 유니코드 순서로 정렬된다.
const numbers = [4, 2, 3, 1, 5];
numbers.sort();
console.log(numbers);   // [1, 2, 3, 4, 5]
```

<br>

`sort` 메소드의 인자로 비교 함수를 전달할 수 있으며  
**리턴값이 0보다 작으면 첫 번째 인수를 우선하여 정렬하고**  
**리턴값이 0보다 크면 두 번째 인수를 우선하여 정렬한다.**

``` js
const numbers = [100, 10, 50, 30, 200];

// 숫자 배열을 정렬할 때는 비교 함수를 매개변수로 전달해야 한다.
numbers.sort((a, b) => a - b);
console.log(numbers);   // [10, 30, 50, 100, 200]

numbers.sort((a, b) => b - a);
console.log(numbers);   // [200, 100, 50, 30, 10]
```



<br>

#### Array.prototype.forEach
배열의 각 요소에 대해 주어진 함수를 실행한다.  
원본 배열을 직접 변경하지 않는다.

``` js
const numbers = [1, 2, 3, 4, 5];

// 배열의 각 요소에 대해 주어진 함수를 실행
numbers.forEach(item => console.log(item));
// 1
// 2
// 3
// 4

```

`forEach` 메소드는 콜백 함수를 호출하면서 3개의 인수를 리턴한다.

``` js
[1, 2, 3].forEach((item, index, arr) => {
    console.log(`요소값: ${item}, 요소인덱스: ${index}, 배열: ${arr}`);
});
// 요소값: 1, 요소인덱스: 0, 배열: [1,2,3]
// 요소값: 2, 요소인덱스: 1, 배열: [1,2,3]
// 요소값: 3, 요소인덱스: 2, 배열: [1,2,3]
```

<br>

#### Array.prototype.map
배열의 각 요소에 대해 주어진 함수를 실행하고 그 결과를 모아서 새로운 배열을 반환한다.  
원본 배열을 직접 변경하지 않는다.

``` js
const numbers = [1, 2, 3, 4, 5];

// 배열의 각 요소에 대해 주어진 함수를 실행하고 그 결과를 모아서 새로운 배열을 반환
const result = numbers.map(item => item * 2);
console.log(result);   // [2, 4, 6, 8, 10]
```

- 단순히 배열 반복하면서 각 요소로 무언가를 하고 싶을 때 (`forEach`)
- 배열의 각 요소를 변환해서 새로운 배열을 만들고 싶을 때 (`map`)


<br>

#### Array.prototype.filter
원본 배열의 일부 요소를 필터링하여 새로운 배열을 반환한다.  
원본 배열을 직접 변경하지 않는다.

``` js
const numbers = [1, 2, 3, 4, 5];

// 배열의 각 요소에 대해 주어진 함수를 실행하고 그 결과를 모아서 새로운 배열을 반환
// 콜백 함수의 결과가 true인 요소만 추출함
const result = numbers.filter(item => item % 2);
console.log(result);   // [1, 3, 5]
```

<br>

유사한 메소드로 `find`가 있다.  
`find` 메소드는 콜백 함수의 반환값이 true인 첫 번째 요소를 반환한다.  

또한 `findIndex` 메소드가 있는데,  
`find` 메소드와 동일하게 동작하지만 콜백 함수의 반환값이 true인 첫 번째 요소의 인덱스를 리턴한다.  

<br>

#### Array.prototype.reduce
원본 배열의 요소를 하나로 합치고 리턴한다.  
원본 배열을 직접 변경하지 않는다.


``` js
const numbers = [1, 2, 3, 4, 5];

// 콜백 함수의 결과를 누적한다.
// acc: 누적 값, cur: 현재 요소
const result = numbers.reduce((acc, cur) => acc + cur, 0);
console.log(result);   // 15
```

<br>

콜백 함수는 최대 4개의 인수를 가질 수 있다.
- 첫 번째 인수: 누적 값
- 두 번째 인수: 현재 요소
- 세 번째 인수: 현재 요소의 인덱스
- 네 번째 인수: 원본 배열
``` js
const numbers = [1, 2, 3, 4, 5];

// 콜백 함수는 최대 4개의 인수를 가질 수 있다.
// 첫 번째 인수: 누적 값 (accumulator)
// 두 번째 인수: 현재 요소 (current item)
// 세 번째 인수: 현재 요소의 인덱스 (current index)
// 네 번째 인수: 원본 배열 (source array)
const result = numbers.reduce((acc, cur, index, array) => {
    console.log(`누적값: ${acc}, 현재요소: ${cur}, 인덱스: ${index}, 배열: ${array}`);
    return acc + cur;
}, 0);
// 누적값: 0, 현재요소: 1, 인덱스: 0, 배열: [1,2,3,4,5]
// 누적값: 1, 현재요소: 2, 인덱스: 1, 배열: [1,2,3,4,5]
// 누적값: 3, 현재요소: 3, 인덱스: 2, 배열: [1,2,3,4,5]
// 누적값: 6, 현재요소: 4, 인덱스: 3, 배열: [1,2,3,4,5]
// 누적값: 10, 현재요소: 5, 인덱스: 4, 배열: [1,2,3,4,5]
console.log(result);   // 15
```

끝