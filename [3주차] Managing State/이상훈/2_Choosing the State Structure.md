> 이번 파트에서 배울 내용
> - 단일 상태와 다중 상태를 언제 사용해야 하는지    
> - 상태를 구성할 때 피해야 할 요인  
> - 상태 구조와 관련한 일반적인 문제를 어떻게 해결하는지  

## 상태를 구조화할 때의 원칙
- **그룹 관련 상태**: 두 개 이상의 상태가 늘 같은 시각에 업데이트 된다면, 하나의 상태로 합칠 것을 고려해야 합니다.  
- **상태의 모순**: 각각의 상태가 표현하는 정보에 모순이 발생한다면, 하나의 상태로 합칠 것을 고려해야 합니다.  
- **불필요한 상태**: 이미 존재하는 props나 state로부터 유추할 수 있는 정보라면 굳이 상태로 만들지 않아도 됩니다.  
- **상태 중복**: 같은 데이터가 여러 state에 흩뿌려져 있다면 동기화에 어려움이 발생합니다. 따라서 유의해야 합니다.  
- **깊게 중첩된 상태**: 깊게 계층화된 상태는 관리하기에 어렵기 때문에 상태를 가능한 평평한 형태로 구성하는 것이 좋습니다.  

## 그룹 관련 상태
x, y 좌표는 항상 함께 업데이트되기 때문에 각각의 상태 변수로 두는 것 보다는 하나의 상태로 통합하는 것이 좋습니다.

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

## 상태의 모순
`isSending` 과  `isSent` 는 서로 표현하는 정보의 동기가 맞지 않을 가능성이 있습니다. 따라서 하나의 상태로 합치는 것이 좋습니다.  

```jsx
const [text, setText] = useState('');
const [isSending, setIsSending] = useState(false);
const [isSent, setIsSent] = useState(false);
```

```jsx
const [text, setText] = useState('');
const [status, setStatus] = useState('typing');
```

## 불필요한 상태
`fullName` 은 이미 존재하는 `firstName` 과 `lastName` 의 조합을 통해서 유추할 수 있습니다.  
따라서 굳이 상태로 만들지 않아도 됩니다.

```jsx
const [firstName, setFirstName] = useState('');
const [lastName, setLastName] = useState('');
const [fullName, setFullName] = useState('');
```

```jsx
const [firstName, setFirstName] = useState('');
const [lastName, setLastName] = useState('');
const fullName = firstName + ' ' + lastName;
```

## 상태 중복
사용자가 선택한 아이템 정보 자체를 새로운 상태로 만든다면 상태 간 동기가 맞지 않을 가능성이 생깁니다. 따라서 가장 필수적인 정보인 `ID` 만 상태로 만드는 것이 좋습니다.

```jsx
const initialItems = [
  { title: 'pretzels', id: 0 },
  { title: 'crispy seaweed', id: 1 },
  { title: 'granola bar', id: 2 },
];

export default function Menu() {
  const [items, setItems] = useState(initialItems);
  const [selectedItem, setSelectedItem] = useState(items[0]);

  // ... 생략
}
```

```jsx
const [items, setItems] = useState(initialItems);
const [selectedId, setSelectedId] = useState(0);
```

## 깊게 중첩된 상태
깊게 중첩된 상태는 관리하기 어렵기 때문에 왠만하면 평평한 형태의 상태로 만들어 주는 것이 좋습니다.  

```jsx
export const initialTravelPlan = {
  id: 0,
  title: '(Root)',
  childPlaces: [{
    id: 1,
    title: 'Earth',
    childPlaces: [{
      id: 2,
      title: 'Africa',
      childPlaces: [{
        id: 3,
        title: 'Botswana',
        childPlaces: []
      }, {
        id: 4,
        title: 'Egypt',
        childPlaces: []
      }]
    }]
}]}
```

```jsx
export const initialTravelPlan = {
  0: {
    id: 0,
    title: '(Root)',
    childIds: [1, 43, 47],
  },
  1: {
    id: 1,
    title: 'Earth',
    childIds: [2, 10, 19, 27, 35]
  },
}
```
