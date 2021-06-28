# 05-10-2021

## git

```bash
git <command> --help
git checkout -- <file name> # revert
git branch -M [<old branch>] <new branch> # rename branch
git config --get remote.origin.url
git remote
git shortlog
git reflog
git remote add origin <url>
git push -u origin main
git push --set-upstream origin main
```

---

## javascript

- string.slice()
  - if beginIdx >= (str.length || not a number), treated as 0
  - if endIdx >= str.length, to the end of string
- chrome debugging tool
- primitive types
  - string, number(integer, float), boolean
  - null, undefined, NaN(전역 스코프 변수)
  - == type coercion
    ```javascript
    undefined == null; // true
    undefined === null; // false
    ```
  - if not declared, throw error
  - if declared and not assign, undefined
    ```javascript
    var a = null; // null is primitive type
    console.log(typeof a); // object because of bug
    // type is not decided
    var a;
    console.log(typeof a); // undefined
    ```
- console.dir()

---

---

# 05-11-2021

## javascript

- Number.MAX_SAFE_INTEGER
  - infinty standard, 2^1024, 1.79E+308
- Number.MAX_VALUE
  - numbers btw -(2^53 - 1) and 2^53 - 1
- setInterval(func, milisecs);
- Promise chaining

  - error는 .catch()로 잡는다
  - new Promise((resolve, reject) => { }).than().catch();

  ```javascript
  function getData() {
    return new Promise(function (resolve, reject) {
      $.get("url", function (response) {
        if (isValid(response)) {
          resolve(response);
        } else {
          reject(new Error("request is failed"));
        }
      });
    });
  }

  getData()
    .then(function (resolvedData) {
      return new Promise((resolve, reject) => {
        $.get("url", function (response) {
          // with response
          if (isValidResponse) {
            resolve(responseParsed);
          } else {
            reject(new Error("request is failed"));
          }
        });
      });
    })
    .then((responseParsed) => {
      console.log(responseParsed);
    })
    .catch(function (err) {
      console.erroor(err);
    });
  ```

- Promise.all().then();

  - 최종적으로 끝나는 promise에 맞춤

  ```javascript
  let p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("p1");
    }, 1000);
  });

  let p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("p2");
    }, 3000);
  });

  Promise.all([p1, p2]).then((result) => {
    console.log(result[0]);
    console.log(result[1]);
  });
  ```

## vscode

- alt + left, move focus backward
- ctrl + number, move focus to the corresponding window
- ctrl + alt + right, move tab right

## dom api

- document.title
- element.innerHTML
- element.classList
  - element.classList.contains()
  - element.classList.add()
  - element.classList.remove()
  - element.classList.toggle()
- Date()
  ```javascript
  let date = new Date();
  date.getDate();
  ```

## css

- margin
  - default는 0?, 왼쪽 정렬
  - margin: auto는 가운데 정렬
    - inline 요소는 margin: auto가 안 먹힘, block으로 바꿔서 해결
    - 폭 연산이 불가능하면 안 먹힘, overflow: hidden으로 해결
- inline 요소
  ```html
  <span>, <a>, <input>, <textarea>, <label>, <button>
  ```
  - margin, padding 전부 공간에 없음, padding은 시각적으로 존재
  - width, height는 내용의 길이 의해 결정
  - 자동으로 줄 바꿈 없음
- block 요소
  ```css
  <div>, <form>, <ul>, <ol>, <h1>, <header>
  ```
  - 지정하지 않을 경우 width: 100%; height는 컨텐츠 높이
  - 자동 줄 바꿈
- attribute 선택자
  - tagName[attr="val"]
- 자손 선택자 '&nbsp;', 자식 선택자 '>'

---

---

# 05-12-2021

## DOMapi

- classList = event.target.classList
  - classList.add(), classList.remove(), classList.toggle()
- Ele = document.querySelector('Ele')
  - Ele.tagName, Ele.NodeName
  - Ele.className = 'Breadcumb';
  - Ele.removeAttribute() -- HTML Element prototype
- target = event.target
  - target.parentElement

## javascript

- export
  - { }로 감싸서 내보내는게 원칙
    - 한 개일 경우 { } 생략 가능
  - default로 보내고 싶으면
    - export { example as default }
    - export default example
- import

  - { }로 감싸서 받는게 원칙
    - import { exmaple as exam } from './dir';
  - default를 받을 때
    - 이름 바꿀 수 있음
      - import exam from './dir';
    - default랑 아닌 거랑 같이 받을 때
      - import { default as myModule, exam as example } from './dir';
  - 전체를 받을 때

    - import \* as myModule from './dir';

- Array.prototype.map()
  - return new Array()
  - callback(currentValue[, index[, array]])[, thisArg])
    - thisArg: callback을 실행할 때 this로 사용되는 값
    ```javascript
    ["1", "2", "3"].map((num) => parseInt(num));
    // .map(parseInt)는 parseInt에 index와 array도 같이 넘어가서 정상 동작을 안한다.
    ```
- Array.from(arrayLike[, mapFn[, thisArg]])

  - 배열이 아닌 객체로 배열을 만들 때

  ```javascript
  let str = "hello";
  // h, e, l, l, o
  Array.from(str);
  ```

- 유사 배열 객체, 이터러블 객체, for in, for of

  - for in 반복문: 객체의 모든 열거 가능한 속성에 대해 반복
    - value에 접근하는 방법은 제공하지 않는다
  - for of 반복문, [Symbol.iterator] 속성을 가지는 컬렉션 전용

- 구조 분해 할당

  - 배열

    ```javascript
    // 두 번쨰 요소 무시
    let [a, , b] = [1, 2, 3];
    // name2는 array
    let [name1, ...name2] = ["a", "b", "c"];
    // undefined, undefined
    let [firstName, surName] = [];
    // 에러는 아니다
    ```

  - 객체

    ```javascript
    // 없는 패턴이 들어오면 무시
    // 객체가 아예 안 들어오는 에러 방지
    function({ incomingProperty : varName = defaultValue } } = {}) {}

    // 정의 후 할당
    let a, b, c;
    // 'a', undefined
    ({a, c} = {a: 'a', b: 'b'})
    // ()로 감싸 {}를 코드블럭이 아닌 표현식으로 해석
    // options 중 있는 것만 쓰고 없는 건 undefined
    ```

- 나머지 매개변수와 전개 문법

  - 매개변수로 사용될 때

    - ...args로 받음, args는 배열
    - 매개변수 중 젤 뒤에 있어야 됨

  - 자바스크립트는 매개변수 선언 없이도 매개변수를 받을 수 있음

    - 화살표 함수는 this가 없어서 arguments(유사 배열 객체)가 없음

      ```javascript
      function f() {
        let showArg = () => console.log(arguments[0]);
        showArg();
      }

      f(1);
      ```

  - 객체를 전개 할 때 똑같은 property는 더 뒤에 있는 것 대입
    ```javascript
    let abc = { a: "a", b: "b", c: "c" };
    // a: 'a', b: 'b', c: 'c'
    let aabc = { a: "aa", ...abc };
    // a: 'aaa', b: 'b', c: 'c'
    let aaabc = { ...abc, a: "aaa" };
    ```

- fetch(url)

  ```javascript
  fetch(`${API_END_POINT}/${nodeId}`)
    .then( (response) => {
      if (!response.ok) {
        throw new Error('서버의 상태가 이상합니다');
      }
      return response.json();
    })
    .then(myJson => {
      console.log(JSON.stringify(myJson));
    })
    .catch((e) => {
      throw new Error(`무언가 잘못 되었습니다! ${e.message}`);
    };

  const request = async () => {
    try {
      const res = await fetch(`${API_END_POINT}/${nodeId}`);
      if (!res.ok) {
        throw new Error('서버의 상태가 이상합니다');
      }

      const myJson = await res.json();

      return JSON.stringify(myJson);

    } catch (e) {
      throw new Error(`무언가 잘못 되었습니다! ${e.meessage}`);
    }
  }
  ```

---

---

# 05-17-2021

## CSS

- [box-sizing](https://developer.mozilla.org/ko/docs/Web/CSS/box-sizing)
  ```css
  * {
    /* default: content-box */
    box-sizing: border-box; /* content + border + padding */
  }
  ```
- [flex](https://heropy.blog/2018/11/24/css-flexible-box/)
  ```css
  body {
    display: flex;
    justify-content: center; /* default: flex-start */
    align-items: center; /* default: stretch, 한 줄일 때 사용 */
  }
  .Nodes {
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    flex-wrap: wrap;
    /* align-content: stretch; */
  }
  ```

## CS

```bash
clang hello.c             # a.out
clang -o hello hello.c    # hello
clang -o hello hello.c -lcs50
make hello
```

- compiling

  - preprocessing

  ```c++
   // preprocessing # lines
  #include <stdio.h>
  // stdio.h 파일 내의 해당하는 코드로 대체
  // int printf(string format, ...);
  ```

  - compiling
    - to assembly
  - assembling
    - to machine code
  - linking
    - 여러 machine code를 하나로 합침

- debugger

- big O
  - upper-bound
- omega
  - lower-bound

---

---

# 05-21-2021

## javascript

- 숫자를 모두 64비트 부동 소수점으로 표현
  - 표현할 수 있는 최댓값: 1.7976931348623157 \* 10^308
  - 표현할 수 있는 최솟값: 4.940656458412465 \* 10^-324
  - 정수 값은 -2^53 ~ 2^53 범위의 값을 정확하게 처리 가능

| 분류              | 표기법                   |         설명          |
| ----------------- | ------------------------ | :-------------------: |
| 전역 변수         | Infinity                 |     플러스 무한대     |
| 전역 변수         | NaN                      |     Not a Number      |
| Number의 프로퍼티 | Number.POSITIVE_INFINITY |     플러스 무한대     |
| Number의 프로퍼티 | Number.NEGATIVE_INFINITY |    마이너스 무한대    |
| Number의 프로퍼티 | Number.MAX_VALUE         | 표현할 수 있는 최댓값 |
| Number의 프로퍼티 | Number.MIN_VALUE         | 표현할 수 있는 최솟값 |
| Number의 프로퍼티 | Number.NaN               |     Not a Number      |
| Number의 프로퍼티 | Number.EPSILON           | 2.22044605925031e-16  |
| Number의 프로퍼티 | Number.MIN_SAFE_INTEGER  |   -9007199254740990   |
| Number의 프로퍼티 | Number.MAX_SAFE_INTEGER  |   9007199254740990    |

<br>

- Symbol

  ```javascript
  var NONE = Symbol("none");
  var BLACK = Symbol("black");
  var WHITE = Symbol("white");

  // cell 값을 확인할 때 NONE, BLACK, WHITE만 사용하도록 제한
  ```

- hoisting

  - 선언부만 함수 처음 부분으로 올라옴

    ```javascript
    function f() {
      console.log(a); // undefined
      var a = "local";
      console.log(a); // local
      return a;
    }

    function g() {
      console.log(x); // ReferenceError: x is not defined
      let x = 5;
      // 똑같은 이름을 가진 변수 선언 시 문법 오류
      let x; // Uncaught SyntaxError
    }
    ```

- 프로퍼티

  - 참조 시 . 연산자 또는 [] 연산자 사용
    ```javascript
    var card = { suit: "하트", rank: "A" };
    card.suit;
    card["rank"];
    ```
  - 없는 변수 참조 -> 참조 에러
  - 없는 프로퍼티 참조 -> undefined
  - 없는 프로퍼티 이름에 값 대입 -> 새로운 프로퍼티 추가
  - delete를 이용하여 프로퍼티 삭제 가능
    ```javascript
    delete card.rank;
    ```
  - in 연산자로 프로퍼티가 있는지 확인

    ```javascript
    console.log("suit" in card); // true
    console.log("color" in card); // false

    //card는 Object 객체를 상속 받았음
    console.log("toString" in card); // true
    ```

- 메서드

  - 객체의 프로퍼티 중에서 함수 객체의 참조를 값으로 담고 있는 프로퍼티

  ```javascript
  var circle = {
    center: { x: 1.0, y: 2.0 },
    radius: 2.5,
    // this는 이 함수를 메소드로 가지고 있는 객체를 가리킴
    area: function () {
      return Math.PI * this.radius * this.radius;
    },
  };
  ```

- 생성자
  - 생성자 자체가 함수 객체
  - 생성자가 클래스처럼 객체를 생성하는 역할을 담당

```javascript
// 첫 글자를 대문자
function Card(suit, rank) {
  this.suit = suit;
  this.rank = rank;
}

// 메서드를 가진 객체를 생성하는 생성자
function Circle(center, radius) {
  this.center = center;
  this.radius = radius;
  this.area = function () {
    return Math.PI * this.radius * this.radius;
  };
}
```

- 객체의 분류

  - Native 객채
    - ECMAScript 사양에 정의된 객체
    - 내장 생성자 객체
      - Object, String, Number, Boolean, Array, Date, Function, RegExp, Error, Symbol 등
    - 기타 객체
      - JSON, Math
  - 호스트 객체
    - ECMAScript에 정의되어 있지 않지만 자바스크립트 실행 환경에 정의된 객체
    - 브라우저 객체
      - Window, Navigator, History, Location, Screen, Document
    - DOM 객체
      - HTML 요소를 조작하는 객체
    - 기타 객체
      - XMLHttpRequest, HTML5의 각종 API 등
  - 사용자 정의 객체
    - 사용자가 정의한 자바스크립트 코드를 실행한 결과로 생성된 객체

- 배열

  - 자바스크립트 배열은 C나 Java 같은 프로그래밍 언어의 배열의 기능을 흉내 낸 것
  - Array 객체는 배열의 인덱스를 문자열로 변환해서 그것을 프로퍼티로 이용

    - 배열에 대괄호 연산자를 사용하는 것은 객체에 대괄호 연산자를 사용하는 것과 마찬가지, 배열의 요소 번호로 숫자 값 대신 문자열 사용 가능

    ```javascript
    var a = ["A", "B", "C", "D"];
    console.log(a["2"]); // C
    ```

  - 배열 요소 삭제
    - 배열의 length 프로퍼티 값은 바뀌지 않음, 삭제한 요소만 사라짐

  ```javascript
  delete a[1];
  console.log(a); // ['A', undefined, 'C', 'D']
  ```

  - 배열 요소 확인

    - 객체의 프로퍼티 확인 방법과 같음

    ```javascript
    var a = ["A", "B", "C"];
    a[4] = "E";
    console.log(a); // ['A', 'B', 'C', undefined, 'E']

    for (var i in a) console.log(i); // 0, 1, 2, 4
    a.hasOwnProperty("3"); // false
    a.hasOwnProperty("4"); // true
    ```

  - 산술 연산자
    - \+ 연산자는 피연산 중 하나가 문자열이면 나머지 피연산자를 문자열로 만든다.
    - 계산할 수 없는 경우에는 NaN으로 평가한다. 피연산자가 true면 1, false와 null이면 0으로 평가, undefined면 NaN으로 평가
    ```javascript
    1 + "2month"; // "12month"
    0 / 0; // NaN
    "one" * 1; // NaN
    true + true; // 2
    1 + null; // 1
    1 + undefined; // NaN, undefined를 NaN으로 바꾸어 더함
    ```

- Math 객체 메서드
  |메서드|설명|
  |---|---|
  |Math.abs(x)|\|x\||
  |Math.ceil(x)|x 이상의 최소 정수|
  |Math.exp(x)|e의 x제곱|
  |Math.floor(x)|x 이하의 최대 정수|
  |Math.fround(x)|x에 가까운 단정밀도 부동소수점 값|
  |Math.log(x)|log(x)|
  |Math.max(x, y)| x와 y 중에서 큰 값|
  |Math.min(x, y)| x와 y 중에서 작은 값|
  |Math.pow(x, p)| x의 p 제곱|
  |Math.random()|0 이상 1미만의 난수|
  |Math.round(x)|x의 반올림|
  |Math.sign(x)|x의 부호(양수면 1, 0이면 0, 음수면 -1|
  |Math.sqrt(x)|x의 제곱부|
  |Math.truc(x)|x의 정수부|

- 오차

  - 부호 1 bit, 지수 11 bit, 가수 52 bit
  - 정규화 시켜서 2진수 53 자리가 유효 자릿수, 10진수 자릿수는 약 16자리(10^15.95)

  ```javascript
  let a = 0.16;
  let b = 0.2;
  a / b; // 0.7999999999999999
  a / b == 0.8; // false
  Math.abs(a / b - 0.8) < 1e-10;
  ```

- String

  - 메서드

  | 메서드                | 설명                                                                                                    |
  | --------------------- | ------------------------------------------------------------------------------------------------------- |
  | codePointAt(n)        | 대상 문자열 n번째 문자의 코드 포인트의 스칼라 값을 10진수로 표기한 값                                   |
  | charAt(n)             | 대상 문자열의 n번째 문자                                                                                |
  | charCodeAt(n)         | 대상 문자열의 n번째 문자의 UTF-16코드를 10진수로 표기한 값                                              |
  | concat([s1, s2, ...]) | 대상 문자열과 인수의 문자열을 연결해서 반환                                                             |
  | includes(s [, n])     | 대상 문자열의 n번쨰 문자부터 문자열 s를 포함하는지를 판별한 논리값, n을 생략하면 문자열의 끝부터 검색함 |
  | indexOf(s)            | 대상 문자열에서 s가 처음 나오는 위치                                                                    |
  | lastIndexOf(s)        | 대상 문자열에서 s가 마지막으로 나오는 위치                                                              |
  | localeCompare(s)      | 대상 문자열이 문자의 정렬 순서에 따라 s의 앞뒤에 있는지 또는 같은 위치에 있는지 표시                    |
  | match(r)              | 대상 문자열에 정규 표현식 r을 매칭한 결괏값                                                             |
  | repeat(n)             | 대상 문자열을 n개 연결한 문자열                                                                         |
  | replace(s1, s2)       | 대상 문자열에 포함된 문자열 s1을 문자열 s2로 치환한 결괏값                                              |
  | search(r)             | 대상 문자열에서 정규식 r이 일치한 위치의 인덱스 값                                                      |
  | slice(m, n)           | 대상 문자열의 m번째 이후 n번쨰 미만의 부분 문자열을 반환, m과 n이 음수면 문자열 끝이 시작 위칟가 됨     |
  | split(s, [, n])       | 대상 문자열을 s로 분할한 문자열 배열을 반환, n은 발견된 문자열의 개수를 제한                            |
  | substring(m, n)       | 대상 문자열의 m번쨰 이후 n번쨰 미만의 부분 문자열을 반환                                                |
  | toLowerCase()         | 대상 문자열을 소문자로 변환                                                                             |
  | toString()            | String 객체를 문자열 값으로 변환                                                                        |
  | toUpperCase()         | 대상 문자열을 대문자로 변환                                                                             |
  | trim()                | 대상 문자열에서 앞뒤 공백을 제거                                                                        |
  | valueOf()             | String 객체를 문자열 값으로 변환                                                                        |

  - slice, substring 차이

    - start > end일 경우
      - slice: "" 리턴, substring: end, start로 바꿔서 리턴
    - start, end가 음수일 경우
      - slice: length만큼 더해서 사용, substring 0으로 취급
    - slice에서 음수 절댓값이 length보다 큰 경우
      - 0으로 취급

  - wrapper object

    - 문자열에서 프로퍼티를 사용하려고 하면 문자열이 자동으로 String 객체로 변환

  - surrogate pair

    - 유니코드는 국제 표준 문자표, UTF-8, UTF-16은 인코딩 방식
    - UTF-8이 대세, 자바스크립트는 UTF-16 방식
    - 코드 포인트를 2byte로 인코딩
    - UTF-16에서 U+10000 미만의 코드 포인트 값은 UTF-16의 인코딩 값과 같음
    - U+10000 이상의 코드 포인트 값은 UTF-16의 인코딩 값(2바이트) 두 개를 합쳐서 정의
    - 상위 써로게이트: 0x10000을 빼고 나머지 값 중 10bit 이후 값에 0xD800을 더한 값
    - 하위 써로게이트: 0x10000을 뺴고 나머지 값 중 뒤 10bit 값에 0xDC00을 더한 값

    ```javascript
    // 써로 게이트 페어(a, b)
    a = (u - 0x10000) * 0x400 + 0xd800;
    b = ((u - 0x10000) % 0x400) + 0xdc00;

    u = (a - 0xd800) * 0x400 + (b - 0xdc00) + 0x10000;

    // 써로게이트 페어가 포함된 문자열을 배열로 만들기
    function stringToArray(s) {
      return s.match(/[\uD800-\uDBFF][\uDC00-\uDFFF]|[^\uD800-\uDFF]/g) || [];
    }
    stringToArray("\u{1f4d6} 모던 자바스크립트 입문");
    ```

    ```javascript
    String.fromCharCode(0xd83d, 0xdcd6); // 이모티콘 "Open Book"
    // 써로게이트 페어의 코드 포인트 스칼라 값을 인수로 넘김
    String.fromCodePoint(0x1f4d6); // 이모티콘 "Open Book"
    // 유티코드 리터럴 사용
    ("\u{1f4d6}"); // 이모티콘 "Open Book"
    // String.fromCharCode는 다섯 자릿수 코드 포인트 지원 안 함
    String.fromCharCode(0x1f4d6); // "네모 모양", 예기치 않은 동작

    var s = "이모티콘 Open Book";
    s.codePointAt(0).toString(16); // "1f4d6" 코드 포인트의 스칼라 값
    s[0].codePointAt(0).toString(16); // "d83d" 상위 써로게이트
    s[1].codePointAt(0).toString(16); // "dcd6" 하위 써로게이트
    ```

  - charAt 대신 [] 연산자 사용 가능
    - 그러나 배열처럼 값을 대입해서 수정은 불가능

- 동일 연산자

  - 좌우 피연산자 타입이 같을 때

  ```javascript
  var a = [1, 2, 3];
  var b = [1, 2, 3];
  var c = a;
  console.log(a == b); // false
  console.log(a == c); // true
  ```

  - 좌우 피연산자의 타입이 다를 때

  ```javascript
  null == undefined; // true

  // 숫자와 문자열 비교는 숫자로 형변환 후 비교
  1 == "1"; // true
  "0xff" == 255; // true

  // 한 쪽이 논리값이면 논리값으로 형변환 후 비교
  true == 1; // true
  true == "1"; // true

  // 한 쪽이 객체고 한 쪽이 숫자, 문자열이면 객체를 toString이나 valueOf 메서드를 사용해서 원시 타입으로 변환한 다음에 비교
  new String("a") == "a"; // true
  new Number(2) == 2; // true
  [2] == 2; // true
  ```

- 일치 연산자

  - 타입과 값이 모두 같으면 같다고 판정
    - NaN은 NaN을 포함한 모든 값과 같지 않다고 판정

  ```javascript
  NaN === NaN; // false

  null === undefined; // false
  1 === "1"; // false
  "0xff" === 255; // false
  true === 1; // false
  true === "1"; // false
  new String("a") === "a"; // false
  new Number(2) === 2; // false
  [2] === 2; // false
  ```

- 논리곱 연산자

  - 0, -0, 빈 문자열, NaN, null, undefined: false로 평가
  - 나머지 true로 평가(모든 객체, 심벌도 true로 평가)

  ```javascript
  {} == true; // false
  new String("") == true; // false
  {} && true; // true
  new String("") && true; // true

  var p = null;
  p && p.name; // null: p가 false로 평가되므로 p를 반환함. 우변은 평가하지 않음
  p = { name: "Tom", age: 18 };
  p && p.name; // "Tom": p가 true로 평가되므로 p.name을 반환

  var time = time_interval || animationSettings.time || 33;
  ```

- 비트 연산자

  - '\>\>' 연산자: 원래 가지고 있던 부호 비트로 채워짐
  - '\>\>\>' 연산자: 0으로 채워짐

- 묵시적 형변환

```javascript
var s = "2";
s - 0 + s; // 4
```

- Number -> String

```javascript
var x = 1234.567;
x.toString(16); // 4d2.9126e978d5
x.toFixed(0); // 1235
x.toFixed(2); // 1234.57

String(true); // "true"
String(NaN); // "NaN"
String(null); // "null"
String(undefined); // "undefined"
String({ x: 1, y: 2 }); // "[object Object]"
String([1, 2, 3]); // "1, 2, 3"
```

- String -> Number

```javascript
parseInt("101", 2); // 5
parseInt("ff", 16); // 255

Number("2.71828"); // 2.71828
Number(true); // 1
Number(false); // 0
Number(NaN); // NaN
Number(undefined); // NaN
Number(null); // 0
Number({ x: 1, y: 2 }); // NaN
Number([1, 2, 3]); // NaN
```

---

---

# 05-22-2021

## Vue

- [Slots](https://kr.vuejs.org/v2/guide/components-slots.html)

  - Named Slots
    - 이름이 있는 슬롯에 내용을 전달하려면 \<template>에 v-slot 디렉티브를 쓰고 그 속성에 앞에서 지정한 'name'을 넣으면 됨
    - name이 지정되지 않은 \<slot>에는 암묵적으로 default 값이 사용됨

  ```html
  <!-- 자식 컴포넌트 -->
  <div class="container">
    <header>
      <slot name="header"></slot>
    </header>
    <main>
      <slot></slot>
    </main>
    <footer>
      <slot name="footer"></slot>
    </footer>
  </div>
  ```

  ```html
  <!-- 부모 컴포넌트 -->
  <base-layout>
    <!-- slotPorps는 원하는 대로 바꿀 수 있음 -->
    <template v-slot:header>
      <h1>Here might be a page title</h1>
    </template>

    <!-- 명시적으로 default 표시도 가능
    <template v-slot:default> 
    -->
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
    <!-- </template> -->

    <template v-slot:footer>
      <p>Here's some contact info</p>
      <p></p
    ></template>
  </base-layout>
  ```

  - 축약형

  ```html
  <!-- 축약 전 -->
  <!-- template이 하나 밖에 없으므로 축약 가능 -->
  <current-user>
    <template v-slot:default> </template>
  </current-user>

  <!-- 축약 후 -->
  <current-user v-slot:default> </current-user>
  ```

  - 슬롯 속성

  ```html
  <!-- 자식 컴포넌트 -->
  <slot v-bind:user="user"></slot>

  <!-- 부모 컴포넌트 -->
  <current-user v-slot="slotProps">
    {{ slotProps.user.firstName }}
  </current-user>
  ```

  - 구조 분해 가능
    - 프레임워크 내부에서 범위가 있는 슬롯은 하나의 인수(슬롯 속성)를 가지는 함수로 슬롯에 들어가는 내용을 감싸는 방식으로 작동

  ```javascript
  function(slotProps) {
    // ...slot content...
  }
  ```

  ```html
  <current-user v-slot="{ user : person = { firstName: 'Guest' } }">
    {{ person.firstName }}
  </current-user>
  ```

  - 다른 사례들
    - 부모 컴포넌트를 레이아웃 용도로만 사용하고 데이터 로직을 캡슐화

  ```html
  <!-- 자식 컴포넌트 -->
  <ul>
    <li v-for="todo in filteredTodos" v-bind:key="todo.id">
      <slot name="todo" v-bind:todo="todo">
        <!-- 기본값 -->
        {{ todo.text }}
      </slot>
    </li>
  </ul>

  <!-- 부모 컴포넌트 -->
  <todo-list>
    <template #todo="{ todo }">
      <span v-if="todo.isComplete">Complete-</span>
      {{ todo.text }}
    </template>
  </todo-list>
  ```

## javascript

- [this](https://ko.javascript.info/js)

  - 화살표 함수가 아닐 때

    - 함수를 호출할 때 암묵적으로 this가 넘어감
    - this는 함수를 호출한 객체를 가리킴
    - 함수를 호출한 객체가 없을 때는 실행 환경에 따라 window나 undefined
    - 어디서 정의되었는지는 아무 상관 없음(다른 언어와 차이점)

    ```javascript
    let a = {
      name: "max",
      sayHi() {
        console.log(this.name);
      },
    };
    let b = {
      sayHi: a.sayHi,
    };

    b.sayHi(); // undefined
    ```

  - 화살표 함수일 때
    - 화살표 함수엔 단지 this가 없을 뿐
    - 일반 변수 서칭과 마찬가지로 this의 값을 외부 렉시컬 환경에서 찾음

  ```javascript
  let f1 = () => {
    console.log(this);
  };

  let a = {
    aF1: f1,
    aF2: () => {
      console.log(this);
    },
  };
  let f2 = a.aF2;

  f1(); // window 또는 undefined
  a.aF1(); // window 또는 undefined
  f2(); // window 또는 undefined
  a.aF2(); // window 또는 undefined
  ```

  - this 예시

  ```javascript
  let group1 = {
    title: "1모둠",
    students: ["보라", "호진", "지민"],

    showList() {
      this.students.forEach((student) =>
        console.log(this.title + ": " + student)
      );
    },
  };

  group1.showList();

  let group2 = {
    title: "2모둠",
    students: ["보라", "호진", "지민"],

    showList() {
      this.students.forEach(function (student) {
        // TypeError: Cannot read property 'title' of undefined
        // 여기서 this는 window 또는 undefined
        console.log(this.title + ": " + student);
      });
    },
  };

  group2.showList();

  let group3 = {
    title: "3모둠",
    students: ["보라", "호진", "지민"],

    showList: group1.showList,
  };

  group3.showList(); // 3모둠: 보라, ...
  ```

  ```javascript
  let a = {
    aFunc: function (callback) {
      callback();
    },
  };

  let b = {
    bFunc: () => console.log(this),
  };

  let c = {
    cFunc: function () {
      console.log(this);
    },
  };

  a.aFunc(b.bFunc); // window 또는 undefined
  a.aFunc(c.cFunc); // window 또는 undefined
  ```

  ```javascript
  let a = {
    aFunc() {
      let arrowFunc = () => console.log(this);
      arrowFunc();

      let func = function () {
        console.log(this);
      };
      func();
    },
  };
  let b = {
    bFunc: a.aFunc,
  };

  let c = a.aFunc;

  b.bFunc(); // b 객체, window 또는 undefined
  c(); // window 또는 undefined, window 또는 undefined
  ```

---

---

# 06-09-2021

## C++

- 배열 포인터

```cpp
int arr[4] = { 1, 2, 3, 4 };
int (*p_arr)[4] = &arr;

// 에러
int (*p_arr)[4] = arr;
int (*p_arr)[3] = &arr
```

- [구조체 정렬(data structure alignment)](https://dojang.io/mod/page/view.php?id=430)

  - n byte alignment
    - 변수의 주소값이 그 변수의 크기의 배수임
    - char는 1 byte alignment
    - int는 4 byte alignment, int형 변수의 주소는 4의 배수여야 함
  - 구조체 변수는 가장 큰 멤버의 자료형 크기로 alignment

    - 멤버의 가장 큰 자료형 크기가 4 byte면 구조체 변수의 주소도 4의 배수
    - 구조체 멤버들도 구조체 내에서 자신의 크기에 맞게 align되어 있음

    ```cpp
    struct ProductInfo {
      int num;
      char name[100];
      int cost;
    };

    printf("%d\n", sizeof(ProductInfo)); // 108

    struct ProdcutInfo2 {
      int num;
      char name[101];    // 100 -> 101
      int cost;
    };

    printf("%d\n", sizeof(ProductInfo2)); // 112

    struct ProductInfo3 {
      int num;
      char name[101];
      double cost;    // int -> double
    };

    printf("%d\n", sizeof(ProductInfo3))  // 120

    struct ProductIfno4 {
      int num;
      char name[101];
      double cost;
      bool isSoldout // add bool member
    };

    printf("%d\n", sizeof(PrudoctInfo4)) // 128
    ```

  - 레퍼런스 변수
    - 변수에 별칭 부여
    - 메모리상에 저장된 공간이 있는 값만 가리킬 수 있음
    - call-by-reference 가능
      - 원본 변수를 다른 함수에서 직접 수정 가능
      - 불필요한 변수의 복사를 피할 수 있음
  - r-value

    - 대입 연산자(=) 좌변에 올 수 있으면 l-value, 올 수 없으면 r-value
    - r-value 예시: 상수, 식, 주솟값, 함수의 리턴 값 등

    ```cpp
    // 아래 예시는 전부 오류 발생

    10 = 15 // 10은 r-value(상수)

    int b = 1;
    b + 1 = 10; // b + 1은 r-value(식)

    int c, d, e[10], f[10];
    &c = &d;  // &c는 r-value(주소값)
    e = f;    // e는 r-value(주소값, e는 &e[0]와 같은 말이므로)

    func() = 1; // func()는 r-value(함수의 리턴값)
    ```

  - r-value 참조
    - r-value를 가리키는 무언가를 만들 때
    ```cpp
    int a = 10;
    int &&b = a + 1;
    int &&c = func();
    ```

## java

- 배열

```java
Geo[] geo = new Geo[]{
  new Geo(37.5, 126.9), new Geo(47.0, 10.2), new Geo(40.7, -74.2)
}
```

- List 자료 구조(java.util.ArrayList)

```java
ArrayList<Geo> geolist = new ArrayList<>();

geolist.clear();
geolist.add(new Geo(37.5, 126.9));
Geo gt = getlist.get(0);

System.out.println(geolist.size());
```

- Map 자료 구조(java.util.HashMap)

```java
HashMap<String, Geo> cities = new HashMap<String, Geo>();

cities.clear();
cities.put("Korea", new Geo(37.5, 126.9));

if (cities.containsKey("Korea"))
  Geo geo = cities.get("Korea");

System.out.println(cities.size());
```

- Object 클래스(java.lang.Object)
  - 자바 최상위 클래스
  - JVM의 메소드(네이티브 메소드)를 이용하여 메모리와 주소를 관리
    - 힙에 객체가 생성되면 부모 객체도 생성되고 생성된 주소는 부모의 주소
  - Object의 4대 메소드
    - getClass(), hashCode(), equals(), toString()
  - 그 외 메소드(깊은 복사, 쓰레드 동기화 관련)
    - clone(), notify(), notifyAll(), wait()

---
