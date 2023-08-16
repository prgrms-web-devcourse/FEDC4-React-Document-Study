> 이번 파트에서 배울 내용
> - root 컴포넌트란 무엇인가?
> - 언제 `default import/export` 를 쓰고, 언제 `named import/export` 를 쓰는가?  
> - 어떻게 컴포넌트를 작성하는가?  
> - 한 파일에서 어떻게 여러 컴포넌트를 `import/export` 하는가?  
> - 컴포넌트를 어떻게 여러 파일로 분리하는가?  

## Root 컴포넌트
Root 컴포넌트는 리액트 애플리케이션에서 가장 최상위에 존재하는 컴포넌트로 Create React App 에서는 `src/App.js` 가 됩니다.

## 컴포넌트 import/export 하는 방법
`ESM` 모듈과 비슷한 형태로 컴포넌트를 내보내고 가져올 수 있습니다.

### default import/export
```jsx
import Gallery from './Gallery.js';

export default function App() {
  return (
    <Gallery />
  );
}
```

```jsx
function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

### named import/export
```jsx
import { Profile } from './Gallery.js';

export default function App() {
  return <Profile />;
}
```

```jsx
export function Profile() {
  // ...
}
```
