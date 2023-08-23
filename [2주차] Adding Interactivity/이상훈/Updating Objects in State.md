> 이번 파트에서 배울 내용
> - 리액트의 상태에서 객체를 올바르게 변경하는 방법  
> - 중첩된 객체를 변경하지 않고 업데이트하는 방법  
> - 불변성이 무엇이고 깨트리지 않는 방법  
> - 이벤트 핸들러가 상태의 `snapshot` 에 접근하는 방법  

## State를 읽기 전용인 것처럼 다루세요  
상태에는 원시 타입부터 참조 타입까지 모든 종류의 값을 저장할 수 있는데  
참조 타입인 경우 반드시 읽기 전용인 것처럼 다루어야 합니다.  

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
        position.x = e.clientX;
        position.y = e.clientY;
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

위 예시 코드에서 문제가 되는 부분은:

```jsx
onPointerMove={e => {
  position.x = e.clientX;
  position.y = e.clientY;
}}
```

이 부분인데 이렇게 상태에 저장된 객체를 직접 수정하면 리액트가 변화를 감지하지 못하기 때문에 새롭게 렌더링하는 역할을 수행하지 못합니다.  

대신에 새 객체를 생성하여 상태 설정 함수로 전달해야 합니다:  

```jsx
onPointerMove={e => {
  setPosition({
    x: e.clientX,
    y: e.clientY
  });
}}
```
