# Chapter 08 - 제어문
제어문은 조건에 따라 블록 분기를 하거나 반복 실행할 때 사용됩니다.  
일반적으로 코드는 위에서 아래로 순차적으로 진행되지만,   
제어문을 사용하면 코드의 실행 흐름을 인위적으로 제어할 수 있습니다.

하지만 코드의 실행 순서가 변경된다는 것은 단순히 위에서 아래로 순차적으로 진행하는  
직관적인 코드의 흐름을 혼란스럽게 합니다.

`forEach`, `map`, `filter`, `reduce`와 같은 고차 함수를 사용한 함수형 프로그래밍 기법에서는  
제어문의 사용을 억제하여 복잡성을 해결하려 노력합니다.  


<br><br>

### 블록문
블록문은 0개 이상의 0개 이상의 문을 중괄호로 묶은 것입니다.  
자바스크립트는 블록을 하나의 실행 단위로 취급합니다.  
블록을 단독으로 사용할 수도 있지만, 일반적으로 제어문이나 함수를 정의할 때 사용하는 것이 일반적입니다.

문의 끝에는 세미콜론(`;`)을 붙이는 것이 일반적이지만,
블록문은 언제나 문의 종료를 의미하는 자체 종결성을 가지고 있기 때문에  
블록문의 끝에는 세미콜론을 붙이지 않습니다.  

``` js
// 블록문
{
    let tmp = 10;
}

// 제어문
let tmp = 1;
if (tmp < 10) {
    x++;
}

// 함수 선언문
function sum(a, b) {
    return a + b;
}
```

<br><br>

### 조건문
조건문은 조건식의 평가 결과에 따라   
코드 블록의 실행을 결정합니다.  
조건식은 boolean 값으로 평가될 수 있는 표현식입니다.

자바스크립트에서는 `if ... else`, `switch`문이 있다.

<br>

#### if ... else 문
`if ... else`문은 주어진 조건식의 평가 결과(참 또는 거짓)에 따라 실행할 코드 블록을 결정합니다.  

``` js
if (조건식) {
    // true일 경우
} else {
    // false일 경우
}
```

조건식을 추가하여 조건에 따라 실행할 블록을 늘리고 싶다면 `else if` 문을 추가하면 됩니다.

``` js
if (조건식 1){
    
} else if (조건식 2) {

} else {

}
```

``` js
let num = 10;
let kind;

if (num > 0)        kind = "양수";
else if (num < 0)   kind = "음수";
else                kind = "0"
```

<br>

대부분의 `if ... else`문은 삼항 조건 연산자로 변환이 가능합니다.

``` js
let x = 1;
let result;

if (x % 2) {
    result = "홀수";
} else {
    result = "짝수";
}
```

위 예제는 아래와 같이 삼항 조건 연산자로 바꿀 수 있습니다.

``` js
let x = 1;

let result = x % 2 ? "홀수" : "짝수"
```

<br>

만약 경우의 수가 3가지 이상이라면 다음과 같이 바꿔 쓸 수도 있습니다.

``` js
let num = 1;
let kind = num ? (num > 0 ? "양수" : "음수") : "0";
```

<br>

조건에 따라 단순히 값을 결정하여 변수에 할당하는 경우에는 삼항 조건 연산자를 사용하는 편이 가독성이 좋지만,  
조건에 따라 실행해야 할 내용이 복잡하여 여러 줄의 문이 필요하다면 `if ... else`를 사용하는 편이 좋습니다.


<br><br>

#### switch 문
switch 문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 `case` 문으로 실행 흐름을 옮깁니다.  
`case` 문은 상황을 의미하는 표현식을 지정하고 콜론으로 마칩니다.  
그리고 그 뒤에 실행할 문들을 위치시킵니다.

<br>

`switch` 문의 표현식과 일치하는 `case` 문이 없다면 실행 순서는 `default` 문으로 이동합니다.  
`default` 문은 선택 사항으로, 사용할 수도 있고 사용하지 않을 수도 있습니다.  

``` js
switch (표현식) {
    case 표현식 1:
        console.log("표현식 1");
        break;
    case 표현식 2:
        console.log("표현식 2");
        break;
    default:
        console.log("default");
}
```

`switch` 문의 표현식은 boolean 값 보다는 문자열이나 숫자 값인 경우가 많습니다.  
즉, `switch` 문은 논리적 차모, 거짓 보다는 상황(case)에 따라 실행할 블록을 결정할 때 사용되빈다.  


<br>

`default`를 제외한각 각 블록에는 `break`를 넣는 것이 일반적이지만,  
아래와 같이 `break`를 넣지 않는 것이 유용한 경우도 있다.

``` js
let year = 2024;
let month = 3;
let days = 15;

switch (month) {
    case 1: case 3: case 5: case 7: case 8: case 10: case 12:
        days = 31;
        break;
    case 4: case 6: case 9: case 11:
        days = 30;
        break;
    case 2:
        days = ((year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0)) ? 29 : 28;
        break;
    default:
        console.log("잘못 된 month 입니다.");
}
```

C언어를 기반으로 하는 프로그래밍은 대부분 `switch`문을 지원하지만  
파이썬과 같이 `switch` 문을 지원하지 않는 프로그래밍 언어도 있습니다.

<br><br>

### 반복문
반복문은 평가 결과가 참인 경우 코드 블록을 실행합니다.  
그 후 조건식을 다시 평가하여 여전히 참일 경우에 코드 블록을 다시 실행시킵니다.

자바스크립트는 `for`, `while`, `do ... while` 문을 제공합니다.

<br>

#### for 문
`for` 문은 조건식이 거짓으로 평가될 때 까지 코드 블록을 반복 실행합니다.  

``` js
for (변수 선언문 또는 할당문; 조건식; 증감식) {
    // 조건식이 참일 경우 실행될 문
}
```

``` js
for (let i = 0; i < 12; i++) {
    console.log(i);
}
```

<br>

`for`문의 변수 선언문, 조건식, 증감식은 모두 옵션이므로 반드시 사용할 필요는 없습니다.  
단, 어떤 식도 선언하지 않으면 무한 루프가 됩니다.  

``` js
// infinity loop
for (;;) { ... }
```

<br>

`for` 문을 중첩해서 사용할 수도 있습니다.  

``` js
for (let i = 0; i < 12; i++) {
    for (let j = 0; j < 12; j++) {
        console.log(i * j);
    }
}
```

<br><br>

#### while 문
`while` 문은 조건식의 평가 결과가 참이면 코드 블록을 반복 실행합니다.  
<font color='orange'>`for` 문은 반복 횟수가 명확할 때 사용하고, `while`문은 반복 횟수가 불명확할 때 주로 사용합니다.</font>

``` js
let cnt = 0;

while (cnt < 3) {
    console.log(cnt);
    cnt++;
}
```

조건식의 평가 결과가 언제나 참이면 무한 루프가 됩니다.  
무한 루프 탈출을 위해서는 `break` 문을 추가하면 됩니다.

``` js
while (true) { ... }
```

<br><br>

#### do ... while 문
`do ... while` 문은 코드 블록을 먼저 실행하고 조건식을 평가합니다.  
따라서 코드 블록은 무조건 한 번 이상 실행됩니다.

``` js
let cnt = 0;

do {
    console.log(cnt); 
    cnt++;
} while (cnt < 3);
```


<br><br>

### break 문
`break` 문은 `while` 문에서 살펴보았듯이 코드 블록을 탈출합니다.  
정확히는 코드 블록을 탈출하는 것이 아니라, 레이블 문, 반복문 또는 `switch` 문의 코드 블록을 탈출합니다.  

> 레이블 문 (label statement)
> ``` js
> // foo라는 레이블 식별자가 붙은 레이블 문
> foo: console.log('foo');
> ```

<br>

레이블 문은 프로그램의 실행 순서를 제어하는 데 사용됩니다.  
`switch` 문의 `case` 문과 `default` 문도 레이블 문입니다.  

``` js 
foo: {
    console.log(1);
    break foo;  // foo 레이블 블록문을 탈출
    console.log(2);
}
```

<br>

중첩된 `for` 문의 내부 `for` 문에서 break 문을 사용하면  
내부 `for` 문을 탈출하여 외부 `for` 문으로 진입하게 됩니다.  

이때 내부 `for` 문이 아닌 외부 `for` 문을 탈출하려면 레이블 문을 사용하면 됩니다.  

``` js
// 즉시 외부로 탈출
outer: for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
        break outer;
    }
}
``` 

레이블 문은 중첩 for 문으 외부로 탈출할 때 유용하지만 그 밖의 경우에는 일반적으로 권장하지 않습니다.  
레이블 문을 사용하면 프로그램의 흐름이 복잡해져서 가독성이 나빠지고 오류를 발생시킬 가능성이 높아지기 때문이다. 

<br><br>

### continue 문
`continue` 문은 반복문의 코드 블록 실행을 현 시점에서 중단하고  
반복문의 증감식으로 실행 흐름을 이동시킵니다.  

아래 코드는 특정 문자의 수를 세는 예입니다.  
``` js
let string = "hello world";
let search = "l";
let cnt = 0;

for (let i = 0; i < string.length; i++) {
    if (string[i] !== search) continue;
    cnt++;
}
```

위 예제의 for 문은 아래와 동일하게 동작한다.

``` js
for (let i = 0; i < string.length; i++) {
    if (string[i] === string) count ++;
}
```

if 문 내에서 실행해야 할 코드가 한 줄이라면 `continue` 문을 사용했을 때보다 간편하고 가독성이 좋다.  
하지만 `if` 문 내에서 실행해야 할 코드가 길다면  
들여쓰기가 한 단계 더 깊어지므로 `continue` 문을 사용하는 편이 더 좋다.

<br><br>