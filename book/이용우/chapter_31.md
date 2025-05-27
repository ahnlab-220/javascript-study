### Chapter 31 - RegExp

정규 표현식은 문자열을 대상으로 패턴 매칭 기능을 제공한다.


<br>

#### 정규 표현식의 생성

정규 표현식은 리터럴 표기법과 `RegExp` 생성자 함수를 사용하여 생성한다.  

정규 표현식 리터럴은 패턴과 플래그로 구성된다.  

<img width="250" src="https://github.com/user-attachments/assets/930e38e7-6535-472d-a405-ee330fd160ec" />

``` js
// 정규 표현식 리터럴 표기법
const target = 'Is this all there is?';

// 패턴: is
// 플래그: i => 대소문자를 구분하지 않고 검색
const regExp = /is/i;

// test 메소드는 target 문자열에 대해 정규 표현식 regExp의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.
regExp.test(target);    // true
```

<br>

RegExp 생성자 함수를 사용하여 정규 표현식을 생성할 수도 있다.  

``` js
const regExp = new RegExp(/is/i);
```



<br><br>

### RegExp 메소드

#### RegExp.prototype.exec

exec 메소드는 인수로 전달받은 문자열에 대해  
정규 표현식의 패턴을 검색하여 **매칭 결과를 배열로 반환**한다.  

``` js
const target = 'Is this all there is?';
const regExp = /is/;

regExp.exec(target);    // ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

exec 메소드는 문자열 내 모든 패턴을 검색하는 `g` 플래그를 지정해도  
첫 번째 매칭 결과만 반환하므로 주의해야 한다.

<br>

#### RegExp.prototype.test

test 메소드는 인수로 전달받은 문자열에 대해  
정규 표현식의 패턴을 검색하여 **매칭 결과를 불리언 값으로 반환**한다.

``` js
const target = 'Is this all there is?';
const regExp = /is/;

regExp.test(target);    // true
```

<br>

#### String.prototype.match

match 메소드는 대상 문자열에 대해 정규 표현식의 패턴을 검색하여  
매칭 결과를 배열로 반환한다.

``` js
const target = 'Is this all there is?';
const regExp = /is/;

target.match(regExp);    // ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

exec 메소드는 문자열 내 모든 패턴을 검색하는 `g` 플래그를 지정해도 첫 번째 매칭 결과만 반환한다.  
하지만 `String.prototype.match` 메소드는 대상 문자열에 대해 정규 표현식의 패턴을 검색하여  
매칭 결과를 배열로 반환한다.

``` js
const target = 'Is this all there is?';
const regExp = /is/g;

target.match(regExp);    // ["is", "is"]
```

<br>

#### String.prototype.replace

replace 메소드는 대상 문자열에 대해 정규 표현식의 패턴을 검색하여  
매칭 결과를 치환한 문자열을 반환한다.

``` js
const target = 'Is this all there is?';
const regExp = /is/;

target.replace(regExp, 'was');    // "Was this all there is?"
```

<br><br>

### 플래그

패턴과 함께 정규 표현식을 구성하는 플래그는  
정규 표현식의 검색 방식을 설정하기 위해 사용한다.  
플래그는 총 6개가 있으며 자주 사용되는 3개의 플래그를 다룬다.

| 플래그 | 설명 | 예시 |
| :---: | :---: | :---: |
| i | 대소문자를 구분하지 않고 검색 (ignore case) | `/is/i` |
| g | 대상 문자열 내 모든 패턴을 검색 (global) | `/is/g` |
| m | 대상 문자열 내 여러 줄의 문자열을 검색 (multi line) | `/is/m` |

<br>

플래그는 옵션이므로 선택적으로 사용하면 된다.  
순서와 상관없이 하나 이상의 플래그를 동시에 설정할 수도 있다.

<font color='orange'>플래그를 사용하지 않을 경우 대소문자를 구별하여 패턴을 검색한다.</font>

``` js
const target = 'Is this all there is?';

// target 문자열에서 is 패턴을 검색한다.
// 플래그를 생략하면 대소문자를 구별하여 검색한다.
target.match(/is/);    // ["is", index: 5, input: "Is this all there is?", groups: undefined]

// 플래그를 사용하면 대소문자를 구별하지 않고 검색한다.
target.match(/is/i);    // ["Is", index: 0, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하여 전역 검색한다.
target.match(/is/g);    // ["Is", "is"]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 전역 검색한다.
target.match(/is/gi);    // ["Is", "is"]

// target 문자열에서 is 문자열을 대소문자를 구별하여 여러 줄에서 검색한다.
target.match(/is/m);    // ["Is", "is"]
```

<br><br>

### 패턴
정규 표현식은 "일정한 규칙(패턴)을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어(formal language)"이다.  

패턴은 /로 열고 닫으며 문자열의 따옴표는 생략한다.  
> 따옴표를 포함하면 따옴표까지 패턴에 포함되어 검색된다.

또한 패턴은 특별한 의미를 가지는 메타 문자(meta character) 또는 기호로 표현할 수 있다.  

<br>

#### 문자열 검색
정규 표현식의 패턴에 문자 또는 문자열을 지정할 경우 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열을 검색한다.  

``` js
const target = 'Is this all there is?';

// is 패턴을 검색한다.
target.match(/is/);    // ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

<br>

#### 임의의 문자열 검색
`.`은 임의의 문자 1개를 의미한다.  
아래 예제의 경우 `.`을 3개 연속하여 패턴을 생성했기 때문에 문자의 내용과 상관없이 3자리 문자열과 매치한다.

``` js
const target = 'Is this all there is?';

// 3자리 문자열을 대소문자 구별하여 전역 검색한다.
const regExp = /.../g;

target.match(regExp);    // ["Is ", "is ", "is"]
```

<br>

#### 반복 검색

`{m, n}`은 앞선 패턴이 최소 `m`번, 최대 `n`번 반복되는 문자열을 의미한다.  

``` js
const target = 'A AA B BB Aa Bb';

// 'A'가 최소 1번, 최대 2번 반복되는 문자열을 대소문자 구별하여 전역 검색한다.
const regExp = /A{1,2}/g;

target.match(regExp);    // ["A", "AA", "A"]
```

<br>

`{n}`은 앞선 패턴이 정확히 `n`번 반복되는 문자열을 의미한다.  

``` js
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 2번 반복되는 문자열을 대소문자 구별하여 전역 검색한다.
const regExp = /A{2}/g;

target.match(regExp);    // ["AA", "AA"]
```

<br>

`{n,}`은 앞선 패턴이 최소 `n`번 이상 반복되는 문자열을 의미한다.  

``` js
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 2번 이상 반복되는 문자열을 대소문자 구별하여 전역 검색한다.
const regExp = /A{2,}/g;

target.match(regExp);    // ["AA", "AAA"]
```

<br>

`+`은 앞선 패턴이 최소 1번 이상 반복되는 문자열을 의미한다.  

``` js
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 1번 이상 반복되는 문자열을 대소문자 구별하여 전역 검색한다.
const regExp = /A+/g;

target.match(regExp);    // ["A", "AA", "A", "AAA"]
```

<br>

`?`은 앞선 패턴이 있거나 없거나 둘 중 하나를 의미한다. (`{0, 1}`)  
예를 들어 `/colou?r/`는 `colo` 다음 `u`가 최대 한 번 이상 반복되고 `r`이 이어지는 문자열을 검색한다.  
> `color`, `colour` 모두 매칭된다.

``` js
const target = 'color colour';

const regExp = /colou?r/g;

target.match(regExp);    // ["color", "colour"]
```

<br><br>

#### OR 검색
`|`은 or의 의미를 갖는다.

예를 들어 `/A|B/`는 `A` 또는 `B`를 검색한다.

``` js
const target = 'A AA B BB Aa Bb';

const regExp = /A|B/g;

target.match(regExp);    // ['A', 'A', 'A', 'B', 'B', 'B', 'A', 'B']
```


<br>

#### 범위

`[]`는 문자열의 범위를 지정하여 검색한다.

예를 들어 `/[0-9]/`는 0부터 9까지의 모든 숫자를 검색한다.

``` js
const target = 'A1B2C3';

const regExp = /[0-9]/g;

target.match(regExp);    // ['1', '2', '3']
```

<br>

쉼표를 패턴에 포함시킬 수도 있다.

``` js
const target = 'AA BB 12,345';

const regExp = /[0-9,]+/g;  // 숫자 또는 쉼표가 1번 이상 반복되는 문자열을 검색한다.

target.match(regExp);    // ['12,345']
```


<br>

대소문자를 구별하지 않고 알파벳만 검색하는 방법은 아래와 같다.

``` js
const target = 'A1B2C3';

const regExp = /[a-zA-Z]/g;     // 또는 /[a-z]/g

target.match(regExp);    // ['A', 'B', 'C']
```


<br>

#### 숫자

`\d`는 0부터 9까지의 숫자를 의미한다.

``` js
const target = '12345';

const regExp = /\d+/g;

target.match(regExp);    // ['12345']
```

<br>

참고로 `\D`는 숫자가 아닌 문자를 의미한다.

``` js
const target = '12345';

const regExp = /\D+/g;

target.match(regExp);    // null
```

<br>

#### 문자열

`\w`는 알파벳 대소문자, 숫자, 언더바를 의미한다.

``` js
const target = 'A1B2C3';

const regExp = /\w+/g;

target.match(regExp);    // ['A1B2C3']
```

<br>

참고로 `\W`는 알파벳 대소문자, 숫자, 언더바가 아닌 문자를 의미한다.

``` js
const target = 'A1B2C3';

const regExp = /\W+/g;

target.match(regExp);    // null
```

<br><br>

#### NOT

`[ ]` 내의 `^`는 not의 의미를 갖는다.

``` js
const target = 'AA BB 12,345';

const regExp = /[^0-9]+/g;  // 숫자가 아닌 문자열을 검색한다.

target.match(regExp);    // ['AA BB', ',']
```

<br>

#### 시작

`[ ]` 밖의 `^`는 문자열의 시작을 의미한다.

``` js
const target = 'AA BB 12,345';

const regExp = /^[0-9]+/g;  // 숫자로 시작하는 문자열을 검색한다.

target.match(regExp);    // ['12,345']
```


<br>

#### 끝

`[ ]` 밖의 `$`는 문자열의 끝을 의미한다.

``` js
const target = 'https://example.com';

const regExp = /com$/g;  // com으로 끝나는 문자열을 검색한다.

target.test(regExp);    // true
```

<br>

#### 공백

`\s`는 공백 문자를 의미한다.

``` js
const target = 'Is this all there is?';

const regExp = /\s+/g;  // 공백 문자를 검색한다.

target.match(regExp);    // [' ', ' ', ' ', ' ']
```

<br>

참고로 `\S`는 공백 문자가 아닌 문자를 의미한다.

``` js
const target = 'Is this all there is?';

const regExp = /\S+/g;  // 공백 문자가 아닌 문자를 검색한다.

target.match(regExp);    // ['Is', 'this', 'all', 'there', 'is?']
```

<br>


끝.

