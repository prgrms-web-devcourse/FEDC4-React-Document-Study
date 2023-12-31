# 조건부 렌더링

컴포넌트를 로직의 특정 조건에 따라 렌더링을 달리하고 싶다면 분기를 나타낼 수 있는 코드와 함께 렌더링을 사용할 수 있다. 관련 코드의 종류는 다양하다.

- if

  가장 기본적으로 if 조건에 따라 분기한 뒤 조건마다 다른 마크업을 반환해줄 수 있다.

  조금 다르게 조건에 따라 변수에 다른 JSX를 할당한 뒤 변수를 최종 반환문에 활용하는 방법도 있다.

- 삼항 연산자

  JSX 내부에서 중괄호를 통해 삼항 연산사를 사용하면 더 깔끔하게 두 컴포넌트 사이 분기를 조절할 수 있다.

- 논리 AND 연산자

  두 컴포넌트 사이가 아니라 한 컴포넌트의 렌더링 여부를 조절하고 싶다면 boolean으로 나타낼 수 있는 조건식과 함께 `&&` 연산자를 사용하여 분기를 나타낼 수 있다. 

