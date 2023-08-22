> 이번 파트에서 배울 내용
> - "batching" 이란 무엇이며 React가 여러 state 업데이트를 처리하는 방법  
> - 동일한 state 변수를 가지고 여러 업데이트를 연속적으로 적용하는 방법  

## 상태 batch 업데이트
```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 1);
        setNumber(number + 1);
        setNumber(number + 1);
      }}>+3</button>
    </>
  )
}
```

위 예시 코드에서 버튼을 눌러도 `setNumber(0 + 1)` 만 세 번 호출되는 결과가 나오는데 이것은 React가 상태를 업데이트하기 전에 이벤트 핸들러의 모든 코드가 실행될 때까지 기다리기 때문입니다.  

즉, `setNumber` 를 호출할 때마다 상태 업데이트 큐에 동작을 넣어놓고 모든 이벤트 핸들러가 완료됬을 때 순차적으로 큐에 있는 내용을 실행하게 됩니다.  

만약 위 소스코드의 원래 위도대로 처리하려면 상태 변경함수의 값으로 콜백을 인자로 보내주면 됩니다.  

```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(n => n + 1);
        setNumber(n => n + 1);
        setNumber(n => n + 1);
      }}>+3</button>
    </>
  )
}
```
