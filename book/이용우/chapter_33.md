### Chapter 33 - Symbol

자바스크립트에는 6개의 타입이 있다.
- 문자열
- 숫자
- 불리언
- `undefined`
- `null`
- 객체

심볼(`symbol`)은 ES6에서 도입된 7번째 데이터 타입으로, 변경 불가능한 원시 타입의 값이다.  
심볼 값은 다른 값과 중복되지 않는 유일한 값이다.  

따라서 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.

<br>

**프로퍼티 키로 사용할 수 있는 값**은  
빈 문자열을 포함하는 모든 <font color='orange'>문자열 또는 심볼 값이다.</font>

<br><br>

### Symbol 생성
#### Symbol 함수

심볼 값은 `Symbol` 함수를 호출하여 생성한다.  
다른 원시값들은 리터럴 표기법으로 값을 생성할 수 있지만, 심볼 값은 `Symbol` 함수를 통해서만 생성할 수 있다.

이때 생성된 심볼 값은 외부로 노출되지 않아 확인할 수 없으며,  
**다른 값과 절대 중복되지 않는 유일한 값이다.**

``` js
// Symbol 함수를 호출하여 유일한 심볼 값을 생성한다.
const mySymbol = Symbol();

// 심볼 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol); // Symbol()
```

<br>

심볼 함수는 `String`, `Number`, `Boolean` 생성자 함수와는 달리  
`new` 연산자와 함께 호출하지 않는다.

`new` 연산자와 함께 생성자 함수 또는 클래스를 호출하면  
객체(인스턴스)가 생성되지만 심볼 값은 변경 불가능한 원시 값이다.

``` js
new Symbol(); // TypeError: Symbol is not a constructor
```

<br>

`Symbol`은 선택적으로 문자열을 인수로 전달할 수 있다.  
> 심볼 값에 대한 설명(description)으로 디버깅 용도로만 사용된다.

하지만 이는 심볼 값 생성에 어떠한 영향도 주지 않기 때문에  
설명이 같더라도 생성된 심볼 값은 유일하다.

``` js
// 심볼 값에 대한 설명(디버깅 용도)
const mySymbol1 = Symbol('mySymbol');
const mySymbol2 = Symbol('mySymbol');

console.log(mySymbol1 === mySymbol2); // false
```

<br><br>

심볼 값도 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성한다.
> 래퍼 객체: 원시값을 객체처럼 사용할 수 있도록 임시로 생성되는 객체

``` js
const mySymbol = Symbol('mySymbol');

console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)
```

심볼 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.  
하지만 불리언 타입으로는 암묵적으로 타입 변환한다. (if 문 등에서 존재 확인 가능)

``` js
const mySymbol = Symbol('mySymbol');

console.log(mySymbol); // Symbol(mySymbol)
console.log(mySymbol + ''); // TypeError: Cannot convert a Symbol value to a string

console.log(Boolean(mySymbol)); // true
console.log(!mySymbol); // false

if (mySymbol) {
  console.log('mySymbol is not empty');
}
```

<br><br>

#### Symbol.for / Symbol.keyFor 메소드
`Symbol.for` 메소드는 인수로 전달받은 **문자열을 키로 사용**하여  
키와 심볼 값의 쌍들이 저장되어 있는 <font color='orange'>전역 심볼 레지스트리(global symbol registry)에서  
해당 키와 일치하는 심볼 값을 검색한다.</font>

- 검색 성공 시, 검색된 심볼 값을 반환
- 검색 실패 시, 새로운 심볼을 생성하여 `Symbol.for` 메소드의 인수로 전달된 키로 전역 심볼 레지스트리에 저장한 뒤 생성된 심볼 값을 반환

``` js
// 전역 심볼 레지스트리에 mySymbol이라는 키로 저장된 심볼 값이 없으면 새로운 심볼 값을 생성
const s1 = Symbol.for('mySymbol');

// 전역 심볼 레지스트리에 mySymbol이라는 키로 저장된 심볼 값이 있으면 해당 심볼 값을 반환
const s2 = Symbol.for('mySymbol');

console.log(s1 === s2); // true
```

<br>


`Symbol.keyFor` 메소드를 사용하면 전역 심볼 레지스트리에 저장된 심볼 값의 키를 추출할 수 있다.

```js
// 전역 심볼 레지스트리에 mySymbol이라는 키로 저장된 심볼 값이 없으면 새로운 심볼 값을 생성
const s1 = Symbol.for('mySymbol');

// 전역 심볼 레지스트리에 저장된 심볼 값의 키를 추출
Symbol.keyFor(s1); // mySymbol
```

<br><br>


### 심볼과 상수
``` js
const Direction = {
    UP: 1,
    DOWN: 2,
    LEFT: 3,
    RIGHT: 4,
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
    console.log('You are going up.');
}
```

값에는 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우가 종종 있다.  
이때 문제는 변경되면 안되는 상수 값(1, 2, 3, 4)이 변경될 수 있다는 리스크가 있다.

이런 경우 유일한 값인 심볼 값을 사용할 수 있다.

``` js
const Direction = {
    UP: Symbol('up'),
    DOWN: Symbol('down'),
    LEFT: Symbol('left'),
    RIGHT: Symbol('right'),
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
    console.log('You are going up.');
}
```



<br><br>

### 심볼과 프로퍼티 키
심볼 값을 프로퍼티 키로 사용하려면  
프로퍼티 키로 사용할 **심볼 값에 대괄호를 사용해야 한다.**

> 프로퍼티에 접근할 때도 마찬가지로 대괄호를 사용해야 한다.

``` js
const obj = {
    // 심볼 값으로 프로퍼티 키를 생성
    [Symbol.for('mySymbol')]: 1,
};

console.log(obj[Symbol.for('mySymbol')]); // 1
```

심볼 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.



<br>

끝.