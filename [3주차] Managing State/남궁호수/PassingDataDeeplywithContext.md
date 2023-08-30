# context로 데이터를 전달해보자

prop drilling 문제를 해결하기 위한 context

heading 컴포넌트에 prop값을 줄 필요가 없다.
위에 가장 가까운 section에 요청하여 level 값을 알게된다.
```jsx
App.js
import Heading from './Heading.js';
import Section from './Section.js';

export default function Page() {
  return (
    <Section level={1}>
      <Heading>Title</Heading>
      <Section level={2}>
        <Heading>Heading</Heading>
        <Heading>Heading</Heading>
        <Heading>Heading</Heading>
        <Section level={3}>
          <Heading>Sub-heading</Heading>
          <Heading>Sub-heading</Heading>
          <Heading>Sub-heading</Heading>
          <Section level={4}>
            <Heading>Sub-sub-heading</Heading>
            <Heading>Sub-sub-heading</Heading>
            <Heading>Sub-sub-heading</Heading>
          </Section>
        </Section>
      </Section>
    </Section>
  );
}

Section.js
import { LevelContext } from './LevelContext.js';

export default function Section({ level, children }) {
  return (
    <section className="section">
      <LevelContext.Provider value={level}>
        {children}
      </LevelContext.Provider>
    </section>
  );
}

Heading.js
import { useContext } from 'react';
import { LevelContext } from './LevelContext.js';

export default function Heading({ children }) {
  const level = useContext(LevelContext);
  switch (level) {
    case 1:
      return <h1>{children}</h1>;
    case 2:
      return <h2>{children}</h2>;
    case 3:
      return <h3>{children}</h3>;
    case 4:
      return <h4>{children}</h4>;
    case 5:
      return <h5>{children}</h5>;
    case 6:
      return <h6>{children}</h6>;
    default:
      throw Error('Unknown level: ' + level);
  }
}

LevelContext.js
import { createContext } from 'react';

export const LevelContext = createContext(1);

```

section에도 level을 넣을 필요가 없다.

```jsx
Section.js
import { useContext } from 'react';
import { LevelContext } from './LevelContext.js';

export default function Section({ children }) {
  const level = useContext(LevelContext);
  return (
    <section className="section">
      <LevelContext.Provider value={level + 1}>
        {children}
      </LevelContext.Provider>
    </section>
  );
}

```
context를 사용하여 색상 테마, 현재 로그인한 사용자 등 전체 하위 트리에 필요한 모든 정보를 전달할 수 있다.


수십개의 props를 수십개의 컴포넌트에 전달해야 하는 경우 (존재) 우선은 props 전달을 기본으로 할 것. 

### context 사례
1. 색상 테마
2. 현재 로그인한 사용자에 대한 정보
3. 라우팅?!

### 정리
 context는 컴포넌트가 그 아래 전체 트리에 일부 정보를 제공할 수 있도록 한다.
 context를 사용하면 '주변환경과 비슷한' 컴포넌트를 작성할 수 있다.
 context를 사용하기 전에는, 1. props 전달. 2. JSX를 children으로 전달.