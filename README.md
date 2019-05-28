# study
스터디

## Javascript

### Array
- `arr.reduce((acc, curr, idx, arr) => acc + curr, 0)`
- fill
  - `arr.fill(val, start, end)` : 지정된 인덱스 채우기
  - `const arr = Array(length).fill(value)` : 배열을 생성 및 채우기
- `arr.pop()` : 배열에서 마지막 요소를 제거
- `arr.unshift(val1, val2)` : 배열의 맨 앞쪽에 추가
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
