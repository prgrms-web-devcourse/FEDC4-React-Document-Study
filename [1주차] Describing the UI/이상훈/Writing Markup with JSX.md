> 이번 파트에서 배울 내용
> - 리액트는 왜 마크업과 렌더링 로직을 같이 사용하는가?  
> - JSX와 HTML은 어떤 점이 다른가?  
> - JSX를 통해서 정보를 보여주는 법  

## JSX란?
JSX는 JavaScript 파일에서 HTML과 비슷한 마크업을 작성할 수 있도록 해주는 JavaScript 확장 문법입니다.  

## JSX 사용 이유  
원래 웹은 HTML, CSS, JavaScript를 기반으로 각각 다른 파일에 작성되어 왔습니다.  
그러나 Web이 점점 인터랙티브해지면서 로직이 마크업을 결정하는 경우가 많아졌고, 그래서 리액트는 렌더링 로직과 마크업을 같은 위치에 놓도록 결정했습니다.  

## JSX 규칙
JSX는 HTML과는 다르게 몇몇 엄격한 규칙이 존재합니다.

### 1. 하나의 루트 엘리먼트를 반환해야 함
JSX는 HTML처럼 보이지만 사실 일반 JavaScript 객체로 변환되기 때문에, 반드시 하나의 객체를 반환하는 형태여야 합니다.  
`React.Fragment` 를 사용해서 최상위 객체를 하나로 묶어주는 방법도 있습니다.  

```jsx
<>
  <div>
    <h1>Hedy Lamarr's Todos</h1>
    <img
      src="https://i.imgur.com/yXOvdOSs.jpg"
      alt="Hedy Lamarr"
      class="photo"
    >
    <ul>
      ...
    </ul>
  </div>
<>
```

### 2. 모든 태그를 닫아야 함  
일반 HTML 에서는 `<img>` 처럼 태그를 닫지 않아도 되지만, JSX에서는 반드시 `<img />` 형태로 모든 태그를 닫아줘야 합니다.  

### 3. 속성을 대부분의 경우 카멜 케이스로 써야 함  
JSX로 작성된 요소들의 어트리뷰트는 JavaScript 객체의 키로 들어가기 때문에 카멜 케이스로 써야 합니다. `(stroke-width 대신 strokeWidth)`  

또한 `class` 와 같은 예약어는 사용할 수 없기 때문에 `className` 으로 표현합니다.  

단, `aria-*` 와 `data-*` 어트리뷰트는 기존 HTML과 동일하게 대시를 사용하여 표현합니다.  
