> 이번 파트에서 배울 내용
> - `useState` 로 상태 변수를 추가하는 방법  
> - `useState` 가 반환하는 한 쌍의 데이터  
> - 하나 이상의 상태 변수를 추가하는 방법  
> - 상태를 로컬이라고 부르는 이유  

## 일반 지역 변수를 사용하기에 적합하지 않은 경우
```jsx
import { sculptureList } from './data.js';

export default function Gallery() {
  let index = 0;

  function handleClick() {
    index = index + 1;
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

위 예시 코드에서는 `Next` 버튼을 누르면 다음 번호의 조각상에 대한 정보가 나타나야 하는데 아무런 변화가 생기지 않습니다. 즉, 의도한 동작을 정상적으로 하지 않습니다.  

이 이유는 2가지입니다.

1. 일반 지역 변수는 렌더링이 발생할 때 이전의 값이 유지되지 않습니다.  
2. 일반 지역 변수를 변경해도 `React` 가 이를 감지하지 못하기에 렌더링이 새롭게 발생하지 않습니다.  

그렇기 때문에 상태 값이 변경되는 것을 React가 감지하도록 하고, 이전 렌더링에서의 값을 유지하기 위해서는 `useState` 를 통해 상태 변수를 관리해야 합니다.  

## useState 사용
```jsx
const [index, setIndex] = useState(0);
```
`useState` 는 **상태 변수**와 **상태 변경 함수**를 반환합니다.  

만약 컴포넌트 내부의 이벤트 핸들러같은 곳에서 상태 변경 함수를 호출해서 상태 값에 변화가 발생하면 리액트는 컴포넌트를 다시 실행하여 변경된 상태 값으로 새롭게 렌더링을 합니다.  

조각상 예시 코드를 `useState` 로 올바르게 사용해보면 다음과 같습니다:  

```jsx
import { useState } from 'react';
import { sculptureList } from './data.js';

export default function Gallery() {
  const [index, setIndex] = useState(0);

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

> 상태는 격리되고 지역적입니다. 즉, 어떤 컴포넌트의 상태가 변화한다고 해도 다른 컴포넌트에는 영향을 주지 않습니다.  

