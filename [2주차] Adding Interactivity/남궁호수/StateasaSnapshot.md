
state는 일반 변수와 다르게 함수 외부에 마치 선반에 있는 것처럼 react 자체에 존재한다. 무슨 의미냐, react가 컴포넌트를 호출하면 state의 스냅샷을 제공한다. 컴포넌트는 해당 렌더링(=컴포넌트 호출)의 state 값을 사용하여,
계산된 새로운 props와 이벤트 헨들러가 포함된 UI의 스냅샷을 JSX로 반환한다. 


***
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
setNumber을 세 번 호출했지만, state를 1로 세 번 설정한 것 뿐이다. 한번 버튼 클릭 시, 3이아닌 1로 렌더링이 된다.

```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 5);
        alert(number);
      }}>+5</button>
    </>
  )
}
```
또 다른 예제로, number는 처음에 0인 것을 확인할 수 있다.

***
```jsx
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 5);
        setTimeout(() => {
          alert(number);
        }, 3000);
      }}>+5</button>
    </>
  )
  ```
  react에 저장된 state는 settimeout의 alert가 실행 될 때 변경 되었을 수 있지만, 사용자가 상호작용(click)한 시점에 state의 스냅샷을..
  
  state 변수의 값은 이벤트 헨들러의 코드가 비동기적이더라도 렌더링 내에서 절대 변경되지 않는다.

정리: 
1. state를 설정하면 새 렌더링을 요청한다.
2. react는 컴포넌트 외부에 선반에 보관하듯? state를 저장한다.
3. useState를 호출하면 react는 렌더링에 대한 state 스냅샷을 제공한다.
4. 과거에 생성된 이벤트 헨들러는 그것이 생성된 레넏링 시점의 state를 갖는다.

