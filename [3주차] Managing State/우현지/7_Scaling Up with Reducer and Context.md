# reducer와 context로 확장하기

> reducer의 state, dispatch를 props로 전달하는 대신 context에 넣어서 사용할 수 있다.

1. Context를 생성한다.

   - state 에 대한 Context 생성
   - dispatch 에 대한 Context 생성

   ```js
   import { createContext } from "react";

   export const TasksContext = createContext(null);
   export const TasksDispatchContext = createContext(null);
   ```

2. State 와 dispatch 함수를 context에 넣는다.
3. 트리 안에서 context를 사용한다.
