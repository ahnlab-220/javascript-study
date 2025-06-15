### Chapter 44 - REST API

REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처이고  
REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미한다.

<br>

### REST API의 구성

REST API는 자원, 행위, 표현 3가지 요소로 구성된다.

- 자원: URI
- 행위: HTTP 요청 메서드
- 표현: 페이로드

<br><br>

### REST API 설계 원칙
REST에서 가장 중요한 기본 원칙은 2가지이다.  
- URI는 리소스를 표현하는데 집중한다.
- 행위에 대한 정의는 HTTP 요청 메소드를 통한다.

<br>

#### URI는 리소스를 표현하는데 집중한다.
URI는 리소스를 표현하는 데 중점을 두어야 한다.  
리소스를 식별할 수 있는 이름은 동사보다는 명사를 사용한다. 
``` text
GET /todos/1
GET /users/1
```

<br>

#### 행위에 대한 정의는 HTTP 요청 메소드를 통한다.
HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적을 알리는 수단이다.  
주로 5가지 요청 메소드(GET, POST, PUT, PATCH, DELETE)를 사용하여 CRUD를 구현한다.

| 요청 메서드 | 종류 |
|:---:|:---:|
|GET|리소스 조회|
|POST|리소스 생성|
|PUT|리소스 전체 변경|
|PATCH|리소스 부분 변경|
|DELETE|리소스 삭제|

<br>

리소스에 대한 행위는 HTTP 요청 메소드를 통해 표현한다.

``` text
DELETE /todos/1
```

<br><br>

### JSON Server를 이용한 REST API 실습

#### JSON Server 설치

``` bash
$ mkdir json-server-exam && cd json-server-exam
$ npm init -y
$ npm install json-server --save-dev
```

<br>

#### db.json 파일 생성
프로젝트 루트 폴더(/json-server-exam)에 db.json 파일을 생성한다.  
db.json 파일은 리소스를 제공하는 데이터베이스 역할을 한다.

``` json
{
    "todos": [
        { "id": 1, "content": "HTML", "completed": false },
        { "id": 2, "content": "CSS", "completed": true },
        { "id": 3, "content": "JavaScript", "completed": false }
    ]
}
```

<br>

#### JSON Server 실행
JSON Server를 실행한다.  
JSON Server가 데이터베이스 역할을 하는 db.json 파일의 변경을 감지하게 하려면 `watch` 옵션을 추가한다.

``` bash
$ json-server --watch db.json --port 4000
```

<br>

매번 명령어 입력하는 것이 번거롭다면 `package.json` 파일의 scripts를 아래와 같이 수정하여 JSON Server를 실행하면 된다.

``` json
{
    "name": "json-server-exam",
    "version": "1.0.0",
    "scripts": {
        "start": "json-server --watch db.json --port 4000"
    },
    "devDependencies": {
        "json-server": "^0.17.0"
    }
}
```

<br>

터미널에서 `npm start` 명령어를 실행하면 JSON Server가 실행된다.  
``` bash
$ npm start
```

<br><br>

#### GET 요청
JSON Server의 루트 폴더(/json-server-exam)에 `public` 폴더를 생성하고 그 안에 `get_index.html` 파일을 생성한다.  
이후 서버를 종료 후 재시작하자.

`todos` 리소스에서 모든 todo를 취득한다.

``` html
<!DOCTYPE html>
<html>
<body>
    <pre></pre>
    <script>
        const xhr = new XMLHttpRequest();
        xhr.open('GET', '/todos');
        xhr.send();

        xhr.onload = () => {
            if (xhr.status === 200) {
                document.querySelector('pre').textContent = xhr.response;
            } else {
                console.error('Error', xhr.status, xhr.statusText);
            }
        }
    </script>
</body>
</html>
```

<br>

`todos` 리소스에서 id를 사용하여 특정 `todo`를 취득(retrieve)해보자.  
public 폴더에 `get_retrieve.html`을 추가해보자. 

`todo` 리소스에서 특정 todo를 취득한다.

``` html
<!DOCTYPE html>
<html>
<body>
    <pre></pre>
    <script>
        const xhr = new XMLHttpRequest();
        xhr.open('GET', '/todos/1');
        xhr.send();

        xhr.onload = () => {
            if (xhr.status === 200) {
                document.querySelector('pre').textContent = xhr.response;
            } else {
                console.error('Error', xhr.status, xhr.statusText);
            }
        }
    </script>
</body>
</html>
```

<br><br>

#### POST 요청
public 폴더에 `post.html` 파일을 추가해보자. 

todo 리소스에 새로운 todo를 생성한다.   
POST 요청 시에는 `setRequestHeader` 메소드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야 한다.  

``` html
<!DOCTYPE html>
<html>
<body>
    <pre></pre>
    <script>
        const xhr = new XMLHttpRequest();
        xhr.open('POST', '/todos');
        
        // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정
        xhr.setRequestHeader('Content-type', 'application/json');
        
        // HTTP 요청 전송
        // 요청 몸체에 담아 서버로 전송할 페이로드를 인수로 전달
        xhr.send(JSON.stringify({
            id: 4,
            content: 'Angular',
            completed: false
        }));

        xhr.onload = () => {
            // 응답 상태 코드가 200(OK) 또는 201(Created)인 경우 정상 처리된 것으로 판단
            if (xhr.status === 200 || xhr.status === 201) {
                document.querySelector('pre').textContent = xhr.response;
            } else {
                console.error('Error', xhr.status, xhr.statusText);
            }
        }
    </script>
</body>
</html>
``` 

<br><br>

#### PUT 요청
`PUT`은 특정 리소스 전체를 교체할 때 사용한다.   
PUT 요청 시에는 `setRequestHeader` 메소드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야 한다.  

``` html
<!DOCTYPE html>
<html>
<body>
    <pre></pre>
    <script>
        const xhr = new XMLHttpRequest();

        // todos 리소스에서 id가 1인 리소스 전체를 교체
        xhr.open('PUT', '/todos/4');
        
        xhr.setRequestHeader('Content-type', 'application/json');

        xhr.send(JSON.stringify({
            id: 4,
            content: 'TypeScript',
            completed: false
        }));

        // 요청이 완료되었을 경우 발생하는 이벤트 핸들러
        xhr.onload = () => {
            if (xhr.status === 200 || xhr.status === 201) {
                document.querySelector('pre').textContent = xhr.response;
            } else {
                console.error('Error', xhr.status, xhr.statusText);
            }
        }
    </script>
</body>
</html>
```

<br>

#### PATCH 요청
`PATCH`는 특정 리소스 일부를 변경할 때 사용한다.  
PATCH 요청 시에는 `setRequestHeader` 메소드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야 한다.  

``` html
<!DOCTYPE html>
<html>
<body>
    <pre></pre>
    <script>
        const xhr = new XMLHttpRequest();
        xhr.open('PATCH', '/todos/4');
        
        xhr.setRequestHeader('Content-type', 'application/json');

        xhr.send(JSON.stringify({
            content: 'TypeScript',
            completed: false
        }));
        
        // 요청이 완료되었을 경우 발생하는 이벤트 핸들러
        xhr.onload = () => {
            if (xhr.status === 200 || xhr.status === 201) {
                document.querySelector('pre').textContent = xhr.response;
            } else {
                console.error('Error', xhr.status, xhr.statusText);
            }
        }
    </script>
</body>
</html>
```


<br><br>

#### DELETE 요청
`DELETE`는 특정 리소스를 삭제할 때 사용한다.  
DELETE 요청 시에는 `setRequestHeader` 메소드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야 한다.  

``` html
<!DOCTYPE html>
<html>
<body>
    <pre></pre>
    <script>
        const xhr = new XMLHttpRequest();
        xhr.open('DELETE', '/todos/4');
        
        xhr.send();

        xhr.onload = () => {
            if (xhr.status === 200 || xhr.status === 201) {
                document.querySelector('pre').textContent = xhr.response;
            } else {
                console.error('Error', xhr.status, xhr.statusText);
            }
        }
    </script>
</body>
</html>
```

