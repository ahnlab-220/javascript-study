# Chapter 16 - 프로퍼티 어트리뷰트

프로퍼티 어트리뷰트를 이해하기 전에 내부 슬롯(internal slot)과 내부 메소드(internal method)의 개념에 대해 알아보자.  

내부 슬롯과 내부 메소드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해  
ECMAScript 사양에서 사용하는 의사 프로퍼티(persudo property)와 의사 메소드(persudo method)이다.

<br>

|구분|의사 프로퍼티|의사 메소드|
|--|--|--|
|역할|속성처럼 보이지만 함수 호출로 동작|메소드처럼 보이며 특별한 상황에서 호출|
|사용 기술|`getter`, `setter`|`toString()`, `valueOf()`, `Symbol.*`|
|주요 목적|데이터 접근/수정 시 검증 및 추가 동작|객체의 특별한 동작 정의|
|사용 사례|데이터 캡출화, 속성 검증|반복, 문자열 변환, 산술 연산 정의|


- 의사 프로퍼티는 객체의 속성 접근을 제어하는 데 유용함
- 의사 메서드는 객체의 특별한 동작(문자열 변환, 반복, 비교 등)을 정의하거나 수정할 때 사용함


<br>

내부 슬롯과 내부 메소드는 ESMAScript 사양에 정의된 대로 구현되어  
자바스크립트 엔진에서 실제로 동작하지만  
개발자가 직접 접근할 수 있도록 **외부로 공개된 객체의 프로퍼티는 아니다.**  
다만, 일부 내부 슬롯과 내부 메소드에 한해서 간접적으로 접근할 수 있는 수단을 제공한다.  

예를 들어, 모든 객체는 `[[Prototype]]`라는 내부 슬롯을 가진다.  
내부 슬롯은 자바스크립트 내부 로직이므로 원칙적으로 접근이 불가능하지만,  
`[[Prototype]]`의 경우,`__proto__`를 통해 간접적으로 접근할 수 있다.

``` js
const o = {};

// 내부 슬롯은 직접 접근 불가능
o.[[Prototype]]     // Uncaught SyntaxError: Unexpected token '['

// 일부 내부 슬롯이나 내부 메소드에 한해서 간접적으로 접근 가능함
o.__proto__     // Object.prototype
```

<br><br><hr>

### 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체
자바스크립트 엔진은 <font color='orange'>프로퍼티를 생성할 때  
프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다. </font>

프로퍼티 상태는 다음을 의미한다.
- 프로퍼티의 값(value)
- 값의 갱신 여부(writable)
- 열거 가능 여부(enumeratble)
- 재정의 가능 여부(configurable)

프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태값(meta-property)인  
내부 슬롯 `[[Value]]`, `[[Writeable]]`, `[[Enumerable]]`, `[[Configurable]]`이다.

따라서 프로퍼티 어트리뷰트에 직접 접근할 수는 없지만,  
`Object.getOwnPropertyDescriptor` 메소드를 사용하여 간접적으로 확인할 수 있다.

<br>

``` js
const person = {
    name: 'Lee'
};

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환함.
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// {value: "Lee", writable: true, enumerable: true, configurable: true}
```

`Object.getOwnPropertyDescriptor` 메소드를 호출할 때 첫 번째 매개변수에는 객체의 참조를 전달하고, 두번째 매개변수에는 프로퍼티 키를 문자열로 전달한다.

이 메소드는 프로퍼티 어트리뷰트 정보를 제공하는  
프로퍼티 디스크립터(Property-Descriptor) 객체를 반환한다.

ES8에서 도입된 `Object.getOwnPeopertyDescriptors` 메소드는  
모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.  

``` js
const person = {
    name: 'Lee'
};

// 프로퍼티 동적 생성
person.age = 20;

// 모든 프로퍼티의 프로퍼티 어트리뷰트 정볼르 제공하는 프로퍼티 디스크립터 객체들을 반환
console.log(Object.getOwnPropertyDescriptors(person));

/*
{
    name: {value: "Lee", writable: true, enumerable: true, configurable: true},
    age: {value: 20, writable: true, enumerable: true, configurable: true}
}
*/
```

<br><br><hr>

### 데이터 프로퍼티와 접근자 프로퍼티
프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분이 가능하다.
- 데이트 프로퍼티 (data property)
    - 키(key)와 값(value)으로 구성된 일반적인 프로퍼티
- 접근자 프로퍼티 (accessor property)
    - 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수(accessor function)로 구성된 프로퍼티

<br>

#### 데이터 프로퍼티
데이터 프로퍼티는 다음의 프로퍼티 어트리뷰트를 갖는다.  
이는 프로퍼티 생성 시 기본값으로 자동 정의된다.

- `[[Value]]`
    - 프로퍼티 키를 통해 프로퍼티 값에 접근하면 변환되는 값
    - 프로퍼티 키를 통해 프로퍼티 값을 변경하면 `[[Value]]` 값에 재할당한다. 이때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 `[[Value]]`에 값을 저장한다.
- `[[Writable]]`
    - 프로퍼티 값의 변경 가능 여부를 나타낸다.
    - `[[Writable]]`의 값이 false인 경우 해당 프로퍼티의 `[[Value]]`의 값을 변경할 수 없는 읽기 전용 프로퍼티가 된다.
- `[[Enumerable]]` 
    - 프로퍼티 열거 가능 여부를 나타낸다.
    - `[[Enumerable]]`의 값이 false인 경우 해당 프로퍼티는 `for ... in` 문이나 `Object.keys` 메소드 등으로 열거할 수 없다.
- `[[Configurable]]`
    - 프로퍼티의 재정의 가능 여부를 나ㅏㅌ내며 불리언 값을 갖는다.
    - `[[Configurable]]` 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다. 단, `[[Writable]]`이 true인 경우, `[[Value]]`의 변경과 `[[Writable]]`을 false로 변경하는 것을 허용한다.


``` js
const person = {
    name: 'Lee'
};

// 프로퍼티 동적 생성
person.age = 20;

// 모든 프로퍼티의 프로퍼티 어트리뷰트 정볼르 제공하는 프로퍼티 디스크립터 객체들을 반환
console.log(Object.getOwnPropertyDescriptors(person));

/*
{
    name: {value: "Lee", writable: true, enumerable: true, configurable: true},
    age: {value: 20, writable: true, enumerable: true, configurable: true}
}
*/
```

<br><br>

#### 접근자 프로퍼티
접근자 프로퍼티(accessor property)는 자체적으로 값을 갖지 않고   
다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수(accessor function)로 구성된 프로퍼티이다.

접근자 프로퍼티는 다음과 같은 프로퍼티 어트리뷰트를 갖는다.

- `[[Get]]`
    - 접근자 프로퍼티를 통해 데이터 프로퍼티 값을 읽을 때 호출되는 접근자 함수.
    - 접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 `[[Get]]`의 값, 즉 `getter` 함수가 호출되고 그 결과가 프로퍼티 값으로 반환된다.
- `[[Set]]`
    - 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수.
    - 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰터 `[[Set]]`의 값, 즉 `setter` 함수가 호출되고 그 결과가 프로퍼티 값으로 저장된다.
- `[[Enumerable]]` 
    - 프로퍼티 열거 가능 여부를 나타낸다.
    - `[[Enumerable]]`의 값이 false인 경우 해당 프로퍼티는 `for ... in` 문이나 `Object.keys` 메소드 등으로 열거할 수 없다.
- `[[Configurable]]`
    - 프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖는다.
    - `[[Configurable]]` 값이 false인 경우 **해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다.** 단, `[[Writable]]`이 true인 경우, `[[Value]]`의 변경과 `[[Writable]]`을 false로 변경하는 것을 허용한다.


<br>

접근자 프로퍼티는 getter와 setter 함수를 모두 정의할 수도 있고 하나만 정의할 수도 있다.

``` js
const person = {
    // 데이터 프로퍼티
    firstName: 'Ungmo',
    lastName: 'Lee',

    // 접근자 프로퍼티
    // getter 함수
    get fullName() {
        return `${this.firstName} ${this.lastName}`
    },

    // setter 함수
    set fullName(name) {
        // 배열 디스트럭처링 할당
        [this.firstName, this.lastName] = name.split(' ');
    }
};

// 데이터 프로퍼티를 통한 프로퍼티 값 참조
console.log(person.firstName + ' ' + person.lastName);

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
person.fullName = 'Heegun Lee';

// 접근자 프로퍼티를 통한 프로퍼티 값 참조
console.log(person.fullName);

// 데이터 프로퍼티의 프로퍼티 어트리뷰트
let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log(descriptor);
// {value: 'Heegun', writable: true, enumerable: true, configurable: true}

// 접근자 프로퍼티의 프로퍼티 어트리뷰트
descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log(descriptor);
// {get: f, set: f, enumerable: true, configurable: true}

```

<br><br><hr>

### 프로퍼티 정의
프로퍼티 정의란, 새로은 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나,  
기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다. 

`Object.defineProperty` 메소드를 사용하면 프로퍼티의 어트리뷰트를 정의할 수 있다.  
객체의 참조와 데이터 프로퍼티의 키인 문자열, 프로퍼티 디스크립터 객체를 인수로 전달한다.

``` js
const person = {};

// 데이터 프로퍼티 정의
Object.defineProperty(person, 'firstName', {
    value: 'Ungmo', 
    writable: true,
    enumerable: true,
    configurable: true
});

// 디스크립트 객체의 프로퍼티를 누락시키면 undefined, false가 기본값이다.
Object,defineProperty(person, 'lastName', {
    value: 'Lee'
});

// 접근자 프로퍼티 정의
Object.defineProperty(person, 'fullName', {
    // getter 함수
    get() {
        return `${this.firstName} ${this.lastName}`;
    },

    // setter 함수
    set(name) {
        [this.firstName, this.lastName] = name.split(' ');
    },
    enumerable: true,
    configurable: true
});
```

`Object.defineProperty` 메소드는 한번에 하나의 프로퍼티만 정의할 수 있다.  
`Object.defineProperties` 메소드를 사용하면 여러 개의 프로퍼티를 한 번에 정의할 수 있다.

``` js
const person = {};

Object.defineProperties(person, {
    // 데이터 프로퍼티 정의
    firstName: {
        value: 'Ungmo',
        writable: true,
        enumerable: true,
        configurable: true
    },
    lastName: {
        value: 'Lee',
        writable: true,
        enumerable: true,
        configurable: true
    },

    // 접근자 프로퍼티 정의
    fullName: {
        // getter 함수
        get() {
            return `${this.firstName} ${this.lastName}`;
        },

        // setter 함수
        set(name) {
            [this.firstName, this.lastName] = name.split(' ');
        },
        enumerable: true,
        configurable: true
    }
});
```

<br><br><hr>

### 객체 변경 방지
객체는 변경 가능한 값이므로 재할당 없이 직접 변경이 가능하다.  
즉, 프로퍼티를 추가하거나 삭제할 수 있고 프로퍼티 값을 갱신할 수 있으며,  
`Object.defineProperty` 또는 `Object.defineProperties` 메소드를 사용하여 프로퍼티 어트리뷰트를 재정의할 수도 있다.  

자바스크립트는 객체의 변경을 방지하는 다양한 메소드를 제공한다.  
객체 변경 방지 메소드들은 객체의 변경을 금지하는 강도가 각각 다르다.

|구분|메소드|프로피터 추가|프로퍼티 삭제|프로퍼티 값 읽기|프로퍼티 값 쓰기|프로퍼티 어트리뷰트 재정의|
|--|--|--|--|--|--|--|
|객체 확장 금지|`Object.preventExtensions`|X|O|O|O|O|
|객체 밀봉|`Object.seal`|X|X|O|O|X|
|객체 동결|`Object.freeze`|X|X|O|X|X|


<br><br>

#### 객체 확장 금지
`Object.preventExtensions` 메소드는 객체의 확장을 금지한다.  
확장이 금지된 객체는 프로퍼티 추가가 금지된다.

``` js
const person = { name: 'Lee' };

// 객체 확장 금지
Object.preventExtensions(person);

// person 객체는 확장이 금지된 객체
console.log(Object.isExtensible(person));   // false
```

<br><br>

#### 객체 밀봉
`Obejct.seal` 메소드는 객체를 밀봉한다.
> ※ 객체 밀봉  
> 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의 금지

밀봉된 객체는 읽기와 쓰기만 가능하다.

``` js
const person = { name: 'Lee' };

// 객체 밀봉
Object.seal(person);

// person 객체는 밀봉된 객체
console.log(Object.isSealed(person));   // true
```

<br><br>

#### 객체 동결
`Object.freeze` 메소드는 객체를 동결한다.  
> ※ 객체 동결  
> 프로퍼티 추가 및 삭제와 프로피터 어트리뷰트 재정의 금지, 프로퍼티 값 갱신 금지

동결된 객체는 읽기만 가능하다.

``` js
const person = { name: 'Lee' };

// 객체 밀봉
Object.freeze(person);

// person 객체는 동결된 객체
console.log(Object.isFrozen(person));   // true
```

<br>

<font color='orange'>지금까지 살펴본 변경 방지 메소드들은 얕은 변경 방지(shallow only)로  
직속 프로퍼티만 변경이 방지되고, 중첩 객체까지는 영향을 주지 못한다.  </font>

``` js
const person = {
    name: 'Lee',
    address: { city: 'Seoul' }
};

// 객체 동결
Object.freeze(person);

// 직속 프로퍼티만 동결된다.
console.log(Object.isFrozen(person));   // true
console.log(Object.isFrozen(person.address));   // false
```

<br>

때문에 중첩 객체까지 동결하고 싶다면  
객체를 값으로 갖는 모든 프로퍼티에 대해서 재귀적으로 Object.freeze 메소드를 호출해야 한다.

``` js
function deepFreeze(target) {
    if (target && typeof target === 'object' && !Object.isFrozen(target)) {
        Object.freeze(target);
        Object.keys(target).forEach(key => deepFreeze(target[key]));
    }
    return target;
}

const person = {
    name: 'Lee',
    address: { city: 'Seoul' }
};

// 깊은 객체 동결
deepFreeze(person);

console.log(Object.isFrozen(person));   // true
console.log(Object.isFrozen(person.address));   // true
```


