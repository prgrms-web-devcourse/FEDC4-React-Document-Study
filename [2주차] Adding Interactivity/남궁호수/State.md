## 컴포넌트의 메모리, state

useState 훅
- 렌더링 사이에 데이터를 유지하기 위한 state 변수
- 변수를 업데이트하고 컴포넌트를 다시 렌더링하도록 하는 state 설정자 함수

*** 
state 변수 추가

```jsx
import { useState } from 'react';

const [state, setState] = useState(0);
```
state는 변수, setState 설정자 함수이다.
여기서 [ 와 ] 구문을 배열 구조분해라고 하며, 배열에서 값을 읽는다. useState가 반환하는 배열에는 항상 정확히 두 개의 항목이 있다.
이 쌍의 이름은 const [something, setSomething]과 같이 지정하는 것이 일반적이다. 원하는 대로 이름을 지을 수 있다.


```jsx
import { useState } from 'react';
import { sculptureList } from './data.js';

export default function Gallery() {
  const [index, setIndex] = useState(0);
    ///useState(0) 0 은 state의 초기값을 의미한다.
  function handleClick() {
    setIndex(index + 1);
  }

  let sculpture = sculptureList[index];
  return (
    <>
      <button onClick={handleClick}>
        Next
      </button>
      <h2>
        <i>{sculpture.name} </i> 
        by {sculpture.artist}
      </h2>
      <h3>  
        ({index + 1} of {sculptureList.length})
      </h3>
      <img 
        src={sculpture.url} 
        alt={sculpture.alt}
      />
      <p>
        {sculpture.description}
      </p>
    </>
  );
}
```

***
훅
use로 시작하는 함수들을 훅이라 한다.
훅은 컴포넌트의 최상위 레벨 (혹은 커스텀 훅)에서만 호출할 수 있다. 조건문, 반복문, 중첩된 함수 내부에서 훅을 호출할 수 없다. 

컴포넌트가 렌더링 될 때마다, useState는 저장한 값인 state와 state 설정자 함수를 가진 배열을 주게 된다.
렌더링할때마다, useState(0)으로 초기값을 바라보긴하지만, state값은 저장한 값을 기억한다. 

여러 값을 가지고 있는 form의 경우는 값 별로, state 변수를 사용하는 것보다 객체를 값으로 하나의 state 변수를 사용하는게 편리하다.
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

***
state는 독립적이다.
동일한 컴포넌트를 두 군데에서 렌더링하면 각 사본은 완전히 격리된 state를 갖게 된다. 즉, state는 화면상의 특정 위치에 '지역적'이라고 할 수 있다.
props와 달리 state는 이를 선언하는 컴포넌트 외에는 완전히 비공개이고, 부모 컴포넌트는 이를 변경할 수 없다. 따라서 다른 컴포넌트에 영향을 주지 않고 state를 추가하거나 삭제할 수 있다.

추가 내용: 
훅은 모듈에서 import할 때와 마찬가지로, 컴포넌트 안에서 조건과 무관하게 항상 호출한다.