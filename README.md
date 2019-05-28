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

### Function
- Javascript의 함수는 1급 객체 : HoC, Currying, Memoization이 가능
  - 변수(variable)에 담음
  - 인자(parameter)로 전달
  - 반환값(return value)으로 전달

### Loops and iteration
- while 과 do while 의 차이 : 최초 실행 여부  
  ``` javascript
  let i = 0;
  do {
    i = i + 1;
  } while (i < 5);
  ```
  > while(true) {break;} 패턴으로 써도 무방할 듯


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
