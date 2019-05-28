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
