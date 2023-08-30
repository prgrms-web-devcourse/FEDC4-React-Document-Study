두 개 이상의 state 변수를 동시에 업데이트 하는 경우, 단일 state 변수로 병합한다.
렌더링 중 컴포넌트의 props나 기존 state 변수에서 일부 정보를 계산할 수 있다면, 새로 state를 만들지 말자.
깊게 중첩된 state는 피하도록 한다.
```jsx
import { useState } from 'react';

export default function MovingDot() {
  const [position, setPosition] = useState({
    x: 0,
    y: 0
  });
  return (
    <div
      onPointerMove={e => {
        setPosition({
          x: e.clientX,
          y: e.clientY
        });
      }}
      style={{
        position: 'relative',
        width: '100vw',
        height: '100vh',
      }}>
      <div style={{
        position: 'absolute',
        backgroundColor: 'red',
        borderRadius: '50%',
        transform: `translate(${position.x}px, ${position.y}px)`,
        left: -10,
        top: -10,
        width: 20,
        height: 20,
      }} />
    </div>
  )
}
```
