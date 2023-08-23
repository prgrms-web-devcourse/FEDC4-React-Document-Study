# State: A Component's Memory (State: 컴포넌트의 메모리)

## state 변수 추가하기

- state 변수를 추가하려면 파일 상단에 있는 React에서 useState를 import한다.

  ```js
  import { useState } from "react";

  const [index, setIndex] = useState(0);
  ```

  - index는 state의 변수이고 setIndex는 설정자 함수이다.

## 첫번째 훅 \_ useState

- "use"로 시작하는 다른 함수를 Hook이라고 한다.
- Hook 은 React가 렌더링 중일 때만 사용될 수 있는 특별한 함수이다.

- 컴포넌트가 렌더링사이에 일부 정보를 기억해야 할 때 state변수를 사용한다.
- state는 컴포넌트 인스턴스에 지역적이다. 즉,동일한 컴포넌트를 두 군데에서 렌더링하면 각 사본은 완전히 격리된 state를 갖게 된다!
  이 중 하나를 변경해도 다른 컴포넌트에는 영향을 미치지 않는다.
