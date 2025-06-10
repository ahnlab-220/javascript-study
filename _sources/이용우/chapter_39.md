### Chapter 39 - DOM

### 노드
HTML 요소는 HTML 문서를 구성하는 개별적인 요소를 의미한다.  

<img src="https://github.com/user-attachments/assets/0b463d11-896e-4003-ab0b-6f27abc36bec">

HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다.  
이때 HTML 요소의 어트리뷰트는 어트리뷰트 노드로, HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환된다.

<br>

HTML 문서는 HTML 요소들의 집합이며, HTML 요소는 중첩 관계를 갖는다.  
즉, HTML 요소의 콘텐츠 영역에는 텍스트 뿐만 아니라 다른 HTML 요소도 포함할 수 있다.  

<br>

#### 노드 객체의 타입
DOM은 노드 객체의 계층적인 구조로 구성된다.  
노드 객체의 종류는 12개이며 상속 구조를 갖는다.  

여러 개의 노드 중 가장 중요한 노드 타입은 4가지이다.

##### 문서 노드(Document Node)
<font color='orange'>문서 노드는 DOM 트리의 최상위에 존재하는 루트 노드이다. </font>  
`document` 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체로서 전역 객체 `window`의 `document` 프로퍼티에 바인딩되어 있다.  
따라서 문서 노드는 `window.document` 또는 `document`로 참조할 수 있다.  

<br>

`document` 객체는 DOM 트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 진입점(entry point) 역할을 담당한다.  
즉, 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 한다.

<br>

##### 요소 노드(Element Node)
요소 노드는 HTML 요소를 가리키는 객체이다.  
요소 노드는 HTML 요소 간 중첩에 의해 부자 관계를 가지며, 이 부자 관계를 통해 정보를 구조화한다.  

따라서 요소 노드는 문서의 구조를 표현한다고 할 수 있다.

<br>

##### 어트리뷰트 노드(Attribute Node)
어트리뷰트 노드는 HTML 요소의 어트리뷰트를 가리키는 객체이다.  
어트리뷰트 노드는 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있다.

단, 요소 노드는 부모 노드와 연결되어 있지만  
<font color='orange'>어트리뷰트 노드는 부모 노드와 연결되어 있지 않고 요소 노드에만 연결되어 있다.</font>

<br>

##### 텍스트 노드(Text Node)
텍스트 노드는 HTML 요소의 텍스트를 가리키는 객체이다.  
<font color='orange'>텍스트 노드는 요소 노드의 자식 노드이며, 요소 노드와 연결되어 있다.</font>

<br>

#### 노드 객체의 상속 구조
DOM은 HTML 문서의 계층적 구조와 정보를 표현하며, 이를 제어할 수 있는 API, 즉 DOM API를 제공한다.  

DOM을 구성하는 노드 객체는 ECMAScript 사양에 정의된 표준 빌트인 객체가 아니라  
브라우저 환경에서 추가적으로 제공하는 호스트 객체(host objects)다.  
하지만 노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 갖는다.  

<br>

<img src="https://github.com/user-attachments/assets/6b360bad-f23a-459f-86c7-97a1f7794ea9">

<br>


노드 객체에는 노드 객체의 종류와 상관없이 모든 노드 객체가 공통으로 갖는 기능도 있고,  
노드 타입에 따라 고유한 기능도 있다. 

예를 들어, 모든 노드 객체는 공통적으로 이벤트를 발생시킬 수 있다.  
이벤트와 관련된 기능(EventTarget.addEventListener, EventTarget.removeEventListener, EventTarget.dispatchEvent)은  
EventTarget 인터페이스가 제공한다.  

이와 같은 노드 관련 기능은 `Node` 인터페이스가 제공한다.

<br>

요소 노드 객체는 HTML 요소의 종류에 따라 고유한 기능도 있다.  
예를 들어, input 요소 노드 객체는 value 프로퍼티가 필요하지만 div 요소 노드 객체는 value 프로퍼티가 필요하지 않다.

<br><br>

DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론  
노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메소드의 집합인 DOM API로 제공한다.  
이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.

> DOM이 제공하는 프로퍼티와 메소드를 사용하여 노드에 접근하고 HTML의 구조나 내용 또는 스타일 등을 동적으로 변경하는 방법을 익히는 것이다.
> HTML은 단순히 태그나 어트리뷰트를 선언적으로 배치하여 뷰를 구성하는 것 이상의 의미를 가진다.  
> HTML은 DOM과 연관 지어 바라봐야 한다.

<br><br>

### 요소 노드 취득
#### id를 이용한 요소 노드 취득
`Document.prototype.getElementById` 메소드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드 객체를 반환한다.  

id 값은 HTML 문서 내 유일한 값이어야 하며, class 어트리뷰트와는 달리 공백 문자로 구분하여 여러 개의 값을 가질 수 없다.  
단, HTML 문서 내에 중복된 id 값을 갖는 HTML 요소가 여러 개 존재하더라도 어떠한 에러도 발생하지 않는다.  
> `getElementById` 메소드는 단 하나의 요소 노드를 반환한다.

<br>

``` js
const h1 = document.getElementById('hello');
console.log(h1);
```

<br><br>

#### 태그 이름을 이용한 노드 취득
`Document.prototype/Element.prototype.getElementsByTagName` 메소드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드 객체들을 반환한다.  
`getElementsByTagName` 메소드는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 `HTMLCollection` 객체를 반환한다.  

<br>

``` js
const $all = document.getElementsByTagName('*');
console.log($all);
```

> DOM 요소를 참조하는 변수 이름 앞에 `$` 기호를 붙이는 것은 자바스크립트 개발자들의 암묵적 규칙이다.

<br><br>

#### class를 이용한 요소 노드 취득
`Document.prototype/Element.prototype.getElementsByClassName` 메소드는 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드 객체들을 반환한다.  
`getElementsByClassName` 메소드는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 `HTMLCollection` 객체를 반환한다.  

<br>

``` js
const $all = document.getElementsByClassName('container');
console.log($all);
```

<br><br>

#### CSS 선택자를 이용한 요소 노드 취득
CSS 선택자(Selector)는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법이다.

``` css
/* 전체 선택자: 모든 요소를 선택 */
* { ... }

/* 태그 선택자: 태그 이름으로 요소를 선택 */
p { ... }

/* id 선택자: id 어트리뷰트 값으로 요소를 선택 */
#container { ... }

/* class 선택자: class 어트리뷰트 값으로 요소를 선택 */
.container { ... }

/* 속성 선택자: 속성 값으로 요소를 선택 */
input[type="text"] { ... }

/* 후손 선택자: 하위 후손 요소를 선택 */
div p { ... }

/* 자식 선택자: 자식 요소를 선택 */
div > p { ... }

/* 가상 클래스 선택자: hover 상태인 a 요소를 모두 선택 */
a:hover { ... }

/* 가상 요소 선택자: p 요소의 콘텐츠의 앞에 위치하는 공간을 선택 */
p::before { ... }

```

<br><br>

`Document.prototype/Element.prototype.querySelector` 메소드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드 객체를 반환한다.  
- 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 여러 개인 경우 첫 번째 요소 노드만 반환한다.
- 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 없는 경우 null을 반환한다.
- 인수로 전달한 CSS 선택자가 문법에 맞지 않으면 DOMException 에러가 발생한다.

<br>

``` js
// class 어트리뷰트 값이 'banana'인 요소 노드를 탐색하여 반환한다.
const $element = document.querySelector('.banana');
console.log($element);
```

<br><br>

#### 특정 요소 노드를 취득할 수 있는지 확인
`Element.prototype.matches` 메소드는 인수로 전달한 CSS 선택자를 만족시키는 요소 노드를 반환한다.  

<br>

``` js
const $element = document.querySelector('.banana');
console.log($element.matches('.banana'));

// #fruits > li.apple 요소를 탐색하여 반환한다.
console.log($element.matches('$fruits > li.apple'));

```

<br><br>

### DOM 조작
DOM 조작은 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것을 의미한다.

<br>

#### innerHTML
요소 노드의 HTML 마크업을 취득하거나 변경할 수 있다.

``` js
const $elem = document.querySelector('.banana');

// 요소 노드의 HTML 마크업을 취득한다.
console.log($elem.innerHTML);

// 요소 노드의 HTML 마크업을 변경한다.
$elem.innerHTML = '<p>Hello</p>';

```

※ HTML 마크업 문자열을 파싱하므로 크로스 사이트 스크립팅 공격에 취약하다.


<br>

#### insertAdjacentHTML
요소 노드의 앞이나 뒤에 HTML 마크업을 추가할 수 있다.

``` js
const $elem = document.querySelector('.banana');

// 요소 노드의 앞에 HTML 마크업을 추가한다.
$elem.insertAdjacentHTML('beforebegin', '<p>Hello</p>');

```

<br>

기존 요소에는 영향을 주지 않고 새롭게 삽입될 요소만을 파싱하여 자식 요소로 추가하기 때문에  
기존의 자식 노드를 모두 제거하고 처음부터  새롭게 자식 노드를 생성하여 자식 요소로 추가하는 innerHTML 프로퍼티보다 효율적이고 빠르다.

※ HTML 마크업 문자열을 파싱하므로 크로스 사이트 스크립팅 공격에 취약하다.


<br><br>

#### 노드 생성과 추가
DOM은 노드를 생성하고 추가하는 메소드를 제공한다.

<br>

##### 노드 생성
`Document.prototype.createElement` 메소드는 인수로 전달한 태그 이름의 요소 노드 객체를 생성하여 반환한다.  

<br>

``` js
const $elem = document.createElement('div');
console.log($elem);
```

<br>

##### 노드 삽입
`Node.prototype.appendChild` 메소드는 인수로 전달한 노드를 자식 노드로 추가하고 추가된 노드 객체를 반환한다.  

<br>

``` js
const $elem = document.createElement('div');
console.log($elem);
```

<br>

##### 노드 이동
DOM에 이미 존재하는 노드를 `appendChild` 메소드 또는 `insertBefore` 메소드에 전달하여 노드를 이동할 수 있다.  
> 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가한다.

``` js
const $fruits = document.getElementById('fruits');

// 이미 존재하는 요소 노드 취득
const [$apple, $banana, ] = $fruits.children;

// 이미 존재하는 $apple 요소 노드를 #fruits 요소 노드의 마지막 노드로 이동
$fruits.appendChild($apple);

// 이미 존재하는 $banana 요소 노드를 $apple 요소 노드의 앞에 이동
$fruits.insertBefore($banana, $apple);

```

<br><br>

#### 노드 복사
DOM에 이미 존재하는 노드를 복사하여 새로운 노드를 생성할 수 있다.

true를 인수로 전달할 경우 노드를 깊은 복사하여 모든 자손 노드가 포함된 사본을 생성한다.  
false를 인수로 전달할 경우 노드를 얕은 복사하여 노드 자신만의 사본을 생성한다.

``` js
const $apple = document.getElementById('apple');

// 이미 존재하는 $apple 요소 노드를 복사하여 새로운 노드를 생성한다.
const $newApple = $apple.cloneNode(true);

```

<br><br>

#### 노드 교체
DOM에 이미 존재하는 노드를 교체할 수 있다.

``` js
const $fruits = document.getElementById('fruits');

// 기존 노드와 교체할 요소 노드를 생성
const $newChild = document.createElement('li');
$newChild.textContent = 'orange';

// #fruits 요소 노드의 첫 번째 자식 요소 노드를 $newChild 요소 노드로 교체
$fruits.replaceChild($newChild, $fruits.firstElementChild);

```


<br><br>

### 스타일
#### 클래스 조작
`.`으로 시작하는 클래스 선택자를 사용하여 CSS class를 미리 정의한 다음  
HTML 요소의 class 어트리뷰트 값을 변경하여 스타일을 변경할 수 있다.

단, class 어트리뷰트에 대응하는 DOM 프로퍼티는 class가 아닌 className과 classList이다.  
> 자바스크립트에서 classs는 예약어이기 때문이다.


<br>

##### className
Element.prototype.className 프로퍼티는 요소 노드의 class 어트리뷰트 값을 취득하거나 변경할 수 있다.

``` js
const $box = document.getElementById('.box');

// .box 요소의 class 어트리뷰트 값 취득
console.log($box.className);

// .box 요소의 class 어트리뷰트 값 중에서 'red'만 'blue'로 변경
$box.className = $box.className.replace('red', 'blue');
```

className 프로퍼티는 문자열을 변환하므로 공백으로 구분된 여러 개의 클래스를 반환하는 경우 다루기 불편하다.

<br>

##### classList
Element.prototype.classList 프로퍼티는 요소 노드의 class 어트리뷰트 값을 취득하거나 변경할 수 있다.  
className과 달리 classList는 DOMTokenList 객체를 반환하며, 이 객체는 class 어트리뷰트의 정보를 담은 유사 배열 객체이면서 이터러블이다.  
따라서 classList를 사용하면 개별 클래스를 조작할 수 있어 className보다 더 편리하게 사용할 수 있다.

``` js
const $box = document.getElementById('.box');

// .box 요소의 class 어트리뷰트 값 취득
console.log($box.classList);

// .box 요소의 class 어트리뷰트 값 중에서 'red'만 'blue'로 변경
$box.classList.replace('red', 'blue');

```

<br>

`DOMTokenList` 객체는 아래와 같은 유용한 메소드를 제공한다.  

**add( ... className)**  
add 메소드는 인수로 전달한 1개 이상의 문자열을 class 어트리뷰트 값으로 추가한다.

``` js
$box.classList.add('foo');  // 'foo' 클래스 추가
$box.classList.add('foo', 'bar');  // 'foo'와 'bar' 클래스 추가
```

**remove( ... className)**  
remove 메소드는 인수로 전달한 1개 이상의 문자열을 class 어트리뷰트 값에서 제거한다.

``` js
$box.classList.remove('foo');  // 'foo' 클래스 제거
```

**item(index)**  
item 메소드는 인수로 전달한 인덱스에 해당하는 클래스 명을 반환한다.

``` js
$box.classList.item(0);  // 'foo' 클래스 반환
```

**contains(className)**  
contains 메소드는 인수로 전달한 문자열을 class 어트리뷰트 값에 포함하고 있는지 확인하여 그 결과를 불리언 값으로 반환한다.

``` js
$box.classList.contains('foo');  // true 또는 false 반환
```

**toggle(className)**  
toggle 메소드는 인수로 전달한 문자열을 class 어트리뷰트 값에 포함하고 있는지 확인하여 그 결과를 불리언 값으로 반환한다.

``` js
$box.classList.toggle('foo');  // 'foo' 클래스 추가 또는 제거
```

**replace(oldClassName, newClassName)**  
replace 메소드는 첫 번째 인수로 전달한 문자열을 두 번째 인수로 전달한 문자열로 교체한다.

``` js
$box.classList.replace('foo', 'bar');  // 'foo' 클래스를 'bar' 클래스로 교체
```


<br>

끝.