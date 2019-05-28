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
  const obj1 = { [value]: 3 };  // { value: 3 }
  const obj2 = { ['ab' + 'c']: 3 };  // { abc: 3 }
  ```

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
  
### Loops and iteration
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

### 기본 타입
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
  const user: { name: string; age: number; } = { name: 'Hello', age: 20 };
  const user: { name: string; age?: number; } = { name: 'Hello' };
  const user: { readonly name: string; age: number; } = { name: 'Hello', age: 20 };
  ```
  

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
