### Chapter 48 - 모듈

### 모듈의 일반적 의미
모듈(module)이란, 어플리케이션을 구성하는 개별적 요소로서 <font color='orange'>재사용 가능한 코드 조각을 의미</font>한다.

<br>

일반적으로 모듈은 기능을 기준으로 파일 단위로 분리한다.  
이때 모듈이 성립하려면 모듈은 자신만의 파일 스코프(모듈 스코프)를 가질 수 있어야 한다.

자신만의 파일 스코프를 갖는 모듈의 리소스(모듈에 포함되어 있는 변수, 함수, 객체 등)는 기본적으로 비공개 상태이다. 다시 말해, 자신만의 파일 스코프를 갖는 모듈의 모든 자산은 캡슐화 되어 다른 모듈에서 접근할 수 없다. 즉, 모듈은 개별적 존재로서 어플리케이션과 분리되어 존재한다.

<br>

하지만 어플리케이션과 완전히 분리되어 개별적으로 존재하는 모듈은 재사용이 불가능하므로 존재의 의미가 없다.  

모듈은 어플리케이션이나 다른 모듈에 의해 재사용되어야 의미가 있다.  
따라서 뮤들은 공개가 필요한 자신에 한정하여 명시적으로 선택적 공개가 가능하다.  

> 이를 export라고 한다.

<br>

공개(export)된 모듈의 자산은 다른 모듈에서 재사용할 수 있다.  
이때 공개된 모듈의 자산을 사용하는 모듈을 모듈 사용자(module consumer)라고 한다.  

모듈 사용자는 모듈이 공개(export)한 자산 중 일부 또는 전체를 선택해  
자신의 스코프 내로 불러들여 재사용할 수 있다.

> 이를 import라고 한다.



<br>

모듈은 어플리케이션과 분리되어 개별적으로 존재하다가 필요에 따라 다른 모듈에 의해 재사용된다.  
모듈은 기능적으로 분리되어 개별적인 파일로 작성된다.  
따라서 코드의 단위를 명확히 분리하여 어플리케이션을 구성할 수 있고,  
재사용성이 좋아서 개발 효율성과 유지보수성을 높일 수 있다.

<br><br>

### 자바스크립트와 모듈
자바스크립트는 웹 페이지의 단순 보조 기능을 처리하기 위한 제한적인 용도로 태어났다.  
이러한 태생적 한계로 인해 다른 프로그래밍 언어와 비교할 때 부족한 부분이 있다.  

대표적인 것이 모듈 시스템을 지원하지 않는다는 것이다.  
> 자바스크립트는 모듈이 성립하기 위해 필요한 파일 스코프와 `import`, `export`를 지원하지 않는다.

C언어는 `#include`, 자바는 `import` 등 대부분의 프로그래밍 언어는 모듈 기능을 가지고 있다.  
하지만 클라이언트 사이드 자바스크립트는 `script` 태그를 사용하여 외부의 자바스크립트 파일을 로드할 수는 있지만 파일마다 독립적인 파일 스코프를 갖지는 않는다.

다시 말해, 자바스크립트 파일을 여러 개의 파일로 분리하여 `script` 태그로 로드해도  
부닐된 자바스크립트 파일들은 결국 하나의 자바스크립트 파일 내에 있는 것처럼 동작한다.  

> 모든 자바스크립트 파일은 하나의 전역을 공유한다.  
> 따라서 분리된 자바스크립트 파일들의 전역 변수가 중복되는 등의 문제가 발생할 수 있다.

<br>

자바스크립트를 클라이언트 사이드, 즉 브라우저 환경에 국한하지 않고 범옹적으로 사용하려는 움직임이 생기면서 모듈 시스템은 반드시 해결해야 하는 핵심 과제가 되었다.  

이런 상황에서 제안된 것이 `CommonJS`와 `AMD(Asynchronous Module Definition)`이다.  

이로써 자바스크립트의 모듈 시스템은 크게 CommonJS와 AMD로 나뉘었다.  
브라우저 환경에서 모듈을 사용하기 위해서는 CommonJS 또는 AMD를 구현한 모듈 로더 라이브러리를 사용해야 한다.

<br>

자바스크립트 런타임 환경인 Node.js는 모듈 시스템의 사실상 표준인 CommonJS를 채택했다.  
따라서 Node.js 환경에서는 파일별로 독립적인 파일 스코프(모듈 스코프)를 갖는다.


<br><br>

### ES6 모듈(ESM)
이러한 상황에서 ES6에서도 클라이언트 사이드 자바스크립틍서도 동작하는 모듈 기능을 추가했다.  
IE를 제외한 대부분의 브라우저에서 ES6 모듈을 사용할 수 있다.  
ES6 모듈의 사용법은 간단하다. script 태그에 `type="module"` 어트리뷰트를 추가하면 된다.

일반적인 자바스크립트 파일이 아닌 ESM임을 명확히 하기 위해  
ESM의 파일 확장자는 `mjs`를 사용할 것을 권장한다.

``` html
<script type="module" src="lib.mjs"></script>
```

<br><br>

#### 모듈 스코프
ESM은 독자적인 모듈 스코프를 갖는다.  
ESM이 아닌 일반적인 자바스크립트 파일은 scipt 태그로 분리해서 로드해도 독자적인 모듈 스코프를 갖지 않는다.  

``` js
// foo.js
var x = foo;
console.log(window.x);  // foo
```

<br>

``` js
// bar.js
var x = 'bar';
console.log(window.x);  // bar
```

<br>

``` html
<script type="module" src="foo.js"></script>
<script type="module" src="bar.js"></script>
```

<br>

위 예제의 HTML에서 script 태그로 분리해서 로드된 2개의 자바스크립트 파일은 하나의 자바스크립트 파일 내에 있는 것처럼 동작한다.
> 하나의 전역을 공유한다.

따라서 foo.js와 bar.js는 하나의 전역을 공유하므로 전역 변수 x는 중복 선언되어 의도치 않게 값이 변경될 수 있다.

<br>

ESM은 파일 자체의 독자적인 모듈 스코프를 제공한다.  
따라서 모듈 내에서 var 키워드로 선언한 변수는 더는 전역 변수가 아니며 window 객체의 프로퍼티도 아니다.

``` js
// foo.mjs
var x = 'foo';
console.log(x);  // foo
console.log(window.x);  // undefined
```

<br>

``` js
// bar.mjs
var x = 'bar';
console.log(x);  // bar
console.log(window.x);  // undefined
```

<br>

``` html
<script type="module" src="foo.mjs"></script>
<script type="module" src="bar.mjs"></script>
```

<br><br>

#### export 키워드
모듈은 독자적인 모듈 스코프를 갖는다.  
따라서 모듈 내부에서 선언한 모든 식별자는 기본적으로 해당 모듈 내부에서만 참조할 수 있다.  

모듈 내부에서 선언한 식별자를 외부에 공개하여 다른 모듈들이 재사용할 수 있게 하려면 `export` 키워드를 사용한다.

`export` 키워드는 선언문 앞에 사용한다.  

이로써 변수, 함수, 클래스 등 모든 식별자를 export 할 수 있다.


<br>

``` js
// lib.mjs
// 변수의 공개
export const pi = Math.PI;

// 함수의 공개
export function square(x) {
    return x * x;
}

// 클래스의 공개
export class Person {
    constructor(name) {
        this.name = name;
    }
}
```

선언문 앞에 매번 `export` 키워드를 붙이는 것이 번거롭다면 `export`할 대상을 하나의 객체로 구성하여 한번에 `export` 할 수도 있다.

``` js
// lib.mjs
const pi = Math.PI;

function square(x) {
    return x * x;
}

class Person {
    constructor(name) {
        this.name = name;
    }
}

// 변수, 함수 클래스를 하나의 객체로 구성하여 공개
export { pi, square, Person };
```

<br><br>

#### import 키워드
다른 모듈에서 공개(export)한 식별자를 자신의 모듈 스코프 내부로 로드하려면 `import` 키워드를 사용한다.  
다른 모듈이 `export`한 식별자 이름으로 `import`해야 하며 ESM의 경우 파일 확장자를 생략할 수 없다.

<br>

``` js
// app.mjs
import { pi, square, Person } from './lib.mjs';

console.log(pi);
console.log(square(10));
console.log(new Person('Lee'));
```

<br>

``` html
<script type="module" src="app.mjs"></script>
```

위 예제의 `app.mjs`는 어플리케이션의 진입점(entry point)이므로 반드시 `script` 태그로 로드해야 한다. 하지만 `lib.mjs`는 `app.mjs`의 `import`문에 의해 로드되는 의존성이다.  

따라서 `lib.mjs`는 `script` 태그로 로드하지 않아도 된다.

모듈이 export한 식별자 일므을 일일이 지정하지 않고 하나의 이름으로 한 번에 import 할 수도 있다. 이때 import되는 식별자는 as 뒤에 지정한 이름의 객체에 프로퍼티로 할당된다.

``` js
// app.mjs
// lib.mjs 모듈이 export한 모든 식별자를 lib 객체의 프로퍼티로 모아 import 한다.
import * as lib from './lib.mjs';

console.log(lib.pi);
console.log(lib.square(10));
console.log(new lib.Person('Lee'));
```

<br><br>

모듈이 export한 식별자 이름을 변경하여 import 할 수도 있다.

``` js
// app.mjs
import { pi as PI, square as sq, Person as P } from './lib.mjs';

console.log(PI);
console.log(sq(10));
console.log(new P('Lee'));
```

끝.