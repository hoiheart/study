# study
스터디


## Javascript

### ES6
- let, const : 블록 수준 스코프, 호이스팅 (변수 선언 후 재할당 가능)
- var : 함수 수준 스코프, 호이스팅 (변수 선언 전 값 할당 가능)
- 비구조화 할당 : `const [a, b] = [1, 2]`
- 계산된 속성 이름
  ```javascript
  const val = 'value';
  const obj1 = {[val]: 3};  // { value: 3 }
  const obj2 = {['ab' + 'c']: 3};  // { abc: 3 }
  ```

### ES7
- [Decorator](https://blog-kr.zoyi.co/channel-frontend-decorator/)

### Math
- Math.max(arr) : 0이상의 숫자 중 가장 큰 숫자를 반환
- Math.min(arr) : 0이상의 숫자 중 가장 작은 숫자를 반환
- Math.abs(val) : 양의 정수로 

### Array
- `arr.reduce((acc, curr, idx, arr) => acc + curr, 0)`
- fill
  - `arr.fill(val, start, end)` : 지정된 인덱스 채우기
  - `const arr = Array(length).fill(value)` : 배열을 생성 및 채우기
- `arr.pop()` : 배열에서 마지막 요소를 제거
- `arr.push()` : 배열 뒤로 추가
- `arr.shift()` : 배열의 맨 앞쪽 제거
- `arr.unshift(val1, val2)` : 배열 앞으로 추가
- splice
  - `arr.splice(start, deleteCount)` : 제거
  - `arr.splice(1, 0, 'Feb')` : 추가
  - `arr.splice(4, 1, 'May')` : 대체
  
### Loops / iteration
- while 과 do while 의 차이 : 최초 실행 여부  
  ``` javascript
  let i = 0;
  do {
    i = i + 1;
  } while (i < 5);
  ```
  > while(true) {break;} 패턴으로 써도 무방할 듯
- forEach : break 나 return 제어가 불가능 (이터러블이라면 for-of 추천)
  ```javascript
  const languages = ['JavaScript', 'TypeScript', 'Python'];
  for (const lang of languages) {
    if (!lang.includes('Script')) {
      break;
    }
    console.log(lang);
  }
  ```
- 이터레이터 프로토콜
  - 객체가 특정 조건을 만족하는 next() 메소드를 가짐
  - next() 가 호출될 때마다 done(boolean), value(done === false) 객체 반환
  ```javascript
  function makeIterator(array) {
      var nextIndex = 0;
      return {
         next: function() {
             return nextIndex < array.length
                 ? { value: array[nextIndex++], done: false } 
                 : { done: true};
         }
      };
  }
  const iter = makeIterator([1, 2, 3]);
  iter.next(); // { value: 1, done: false }
  iter.next(); // { value: 2, done: false }
  iter.next(); // { value: 3, done: false }
  iter.next(); // { done: true }
  ```
- 이터러블 프로토콜
  - 객체가 Symbol.iterator의 키의 값으로 메소드를 갖고, 해당 메소드를 실행했을 때 이터레이터 인스턴스가 반환됨
  - Array, Map, Set 등의 표준 객체는 모두 이터러블 프로토콜을 구현
  - 동일할 문법으로 여러 객체를 순회할 수 있는 수단을 제공
  ```javascript
  function makeIterator(array) {
      var nextIndex = 0;
      return {
         next: function() {
             return nextIndex < array.length
                 ? { value: array[nextIndex++], done: false } 
                 : { done: true};
         }
      };
  }
  const iterableObj = {
    [Symbol.iterator]() { return makeIterator([1, 2, 3]); }
  };
  
  for (const elem of iterableObj) {
    console.log(elem);
  }
  // 1
  // 2
  // 3
  
  console.log(...iterableObj); // [1, 2, 3]
  ```

### Function
- Javascript의 함수는 1급 객체 : HoC, Currying, Memoization이 가능
  - 변수(variable)에 담음
  - 인자(parameter)로 전달
  - 반환값(return value)으로 전달
- 화살표 함수와 function 선언 함수의 차이
  - 생성자로 사용할 수 없다.
  - 함수 내에 arguments 바인딩이 존재하지 않는다.
  - prototype 프로퍼티를 갖고 있지 않는다
- [클로저](https://meetup.toast.com/posts/90)
  - 생성될 당시의 환경을 기억하는 함수 (스코프체인을 통해 접근할 수 있는 변수나 함수가 스코프가 해제되어야 할 시점에도 사라지지 않음)
  - 외부 함수가 종료되더라고 내부 함수가 실행되는 상태이면 내부 함수에서 참조하는 외부 함수는 닫히지 못하고 내부 함수에 의해 닫힘
  - 캡슐화와 은닉화를 구현하는데 사용 (객체 프로퍼티의 외부 접근 차단)
  ``` javascript
  function counterFactory() {
    var _count = 0;

    return function() {
      _count += 1;

      return _count;
    };
  }

  var counter = counterFactory();
  var counter2 = counterFactory();

  console.log(counter()); //1
  console.log(counter()); //2
  console.log(counter2()); //1
  ```
  - 모듈 패턴은 객체와 클로저를 적절히 혼용하여 자바스크립트에는 존재하지 않는 접근제한자를 구현
  - 메모리 소모 및 퍼포먼스 손해
- [Curryng](https://dev-momo.tistory.com/entry/Currying-in-Javascript)
  - 여러개의 인자(parameter)를 갖는 함수를 단일 인자를 갖는 함수들의 연결로 바꾸는 것
  ``` javascript
  const sum = x => y => x + y;
  sum(5)(7); // 12
  ```
  - 재사용성 향상

### Sync / Async
- 콜백을 사용한 비동기 작업 처리 : 작업의 단계가 깊어짐에 따라 들여쓰기 또한 급격히 깊어짐
- Promise
  - resolve()의 호출된 경우, 즉 해당 비동기 작업이 완료 된 경우의 핸들러인 then()
    - then 핸들러의 두 번째 콜백은 에러가 발생했을 때에 실행되며 에러 객체를 인자로 받음
  - reject()의 호출된 경우, 즉 해당 비동기 작업이 거부된 경우의 핸들러인 catch()
  ``` javascript
  function getRandomPromise () {
    return new Promise((resolve, reject) => {
      setTimeout(function () {
        const destiny = Math.random();
        if (destiny > 0.5) {
          resolve();
        } else {
          reject();
        }
      })
    });
  }
  
  fetch('https://this-is-invalid-url.really').catch(err => { 
    const { message } = err;
    console.log(message); // Failed to fetch
  });

  function errorHandler(err) {
    if (err) {
      console.log(err);
    }
  }
  fetchDocument(url)
  .then(document => fetchAuthor(document), errorHandler)
  .then(author => fetchPostsFromAuthor(author), errorHandler)
  .then(posts => /* do something with posts */, errorHandler);
  ```
- Async / Await
  - 함수 선언 앞에 async 키워드를 덧붙여 비동기 함수 정의
  - 프로미스가 완료될 경우 resolve의 인자로 사용된 값을 가지며, 프로미스가 거부될 시 오류를 그대로 위로 던진다 (try catch 이용 가능)
  ```javascript
  async function returnTheAnswer() {
    try {
      const document = await fetchDocument(url);
      const author = await fetchAuthor(document);
      const posts = await fetchPostsFromAuthor(author);
    } catch (err) {
      console.log(err);
    }
  }
  ```


## Typescript

변수의 타입이 런타임에서 결정되는 동적 타입 언어인 javascript의 단점을 보완하는 언어로 javascript의 상위 집합

### 기초 문법
- 타입 표기는 식별자 또는 값 뒤에 콜론(:)을 붙여 value: type 의 형태로 표기  
  `const typescript: string = "great";`
- void : 아무런 값도 반환하지 않는 함수의 반환 타입 (null or undefined)
- never : 아무런 값도 가질 수 없는 타입 (ex: throw new Error를 반환하는 함수)
- 배열 타입
  ```javascript
  const pibonacci: Array<number> = [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55];
  const nameAndAge: [string, number] = ['Hello', 20];
  ```
- 객체 타입
  ```javascript
  const user1: { name: string; age: number; } = { name: 'Hello1', age: 20 };
  const user2: { name: string; age?: number; } = { name: 'Hello2' };
  const user3: { readonly name: string; age: number; } = { name: 'Hello3', age: 25 };
  ```
- 타입 별칭
  ```javascript
  type UUID = string;
  function getUser(uuid: UUID) {}
  ```
- 함수 타입
  - function
  ```javascript
  function sum(a: number, b: number): number {
    return (a + b);
  }
  function fetchVideo(url: string, subtitleLanguage?: string): void {
    const option = { url };
    if (subtitleLanguage) option.subtitleLanguage = true;
  }
  ```
  - 화살표 함수
  ```javascript
  const arrowSum: (a: number, b: number) => number = (a, b) => (a + b);
  ```
 - 제너릭 타입 : 여러 타입에 대해 동작하는 요소를 정의하되, 해당 요소를 사용할 때가 되어야 알 수 있는 타입 정보를 정의에 사용
  ```javascript
  type MyArray<T> = T[];
  const drinks: MyArray<string> = ['Coffee', 'Milk', 'Beer'];
  ```
- 유니온 타입 : 가능한 모든 타입을 파이프(|) 기호로 이어서 표현
  ```javascript
  const stringOrNumber: string | number = square(randomNumber, randomBoolean);
  ```
- 인터섹션 타입 : 이미 존재하는 여러 타입을 모두 만족하는 타입을 표현
- 열거형(enum) 타입 : 유한한 경우의 수를 갖는 값의 집합을 표현
- 타입 별칭과 인터페이스의 차이
  - 타입 별칭을 이용해서 기본 타입, 배열과 튜플, 유니온 타입 등에 새로운 이름을 붙일 수 있다 (type UUID = string). 인터페이스로는 해당 타입을 표현하는 것이 불가능하다.
  - 타입 별칭은 실제로는 새 타입을 생성하지 않는다. 따라서 type User = { name: string; } 타입과 관련된 타입 에러가 발생했을 시 에러 메시지는 User 대신 { name: string; } 를 보여준다. 한편 인터페이스는 실제로 새 타입을 생성하고, interface User { name: string; } 과 관련된 에러 메시지에는 User 가 등장한다.
  - 인터페이스는 곧 다룰 extends 키워드를 이용해 확장할 수 있는 반면, 타입 별칭의 경우는 그런 수단을 제공하지 않는다.
 > 기본적으로 인터페이스로 표현할 수 있는 모든 타입은 인터페이스로 표현하고, 기본 타입에 새로운 이름을 붙이고 싶거나 유니온 타입을 명명하고 싶은 경우 등 인터페이스의 능력 밖인 부분에서만 타입 별칭을 사용


### Interface / Class
- Interface: 값이 특정한 형태(shape)를 갖도록 제약
  ```javascript
  interface User {
    name: string;
    readonly age: number; // 읽기 전용 속성
    hobby?: string; // 선택 속성
  }
  const author: User = { name: 'hello', age: 20 };
  
  interface GetUserName {
    (user: User): string;
  }
  const getUserName: GetUserName = function (user) {
    return user.name;
  };
  ```


## React

### 기능
- Fragment : `<></>` 로 사용 가능
- Portals : UI 를 어디에 렌더링 시킬지 DOM 을 사전에 선택하여 부모 컴포넌트의 바깥에 렌더링
- HOC (Higher Order Component) : 화면에서 재사용 가능한 로직만을 분리해서 component로 만들고, 재사용 불가능한 UI와 같은 다른 부분들은 parameter로 받아서 처리
  ```javascript
  export default class Banners extends Component {
    componentWillMount() { /* 배너 정보 가져오기 */ }

    render() {
        return (this.props.children)(this.state.banners)
    }
  }

  /* 재사용 */
  <Banners>{banners => /* 홈 Banner용 UI */ }</Banners>
  <Banners>{banners => /* 다른 Banner용 UI */ }</Banners>
  ```
  - wrapper hells, component life cycle 종속 등의 문제
- Hooks : class없이 순수 함수로 state와 React life cycle을 hook 할 수 있는 기능을 제공
  - useState
    ```javascript
    import { useState } from 'react';

    function Example() {
      const [name, setName] = useState("이름");
      return <input value={name} onChange={(e) => setName(e.target.value)} />;
    }
    ```
  - useEffect : componentDidMount, componentDidUpdate, componentWillUnmount를 합쳐 hook
    ```javascript
    function useEffect(effect: EffectCallback, inputs?: InputIdentityList)

    useEffect(() => func(), []);
    useEffect(() => func(), [count]); // count state가 변경될 때만 func 실행

    /* effect 함수의 return값이 있는 경우, hook의 cleanup 함수로 인식하고 다음 effect가 실행되기 전에 실행 */
    useEffect(() => {
      window.addEventListener("mousemove", logMousePosition);
      return () => {
        window.removeEventListener("mousemove", logMousePosition);
      };
    }, []);

    window.addEventListener("mousemove", logMousePosition); // mount 
    /* inputs 지정된 특정 값이 업데이트 된 경우 */
    window.removeEventListener("mousemove", logMousePosition); // cleanup
    window.addEventListener("mousemove", logMousePosition); 
    window.removeEventListener("mousemove", logMousePosition); // unmount
    ```
  - useMemo, useCallback : 렌더링 성능 최적화

### 기타
- 스타일링 방식 : CSS, Sass, CSS Module, styled-components(CSS-in-JS)
- redux : 글로벌 상태관리
- redux-saga : 비동기 API 호출을 generator를 이용하여 callback 메소드 없는 코드를 생성


## 알고리즘

- [쇠막대기](https://programmers.co.kr/learn/courses/30/lessons/42585) : [참고](https://medium.com/@nsh235482/java-coding-programmers-stack-queue-lv2-%EC%87%A0%EB%A7%89%EB%8C%80%EA%B8%B0-d3c482da3d98)
  ``` javascript
  function solution(arrangement) {
      let stack = 0;
      const array = arrangement.replace(/\(\)/g, '|').split('');
      const result = array.reduce((acc, curr) => {
          if (curr === '|') {
              return acc = acc + stack;
          } else if (curr === '(') {
              stack = stack + 1;
              return acc;
          } else if (curr === ')') {
              stack = stack - 1;
              return acc = acc + 1;
          }
      }, 0);
      return result;
  }
  ```


## 참고

### React vs Vue
  - Virtual DOM 으로 빠른 렌더링
  - 경량 라이브러리
  - Reactive Component
  - Server Side Rendering
  - 라우터, 번들러, state management 와 결합이 쉬움
  - 훌륭한 개발자 커뮤니티와 지원

#### Vue
  - Template 과 Render Function 을 모두 사용할 수 있는 옵션
  - 간편한 Syntax 와 프로젝트 설정
  - 빠른 렌더링과 더 작은 용량
  
#### React
  - 큰 규모에서 더 빛을 발하고, 테스팅이 수월
  - Web 과 Native 앱 개발에 모두 사용 가능
  - 더 큰 개발자 생태계에서 오는 많은 레퍼런스와 도구들

### 프로그래밍
  - [디자인 패턴](https://joshua1988.github.io/web-development/javascript/javascript-pattern-design/)
  - [함수형 프로그래밍](https://velog.io/@kyusung/함수형-프로그래밍-요약)
  - [메모리 관리](https://developer.mozilla.org/ko/docs/Web/JavaScript/Memory_Management)
  - [이벤트](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)
  - [테스트 주도 개발](https://asfirstalways.tistory.com/296)

### 브라우저
  - [렌더링 과정](http://d2.naver.com/helloworld/59361)
  - [렌더링 성능](https://developers.google.com/web/fundamentals/performance/rendering/?hl=ko)
  - [HTTP, SSL 통신 과정](https://aileen93.tistory.com/119)
  - [응답 코드](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)
  - [header](https://gmlwjd9405.github.io/2019/01/28/http-header-types.html)
  - [Restful](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)