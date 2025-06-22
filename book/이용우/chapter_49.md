### Chapter 49 - Babel과 Webpack을 이용한 ES6+/ES.NEXT 개발 환경 구축

크롬, 사파리, 파이어폭스, 엣지 같은 에버그린 브라우저(evergreen browser)의 ES6 지원율은 약 98%로 거의 대부분의 ES6 사양을 지원한다.

https://compat-table.github.io/compat-table/es6/

<br><br>

하지만 IE 11의 ES6 지원율은 약 11%이다.  
그리고 매년 새롭게 도입되는 ES6 이상의 버전과 제안 단계에 있는 ES 제안 사양(ES.NEXT)은 브라우저에 따라 지원율이 제각각이다.  

<br>

따라서 ES6+와 ES.NEXT의 최신 ECMAScript 사양을 사용하여 프로젝트를 진행하려면  
최신 사양으로 작성된 코드를 경우에 따라 IE를 포함한 구현 브라우저에서 문제 없이 동작시키기 위한 개발 환경을 구축하는 것이 필요하다.  

<br>

그리고 대부분의 프로젝트가 모듈을 사용하므로 모듈 로더도 필요하다.  
ES6 모듈(ESM)은 대부분의 모던 브라우저에서 사용할 수 있다.  
하지만 아래의 이유로 아직까지는 ESM보다는 별도의 모듈 로더를 사용하는 것이 일반적이다.  

<br>

- IE를 포함한 구현 브라우저는 ESM을 지원하지 않는다.
- ESM을 사용하더라도 트랜스파일링이나 번들링이 필요한 것은 변함이 없다.
- ESM이 아직 지원하지 않는 기능이 있고 점차 해결되고는 있지만 아직 이슈는 존재한다.

<br>

트랜스파일러(transpiler)인 Babel과 모듈 번들러(module bundler)인 Webpack을 이용하여 ES6+/ES.NEXT 개발 환경을 구축해보도록 하자.

<br>

그리고 Webpack을 통해 Babel을 로드하여 구형 브라우저에서도 동작하도록  
ES5 사양의 소스코드로 트랜스파일링하는 방법도 알아보도록 하자.


<br><br>

### Babel
아래 예제는 ES6의 화살표 함수와 ES7의 지수 연산자를 사용하고 있다.

``` js
[1, 2, 3].map(n => n ** n);
```

IE와 같은 구형 브라우저에서는 ES6의 화살표 함수와 ES7의 지수 연산자를 지원하지 않을 수 있다.  
Babel을 사용하면 위 코드를 다음과 같이 ES5 사양으로 변할 수 있다.

<br>

``` js
"use strict";

[1, 2, 3].map(function (n) {
    return Math.pow(n, n);
});
```

이처럼 Babel은 ES6+/ES.NEXT로 구현된 최신 사양의 소스코드를 IE 같은 구형 브라우저에서도 동작하는  
ES5 사양의 소스코드로 변환(트랜스파일링)할 수 있다.

<br><br>

#### Babel 설치
npm을 사용하여 Babel을 설치해보자.

``` bash
# 프로젝트 폴더 생성
$ mkdir esnext-project && cd esnext-project

# package.json 생성
$ npm init -y

# babel-core, babel-cli 설치
$ npm install --save-dev @babel/core @babel/cli
```

<br>

Babel, Webpack, 플러그인의 버전은 빈번하게 업그레이드 된다.  
때문에 특정 버전을 설치하고 싶다면 아래와 같이 패키지 이름 뒤에 @와 설치하고 싶은 버전을 지정한다.

``` bash
$ npm install --save-dev @babel/core@7.24.0 @babel/cli@7.24.0
```

<br><br>

#### Babel 프리셋 설치와 babel.config.json 설정 파일 작성
Babel을 사용하려면 `@babel/preset-env`를 설치해야 한다.  
`@babel/preset-env`는 함께 사용되어야 하는 babel 플러그인을 모아 둔 것으로 Babel 프리셋이라 한다.  

Babel이 제공하는 공식 Babel 프리셋은 다음과 같다.
- `@babel/preset-env`
- `@babel/preset-flow`
- `@babel/preset-react`
- `@babel/preset-typescript`

`@babel/preset-env`는 필요한 플러그인들을 프로젝트 지원 환경에 맞춰 동적으로 결정해 준다.  
프로젝트 지원 환경은 Browserslist 형식으로 `.browserslistrc` 파일에 상세히 설정할 수 있다.  

<br>

프로젝트 지원 환경 설정 작업을 생략하면 기본값으로 설정된다.


<br><br>

기본 설정은 모든 ES6+/ES.NEXT 사양의 소스코드를 변환한다.

``` bash
# @babel/preset-env 설치
$ npm install --save-dev @babel/preset-env
```

설치가 완료되면 프로젝트 루트 폴더에 `babel.config.json` 설정 파일을 생성하고 다음과 같이 작성한다.  
지금 설치한 `@babel/preset-env`를 사용하겠다는 의미와 같다.

``` js
{
    "presets": ["@babel/preset-env"]
}
```

<br><br>

#### 트랜스파일링
Babel을 사용하여 ES6+/ES.NEXT 사양의 소스코드로 트랜스파일링해보자.  
Babel CLI 명령을 사용할 수도 있으나 트랜스파일링할 때마다 매번 Babel CLI 명령어를 입력하는 것은 번거로우므로  
`npm scripts`에 Babel CLI 명령어를 등록하여 사용하자.

<br>

``` json
{
    "name": "esnext-project",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
        "build": "babel src/js -w -d dist/js"
    },
    "devDependencies": {
        "@babel/cli": "^7.24.0",
        "@babel/core": "^7.24.0",
        "@babel/preset-env": "^7.24.0"
    }
}
```

<br><br>

위 npm scripts의 build는 src/js 폴더(타깃 폴더)에 있는 모든 자바스크립트 파일들을 트랜스파일링 한 후,  
그 결과물을 `dist/js` 폴더에 저장한다. 사용한 옵션의 의미는 다음과 같다.

- `-w`: 타깃 폴더에 있는 모든 자바스크립트 파일들의 변경을 감지하여 자동으로 트랜스파일한다. (--watch)
- `-d`: 트랜스파일링된 결과물이 저장될 폴더를 저장한다. 만약 지정된 폴더가 존재하지 않으면 자동 생성한다. (--out_dir)

<br>

트랜스파일링을 테스트 하기 위해 ES6+/ES.NEXT 사양의 자바스크립트 파일을 작성하자.  
프로젝트 루트 폴더에 `src/js` 폴더를 생성하고 `lib.js`와 `main.js`를 추가한다.

``` js
// src/js/lib.js
export const pi = Math.PI;

export function power(x, y) {
    return x ** y;  // ES7 지수 연산자
}

// ES6 클래스
export class Foo {
    #private = 10;  // 클래스 필드 정의 제안

    foo() {
        // stage4: Rest/Spread 프로퍼티 제안
        const { a, b, ...x } = { ...{ a: 1, b: 2 }, c: 3, d: 4};
        return { a, b, x };
    }
    bar() {
        return this.#private;
    }
}
```

<br>

``` js
// src/js/main.js
import { pi, power, Foo } from './lib.js';

console.log(pi);
console.log(power(pi, pi));

const f = new Foo();
console.log(f.foo());
console.log(f.bar());
```

<br>

터미널에서 아래의 명령을 입력하여 트랜스파일링을 진행한다.

``` bash
$ npm run build

> esnext-project@1.0.0 build /Users/user/Desktop/esnext-project
> babel src/js -w -d dist/js

SyntaxError: /Users/wooy0ng/Desktop/esnext-project/src/js/lib.js: Support for the experimental syntax 'classProperties' isn't currently enabled (10:3)

8 | // ES6 클래스
9 | export class Foo {
10 |   #private = 10;  // 클래스 필드 정의 제안
   |   ^
11 |
12 |   foo() {
13 |     // stage4: Rest/Spread 프로퍼티 제안
...
```

<br>

privat 필드 정의 제안에서 에러가 발생했다.  
이것은 `@babel/preset-env`가 현재 제안 단계에 있는 사양에 대한 플러그인을 지원하지 않기 때문에 발생한 에러이다.  
따라서 현재 제안 단계에 있는 사양을 프랜스파일링하려면 별도의 플러그인을 설치해야 한다.

<br><br>

#### Babel 플러그인 설치

설치가 필요한 Babel 플러그인은 Babel 홈페이지에서 검색할 수 있다.  
Babel 홈페이지 상단 메뉴의 Search란에 제안 사양의 이름을 입력하면 해당 플러그인을 검색할 수 있다.

https://babeljs.io/

검색된 Babel 플러그인 중에서 `public/private` 클래스 필드를 지원하는  
`@babel/plugin-proposal-class-properties`를 설치하자.

``` bash
$ npm install --save-dev @babel/plugin-proposal-class-properties
```

<br>

<br>

설치 후 다시 터미널에서 명령을 입력하여 트랜스파일링을 실행해보자.  
성공적으로 실행 완료되면 프로젝트 루트 폴더에 dist/js 폴더가 자동으로 생성되고   
트랜스파일링된 main.js와 lib.js가 저장된다.

<br><br>

#### 브라우저에서 모듈 로딩 테스트
위 예제의 모듈 기능은 Node.js 환경에서 동작한 것이고  
Babel이 모듈을 트랜스파일링한 것도 Node.js가 기본 지원하는 CommonJS 방식의 모듈 로딩 시스템에 따른 것이다.  
다음은 src/js/main.js가 Babel에 의해 트랜스파일링된 결과이다.

``` js
// dist/js/main.js
"use strict";

var _lib = require("./lib.js");

console.log(_lib.pi);
console.log((0, _lib.power)(_lib.pi, _lib.pi));

var f = new _lib.Foo();
console.log(f.foo());
console.log(f.bar());
```

브라우저는 CommonJS 방식의 `require` 함수를 지원하지 않으므로  
위에서 트랜스파일링된 결과를 그대로 브라우저에서 실행하면 에러가 발생한다.  

프로파일링된 자바스크립트 파일을 브라우저에서 실행해보자.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script src="dist/js/lib.js"></script>
    <script src="dist/js/main.js"></script>
</body>
</html>
```

위 HTML 파일을 브라우저에서 실행하면 다음과 같은 에러가 발생한다.

``` bash
Uncaught ReferenceError: exports is not defined at lib.js:3
main.js:3 Uncaught ReferenceError: require is not defined at main.js:3
```

브라우저의 ES6 모듈을 사용하도록 Babel을 설정할 수도 있으나  
위와 같이 ESM을 사용하는 것은 문제가 있다.  
**이를 Webapck을 통해 문제를 해결할 수 있다.**


<br><br>

### Webpack
Webpack은 의존관계에 있는 자바스크립트, CSS, 이미지 등의 리소스를  
하나(또는 여러 개)의 파일로 번들링하는 모듈 번들러이다.

Webpack을 사용하면 의존 모듈이 하나의 파일로 번들링되므로 별도의 모듈 로더가 필요없다.  
그리고 여러 개의 자바스크립트 파일을 하나로 번들링하므로 HTML 파일에서 script 태그로 여러 개의 자바스크립트 파일을 로드해야하는 번거로움도 사라진다.

<br><br>

#### Webpack 설치
터미널에서 아래 명령을 입력해 Webpack을 설치한다.

``` bash
$ npm install --save-dev webpack webpack-cli
```

<br><br>

#### babel-loader 설치
Webpack이 모듈을 번들링할 때 Babel을 사용하여 ES6+/ES.NEXT 사양의 소스코드를  
ES5 사양의 소스코드로 트랜스파일링하도록 babel-loader를 설치한다.

``` bash
$ npm install --save-dev babel-loader
```

<br><br>

#### webpack.config.js 설정 파일 작성
webpack.config.js는 Webpack이 실행될 때 참조하는 설정 파일이다.  
프로젝트 루트 폴더에 webpack.config.js 파일을 생성하고 다음과 같이 작성한다.

``` js
const path = require('path');

module.exports = {
    entry: './src/js/main.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
    },
    module: {
        rules: [
            { 
                test: /\.js$/, 
                include: [
                    path.resolve(__dirname, 'src/js')
                ],
                exclude: /node_modules/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['@babel/preset-env'],
                        plugins: [
                            '@babel/plugin-proposal-class-properties'
                        ]
                    }
                }
            }
        ]
    },
    devtool: 'source-map',
    mode: 'development'
}
```


<br>

이제 Webpack을 실행하여 트랜스파일링 및 번들링을 실행해보자.  
트랜스파일링은 Babel이 수행하고 번들링은 Webpack이 수행한다.  

만약 이전에 실행시킨 빌드 명령이 실행 중인 상태라면 중지시키고 다음 명령을 실행한다.

``` bash
$ npm run build
```

<br>

Webpack을 실행한 결과, dist/js 폴더에 bundle.js 파일이 생성되었다.  
이 파일은 main.js, lib.js 모듈이 하나로 번들링된 결과이다.  

index.html을 다음과 같이 수정하고 브라우저에서 실행해보자.

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script src="dist/bundle.js"></script>
</body>
</html>
```

main.js, lib.js 모듈이 하나로 번들링된 bundle.js가 브라우저에서 문제없이 실행된 것을 확인할 수 있다.

<br><br>

#### babel-polyfill 설치
Babel을 사용하여 ES6+/ES.NEXT 사양의 소스코드를 ES5 사양의 소스코드로 트랜스파일링해도 브라우저가 지원하지 않는 코드가 존재할 수 있다.  
특정 객체 및 함수는 ES5에서 대체할 기능이 없기 때문에 에러가 발생할 수 있다.

src/js/main.js를 다음과 같이 수정하여  
ES6에서 추가된 Promise, Object.assign, Array.fron 등이 어떻게 트렌스파일링되는지 확인해보자.

``` js
// src/js/main.js
import { pi, power, Foo } from './lib.js';

console.log(pi);
console.log(power(pi, pi));

const f = new Foo();
console.log(f.foo());
console.log(f.bar());

// polyfill이 필요한 코드
console.log(new Promise((resolve, reject) => {
    setTimeout(() => resolve(1), 100);
}));

// polyfill이 필요한 코드
console.log(Object.assign({}, { x: 1 }, { y: 2 }));

// polyfill이 필요한 코드
console.log(Array.from([1, 2, 3], x => x * x));
```

<br>

다시 트랜스파일링과 번들링을 실행한 다음, dist/js/bundle.js를 확인해보면   
Promise, Object.assign, Array.from 등과 같이 ES5 사양으로 대체할 수 없는 기능은 트랜스파일링 되지 않는다.  

따라서 구형 브라우저에서도 Promise, Object.assign, Array.from 등과 같은 객체나 메소드를 사용하기 위해서는 @babel/polyfill을 사용해야 한다.

``` bash
$ npm install @babel/polyfill
```

<br>

ES6의 import를 사용하는 경우에는 진입점의 선두에서 polyfill을 로드하도록 하자.

``` js
// src/js/main.js
import "@babel/polyfill";
import { pi, power, Foo } from './lib.js';
```

Webpack을 사용하는 경우에는 위 방법 대신 webpack.config.js 파일의 entry 배열에 polyfill을 추가한다.  

``` js
const path = require('path');

module.exports = {
    entry: ['@babel/polyfill', './src/js/main.js'],
    output: {
        path: path.resolve(__dirname, 'dist'),
    }
}
```


끝.

