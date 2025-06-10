### Chapter 38 - 브라우저의 렌더링 과정

대부분의 프로그래밍 언어는 운영체제(OS)나 가상 머신(VM) 위에서 실행되지만  
웹 어플리케이션의 클라이언트 사이드 자바스크립트는 브라우저에서 HTML과 CSS와 함께 실행된다.  
> 브라우저가 HTML, CSS, 자바스크립트로 작성된 텍스트 문서를 어떻게 파싱(해석)하여 브라우저에 렌더링되는지 알아보자.

<br>

**※ 파싱(Parsing)**  
파싱(구문 분석, Syntax Analysis)은 프로그래밍 언어의 문법에 맞게 작성된 텍스트 문서를 읽어 들여 실행하기 위해  
텍스트 문서의 문자열을 토큰(Token)으로 분해하고, 토큰에 문법적인 의미와 구조를 반영하여  
트리 구조의 자료구조인 Parse Tree를 생성하는 일련의 과정을 의미한다.

일반적으로 파싱이 완료된 이후에는 Parse Tree를 기반으로 중간 언어인 바이트코드(Bytecode)를 생성하고 실행한다.

**※ 렌더링(Rendering)**  
랜더링은 HTML, CSS, 자바스크립트로 작성된 문서를 파싱하여 <font color='orange'>브라우저에 시각적으로 출력</font>하는 것을 의미한다.

<br>

브라우저는 아래와 같은 과정을 거쳐 랜더링을 수행한다.
1) 브라우저는 HTML, CSS, 자바스크립트, 이미지, 폰트 파일 등 렌더링에 필요한 리소스를 요청하고 서버로부터 응답을 받는다.
2) 브라우저의 렌더링 엔진은 서버로부터 응답된 HTML과 CSS를 파싱하여 DOM, CSSOM을 생성하고 이들을 결합하여 렌더 트리를 만든다.
3) 브라우저의 자바스크립트 엔진은 서버로부터 응답된 자바스크립트를 파싱하여 AST(Abstract Syntax Tree)를 생성하고 바이트코드로 변환하여 실행한다. 이때 자바스크립트는 DOM API를 통해 DOM이나 CSSOM을 변경할 수 있다. 변경된 DOM과 CSSOM은 다시 렌더 트리로 결합된다.
4) 렌더 트리를 기반으로 HTML 요소의 레이아웃(위치와 크기)을 계산하고 브라우저 화면에 HTML 요소를 페인팅한다.

<br><br>

### 요청과 응답
**브라우저의 핵심 기능**은 필요한 리소스(HTML, CSS, 자바스크립트, 이미지, 폰트 파일 등)를 서버에 요청하고 서버로부터 응답받은 리소스를 파싱하여 렌더링하는 것이다.

<br>

**※ 요청(Request)**  
요청(Request)은 클라이언트에서 서버로 보내는 데이터를 의미한다.

**※ 응답(Response)**  
응답(Response)은 서버에서 클라이언트로 보내는 데이터를 의미한다.

<br>

※ 요청과 응답은 개발자 도구의 Network 패널에서 확인할 수 있다.  

<br><br>

### HTTP 1.1와 HTTP 2.0
HTTP/1.1은 기본적으로 커넥션(Connection)당 하나의 요청과 응답만 처리한다.
> 여러 개의 요청을 한 번에 전송할 수 없고, 응답 또한 순차적으로 처리된다.

이와 같은 특징으로 HTTP/1.1은 동시 전송이 불가능하여  
요청할 리소스의 개수에 비례하여 응답 시간도 증가한다는 단점이 있다.

<br>

HTTP/2는 커넥션 당 여러 개의 요청과 응답, 즉 다중 요청/응답이 가능하다.  
따라서 HTTP/2.0은 여러 리소스의 동시 전송이 가능하므로 HTTP/1.1보다 페이지 로드 속도가 약 50% 정도 빠르다고 알려져 있다.

<br><br>

### HTML 파싱과 DOM 생성
브라우저의 요청에 의해 서버가 응답한 HTML 문서는 순수 텍스트이다.  
순수 텍스트인 HTML 문서를 브라우저에 시각적인 픽셀로 렌더링하려면 HTML 문서를 브라우저가 이해할 수 있는 자료구조로 변환하여 메모리에 저장해야 한다.

예를 들어서 아래와 같은 index.html이 서버로부터 응답되어싿고 가정해보자.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Hello World</h1>
    <script src="app.js"></script>
</body>
</html>
```

브라우저의 렌더링 엔진은 아래의 그림과 같은 과정을 거쳐 응답받은 HTML 문서를 파싱하여  
브라우저가 이해할 수 있는 자료구조인 DOM(Document Object Model)을 생성한다.

<img src="https://github.com/user-attachments/assets/ce843bce-b7fd-4fb4-bc97-14f5de42ab3e">

<br>

1) 서버에 존재하던 HTML 파일이 브라우저의 요청에 의해 응답된다. 이때 서버는 브라우저가 요청한 HTML 파일을 읽어 들여 메모리에 저장한 다음 메모리에 저장된 바이트(2진수)를 응답한다.
2) 응답된 바이트 형태의 HTML 문서는 meta 태그의 `charset` 어트리뷰트에 의해 지정된 인코딩 방식(UTF-8 등)을 기준으로 문자열로 변환된다.  
3) 문자열로 변환된 HTML 문서를 읽어 들여 문법적 의미를 갖는 코드의 최소 단위인 토큰(Token)으로 분해된다.
4) 각 토큰들은 객체로 변환되어 노드(Node)가 된다. 토큰의 내용에 따라 문서 노드, 요소 노드, 어트리뷰트 노드, 텍스트 노드가 생성되고 이들을 트리 자료구조인 DOM을 형성한다.
5) *모든 노드들은 트리 자료구조로 구성된다. 이 노드들로 구성된 트리 자료구조를 DOM(Document Object Model)이라 한다.*

<br><br>

### CSS 파싱과 CSSOM 생성
렌더링 엔진은 HTML을 한줄 씩 순차적으로 파싱하며 DOM을 생성한다.  
DOM을 생성해 나가다가 CSS를 로드하는 `link` 태그나 `style` 태그를 만나면 DOM 생성을 일시 중단한다.  

그리고 `link` 태그의 `href` 어트리뷰트에 지정된 CSS 파일을 서버에 요청하여  
로드한 CSS 파일이나 `style` 태그 내의 CSS를 HTML과 동일한 파싱 과정을 거치며 해석하여 CSSOM(CSS Object Model)을 생성한다.

아래의 예제를 살펴보자.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css">
    <title>Document</title>
</head>
<body>
    <h1>Hello World</h1>
    <script src="app.js"></script>
</body>
</html>
```

위 예제에서 CSS 파일을 로드하는 `link` 태그가 있다.  
렌더링 엔진은 `meta` 태그까지 HTML을 순차적으로 해석한 다음, `link` 태그를 만나면 DOM 생성을 일시 중단하고  
`link` 태그의 `href` 어트리뷰트에 지정된 CSS 파일을 서버에 요청한다.  

서버로부터 CSS 파일이 응답되면 렌더링 엔진은 HTML과 동일한 해석 과정을 거쳐 CSS를 파싱하여 CSSOM을 생성한다.

<br><br>

### 렌더 트리 생성
렌더링 엔진은 서버로부터 응답된 HTML과 CSS를 파싱하여 각각 DOM과 CSSOM을 생성한다.  
그리고 DOM과 CSSOM은 렌더링을 위해 렌더 트리(render tree)로 결합된다.

렌더 트리는 렌더링을 위한 트리 구조의 자료구조이다.  
> 즉, 브라우저 화면에 렌더링 되지 않는 노드(meta 태그, script 태그 등)와 CSS에 의해 비표시(display: none)되는 노드들은 포함되지 않는다.

<br>

이후 완성된 렌더 트리는 각 HTML 요소의 레이아웃을 계산하는 데 사용되며  
브라우저 화면에 픽셀을 렌더링하는 페인팅(painting) 처리에 입력된다.  

<img src="https://github.com/user-attachments/assets/878a2422-8aef-4bb9-ab38-b0ed2c0c8d24">

<br>

렌더링 과정은 반복해서 실행될 수 있다.  
아래와 같은 상황에서는 반복해서 레이아웃 계산과 페인팅이 재차 실행된다.
- 자바스크립트에 의한 노드 추가 또는 삭제
- 브라우저 창의 리사이징에 의한 뷰포트(viewport) 크기 반영
- HTML 요소의 레이아웃(위치, 크기)에 변경을 발생시키는 `width`, `height`, `margin`, `padding` 등의 스타일 변경

<br>

<font color='orange'>레이아웃 계산과 페인팅을 다시 수행하는 리렌더링은 비용이 많이 드는, 성능에 영향을 주는 작업이다.</font>


<br><br>

### script 태그의 async/defer 어트리뷰트

앞에서 살펴본 자바스크립트 파싱에 의해 DOM 생성이 중단(blocking)되는 문제를 근본적으로 해결하기 위해  
HTML5부터 script 태그에 `async` 어트리뷰트와 `defer` 어트리뷰트가 도입되었다.

<br>

#### async 어트리뷰트
HTML 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행된다.  
단, 자바스크립트의 파싱과 실행은 자바스크립트 파일의 로드가 완료된 직후 진행되며, 이때 HTML 파싱이 중단된다.

여러 개의 `script` 태그에 `async` 어트리뷰트를 지정하면 `script` 태그의 순서와는 상관없이 로드가 완료된다.  
> 순서 보장이 필요한 `script` 태그에는 `async` 어트리뷰트를 지정하지 않는 것이 좋다.

<br>

#### defer 어트리뷰트
HTML 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행된다.  
단, 자바스크립트의 파싱과 실행은 HTML 파싱이 완료된 직후, 즉 DOM 생성이 완료된 이후에 진행된다.

여러 개의 `script` 태그에 `defer` 어트리뷰트를 지정하면 `script` 태그의 순서대로 로드되며, 로드가 완료된 직후 파싱이 시작된다.  
> 순서 보장이 중요한 `script` 태그에는 `defer` 어트리뷰트를 지정하는 것이 좋다.

<br>

끝.