> 이번 파트에서 배울 내용
> - 컴포넌트란 무엇인가?
> - React 애플리케이션에서 컴포넌트의 역할은?  
> - 어떻게 컴포넌트를 작성하는가?  

## UI 구성요소
```html
<article>
  <h1>My First Component</h1>
  <ol>
    <li>Components: UI Building Blocks</li>
    <li>Defining a Component</li>
    <li>Using a Component</li>
  </ol>
</article>
```

웹에서는 HTML을 통해 위와 같은 마크업 기반의 구조로 문서를 구성합니다.  

리액트를 사용하면 마크업, CSS, JavaScript를 **하나의 사용자 정의 컴포넌트로 결합**하여 재사용할 수 있습니다. 만약 위 마크업을 `<TableOfContents />` 컴포넌트라고 한다면, 위 내용의 마크업이 필요할 때 마다 일일히 중복적으로 작성하지 않더라도 `TableOfContents` 컴포넌트만 가져와서 **재사용**하는 것이 가능합니다.  

HTML 태그와 마찬가지로 컴포넌트도 내부적으로 `article`, `h1`, `li` 등의 태그는 동일하게 사용됩니다.  

## 컴포넌트 정의
리액트에서 컴포넌트는 마크업으로 뿌릴 수 있는 **JavaScript 함수** 입니다.  
컴포넌트를 빌드하는 방법은 다음과 같습니다:

### 1단계: 컴포넌트 내보내기
`export default` 키워드로 함수를 반환해야 합니다.  

### 2단계: 함수 정의  
`function Profile() { }` 와 같은 내용으로 함수를 정의합니다.

### 3단계: 마크업 반환
함수가 반환하는 내용은 마크업의 내용이어야 합니다.  

```jsx
return (
  <div>
    <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  </div>
);
```

### 최종 컴포넌트
즉 컴포넌트는 위 3가지의 단계를 통해서 다음과 같은 형태를 지닙니다:  

```jsx
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3Am.jpg"
      alt="Katherine Johnson"
    />
  )
}
```

## 주의사항
- 리액트 컴포넌트는 대문자로 시작해야 합니다.  
- 컴포넌트 내부에서 다른 컴포넌트를 중복해서 정의하면 안됩니다.  

```jsx
export default function Gallery() {
  // 🔴 이렇게 하면 안됩니다!!
  function Profile() {
    // ...
  }
}
```
