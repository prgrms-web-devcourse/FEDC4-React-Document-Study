> 이번 파트에서 배울 내용
> - 리액트가 컴포넌트 구조를 어떻게 바라보는지  
> - 리액트가 언제 상태를 보존하고 초기화하는지  
> - 리액트가 컴포넌트 상태를 초기화 하도록 만드는 방법  
> - 키와 타입이 상태 보존에 어떻게 영향을 주는지  

## UI 트리
리액트 컴포넌트는 트리 형태의 구조로 구성되어 있습니다.  

![트리](https://ko.react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fpreserving_state_dom_tree.png&w=1080&q=75)  

### 상태는 트리에서의 위치에 묶여있습니다.
각각 트리에서 자기 고유의 위치에 렌더링되어 있는 컴포넌트의 상태는 완전히 분리되어 있습니다.

![카운터](https://ko.react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fpreserving_state_tree.png&w=640&q=75)  

```jsx
import { useState } from 'react';

export default function App() {
  return (
    <div>
      <Counter />
      <Counter />
    </div>
  );
}

function Counter() {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
        Add one
      </button>
    </div>
  );
}
```

### 컴포넌트가 제거되면 상태도 함께 제거됩니다.  
![삭제시](https://ko.react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fpreserving_state_remove_component.png&w=640&q=75)  

![추가시](https://ko.react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fpreserving_state_add_component.png&w=640&q=75)  

### 같은 자리의 같은 컴포넌트는 상태를 보존합니다
`isFancy` 상태가 바뀔 때마다 카운터에게 전달되는 `props` 가 달라져서 리렌더링이 발생하지만, UI 트리 상에서 `Counter` 컴포넌트의 위치는 동일하기 때문에 `Counter` 컴포넌트 가 갖고 있는 상태는 계속 유지되고 있다는 것을 확인할 수 있습니다.  

```jsx
import { useState } from 'react';

export default function App() {
  const [isFancy, setIsFancy] = useState(false);
  return (
    <div>
      {isFancy ? (
        <Counter isFancy={true} /> 
      ) : (
        <Counter isFancy={false} /> 
      )}
      <label>
        <input
          type="checkbox"
          checked={isFancy}
          onChange={e => {
            setIsFancy(e.target.checked)
          }}
        />
        Use fancy styling
      </label>
    </div>
  );
}

function Counter({ isFancy }) {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }
  if (isFancy) {
    className += ' fancy';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
        Add one
      </button>
    </div>
  );
}
```

### 같은 위치의 다른 컴포넌트는 상태를 초기화합니다
`isPaused` 상태가 변경될 때 `<p>` 컴포넌트나 `<Counter>` 컴포넌트를 렌더링하고 있어서 컴포넌트가 변경되기 때문에 상태의 초기화가 발생하고 있습니다.  

![](https://ko.react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fpreserving_state_diff_pt1.png&w=828&q=75)  

```jsx
import { useState } from 'react';

export default function App() {
  const [isPaused, setIsPaused] = useState(false);
  return (
    <div>
      {isPaused ? (
        <p>See you later!</p> 
      ) : (
        <Counter /> 
      )}
      <label>
        <input
          type="checkbox"
          checked={isPaused}
          onChange={e => {
            setIsPaused(e.target.checked)
          }}
        />
        Take a break
      </label>
    </div>
  );
}

function Counter() {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
        Add one
      </button>
    </div>
  );
}
```

### 같은 위치에 다른 컴포넌트를 렌더링할 때 컴포넌트는 그의 전체 서브 트리의 상태를 초기화합니다.
`<Counter>` 컴포넌트의 위치는 동일하지만, 부모 컴포넌트가 `<div>` 와 `<section>` 으로 달라지고 있으므로 서브 트리의 상태도 모두 초기화되고 있습니다.  

이것이 컴포넌트 함수를 중첩해서 정의하면 안 되는 이유입니다.

```jsx
import { useState } from 'react';

export default function App() {
  const [isFancy, setIsFancy] = useState(false);
  return (
    <div>
      {isFancy ? (
        <div>
          <Counter isFancy={true} /> 
        </div>
      ) : (
        <section>
          <Counter isFancy={false} />
        </section>
      )}
      <label>
        <input
          type="checkbox"
          checked={isFancy}
          onChange={e => {
            setIsFancy(e.target.checked)
          }}
        />
        Use fancy styling
      </label>
    </div>
  );
}

function Counter({ isFancy }) {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }
  if (isFancy) {
    className += ' fancy';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
        Add one
      </button>
    </div>
  );
}
```

### 같은 위치에서 상태를 초기화하기 
기본적으로 같은 위치의 상태는 유지되지만, 이걸 초기화하고 싶은 경우에는 `key` 속성을 다른 값으로 주면 됩니다.  

컴포넌트에 `key` 를 명시하면 리액트는 UI 트리의 순서 대신에 `key` 자체를 위치의 일부로 사용하기 때문입니다.  

> 단, `key` 는 전역적으로 유일하지는 않고 오직 부모 안에서만 자리를 명시합니다.  

```jsx
import { useState } from 'react';

export default function Scoreboard() {
  const [isPlayerA, setIsPlayerA] = useState(true);
  return (
    <div>
      {isPlayerA ? (
        <Counter key="Taylor" person="Taylor" />
      ) : (
        <Counter key="Sarah" person="Sarah" />
      )}
      <button onClick={() => {
        setIsPlayerA(!isPlayerA);
      }}>
        Next player!
      </button>
    </div>
  );
}

function Counter({ person }) {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{person}'s score: {score}</h1>
      <button onClick={() => setScore(score + 1)}>
        Add one
      </button>
    </div>
  );
}
```
