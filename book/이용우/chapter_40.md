### Chapter 40 - 이벤트

### 이벤트 드리븐 프로그래밍
브라우저는 처리해야 할 특정 사건이 발생하면 이를 감지하여 이벤트를 발생시킨다.  
> 마우스 클릭, 키보드 입력, 마우스 이동 등

어플리케이션이 특정 타입의 이벤트에 대해 반응하여 어떤 일을 하고 싶다면,  
원하는 타입의 이벤트가 발생했을 때 호출될 함수를 브라우저에게 알린다.  

이때 <font color='orange'>이벤트가 발생했을 때 호출될 함수를 이벤트 핸들러</font>라고 하고,  
<font color='orange'>x이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것을 이벤트 핸들러 등록</font>이라고 한다.

<br><br>

이벤트와 그에 대응하는 함수(이벤트 핸들러)를 통해 사용자와 어플리케이션은 서로 상호작용(interaction)을 할 수 있다.   
프로그램의 흐름을 이벤트 핸들러의 호출에 따라 제어하는 프로그래밍 방식을 이벤트 드리븐 프로그래밍이라고 한다.

<br>

### 이벤트 타입
이벤트 타입은 약 200여가지가 있다.  
여기서는 사용 빈도가 높은 이벤트만 다룰 것이다.

<br>

#### 마우스 이벤트

|이벤트 타입|이벤트 발생 시점|
|:---:|:---:|
|click|사용자가 마우스 버튼을 클릭했을 때|
|dblclick|사용자가 마우스 버튼을 더블 클릭했을 때|
|mousemove|사용자가 마우스를 움직였을 때|
|mouseover|사용자가 마우스를 요소 위로 올렸을 때|
|mouseout|사용자가 마우스를 요소 밖으로 나갔을 때|
|mousedown|사용자가 마우스 버튼을 눌렀을 때|
|mouseup|사용자가 마우스 버튼을 놓았을 때|
|mouseenter|사용자가 마우스를 요소 위로 올렸을 때|
|mouseleave|사용자가 마우스를 요소 밖으로 나갔을 때|

<br>

#### 키보드 이벤트

|이벤트 타입|이벤트 발생 시점|
|:---:|:---:|
|keydown|사용자가 키를 눌렀을 때|
|keypress|사용자가 키를 눌렀을 때|
|keyup|사용자가 키를 놓았을 때|

<br>

#### 포커스 이벤트

|이벤트 타입|이벤트 발생 시점|
|:---:|:---:|
|focus|요소가 포커스를 받았을 때|
|blur|요소가 포커스를 잃었을 때|
|focusin|요소가 포커스를 받았을 때|
|focusout|요소가 포커스를 잃었을 때|

<br>

#### 폼 이벤트

|이벤트 타입|이벤트 발생 시점|
|:---:|:---:|
|submit|사용자가 폼을 제출했을 때|
|reset|사용자가 폼을 리셋했을 때(최근에는 사용 안 함)|

<br>

#### 값 변경 이벤트

|이벤트 타입|이벤트 발생 시점|
|:---:|:---:|
|input|사용자가 입력 필드에 입력한 내용을 변경했을 때|
|change|사용자가 입력 필드에 입력한 내용을 변경하고 입력 필드가 포커스를 잃었을 때|
|readystatechange|DOM의 상태가 변경되었을 때|

<br>

#### DOM 뮤테이션 이벤트

|이벤트 타입|이벤트 발생 시점|
|:---:|:---:|
|DOMContentLoaded|DOM이 완전히 로드되었을 때|


<br>

#### 뷰 이벤트

|이벤트 타입|이벤트 발생 시점|
|:---:|:---:|
|scroll|브라우저 창이 스크롤되었을 때|
|resize|브라우저 창 크기가 변경되었을 때|

<br>

#### 리소스 이벤트

|이벤트 타입|이벤트 발생 시점|
|:---:|:---:|
|load|리소스 로드가 완료되었을 때|
|unload|문서가 언로드되었을 때|
|abort|리소스 로드가 중단되었을 때|
|error|리소스 로드가 실패했을 때|

<br><br>

### 이벤트 핸들러 등록
이벤트 핸들러는 이벤트가 발생했을 때 브라우저에 호출을 위임한 함수이다.  
> 이벤트가 발생하면 브라우저에 의해 호출될 함수가 이벤트 핸들러이다.

<br><br>

#### 이벤트 핸들러 어트리뷰트 방식
**HTML 요소의 어트리뷰트 중에는 이벤트에 대응하는 이벤트 핸들러 어트리뷰트가 있다.**  

이벤트 핸들러 어트리뷰트의 이름은 `onclick`과 같이 `on` 접두사와 이벤트 종류를 나타내는 이벤트 타입으로 이루어져 있다.  
이벤트 핸들러 어트리뷰트 값으로 함수 호출문 등의 문(statement)을 할당하면 이벤트 핸들러가 등록된다.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <button onclick="sayHi('Lee')">Click me!</button>
    <script>
        function sayHi(name) {
            alert(`Hi ${name}!`);
        }
    </script>
</body>
</html>
```
이벤트 핸들러 등록은 함수 호출을 브라우저에게 위임하는 것을 의미한다.  
따라서 이벤트 핸들러를 등록할 때 콜백 함수와 마찬가지로 함수 참조를 등록해야 브라우저가 이벤트 핸들러를 호출할 수 있다.  
만약 함수 참조가 아니라 함수 호출문을 등록하면 함수 호출문의 평가 결과가 이벤트 핸들러로 등록된다.  

- 함수 참조: `sayHi` (함수 자체를 가리킴)
- 함수 호출문: `sayHi()` (함수를 실행하고 그 결과값을 반환)

함수를 반환하는 고차 함수 호출문을 이벤트 핸들러로 등록하면 문제가 없지만,  
함수가 아닌 값을 반환하는 함수 호출문을 이벤트 핸들러로 등록하면 브라우저가 이벤트 핸들러를 호출할 수 없다.

<br>

하지만 위 예제는 이벤트 핸들러 어트리뷰트 값으로 함수 호출문을 할당했다.  
<font color='orange'>이때 이벤트 핸들러 어트리뷰트 값은 사실 암묵적으로 생성될 이벤트 핸들러의 함수 몸체를 의미한다.</font>

즉, `onclick="sayHi('Lee')"` 어트리뷰트는 파싱되어 아래와 같은 함수를 **암묵적으로 생성**하고,  
이벤트 핸들러 어트리뷰트 이름과 동일한 키 `onclick` 이벤트 핸들러 프로퍼티에 할당한다.

``` js
function onclick(event) {
    sayHi('Lee');
}
```

> "이벤트 핸들러를 등록하면 이벤트 핸들러의 콜백 함수처럼 동작한다"

<br><br>

#### 이벤트 핸들러 프로퍼티 방식
`window` 객체와 `Document`, `HTMLElement` 타입의 DOM 노드 객체는 이벤트 핸들러 프로퍼티를 가지고 있다.  
이벤트 핸들러 프로퍼티의 키는 이벤트 핸들러 어트리뷰트와 마찬가지로 `onclick`과 같이  
`on` 접두사와 이벤트의 종류를 나타내는 이벤트 타입으로 이루어져 있다.

이벤트 핸들러 프로퍼티에 함수를 바인딩하면 이벤트 핸들러가 등록된다.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <button>Click me!</button>
    <script>
        const $button = document.querySelector('button');

        // 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩
        $button.onclick = function () {
            console.log('button click');
        }
    </script>
</body>
</html>

```
<br><br>

<img src="https://github.com/user-attachments/assets/b362ef9a-a6a7-4fe8-b1af-c14b82aacc65"/>

<br>

"이벤트 핸들러 어트리뷰트 방식"도 결국 DOM 노드 객체의 "이벤트 핸들러 프로퍼티"로 변환되므로  
결과적으로는 이벤트 핸들러 프로퍼티 방식과 동일하다.

<br><br>


#### addEventListener 메서드 방식
EventTarget.prototype.addEventListener 메서드를 사용하여 이벤트 핸들러를 등록할 수 있다.

<img src="https://github.com/user-attachments/assets/eee01845-08a2-46e4-8afc-441291a56fb8"/>

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <button>Click me!</button>
    <script>
        const $button = document.querySelector('button');

        // 이벤트 핸들러 프로퍼티 방식
        $button.onclick = function () {
            console.log('button click');
        }

        // 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩
        $button.addEventListener('click', function () {
            console.log('button click');
        });
    </script>
</body>
</html>
```

이벤트 핸들러 프로퍼티 방식은 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩하지만  
`addEventListener` 메서드 방식은 이벤트 핸들러를 인수로 전달한다.

<br>

``` js
const $button = document.querySelector('button');

// 이벤트 핸들러 프로퍼티 방식
$button.onclick = function () {
    console.log('이벤트 핸들러 프로퍼티 방식');
}

// addEventListener 메서드 방식
$button.addEventListener('click', function () {
    console.log('addEventListener 메서드 방식');
});
```

<br>

이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식의 주요 차이점은 다음과 같다

1. **이벤트 핸들러 중복 등록**
   - 이벤트 핸들러 프로퍼티 방식: **하나의 이벤트 핸들러만 등록 가능**
   - addEventListener 메서드 방식: **여러 개의 이벤트 핸들러 등록 가능**

2. **이벤트 전파 단계 제어**
   - 이벤트 핸들러 프로퍼티 방식: <font color='orange'>이벤트 전파 단계(캡처링/버블링) 제어 불가</font>
   - addEventListener 메서드 방식: <font color='orange'>세 번째 인수로 이벤트 전파 단계 제어 가능</font>

3. **이벤트 핸들러 제거**
   - 이벤트 핸들러 프로퍼티 방식: null을 할당하여 제거
   - addEventListener 메서드 방식: removeEventListener 메서드로 제거

4. **이벤트 위임(Event Delegation)**
   - 이벤트 핸들러 프로퍼티 방식: 이벤트 위임 구현이 어려움
   - addEventListener 메서드 방식: 이벤트 위임 구현이 용이

따라서 addEventListener 메서드 방식을 사용하는 것이 더 유연하고 강력한 이벤트 처리가 가능하다.


<br><br>

### 이벤트 핸들러 제거
`addEventListener` 메서드 방식으로 등록한 이벤트 핸들러는 `removeEventListener` 메서드를 사용하여 제거할 수 있다.

``` js
const $button = document.querySelector('button');

const handleClick = () => {
    console.log('button click');
}

// 이벤트 핸들러 등록
$button.addEventListener('click', handleClick);

// 이벤트 핸들러 제거
$button.removeEventListener('click', handleClick);
```

<br><br>

### 이벤트 객체
이벤트가 발생하면 이벤트에 관련한 다양한 정보를 담고 있는 이벤트 객체가 동적으로 생성된다.  
생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.

``` js
const $msg = document.querySelector('.message');

// 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.
function showCoords(e) {
    $msg.textContent = `clientX: ${e.clientX}, clientY: ${e.clientY}`;
}

// 이벤트 핸들러 등록
document.onclick = showCoords;

```

<br>

<font color='orange'>이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 암묵적으로 할당된다.</font>  
> 브라우저가 이벤트 핸들러를 호출할 때 이벤트 객체를 인수로 전달하기 때문이다.  


<br>

이벤트 핸들러 어트리뷰트 방식의 경우에는 event가 아닌 다른 이름으로는 이벤트 객체를 전달받지 못한다.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<!-- 이벤트 핸들러 어트리뷰트 방식의 경우에는 event가 아닌 다른 이름으로는 이벤트 객체를 전달받지 못한다. -->
<body onclick="showCoords(event)">
    <p>click me</p>
    <em class="message"></em>
    <script>
        const $msg = document.querySelector('.message');

        // 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.
        function showCoords(e) {
            $msg.textContent = `clientX: ${e.clientX}, clientY: ${e.clientY}`;
        }
    </script>
</body>
</html>
```

현대 웹 개발에서는 이벤트 핸들러 어트리뷰트 방식보다는  
이벤트 핸들러 프로퍼티 방식이나 addEventListener 메서드 방식을 권장한다.
> 브라우저가 자동으로 이벤트 객체를 전달하기 때문에 별도로 event를 명시적으로 전달할 필요가 없다.


<br><br>

### 이벤트 전파
DOM 트리 상에 존재하는 DOM 요소 노드에서 발생한 이벤트는 DOM 트리를 통해 전판된다.  
이를 이벤트 전파(event propagation)라고 한다.  

이벤트 전파는 이벤트가 발생한 후 전파되는 순서를 말한다.  
이벤트 전파는 흔히 3단계로 설명된다.

1. **캡처링 단계(capturing phase)**: 이벤트가 상위 요소로 전파되는 단계
2. **타깃 단계(target phase)**: 이벤트가 타깃 요소에 도달한 단계
3. **버블링 단계(bubbling phase)**: 이벤트가 하위 요소로 전파되는 단계

<br>

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <ul id="fruits">
        <li id="apple">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
    </ul>
    <script>
        const $fruits = document.getElementById('fruits');

        // #fruits 요소의 하위 요소인 li 요소를 클릭한 경우
        $fruits.addEventListener('click', (e) => {
            console.log(`이벤트 단계: ${e.eventPhase}`);
            console.log(`이벤트 타깃: ${e.target}`);
            console.log(`currentTarget: ${e.currentTarget}`);
        })
    </script>
</body>
```

li 요소를 클릭하면 클릭 이벤트가 발생하여 클릭 이벤트 객체가 생성되고 클릭된 li 요소가 이벤트 타깃이 된다.   
이때 클릭 이벤트 객체는 `window`에서 시작해서 이벤트 타깃 방향으로 전파된다 **(캡처링 단계)**
> 실제 클릭된 요소(이벤트 타깃)까지 내려옴

이후 이벤트 객체는 이벤트를 발생시킨 이벤트 타깃에 도달한다. **(타깃 단계)**
> 이벤트가 실제로 클릭된 요소에 도달

이후 이벤트 객체는 이벤트 타깃에서 시작해서 `window` 방향으로 전파된다 **(버블링 단계)**
> 이벤트가 클릭된 요소에서 다시 `window` 방향으로 올라감


<br>

이벤트 핸들러 어트리뷰트/프로퍼티 방식으로 등록한 이벤트 핸들러는  
타깃 단계와 버블링 단계의 이벤트만 캐치할 수 있다.  

하지만 `addEventListener` 메서드 방식으로 등록한 이벤트 핸들러는 캡처링 단계의 이벤트도 캐치할 수 있다.  
> 캡처링 단계의 이벤트를 캐치하려면 addEventListener 메서드의 세 번째 인수로 true를 전달하면 된다.

<br>

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <ul id="fruits">
        <li id="apple">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
    </ul>
    <script>
        const $fruits = document.getElementById('fruits');

        // 캡처링 단계의 이벤트를 캐치하려면 addEventListener 메서드의 세 번째 인수로 true를 전달하면 된다.
        $fruits.addEventListener('click', (e) => {
            console.log(`이벤트 단계: ${e.eventPhase}`);
            console.log(`이벤트 타깃: ${e.target}`);
        }, true);
    </script>
</body>
</html>
```


<br><br>


### 이벤트 위임
이벤트 위임(event delegation)은 여러 개의 하위 요소에 같은 이벤트 핸들러를 등록하는 것이 아니라  
하나의 상위 요소에 이벤트 핸들러를 등록하여 하위 요소의 이벤트를 처리하는 방법이다.  

상위 DOM 요소에 이벤트 핸들러를 등록하면 여러 개의 하위 DOM 요소에 이벤트 핸들러를 등록할 필요가 없다.
> 상속과 비슷한 개념이다.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <nav>
    <ul id="fruits">
        <li id="apple" class="active">Apple</li>
            <li id="banana">Banana</li>
            <li id="orange">Orange</li>
        </ul>
    </nav>
    <div>선택된 네비게이션 아이템: <em class="msg">apple</em></div>
    <script>
        const $fruits = document.getElementById('fruits');
        const $msg = document.querySelector('.msg');

        // 사용자 클릭에 의해 선택된 네비게이션 아이템(li 요소)에 active 클래스를 추가
        // 이외 모든 네비게이션 아이템의 active 클래스를 제거
        function activate({ target }) {
            if (!target.matches('#fruits > li')) return;

            [...$fruits.children].forEach($fruit => {
                $fruit.classList.toggle('active', $fruit === target);
                $msg.textContent = target.id;
            });
        }

        // 이벤트 위임: 상위 요소(ul#fruits)는 하위 요소의 이벤트를 캐치할 수 있다.
        $fruits.onclick = activate;
    </script>
</body>
</html>
```

<br>

이벤트 위임으로 하위 DOM 요소에서 발생한 이벤트를 처리할 때 주의할 점은  
상위 요소에 이벤트 핸들러를 등록하기 때문에 이벤트 타깃(이벤트를 실제로 발생시킨 DOM 요소)이 개발자가 기대한 DOM 요소가 아닐 수도 있다는 점이다.
> 범위를 한정하는 것이 중요하다.

`Event.prototype.matches` 메서드를 사용하여 인수로 전달된 선택자에 의해 특정 노드가 탐색 가능한지 검사할 수 있다.

``` js
function activate({ target }) {
    // 이벤트를 발생시킨 요소(target)이 ul#fruits의 자식 요소가 아니라면 무시한다.
    if (!target.matches('#fruits > li')) return;
    ...
}
```

<br>

일반적으로 이벤트 객체의 `target` 프로퍼티와 `currentTarget` 프로퍼티는 동일한 DOM 요소를 가리킨다.  
하지만 이벤트 위임을 통해 상위 DOM 요소를 바인딩한 경우에는 서로 다른 DOM 요소를 가리킬 수 있다.

``` js
$fruits.onclick = activate;
```

위 예제에서 이벤트 객체의 `currentTarget` 프로퍼티는 언제나 `$fruits` 요소를 가리킨다.  
> 이벤트 핸들러가 바인딩된 DOM 요소를 가리킨다.

하지만 `target` 프로퍼티는 이벤트를 발생시킨 DOM 요소를 가리킨다.  
> 이벤트를 실제로 발생시킨 DOM 요소를 가리킨다.

<br><br>

### 이벤트 핸들러 내부의 this
#### 이벤트 핸들러 어트리뷰트 방식
``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <button onclick="handleClick()">Click me!</button>
    <script>
        function handleClick() {
            console.log(this);  // window
        }
    </script>
</body>
</html>
```

이벤트 핸들러 어트리뷰트 값으로 지정한 문자열은 사실 암묵적으로 생성되는 이벤트 핸들러의 문(statement)이다.    
따라서 `handleClick` 함수는 이벤트 핸들러에 의해 일반 함수로 호출된다.

단, 이벤트 핸들러를 호출할 때 인수로 전달한 `this`는 이벤트를 바인딩한 DOM 요소를 가리킨다.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <button onclick="handleClick(this)">Click me!</button>
    <script>
        function handleClick(button) {
            console.log(button);  // 이벤트를 바인딩한 button 요소
            console.log(this);  // window
        }
    </script>
</body>
</html>
```


<br><br>

#### 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식
이벤트 핸들러 프로퍼티 방식과 addEventListener 메소드 방식 모드 모두  
이벤트 핸들러 내부의 `this`는 이벤트를 바인딩한 DOM 요소를 가리킨다.

> 이벤트 핸들러 내부의 this는 이벤트 객체의 currentTarget 프로퍼티와 같다.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <button class="btn1">0</button>
    <button class="btn2">0</button>

    <script>
        const $btn1 = document.querySelector('.btn1');
        const $btn2 = document.querySelector('.btn2');

        // 이벤트 핸들러 프로퍼티 방식
        $btn1.onclick = function (e) {
            // this는 이벤트를 바인딩한 DOM 요소를 가리킨다
            console.log(this);  // $btn1
            console.log(e.currentTarget);  // $btn1
            console.log(this === e.currentTarget);  // true

            // $btn1 요소의 텍스트 콘텐츠를 1 증가시킨다.
            ++this.textContent;
        }

        // addEventListener 메서드 방식
        $btn2.addEventListener('click', function (e) {
            // this는 이벤트를 바인딩한 DOM 요소를 가리킨다
            console.log(this);  // $btn2
            console.log(e.currentTarget);  // $btn2
            console.log(this === e.currentTarget);  // true

            // $btn2 요소의 텍스트 콘텐츠를 1 증가시킨다.
            ++this.textContent;
        })
    </script>
</body>
</html>
```

<br><br>

화살표 함수로 정의한 이벤트 핸들러 내부의 this는 상위 스코프의 this를 가리킨다.  
> 화살표 함수는 함수 자체의 this 바인딩을 가지지 않는다.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <button class="btn1">0</button>
    <button class="btn2">0</button>

    <script>
        const $btn1 = document.querySelector('.btn1');
        const $btn2 = document.querySelector('.btn2');

        // 이벤트 핸들러 프로퍼티 방식
        $btn1.onclick = (e) => {
            // this는 이벤트를 바인딩한 DOM 요소를 가리킨다
            console.log(this);  // window
            console.log(e.currentTarget);  // $btn1
            console.log(this === e.currentTarget);  // false

            // this는 window를 가리키므로 window.targetContent에 NaN(undefined + 1)이 할당된다.
            ++this.textContent;
        }

        // addEventListener 메서드 방식
        $btn2.addEventListener('click', (e) => {
            // this는 이벤트를 바인딩한 DOM 요소를 가리킨다
            console.log(this);  // window
            console.log(e.currentTarget);  // $btn2
            console.log(this === e.currentTarget);  // false

            // this는 window를 가리키므로 window.targetContent에 NaN(undefined + 1)이 할당된다.
            ++this.textContent;
        })
    </script>
</body>
</html>
```


<br><br>


### 이벤트 핸들러에 인수 전달
함수에 인수를 전달하려면 함수를 호출할 때 전달해야 한다.  

이벤트 핸들러 어트리뷰트 방식은 함수 호출문을 사용할 수 있기 때문에 인수를 전달할 수 있짐나,  
이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식은 함수 호출문을 사용할 수 없기 때문에 인수를 전달할 수 없다.

하지만 인수를 전달할 방법이 완전히 없는 것은 아니다.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <label>User Name <input type='text'></label>
    <em class="message"></em>
    
    <script>
        const MIN_USER_NAME_LENGTH = 5;  // 이름 최소 길이
        const $input = document.querySelector('input[type=text]');
        const $msg = document.querySelector('.message');

        const checkUserNameLength = min => {
            $msg.textContent = $input.value.length < min ? `이름은 ${min}자 이상 입력해주세요.` : '';
        }

        // 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달한다.
        $input.onblur = () => {
            checkUserNameLength(MIN_USER_NAME_LENGTH);
        }
    </script>
</body>
</html>
```

위와 같이 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달할 수 있다.

<br>

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <label>User Name <input type='text'></label>
    <em class="message"></em>
    
    <script>
        const MIN_USER_NAME_LENGTH = 5;  // 이름 최소 길이
        const $input = document.querySelector('input[type=text]');
        const $msg = document.querySelector('.message');

        const checkUserNameLength = min => {
            $msg.textContent = $input.value.length < min ? `이름은 ${min}자 이상 입력해주세요.` : '';
        }

        // 이벤트 핸들러를 반환하는 함수를 호출하면서 인수를 전달한다.
        $input.onblur = checkUserNameLength(MIN_USER_NAME_LENGTH);
    </script>
</body>
</html>
```

또는 이벤트 핸들러를 반환하는 함수를 호출하면서 인수를 전달할 수도 있다.

<br><br>

### 커스텀 이벤트
#### 커스텀 이벤트 생성
이벤트 객체는 `Event`, `UIEvent`, `MouseEvent` 같은 이벤트 생성자 함수로 생성할 수 있다.

이벤트가 발생하면 암묵적으로 생성되는 이벤트 객체는  
발생한 이벤트의 종류에 따라 이벤트 타입이 결정된다.  

하지만 `Event`, `UIEvent`, `MouseEvent` 같은 이벤트 생성자 함수를 호출하여  
명시적으로 생성한 이벤트 객체는 임의의 이벤트 타입을 지정할 수 있다.  

이처럼 개발자의 의도로 생성된 이벤트를 커스텀 이벤트라고 한다.


<br>


``` js
// KeyboardEvent 생성자 함수를 호출하여 커스텀 이벤트 객체를 생성
const keyboardEvent = new KeyboardEvent('keyup');
console.log(keyboardEvent.type);  // keyup

// CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new CustomEvent('foo');
console.log(customEvent.type);  // foo
```

생성된 이벤트 객체는 버블링 되지 않으며 `preventDefault` 메소드로 취소할 수도 없다.  
> 커스텀 이벤트 객체는 `bubbles`와 `cancelable` 프로퍼티 값이 `false`이다.

``` js
// MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체 생성
const customEvent = new MouseEvent('click');
console.log(customEvent.type);  // click

// 커스텀 이벤트 객체는 버블링 되지 않으며 `preventDefault` 메소드로 취소할 수도 없다.
console.log(customEvent.bubbles);  // false
console.log(customEvent.cancelable);  // false
```

<br>

커스텀 이벤트 객체의 `bubbles`와 `cancelable` 프로퍼티 값을 `true`로 설정하려면  
이벤트 생성자 함수의 두 번째 인수로 `bubbles`와 `cancelable` 프로퍼티를 갖는 객체를 전달한다.

``` js
// MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new MouseEvent('click', { bubbles: true, cancelable: true });

console.log(customEvent.bubbles);  // true
console.log(customEvent.cancelable);  // true
```

<br>

<font color='orange'>커스텀 이벤트 객체에는 이벤트 타입에 따라 가지는 이벤트 고유의 프로퍼티 값을 지정할 수도 있다.</font>   

> 예를 들어 MouseEvent 생성자 함수로 생성한 마우스 이벤트 객체는  
> 마우스 포인터 좌표 정보를 나타내는 마우스 이벤트 객체 고유의 프로퍼티 screenX, screenY 등을 가진다.

``` js
// MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체를 생성
const mouseEvent = new MouseEvent('click', {
    bubbles: true,
    cancelable: true,
    clientX: 50,
    clientY: 100
});


// keyboardEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체를 생성
const keyboardEvent = new keyboardEvent('keyup', { key: 'Enter'});
console.log(keyboardEvent.key);  // Enter
```


<br><br>


#### 커스텀 이벤트 디스패치(dispatch)
생성된 커스텀 이벤트는 `dispatchEvent` 메소드를 사용하여 디스패치(이벤트를 발생시키는 행위)할 수 있다.  
`dispatchEvent` 메소드에 이벤트 객체를 인수로 전달하면서 호출하면 인수로 전달한 이벤트 타입의 이벤트가 발생한다.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <button class="btn">Click me!</button>

    <script>
        const $button = document.querySelector('.btn');

        // 버튼 요소에 click 커스텀 이벤트 핸들러 등록
        $button.addEventListener('click', e => {
            console.log(e);
            alert(`${e} clicked!`);
        });

        // 커스텀 이벤트 생성
        const customEvent = new CustomEvent('click');

        // 커스텀 이벤트 디스패치(동기 처리), click 이벤트가 발생함
        $button.dispatchEvent(customEvent);
    </script>
</body>
</html>

```

일반적으로 이벤트 핸들러는 비동기(asynchronous) 처리 방식으로 동작한다.    
하지만 <font color='orange'>dispatchEvent 메소드는 이벤트 핸들러를 동기 처리 방식으로 호출한다.</font>

끝.