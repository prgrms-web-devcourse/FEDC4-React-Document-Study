# JSX

JSX는 리엑트에서 마크업을 나타낼 수 있는 형식이다. 따로 HTML로 작성하지 않고 컴포넌트 내부에서 마크업을 나타내는 이유는 Web이 반응성이 높아지면서 로직이 모양을 결정하는 경우가 많아졌고 렌더링 로직과 같은 곳에서 마크업을 관리하는 게 개발에 용이해졌기 때문이다.

JSX는 마크업을 렌더링하는 문법으로 HTML과 문법이 비슷하지만 일부 상이한 규칙이 존재한다.

- 컴포넌트 내 root 요소는 하나
  
  JSX는 결국 자바스크립트 객체로 변환되기 때문에 하나의 객체로 표현될 수 있도록 작성해야 한다.
  여러 요소를 같은 레벨에 위치시키고 싶다면 Fragment처럼 하나로 묶어줄 수 있는 추가 요소를 사용해야 한다.

- 모든 태그 닫기

  HTML에서 닫지 않고 사용할 수 있는 \<img\> 같은 태그도 \<img /> 처럼 닫는 형식을 추가해서 작성해주어야 한다.

- 속성은 camelCase

  자바스크립트는 변수명에 대시를 포함할 수 없고 일부 예약어도 존재한다. 때문에 대시가 포함된 경우 camelCase로 변경하고 class 처럼 예약어와 겹치는 경우 className으로 대신 작성한다.