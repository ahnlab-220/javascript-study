### Chapter 41 - 타이머

### 호출 스케줄링
함수를 명시적으로 호출하면 함수가 즉시 실행된다.  
만약 함수를 명시적으로 호출하지 않고 일정 시간이 지난 뒤에 호출되도록 함수 호출을 예약하려면 타이머 함수를 사용해야 한다.
> 이를 호출 스케줄링이라 부른다.

타이머 함수 `setTimeout`, `setInterval`은 모두 일정 시간이 경과된 이후 콜백 함수가 호출되도록 타이머를 생성한다.  

자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 가지고 있기 때문에  
두 가지 이상의 Task를 동시에 실행할 수 없다. 즉, 자바스크립트 엔진은 싱글 쓰레드로 동작한다. 
> 이 특징으로 타이머 함수들은 비동기 방식으로 동작한다.

<br>

### 타이머 함수
#### setTimeout / clearTimeout
`setTimeout` 함수는 두 번째 인수로 전달받은 시간으로 단 한번 동작하는 타이머를 생성한다.  
이후 타이머가 만료되면 첫 번째 인수로 전달받은 콜백 함수가 호출된다.

``` js
const timeoutId = setTimeout(func|code, [delay], [arg1], [arg2], ...)
```

- `func|code` : 타이머가 만료된 후 실행할 콜백 함수
- `delay` : 타이머 만료 시간(ms)
- `arg1`, `arg2`, ... : 콜백 함수에 전달할 인수

``` js
const timeoutId = setTimeout(() => console.log('Hi!'), 1000);

// 타이머 취소
clearTimeout(timeoutId);
```


<br>

#### setInterval / clearInterval
`setInterval` 함수는 두 번째 인수로 전달받은 시간으로 반복 동작하는 타이머를 생성한다.  
이후 타이머가 만료될 때마다 첫 번째 인수로 전달받은 콜백 함수가 반복 호출된다.

``` js
const intervalId = setInterval(func|code, [delay], [arg1], [arg2], ...)
```

- `func|code` : 타이머가 만료될 때마다 실행할 콜백 함수
- `delay` : 타이머 만료 시간(ms)
- `arg1`, `arg2`, ... : 콜백 함수에 전달할 인수

``` js
const intervalId = setInterval(() => console.log('Hi!'), 1000);

// 타이머 취소
clearInterval(intervalId);
```

<br>

### 디바운스(debounce)와 스로틀(throttle)
디바운스와 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트(scroll, resize, input, mousemove 등)를 그룹화해서  
과도한 이벤트 핸들러의 호출을 방지하는 패턴이다.

<br>

#### 디바운스(debounce)
디바운스는 짧은 시간 간격으로 <font color='orange'>이벤트가 연속해서 발생하더라도 마지막에 한 번만 이벤트 핸들러가 호출되도록 하는 패턴이다.</font>

``` js
const $input = document.querySelector('input');
const $msg = document.querySelector('.msg');

const debounce = (callback, delay) => {
    let timerId;
    
    // debounce 함수는 timerId를 저장하는 클로저를 반환한다.
    return (...args) => {
        // delay가 경과하기 이전에 이벤트가 발생하면 이전 타이머를 취소하고 새로운 타이머를 재설정한다.
        // delay보다 짧은 간격으로 이벤트가 발생하면 callback은 호출되지 않음
        if (timerId) clearTimeout(timerId);

        // delay 이후에 콜백 함수가 호출되도록 타이머를 설정한다.
        timerId = setTimeout(callback, delay, ...args);
    };
};

// debounce 함수는 이벤트 핸들러를 반환한다.
// 호출되지 않다가 500ms 동안 입력이 없으면 마지막 입력 내용을 출력한다.
$input.oninput = debounce(e => {
    $msg.textContent = e.target.value;
}, 500);
```

실무에서는 Underscore의 `debounce` 함수나 Lodash의 `debounce` 함수를 사용하는 것이 좋다.
> 외부 라이브러리로 편하게 사용하자.

<br><br>

#### 스로틀(throttle)
스로틀은 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도  
<font color='orange'>일정 시간 간격으로 이벤트 핸들러가 호출되도록 하는 패턴이다.</font>

``` js
const $input = document.querySelector('input');
const $msg = document.querySelector('.msg');

const throttle = (callback, delay) => {
    let timerId;

    return (...args) => {
        if (timerId) return;
        
        // delay가 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않음
        // delay가 경과되었을 때 이벤트가 발생하면 새로운 타이머를 재설정
        // 즉, delay 간격으로 callback이 호출
        timerId = setTimeout(() => {
            callback(...args);
            timerId = null;
        }, delay);
    };
};

let normalCount = 0;
$continaer.addEventListener('scroll', () => {
    $normalCount.textContent = ++normalCount;
});

let throttleCount = 0;
$continaer.addEventListener('scroll', throttle(() => {
    $throttleCount.textContent = ++throttleCount;
}, 100));
```

실무에서는 Underscore의 `throttle` 함수나 Lodash의 `throttle` 함수를 사용하는 것이 좋다.
> 외부 라이브러리로 편하게 사용하자.


끝.