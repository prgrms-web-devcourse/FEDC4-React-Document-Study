# Reducer x Context
Reducer를 사용하면 컴포넌트의 state 업데이트 로직을 통합할 수 있다. Context를 사용하면 하위 컴포넌트들에 정보를 전달할 수 있다. 둘을 결합하면 하위 컴포넌트 여러 곳에서 state 업데이트 로직을 선언적으로 사용할 수 있다.

1. tasks context와 dispatch context 두 가지로 나누어 생성한다.
2. 상위 컴포넌트에서 `useReducer`를 통해 tasks와 dispatch를 받고 두 context의 provider로 하위 트리를 감싼다.
3. 하위 컴포넌트에서 `useContext`를 이용해 tasks나 dispatch를 요청한다.

파일을 분리해서 컴포넌트를 더 정리할 수 있다.

1. 두 provider를 하나의 wrapper 컴포넌트로 선언한다. 위치는 context 파일이다.
2. reducer 함수도 해당 위치로 옯기고 provider 컴포넌트에서 사용한다.
3. 합쳐진 provider 컴포넌트를 상위 컴포넌트에서 사용한다.

이렇게 하면 기존 컴포넌트들이 데이터를 어디서 갖고오는 지보다 무엇을 보여줄 것인지에 집중할 수 있어 관심사를 분리할 수 있다.