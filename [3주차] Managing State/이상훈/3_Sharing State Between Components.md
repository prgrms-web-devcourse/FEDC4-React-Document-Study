> 이번 파트에서 배울 내용
> - 상태를 끌어올려서 컴포넌트끼리 공유하는 방법  
> - 제어 컴포넌트와 비제어 컴포넌트  

## 상태 끌어올리기
여러 컴포넌트끼리 공유해서 사용해야 하는 상태가 있는 경우에 각각의 컴포넌트가 갖던 상태를 부모 컴포넌트로 옮기고 props 로 전달받는 형태로 변경하는 것을 '상태 끌어올리기' 라고 합니다.  

상태 끌어올리기 과정은 크게 3단계입니다.  

### 1. 자식 컴포넌트에서 state 제거
자식 컴포넌트에 정의된 상태를 제거합니다.

```jsx
const [isActive, setIsActive] = useState(false); // 제거
```

### 2. 하드 코딩된 데이터를 부모 컴포넌트로 전달하기
자식 컴포넌트에 props 를 추가하고, 부모 컴포넌트가 자식 컴포넌트에게 하드 코딩된 값을 내려줍니다.

```jsx
import { useState } from 'react';

export default function Accordion() {
  return (
    <>
      <h2>Almaty, Kazakhstan</h2>
      <Panel title="About" isActive={true}>
        With a population of about 2 million, Almaty is Kazakhstan's largest city. From 1929 to 1997, it was its capital city.
      </Panel>
      <Panel title="Etymology" isActive={true}>
        The name comes from <span lang="kk-KZ">алма</span>, the Kazakh word for "apple" and is often translated as "full of apples". In fact, the region surrounding Almaty is thought to be the ancestral home of the apple, and the wild <i lang="la">Malus sieversii</i> is considered a likely candidate for the ancestor of the modern domestic apple.
      </Panel>
    </>
  );
}

function Panel({ title, children, isActive }) {
  return (
    <section className="panel">
      <h3>{title}</h3>
      {isActive ? (
        <p>{children}</p>
      ) : (
        <button onClick={() => setIsActive(true)}>
          Show
        </button>
      )}
    </section>
  );
}
```

### 3. 공통 부모에 state 추가하기
부모 컴포넌트에 상태를 선언하고, 자식에게 상태와 이벤트 핸들러를 전달합니다.  
여기서 부모 컴포넌트는 상태의 **단일 진리의 원천(single source of truth)** 가 됩니다.

```jsx
import { useState } from 'react';

export default function Accordion() {
  const [activeIndex, setActiveIndex] = useState(0);
  return (
    <>
      <h2>Almaty, Kazakhstan</h2>
      <Panel
        title="About"
        isActive={activeIndex === 0}
        onShow={() => setActiveIndex(0)}
      >
        With a population of about 2 million, Almaty is Kazakhstan's largest city. From 1929 to 1997, it was its capital city.
      </Panel>
      <Panel
        title="Etymology"
        isActive={activeIndex === 1}
        onShow={() => setActiveIndex(1)}
      >
        The name comes from <span lang="kk-KZ">алма</span>, the Kazakh word for "apple" and is often translated as "full of apples". In fact, the region surrounding Almaty is thought to be the ancestral home of the apple, and the wild <i lang="la">Malus sieversii</i> is considered a likely candidate for the ancestor of the modern domestic apple.
      </Panel>
    </>
  );
}
```

## 제어 컴포넌트와 비제어 컴포넌트
- **제어 컴포넌트**: 상위 컴포넌트가 state 를 갖고 있고, 하위 컴포넌트가 내려받아서 사용하는 형태이면 제어 컴포넌트라고 합니다. props 를 통해서 컴포넌트가 제어되기 때문입니다.  
- **비제어 컴포넌트**: 각각의 하위 컴포넌트가 자체적인 state를 갖고 있어서 상위 컴포넌트에서는 하위 컴포넌트를 제어할 수 없는 형태이면 비제어 컴포넌트라고 합니다.  

이런 용어는 엄격한 기술 용어는 아니며, 일반적으로 컴포넌트는 지역 state와 props를 혼합해서 사용합니다.  
