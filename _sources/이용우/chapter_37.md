### Chapter 37 - Set과 Map

#### Set
Set 객체는 중복되지 않는 유일한 값들의 집합이다.  
배열과 유사하지만 아래와 같은 차이점이 있다.

| 구분 | 배열 | Set 객체 |
|------|------|----------|
|동일한 값들을 중복으로 포함할 수 있다.|O|X|
|요소 순서에 의미가 있다.|O|X|
|인덱스로 요소에 접근할 수 있다.|O|X|

<br>

Set 객체는 이런 특징 때문에 수학에서의 **집합** 의 특성과 일치한다.  
따라서 Set 객체로 교집합, 합집합, 차집합, 여집합 등을 구현할 수 있다.

<br><br>

#### Set 객체 생성
Set 객체는 Set 생성자 함수로 생성한다.

``` js
const set = new Set();
console.log(set); // Set(0) {}
```

<br>

Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다.  
이때 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.

``` js
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set('hello');
console.log(set2); // Set(4) {h, e, l, o}
```

<br>

중복을 허용하지 않는 특징을 이용하여 배열에서 중복된 요소를 제거할 수도 있다.

``` js
// 배열의 중복 요소 제거
const uniq = array => array.filter((v, i, self) => self.indexOf(v) === i);

console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]

// Set 객체르 사용하여 배열의 중복 요소 제거
const uniq = array => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
```

<br><br>

#### 요소 개수 확인
Set 객체의 요소 개수를 확인할 때는 `Set.prototype.size` 프로퍼티를 사용한다.

``` js
const { size } = new Set([1, 2, 3, 3]);
console.log(size);  // 3
```

<br><br>

#### 요소 추가
Set 객체에 요소를 추가할 때는 `Set.prototype.add` 메소드를 사용한다.

``` js
const set = new Set();
console.log(set);  // Set(0) {}

set.add(1);
console.log(set);  // Set(1) {1}
```

<br>

#### 요소 존재 여부 확인
특정 요소가 존재하는지 확인하려면 `Set.prototype.has` 메소드를 사용한다.  

``` js
const set = new Set([1, 2, 3]);

console.log(set.has(2));  // true
console.log(set.has(4));  // false
```

<br>

#### 요소 삭제
`Set` 객체의 특정 요소를 삭제하려면 `Set.prototype.delete` 메소드를 사용한다.  
`delete` 메소드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다.

<br>

``` js
const set = new Set([1, 2, 3]);

set.delete(2);
console.log(set);  // Set(2) {1, 3}
```

<br>

#### 요소 일괄 삭제
모든 요소를 일괄로 삭제하려면 `Set.prototype.clear` 메소드를 사용한다.

``` js
const set = new Set([1, 2, 3]);

set.clear();
console.log(set);  // Set(0) {}
```

<br><br>

#### 요소 순회
`Set` 객체의 요소를 순회하려면 `Set.prototype.forEach` 메소드를 사용한다.  

콜백 함수의 인수로 요소값, 요소값, 집합 객체 자체를 전달받는다.  

`Array.prototype.forEach` 메소드와 유사하지만,  
Array 객체의 forEach 메소드의 2번째 인수로 배열의 index를 전달받는데 반해  
Set 객체의 forEach 메소드의 2번째 인수는 요소값 자체를 전달받는다.
> Set 객체는 순서에 의미가 없기 때문이다.


``` js
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
// 1 1 Set(3) {1, 2, 3}
// 2 2 Set(3) {1, 2, 3}
// 3 3 Set(3) {1, 2, 3}
```

<br><br>

Set 객체는 이터러블하다.  
때문에 for...of 문으로 순회 가능하며, 스프레드 문법 및 배열 디스트럭처링의 대상이 될 수도 있다.

``` js
const set = new Set([1, 2, 3]);

// for...of 문
for (const value of set) {
    console.log(value);  // 1 2 3
}

// 스프레드 문법
console.log([...set]);  // [1, 2, 3]

// 배열 디스트럭처링
const [a, ...rest] = set;
console.log(a, rest);  // 1 Set(2) {2, 3}
```

<br><br>

#### 집합 연산
Set 객체는 수학에서 집합을 표현하는 자료구조다.  

##### 교집합
교집합은 아래와 같이 구현 가능하다.

``` js
Set.prototype.intersection = function (set) {
    const result = new Set();

    for (const value of set) {
        // 2개의 set 요소가 공통되는 요소이면 교집합의 대상이다.
        if (this.has(value)) {
            result.add(value);
        }
    }
    return result;
}

// setA와 setb의 교집합
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.intersection(setB));  // Set(2) {2, 4}

// 교집합 메서드를 직접 구현하는 것이 아니라 아래와 같이 구현할 수도 있다.
console.log(new Set([...setA].filter(v => setB.has(v))));  // Set(2) {2, 4}
```

<br><br>


##### 합집합
``` js
Set.prototype.union = function (set) {
    const result = new Set(this);

    for (const value of set) {
        // 합집합은 2개의 set 객체의 모든 요소로 이루어진 집합이다.
        result.add(value);
    }
    return result;
}

// setA와 setB의 합집합
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4, 5, 6]);

console.log(setA.union(setB));  // Set(6) {1, 2, 3, 4, 5, 6}

// 또는 아래와 같이 구현할 수도 있다.
console.log(new Set([...setA, ...setB]));  // Set(6) {1, 2, 3, 4, 5, 6}
```

<br><br>

##### 차집합
``` js
Set.prototype.difference = function (set) {
    const result = new Set(this);

    for (const value of set) {
        result.delete(value);
    }
    return result;
}

// setA와 setB의 차집합
const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.difference(setB));  // Set(2) {1, 3}

// 또는 아래와 같이 구현할 수도 있다.
console.log(new Set([...setA].filter(v => !setB.has(v))));  // Set(2) {1, 3}
```

<br><br>

### Map
Map 객체는 키와 쌍으로 이루어진 컬렉션이다.  
객체와 유사하지만 아래와 같은 차이점이 존재한다.

| 구분 | 객체 | Map 객체 |
|------|------|----------|
|키로 사용할 수 있는 값|문자열 또는 심볼 값|객체를 포함한 모든 값|
|이터러블|X|O|
|요소 개수 확인|Object.keys(obj).length|map.size|

<br><br>

#### Map 객체 생성
Map 객체는 Map 생성자 함수로 생성한다.  
Map 생성자 함수에 인수를 전달하지 않으면 Map 객체가 생성된다.

``` js
const map = new Map();
console.log(map);  // Map(0) {}
```

<br>

Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다.  
이때 이터러블의 요소가 키와 값으로 구성된 배열인 경우, 이 배열의 중첩 배열 요소들을 각각 키와 값으로 분리하여 Map 객체를 생성한다.

``` js
const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(map1);  // Map(2) {"key1" => "value1", "key2" => "value2"}

const map2 = new Map([1, 2]) // TypeError: Iterator value 1 is not an entry object
```

<br><br>

#### 요소 개수 확인
Map 객체의 요소 개수를 확인할 때는 `Map.prototype.size` 프로퍼티를 사용한다.

``` js
const { size } = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(size);  // 2
```

<br><br>

#### 요소 추가
Map 객체에 요소를 추가할 때는 `Map.prototype.set` 메소드를 사용한다.

``` js
const map = new Map();

map.set('key1', 'value1');
map.set('key2', 'value2');

console.log(map);  // Map(2) {"key1" => "value1", "key2" => "value2"}
```

<br>

Map 객체는 key 타입에 제한이 없다.  
따라서 객체를 포함한 모든 값을 사용할 수 있다.

``` js
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer');
map.set(kim, 'designer');

console.log(map);  // Map(2) {Object {name: "Lee"} => "developer", Object {name: "Kim"} => "designer"}
```


<br>

#### 특정 요소 찾기
Map 객체에서 특정 요소를 찾으려면 `Map.prototype.get` 메소드를 사용한다.

``` js
const map = new Map();

map.set('key1', 'value1');
map.set('key2', 'value2');

console.log(map.get('key1'));  // value1
```

<br><br>

#### 요소 존재 여부 확인
Map 객체에서 특정 요소가 존재하는지 확인하려면 `Map.prototype.has` 메소드를 사용한다.

``` js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

console.log(map.has(lee));  // true
console.log(map.has(kim));  // true

// 키를 기준으로 존재 여부 확인
console.log(map.has({ name: 'Lee' }));  // false
```

<br><br>

#### 요소 삭제
Map 객체에서 특정 요소를 삭제하려면 `Map.prototype.delete` 메소드를 사용한다.  

``` js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.delete(lee);
console.log(map);  // Map(1) {Object {name: "Kim"} => "designer"}
```

<br><br>

#### 요소 일괄 삭제
Map 객체의 모든 요소를 삭제하려면 `Map.prototype.clear` 메소드를 사용한다.

``` js
const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.clear();
console.log(map);  // Map(0) {}
```

<br><br>


#### 요소 순회
Map 객체의 요소를 순회하려면 `Map.prototype.forEach` 메소드를 사용한다.  

콜백 함수의 인수로 요소 값, 요소 키, Map 객체 자체를 전달받는다.  


``` js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.forEach((v, k, map) => console.log(v, k, map));
// developer {name: "Lee"} Map(2) {Object {name: "Lee"} => "developer", Object {name: "Kim"} => "designer"}
// designer {name: "Kim"} Map(2) {Object {name: "Lee"} => "developer", Object {name: "Kim"} => "designer"}
```

<br><br>

Map 객체도 이터러블이라 for...of 문으로 순회 가능하며, 스프레드 문법과 배열 디스트럭처링 할당 대상이 될 수 있다.

``` js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

// for...of 문
for (const entry of map) {
    console.log(entry);  // [Object {name: "Lee"}, "developer"] [Object {name: "Kim"}, "designer"]
}

// 스프레드 문법
console.log([...map]);  // [[Object {name: "Lee"}, "developer"], [Object {name: "Kim"}, "designer"]]

// 배열 디스트럭처링
const [a, ...rest] = map;
console.log(a, rest);  // [Object {name: "Lee"}, "developer"] Map(1) {Object {name: "Kim"} => "designer"}
```

<br>

Map 객체는 이터레이터인 객체를 반환하는 메소드를 추가로 제공한다.
- `Map.prototype.keys`
- `Map.prototype.values`
- `Map.prototype.entries`

이 메소드들은 이터레이터를 반환하므로 for...of 문으로 순회 가능하다.

``` js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

// 키 순회
for (const key of map.keys()) {
    console.log(key);  // Object {name: "Lee"} Object {name: "Kim"}
}

// 값 순회
for (const value of map.values()) {
    console.log(value);  // developer designer
}

// 키와 값 순회
for (const [key, value] of map.entries()) {
    console.log(key, value);  // Object {name: "Lee"} developer Object {name: "Kim"} designer
}
```


<br>

끝.